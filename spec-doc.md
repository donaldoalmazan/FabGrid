# Manufacturing Shop Management — Module Specifications

## Overview

An open-source, modular manufacturing management application built on **PromiseGrid** for small-scale manufacturers, makers, and microfactories.

The initial proof of concept (POC) supports a single shop managing products, customer orders, inventory, production activities, and basic financial operations.

The software should accommodate different fabrication and assembly processes, including 3D printing, electronics, machining, and other manufacturing methods, without requiring specialized workflows for each.

### Core Modules

1. **Products** — What can we make or sell?
2. **Orders** — What has been requested or purchased?
3. **Inventory** — What materials, components, and finished goods do we have?
4. **Production** — What work needs to be performed, and with which resources?

Basic financial capabilities are incorporated into these modules rather than implemented as a separate accounting system.

The modules represent logical domain boundaries within one application, not necessarily independent applications.

### Design Principles

- **Modular:** Maintain clear functional boundaries that allow capabilities to evolve independently.
- **Progressive complexity:** Introduce functionality as operational needs grow, avoiding unnecessary configuration.
- **Local-first and shop-owned:** Shops retain control of their data and infrastructure.
- **Manufacturing-agnostic:** Support different fabrication methods through common concepts such as products, operations, and resources.
- **Agent-oriented:** Model coordination through agents and commitments, informed by Promise Theory.
- **Manual-first:** Support human-operated workflows before introducing automation.
- **Interoperable:** Preserve the ability to connect external applications, including accounting software.

---

## 1. Products

**Purpose:** Define and manage the goods and services the shop offers or manufactures.

The Products module maintains product specifications independently of individual orders and inventory quantities.

### Core Capabilities

- Create, view, edit, and archive products.
- Assign product identifiers, names, descriptions, and categories.
- Define product variants, such as size, color, or material.
- Associate design files, images, and documentation.
- Set selling prices and estimated unit costs.
- Define components and materials through Bills of Materials (BOMs).
- Associate manufacturing requirements or instructions.
- Identify products as made-to-order, stocked, or custom.
- Manage which products are published to the storefront.

### Primary Entities

- Product
- Product Variant
- Product Specification
- Bill of Materials (BOM)
- Design File / Document

### Relationships

- **Orders:** References products and selected configurations.
- **Inventory:** Tracks stocked products and components.
- **Production:** Uses product specifications and BOMs to determine manufacturing requirements.

### Initial POC Scope

A basic product catalog with descriptions, pricing, optional design files, and simple BOMs.

BOMs and manufacturing instructions should remain optional for simple products.

---

## 2. Orders

**Purpose:** Manage customer requests, purchases, commitments, and fulfillment.

The Orders module represents agreements between customers and the shop, including what was requested, quantities, pricing, and delivery expectations.

### Core Capabilities

- Create and manage customer orders.
- Support multiple products within an order.
- Reference existing products and selected variants.
- Accept custom fabrication requests and uploaded files.
- Record quantities, agreed prices, customer information, and due dates.
- Generate and manage quotes.
- Track order status and history.
- Associate orders with production jobs.
- Track invoices, payment status, and outstanding balances.
- Manage fulfillment, including packing, shipping labels, tracking numbers, and delivery status.
- Represent customer and shop commitments.

### Primary Entities

- Customer
- Order
- Order Line Item
- Quote / Request for Quote (RFQ)
- Invoice
- Payment Record
- Shipment / Fulfillment Record

### Relationships

- **Products:** Identifies what the customer purchased or requested.
- **Inventory:** Checks availability of finished products and required materials.
- **Production:** Initiates manufacturing activities required to fulfill the order.

### Initial POC Scope

Manual order creation, product selection, quantities, agreed prices, order statuses, and fulfillment tracking.

Include basic invoice generation and manual payment recording. Payment processing, automated tax calculation, and carrier integrations can be added later.

---

## 3. Inventory

**Purpose:** Track physical materials, components, consumables, and finished goods available to the shop.

The Inventory module maintains information about stock quantities, availability, locations, and movements.

### Core Capabilities

- Create and manage inventory items.
- Track raw materials, purchased components, consumables, work-in-progress, and finished goods.
- Record quantities and units of measurement.
- Track inventory locations.
- Record stock movements, consumption, and replenishment.
- Associate items with suppliers and purchase costs.
- Track inventory acquisition costs.
- Identify shortages for production activities.
- Reserve materials or finished goods for orders.
- Support manual adjustments and physical inventory counts.

### Primary Entities

- Inventory Item
- Stock Balance
- Inventory Transaction / Movement
- Storage Location
- Supplier
- Stock Reservation

### Relationships

- **Products:** References product and component definitions.
- **Orders:** Provides stock availability for fulfillment.
- **Production:** Supplies materials and receives completed products.

### Initial POC Scope

Basic inventory records, quantities, units, purchase costs, and manual stock adjustments.

Support simple associations with products and production jobs. Automated purchasing, advanced reservations, and inventory valuation can be deferred.

---

## 4. Production

**Purpose:** Plan, coordinate, execute, and track manufacturing activities.

The Production module manages the work required to fabricate and assemble products, independently of any particular manufacturing technology.

### Core Capabilities

- Create and manage production jobs or work orders.
- Associate jobs with customer orders or internal stock replenishment.
- Define manufacturing operations and dependencies.
- Identify materials, equipment, and other required resources.
- Assign operations to human, machine, or software agents.
- Track production status, quantities, and completion.
- Record material consumption and finished outputs.
- Estimate and record labor, machine, and material costs.
- Record production time, observations, and issues.
- Associate inspection results and evidence of completed work.
- Represent responsibilities and commitments using Promise Theory concepts.

### Primary Entities

- Production Job / Work Order
- Operation / Task
- Resource / Work Center
- Agent
- Commitment / Promise
- Production Record
- Completion Evidence

### Relationships

- **Products:** Supplies specifications, BOMs, and manufacturing requirements.
- **Orders:** Provides demand for production.
- **Inventory:** Supplies materials and receives completed products.

### Initial POC Scope

Create production jobs, define simple operations, associate them with products or orders, manually update progress, and record completion.

Support basic estimated and actual production costs where useful.

Machine integrations, automated scheduling, detailed resource planning, and complex workflow orchestration are deferred.

---

## 5. Shared Financial Capabilities

**Purpose:** Support essential business transactions and operational cost tracking without implementing a full accounting system.

Financial functionality is distributed across the four core modules, while financial records remain logically distinct from operational records.

| Capability | Module | Initial POC |
|---|---|---|
| Product pricing | Products | Yes |
| Estimated unit costs | Products | Basic |
| Customer quotes | Orders | Basic |
| Invoicing | Orders | Basic |
| Payment status | Orders | Manual |
| Shipping charges | Orders | Basic |
| Supplier purchase costs | Inventory | Basic |
| Inventory valuation | Inventory | Deferred |
| Material consumption costs | Production | Basic |
| Labor and machine costs | Production | Basic |
| General ledger / bookkeeping | External accounting system | Deferred |
| Bank reconciliation | External accounting system | Deferred |
| Tax accounting and financial reporting | External accounting system | Deferred |

### Architectural Considerations

- Financial records should reference operational records without being inseparably embedded within them.
- Order fulfillment and payment status must be tracked independently.
- Quotes and invoices should preserve agreed prices even when product catalog prices change.
- Cost estimates and actual costs should be distinguishable.
- Financial data should remain exportable for use with external bookkeeping systems.
- Full accounting functionality is outside the scope of the initial POC.

The POC's financial records provide operational visibility, not a substitute for formal bookkeeping.

---

## 6. User Interfaces

The initial application provides a small set of web interfaces that interact with the underlying modules.

An interface may use functionality from several modules without constituting a separate module itself.

| Interface | Purpose | Modules |
|---|---|---|
| Product Catalog / Editor | Define and manage products, prices, and specifications | Products |
| Storefront | Display published products and accept customer orders | Products, Orders |
| Order Management | Manage orders, quotes, invoices, payment status, and fulfillment | Orders |
| Inventory Management | Track stock, materials, and inventory movements | Inventory |
| Production Workspace | Manage work orders, operations, resources, and progress | Production |
| Dashboard | Summarize active orders, work in progress, and outstanding activities | All |

### Initial Interface Priorities

Prioritize Product Catalog, Order Management, Inventory Management, and Production Workspace.

The storefront can initially be minimal, while the dashboard can be introduced once meaningful operational data exists.

Shipping and invoicing should initially be capabilities within the Order Management interface rather than separate interfaces.

---

## 7. PromiseGrid Integration

**Purpose:** Demonstrate how a practical, single-shop manufacturing application can operate on PromiseGrid infrastructure.

PromiseGrid provides the underlying data and coordination substrate, while the application defines the manufacturing domain and user-facing workflows.

### Architectural Objectives

- Store and reference manufacturing records through PromiseGrid-compatible structures.
- Maintain relationships between products, orders, inventory, and production records.
- Preserve shop ownership and control over operational data.
- Support locally operated workflows without dependence on a centralized manufacturing platform.
- Model agents, commitments, and observations informed by Promise Theory.
- Keep domain-specific application logic separate from general-purpose infrastructure capabilities.
- Allow future applications to interact with relevant records through explicitly defined interfaces and data structures.

### Agent Model

Potential agents within a single shop include:

- Shop operator or production worker
- Machine or machine-controller agent
- Inventory management agent
- Supplier or supplier-representing agent
- AI or software service agent
- Shop business agent

An agent represents an independently modeled participant capable of making or assessing promises. Physical resources, inventory items, and passive equipment do not necessarily need to be agents themselves.

### Initial POC Scope

Implement only the PromiseGrid capabilities needed to support the initial manufacturing workflow.

Manual actions can represent agent activity until automated integrations become useful.

Multi-shop coordination, generalized agent orchestration, and distributed scheduling are outside the initial scope.

---

## 8. Initial End-to-End Workflow

The POC should demonstrate one complete operational cycle:

1. **Products:** Define a product, selling price, and optional BOM.
2. **Orders:** Create a customer order referencing the product.
3. **Inventory:** Check available finished goods and materials.
4. **Production:** Create a work order for the required quantity.
5. **Production:** Record manufacturing operations, progress, and completion.
6. **Inventory:** Record consumed materials and completed products.
7. **Orders:** Prepare the order for fulfillment and record shipping.
8. **Orders:** Generate an invoice and record payment status.
9. **Orders:** Mark the order fulfilled.

The workflow should accommodate simple products without requiring every optional capability.

---

## 9. Out of Scope for Initial POC

- Multi-shop production coordination.
- Full ERP, MES, or accounting functionality.
- General ledger and double-entry accounting.
- Automated tax calculation and filing.
- Bank synchronization and reconciliation.
- Integrated payment processing.
- Advanced procurement and purchasing workflows.
- Automated shipping carrier integrations.
- Advanced production scheduling and optimization.
- Machine control and industrial IoT integration.
- Automated AI-based decision-making.
- Complex warehouse management.
- General-purpose plugin infrastructure.

These capabilities may be added incrementally based on actual shop requirements.

---

## 10. Initial Success Criteria

The POC succeeds when a single shop operator can:

1. Define a product and its basic specifications.
2. Record a customer order with quantities and agreed prices.
3. Track available materials and inventory quantities.
4. Create and complete a production job.
5. Record inventory consumption and finished products.
6. Track basic production costs.
7. Generate a basic invoice and record payment status.
8. Fulfill and close the customer order.
9. Retain and inspect relevant operational records through the PromiseGrid-backed application.

**Primary objective:** Demonstrate a functional, lightweight manufacturing management application built on PromiseGrid, with modular domain boundaries and an architecture capable of adapting to increasingly complex shop operations.

The initial goal is not to reproduce a commercial ERP or MES system, but to establish a practical foundation that can evolve alongside the needs of a small manufacturing business.