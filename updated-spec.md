# Manufacturing Shop Management — Application Specifications

**Status:** Draft — Proof of Concept  
**Architecture:** Independent applications built on PromiseGrid  
**Scope:** Single-shop, small-scale manufacturing

## 1. Overview

An open-source, modular manufacturing management system built on **PromiseGrid** for independent manufacturers, makers, and microfactories.

The system supports the operation of a small-scale manufacturing business, including product management, customer orders, purchasing, inventory, production, fulfillment, and financial management.

It should accommodate diverse fabrication and assembly processes, including 3D printing, electronics, machining, ceramics, and other manufacturing methods, without requiring specialized workflows for each process.

The initial proof of concept (POC) focuses on operating a single shop. It also serves as a practical experiment in building independent, interoperable applications on PromiseGrid infrastructure.

### Core Applications

1. **Products** — What can we make or sell?
2. **Orders** — What has been requested or purchased?
3. **Inventory** — What materials, components, and finished goods do we have?
4. **Production** — What work needs to be performed, and with which resources?
5. **Accounting** — How is the business performing financially, and what economic activity must be recorded?

These are independent applications, not modules within a monolithic application.

Each application interfaces with the **PromiseGrid kernel** to access and operate on shared data. Applications exchange relevant information through PromiseGrid rather than directly accessing one another's internal state.

Accounting principles inform the design of all applications, rather than being confined to the Accounting App.

---

## 2. Architectural Principles

### 2.1 Independent Applications

Each application has a distinct responsibility and can be developed, operated, and extended independently.

Applications should:

- Maintain clear functional boundaries.
- Interface with the PromiseGrid kernel for relevant data operations.
- Reference and exchange information through PromiseGrid.
- Avoid direct dependencies on another application's internal implementation.
- Remain useful without requiring every other application to be installed or configured, where practical.

Application independence does not imply that every application must maintain its own duplicate dataset.

### 2.2 PromiseGrid as Shared Infrastructure

PromiseGrid provides the underlying data and coordination infrastructure.

Applications use the kernel to work with shared, referenced, or independently maintained information according to the capabilities and conventions supported by PromiseGrid.

The POC should investigate appropriate mechanisms for:

- Representing and referencing application data.
- Exchanging information between applications.
- Maintaining relationships between records.
- Tracking changes and their origins.
- Coordinating activities among agents.
- Controlling access to business information.

Specific kernel APIs, data schemas, and synchronization behaviors remain implementation decisions.

### 2.3 Accounting-First Design

**Economic activity is a foundational concern of the entire system.**

Products, orders, purchasing, inventory, and production should be modeled with consideration for their economic implications.

For example:

- Purchasing materials creates costs and potentially financial obligations.
- Holding inventory represents resources with economic value.
- Manufacturing consumes resources and produces goods.
- Customer orders establish commercial commitments.
- Invoicing and payments affect receivables and cash flow.

The system should distinguish operational events from the accounting entries that may result from them.

Ideally, economically meaningful events can support appropriate accounting records, potentially through double-entry bookkeeping.

**This is an architectural hypothesis to investigate, not a settled implementation requirement.**

At minimum, applications should preserve the information necessary to streamline or automate financial recordkeeping through additional applications.

### 2.4 Progressive Complexity

Applications should scale with the operational complexity of the shop.

A one-person shop should be able to use basic product, order, inventory, and production workflows without configuring enterprise-scale manufacturing infrastructure.

More advanced capabilities should be introduced as needed.

### 2.5 Shop-Owned Infrastructure

The shop maintains ownership and control over its operational and financial data.

The system should support local operation without requiring dependence on a centralized third-party manufacturing platform.

### 2.6 Manufacturing-Agnostic Design

Core manufacturing concepts should remain independent of specific fabrication methods.

Products, components, resources, operations, and production records should support diverse processes without requiring entirely different application architectures.

### 2.7 Agent-Oriented Coordination

Activities should be modeled with awareness of agents and commitments, informed by Promise Theory.

Agents may represent humans, machines, suppliers, software services, or other independently modeled participants.

Not every physical resource must be represented as an autonomous agent.

### 2.8 Manual Operation Before Automation

Human operators should be able to enter information, record activity, and complete workflows manually.

Automation should improve these workflows rather than be required for basic operation.

---

# 3. Application Specifications

## 3.1 Products App

**Purpose:** Define and manage the goods and services the shop offers or manufactures.

The Products App maintains product definitions independently of individual customer orders and physical stock quantities.

### Initial POC

A basic product catalog with descriptions, pricing, optional design files, and simple BOMs.

Complex product configuration and revision management can be deferred.

### Core Capabilities

- Create, view, edit, and archive products.
- Assign identifiers, names, descriptions, and categories.
- Define product variants, such as material, size, or color.
- Associate design files, images, and documentation.
- Define selling prices and estimated unit costs.
- Define Bills of Materials (BOMs).
- Associate manufacturing requirements and instructions.
- Distinguish made-to-order, stocked, and custom products.
- Designate products available through a storefront.
- Preserve relevant product revisions.

### Primary Entities

- Product
- Product Variant
- Product Specification
- Bill of Materials
- Design File / Document

### Economic Considerations

Product definitions should support pricing and cost estimation.

Selling prices, estimated costs, and target margins should be distinguishable. Changes to catalog prices should not retroactively alter agreed prices on existing orders.

---

## 3.2 Orders App

**Purpose:** Manage customer requests, commercial agreements, sales, and fulfillment.

The Orders App represents transactions and commitments between the shop and its customers.

### Initial POC

Manual order creation, quotes, agreed prices, invoicing, payment recording, and basic fulfillment tracking.

Shipping label generation, carrier integrations, payment gateways, and automated tax calculations can be added later.

### Core Capabilities

- Create and manage customer orders.
- Support orders containing multiple products.
- Reference existing products and configurations.
- Accept custom fabrication requests and uploaded files.
- Record quantities, prices, customer information, and due dates.
- Create and manage quotes.
- Track order status and history.
- Associate orders with production jobs.
- Generate and manage invoices.
- Record payments, refunds, and outstanding balances.
- Track customer and shop commitments.
- Manage fulfillment, including packing, shipping, and delivery status.

### Primary Entities

- Customer
- Order
- Order Line Item
- Quote / Request for Quote
- Invoice
- Payment Record
- Shipment / Fulfillment Record

### Economic Considerations

The Orders App should preserve agreed commercial terms and distinguish:

- Order placement
- Order acceptance
- Invoicing
- Payment
- Fulfillment
- Refunds or cancellations

These events have different operational and financial meanings.

Order fulfillment and payment status should remain independent.

---

## 3.3 Inventory App

**Purpose:** Track the materials, components, consumables, and finished goods available to the shop.

The Inventory App records physical stock and the movements that affect availability.

### Initial POC

Basic inventory records, stock quantities, purchase costs, manual adjustments, and material consumption associated with production.

Support simple purchase and supplier records without implementing a comprehensive procurement system.

### Core Capabilities

- Create and manage inventory item records.
- Track raw materials, purchased components, consumables, work-in-progress, and finished goods.
- Record quantities and units of measurement.
- Identify storage locations.
- Record stock receipts, movements, consumption, and adjustments.
- Associate inventory with suppliers and acquisition costs.
- Record purchases and replenishment.
- Identify material shortages.
- Support stock reservations.
- Perform physical inventory counts.

### Primary Entities

- Inventory Item
- Stock Balance
- Inventory Transaction / Movement
- Storage Location
- Supplier
- Purchase Record
- Stock Reservation

### Economic Considerations

Inventory represents both physical resources and economic value.

The application should preserve information needed to understand:

- Material acquisition costs.
- Material consumption.
- Stock on hand.
- Inventory losses and adjustments.
- The costs associated with purchased components and finished products.

Formal inventory valuation methods and accounting treatment require further investigation.

A supplier purchase, physical receipt of goods, and payment should remain distinguishable events.

---

## 3.4 Production App

**Purpose:** Plan, coordinate, execute, and track manufacturing and assembly activities.

The Production App manages work performed to create products, independently of any specific fabrication technology.

### Initial POC

Create production jobs, define simple operations, record material consumption, manually update progress, and record completion.

Support basic estimated and actual production costs where practical.

Automated machine control, advanced scheduling, and detailed costing methods can be introduced later.

### Core Capabilities

- Create and manage production jobs or work orders.
- Associate jobs with customer orders or internal stock replenishment.
- Define manufacturing operations and dependencies.
- Identify required materials, components, and resources.
- Assign activities to human, machine, or software agents.
- Track production quantities, status, and completion.
- Record material consumption and finished output.
- Estimate and record labor, material, and machine costs.
- Record production time and observations.
- Associate inspections and quality records.
- Represent operational commitments and evidence of completion.

### Primary Entities

- Production Job / Work Order
- Operation / Task
- Resource / Work Center
- Agent
- Commitment / Promise
- Production Record
- Completion Evidence

### Economic Considerations

Production transforms resources into goods.

The application should preserve information about:

- Materials consumed.
- Labor performed.
- Machine time and operating costs.
- Finished output.
- Scrap, waste, and rework.
- Estimated versus actual production costs.

These records should support product costing and financial analysis.

Determining how production activity translates into formal accounting entries remains an area for experimentation.

---

## 3.5 Accounting App

**Purpose:** Aggregate, interpret, and manage financial information to support the operation and financial viability of the shop.

The Accounting App provides dedicated financial interfaces and records while working with economic information generated throughout the other applications.

Accounting principles are not exclusive to this application.

### Initial POC

Prioritize basic financial operations:

- Recording expenses and purchases.
- Tracking invoices and payments.
- Monitoring outstanding balances.
- Providing a basic cash-flow view.
- Summarizing revenue and costs.
- Exporting relevant records.

Full general ledger functionality, automated journal posting, bank reconciliation, financial statements, and tax document generation remain roadmap capabilities.

The initial system should not be assumed to provide complete or compliant bookkeeping without validation.

### Core Capabilities

- View and manage financial records.
- Track business income and expenses.
- Track receivables and payables.
- Monitor payments and cash flow.
- Aggregate purchasing, sales, inventory, and production costs.
- Generate financial summaries and reports.
- Support financial forecasting.
- Produce or export records for bookkeeping and tax preparation.
- Support reconciliation and formal accounting workflows as capabilities mature.

### Primary Entities

- Economic Event
- Financial Transaction
- Account
- Journal Entry
- Receivable / Payable
- Expense
- Financial Report
- Forecast

Not all entities require full implementation in the initial POC.

### Relationship to Other Applications

The Accounting App uses information made available through PromiseGrid.

For example:

- **Products:** Pricing and estimated costs.
- **Orders:** Invoices, payments, refunds, and receivables.
- **Inventory:** Purchases, acquisition costs, and material movements.
- **Production:** Resource consumption, production costs, and completed output.

The Accounting App should not be the sole mechanism by which financial considerations enter the system.

---

# 4. Financial Architecture and Research Questions

The POC will investigate how accounting principles can be incorporated into independently developed applications sharing PromiseGrid infrastructure.

## 4.1 Economic Events vs. Accounting Entries

An **economic event** describes something that occurred in the business.

Examples include:

- Materials purchased.
- Goods received.
- Inventory consumed.
- Products manufactured.
- Customer invoiced.
- Payment received.

An **accounting entry** records the financial consequences of an event under an accounting method and set of rules.

Not every operational event necessarily creates a journal entry, and one event may require multiple accounting consequences.

The architecture should preserve this distinction.

## 4.2 Distributed Accounting Responsibilities

Possible approaches include:

1. Applications generate standardized economic event records.
2. Applications generate accounting entries directly.
3. A dedicated Accounting App interprets economic events and produces accounting entries.
4. A hybrid approach distributes some accounting responsibilities while centralizing others.

The appropriate division of responsibility is unresolved.

Experiments should evaluate correctness, complexity, interoperability, and the risk of duplicated or inconsistent financial records.

## 4.3 Financial Integrity

The architecture should investigate mechanisms for:

- Linking financial records to their originating business activities.
- Preserving historical records and corrections.
- Avoiding duplicate recognition of transactions.
- Distinguishing estimates from recorded transactions.
- Tracking obligations independently of payments.
- Reconciling records between applications.
- Establishing authority for creating and modifying financial records.

Financial integrity is particularly important because the applications share infrastructure without relying on direct access to one another's internal state.

## 4.4 Accounting Methods

The initial POC must investigate the implications of cash-basis and accrual-basis accounting.

The system should avoid assuming that receiving an order, issuing an invoice, receiving payment, or fulfilling an order are financially equivalent.

Formal accounting behavior should be based on an explicitly selected method and validated accounting rules.

---

# 5. User Interfaces

Each application may provide its own interface.

Additional interfaces can use information from multiple applications through PromiseGrid.

| Interface | Purpose | Application |
|---|---|---|
| Product Catalog / Editor | Manage products, prices, and specifications | Products |
| Storefront | Display offerings and accept customer requests | Products / Orders |
| Order Management | Manage orders, quotes, invoices, and fulfillment | Orders |
| Inventory Management | Track materials, stock, and purchases | Inventory |
| Production Workspace | Manage work orders and manufacturing activity | Production |
| Financial Workspace | Review income, expenses, payments, and cash flow | Accounting |
| Dashboard | Summarize activity across applications | Cross-application |

A storefront or dashboard is an interface, not necessarily a separate domain application.

Applications do not need identical interfaces or navigation structures.

## Initial Interface Priorities

Prioritize interfaces needed to complete a real business transaction:

1. Product definition and pricing.
2. Order creation and invoicing.
3. Purchase and inventory recording.
4. Production tracking.
5. Financial records and payment tracking.

A unified dashboard and polished storefront can follow after core workflows are functional.

---

# 6. Initial End-to-End Workflow

The POC should demonstrate a complete operational and financial cycle for a single shop.

### Step 1 — Define a Product

**Products App**

Create a product with a selling price, specifications, and optional BOM.

### Step 2 — Receive a Customer Order

**Orders App**

Record a request, prepare a quote if needed, and accept the order with agreed quantities and prices.

### Step 3 — Acquire or Allocate Materials

**Inventory App**

Check material availability, record purchases where necessary, and track incoming stock and associated costs.

### Step 4 — Perform Production

**Production App**

Create work orders, record operations, consume materials, and track completed output.

### Step 5 — Update Inventory

**Inventory App**

Record material consumption and completed products as appropriate.

### Step 6 — Fulfill the Order

**Orders App**

Record packing, shipping, or customer pickup and update fulfillment status.

### Step 7 — Invoice and Record Payment

**Orders App**

Issue an invoice and record payments and outstanding balances.

Invoicing and payment may occur earlier in the workflow depending on the commercial agreement.

### Step 8 — Review Financial Activity

**Accounting App**

Aggregate relevant financial information to review purchases, expenses, customer payments, cash flow, and basic profitability.

### Step 9 — Inspect Shared Records

**PromiseGrid**

Demonstrate that the independent applications can access relevant business records and maintain relationships through the PromiseGrid kernel.

Financial records should remain traceable to the underlying operational activities where applicable.

---

# 7. POC Priorities

Financial functionality is prioritized according to the practical needs of operating a business.

## Priority 1 — Financial Operations

**Primary objective: Run a financially functional shop.**

Demonstrate:

- Purchasing and expense recording.
- Customer invoicing.
- Payment tracking.
- Outstanding balances.
- Basic cash-flow visibility.
- Commercial commitments associated with orders.

## Priority 2 — Costing and Profitability

**Secondary objective: Understand the economics of products and production.**

Demonstrate:

- Material costs.
- Estimated production costs.
- Pricing and margins.
- Actual versus estimated costs where available.
- Basic profitability analysis.

## Priority 3 — Comprehensive Bookkeeping

**Longer-term objective: Support complete accounting practices.**

Potential capabilities include:

- Double-entry general ledger.
- Automated journal entries.
- Bank reconciliation.
- Financial statements.
- Inventory valuation.
- Tax reporting and documentation.
- More sophisticated financial forecasts.

These priorities indicate implementation emphasis, not a strict requirement to complete one category before beginning another.

---

# 8. Experiments and Architectural Hypotheses

The POC should evaluate several questions rather than prematurely fixing their implementation.

### Experiment A — Independent Application Interoperability

Can Products, Orders, Inventory, Production, and Accounting operate as independent applications while exchanging related data through the PromiseGrid kernel?

**Validation:** Complete a workflow involving records created and referenced by multiple applications without direct access to another application's internal state.

### Experiment B — Economic Event Modeling

Can business operations generate sufficient economic information to support financial recordkeeping?

**Validation:** Trace a purchase, material consumption, product sale, invoice, and payment to their respective operational records and financial consequences.

### Experiment C — Accounting Automation

Can economically meaningful events be translated into appropriate accounting entries?

**Validation:** Prototype a limited set of accounting rules, potentially using double-entry bookkeeping, and evaluate correctness, complexity, and duplication risks.

This experiment does not require committing the entire architecture to automatic journal generation.

### Experiment D — Progressive Complexity

Can applications provide useful functionality with minimal initial configuration?

**Validation:** Operate a simple made-to-order product workflow without requiring complex BOMs, routings, accounting configuration, or machine integrations.

### Experiment E — Agent-Oriented Operations

Can internal production activities be represented through agents and commitments without introducing unnecessary coordination overhead?

**Validation:** Record a small number of commitments, resource assignments, and completion observations within one shop.

---

# 9. Out of Scope for Initial POC

- Multi-shop manufacturing coordination.
- Distributed production scheduling.
- Enterprise-scale ERP functionality.
- Complete accounting and tax compliance.
- Automated bank synchronization and reconciliation.
- Advanced procurement optimization.
- Integrated payment processing.
- Automated shipping carrier integrations.
- Advanced warehouse management.
- Machine control and industrial IoT.
- Automated AI production scheduling.
- General-purpose plugin infrastructure.
- Fully configurable manufacturing workflow engines.

These remain potential extensions rather than prerequisites.

---

# 10. Initial Success Criteria

The POC succeeds when a single shop operator can:

1. Define a product with specifications and pricing.
2. Create an order and record its commercial terms.
3. Record material purchases and associated costs.
4. Track stock availability and material consumption.
5. Create and complete a production job.
6. Fulfill the customer order.
7. Generate an invoice and record payment.
8. Review purchases, expenses, outstanding balances, and cash flow.
9. Inspect basic product and production costs.
10. Trace relevant operational and economic records through PromiseGrid.
11. Demonstrate coordination between independent applications without directly coupling their internal state.

The POC should also generate evidence about which accounting responsibilities belong within individual applications and which are better handled through shared conventions or a dedicated Accounting App.

---

## 11. Long-Term Direction

The long-term objective is an ecosystem of independent, open-source applications that help small manufacturers operate financially sustainable businesses while retaining ownership of their data and infrastructure.

Applications should be composable according to the capabilities a shop needs, rather than requiring installation of a comprehensive ERP platform.

PromiseGrid provides the common infrastructure through which applications exchange information and coordinate activity.

Accounting principles provide a common economic framework for understanding resources, obligations, transactions, and business performance.

The initial single-shop implementation serves as both a practical manufacturing management system and a reference implementation for this broader architecture.

**Central architectural hypothesis:**

Can independent manufacturing applications built on PromiseGrid represent operational and economic activity in a way that enables financially viable shop operations and progressively automates accounting, without requiring a monolithic ERP system?