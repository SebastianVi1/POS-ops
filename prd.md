# PRODUCT REQUIREMENTS DOCUMENT (PRD)
# Perfume POS — Retail Management System

**Document version:** 1.0  
**Status:** Ready for technical planning  
**Product type:** Point of Sale (POS)  
**Initial market:** Mexico  
**Currency:** MXN  
**Language:** Spanish  
**Platforms:** Web + Desktop (Windows)  
**Architecture:** Offline-first, modular monolith  
**Primary database:** SQLite (local) / PostgreSQL (central)  

---

# 1. Executive Summary

Perfume POS is a comprehensive Point of Sale and Retail Management System designed for perfume stores.

The application will centralize the store's commercial operations, including sales, inventory, products, customers, suppliers, purchases, cash management, expenses, and business reporting.

The system will be available through two platforms:

- Web application (PWA).
- Desktop application (Windows).

A fundamental requirement is offline-first operation. The system must allow employees to perform essential business operations without an active Internet connection.

When connectivity becomes available, locally stored operations will synchronize with the central backend without creating duplicate transactions or inconsistent inventory records.

The initial product will target a single physical store, with an architecture designed to accommodate future multi-terminal and multi-branch capabilities.

# 2. Product Vision

Build a reliable, efficient, and easy-to-use retail management system that allows perfume store owners to control their entire business from a single platform.

The system should prioritize:

1. Operational reliability.
2. Inventory accuracy.
3. Fast checkout operations.
4. Financial transparency.
5. Offline availability.
6. Data integrity and traceability.
7. Future extensibility.

# 3. Problem Statement

Small and medium-sized perfume stores often manage their operations through disconnected systems, spreadsheets, manual records, or basic checkout applications.

This creates several problems:

- Lack of centralized inventory management.
- Difficulty tracking product costs and profit margins.
- Inconsistent sales records.
- Limited visibility into daily financial operations.
- Difficulty managing suppliers and purchases.
- Lack of customer purchase history.
- Dependence on Internet connectivity.
- Limited accountability for employee operations.

Perfume POS addresses these problems through a unified retail management platform.

# 4. Product Goals

## 4.1 Primary Goals

- Provide a complete POS checkout experience.
- Centralize product and inventory management.
- Track every inventory movement.
- Manage purchases and suppliers.
- Control cash registers and cash sessions.
- Track income, expenses, and estimated profitability.
- Maintain customer purchase history.
- Provide operational and financial reports.
- Support complete offline sales operations.
- Synchronize local operations with a central backend.

## 4.2 Success Metrics

The following are initial product targets to validate during testing.

| Metric | Target |
|---|---|
| Local product search | Under 300 ms |
| Local sale confirmation | Under 1 second, excluding peripherals |
| Offline checkout availability | 100% of supported checkout workflows |
| Duplicate synchronized sales | Zero |
| Untracked inventory movements | Zero |
| Successful backup restoration | 100% of tested recovery scenarios |
| Critical business operations with audit trail | 100% |

# 5. Target Users

## 5.1 Store Owner

Responsible for the entire business.

Needs:
- Complete business overview.
- Financial reporting.
- Inventory visibility.
- Employee management.
- Product and price management.
- Configuration and permissions.

## 5.2 Store Manager

Responsible for daily operations.

Needs:
- Sales supervision.
- Cash management.
- Inventory management.
- Purchase management.
- Operational reports.

## 5.3 Cashier

Responsible for checkout operations.

Needs:
- Fast product search.
- Barcode scanning.
- Checkout processing.
- Payment registration.
- Receipt printing.
- Cash session management.

## 5.4 Inventory Manager

Responsible for stock accuracy.

Needs:
- Product management.
- Inventory adjustments.
- Stock counts.
- Purchase reception.
- Movement history.

# 6. Platform Requirements

## 6.1 Desktop Application

Target platform: Windows 10/11.

Technology:
- Tauri 2.
- React.
- TypeScript.
- SQLite.

Requirements:
- Local database persistence.
- Native printer integration.
- USB barcode scanner compatibility.
- Local file access.
- Automatic backups.
- Offline application startup.
- Local synchronization queue.

Desktop will be the primary operational environment for the initial release.

## 6.2 Web Application

Technology:
- React.
- TypeScript.
- Vite.
- PWA.
- IndexedDB.
- Dexie.js.

Requirements:
- Installable application.
- Offline application shell.
- Local data persistence.
- Offline sales.
- Synchronization support.
- Responsive interface.

The PWA must be installed or previously loaded before it can operate offline.

## 6.3 Shared Application Architecture

Both applications must share:

- Domain models.
- Business rules.
- Application use cases.
- Validation schemas.
- UI components where practical.
- API contracts.
- Synchronization protocols.

Platform-specific functionality must be isolated behind adapters.

# 7. Functional Requirements

## FR-01: Authentication and Authorization

The system must provide secure access management.

Requirements:

- Login.
- Logout.
- User creation.
- User modification.
- Password management.
- Role management.
- Permission management.
- Session management.
- PIN-based cashier access.
- Login attempt protection.
- Audit logging.

Initial roles:

- OWNER
- MANAGER
- CASHIER
- INVENTORY_MANAGER

Permissions must be configurable independently of roles.

## FR-02: Store Configuration

The system must allow administrators to configure:

- Store name.
- Business information.
- Address.
- Contact information.
- Currency.
- Default tax configuration.
- Receipt information.
- Receipt numbering.
- Default payment methods.
- Inventory policies.
- Cash register settings.

The initial store configuration must be created during first-time application setup.

## FR-03: Product Catalog

The system must allow complete product administration.

Requirements:

- Create products.
- Edit products.
- Activate and deactivate products.
- Assign brands.
- Assign categories.
- Manage product variants.
- Assign SKUs.
- Assign barcodes.
- Upload product images.
- Configure purchase cost.
- Configure selling price.
- Configure minimum stock.
- Search products.
- Filter products.
- Import products.
- Export products.

Supported product attributes:

- Product name.
- Description.
- Brand.
- Category.
- Product type.
- Gender classification.
- Fragrance concentration.
- Volume in milliliters.
- SKU.
- Barcode.
- Cost.
- Selling price.
- Tax configuration.
- Status.

The data model must support:

- Original perfumes.
- Equivalent fragrances.
- Decants.
- Samples.
- Testers.
- Gift sets.

Each sellable presentation must have an independent inventory identity.

## FR-04: Inventory Management

Requirements:

- View current inventory.
- Register initial inventory.
- Register stock entries.
- Register stock exits.
- Register inventory adjustments.
- Register damaged products.
- Register lost products.
- Register returns.
- View inventory movements.
- Configure stock minimums.
- Generate low-stock alerts.
- Perform physical stock counts.
- Export inventory reports.

Business rules:

1. Every inventory change must generate a movement record.
2. Historical movements cannot be deleted.
3. Inventory adjustments must require a reason.
4. Negative inventory must be disabled by default.
5. Inventory changes must be transactional.
6. Stock corrections must generate compensating movements.

## FR-05: Point of Sale

The POS is the primary operational module.

Requirements:

- Search products.
- Scan barcodes.
- Add products to cart.
- Remove products.
- Modify quantities.
- Calculate subtotals.
- Apply authorized discounts.
- Associate customers.
- Associate sales employees.
- Calculate taxes.
- Calculate final totals.
- Register payments.
- Support mixed payments.
- Calculate cash change.
- Complete sales.
- Suspend sales.
- Resume suspended sales.
- Print receipts.
- Reprint receipts.
- View sales history.
- Cancel sales with authorization.
- Process returns.

Payment methods:

- Cash.
- Card (manual registration).
- Bank transfer.
- Mixed payment.

Business rules:

- A sale must not be completed without sufficient payment.
- Sale items must preserve historical product and price information.
- Payments must be associated with a sale.
- Inventory must only be deducted after successful sale confirmation.
- A failed transaction must not leave partially registered sales.
- Every completed sale must have a unique identifier.
- Sale cancellation must be auditable.

## FR-06: Cash Management

Requirements:

- Create cash registers.
- Open cash sessions.
- Register opening balance.
- Register cash deposits.
- Register withdrawals.
- Register cash expenses.
- Calculate expected cash.
- Register actual cash count.
- Calculate cash differences.
- Close cash sessions.
- View previous sessions.
- Generate cash reports.

A cash session must belong to a specific user and cash register.

A closed session must not be editable through normal operations.

Corrections must be recorded through authorized adjustment operations.

## FR-07: Customers

Requirements:

- Create customers.
- Edit customer information.
- Search customers.
- Register contact information.
- Associate customers with sales.
- View purchase history.
- Register preferences.
- View purchase frequency.
- Calculate historical spending.

Customer registration must remain optional during checkout.

## FR-08: Suppliers

Requirements:

- Create suppliers.
- Edit suppliers.
- Manage contact information.
- Associate products with suppliers.
- View supplier history.
- Register supplier purchases.
- Track pending payments.

## FR-09: Purchase Management

Requirements:

- Create purchase orders.
- Add products.
- Register quantities.
- Register purchase costs.
- Track purchase status.
- Receive merchandise.
- Support partial receipts.
- Update inventory after receipt.
- Track supplier invoices.
- Register supplier payments.

Inventory must only increase after a merchandise receipt is confirmed.

## FR-10: Finance and Expenses

Requirements:

- Register expenses.
- Categorize expenses.
- Register additional income.
- Track supplier payments.
- View income.
- View expenses.
- Calculate gross profit.
- Estimate net operating results.
- Export financial reports.

Financial calculations must distinguish:

- Sales revenue.
- Discounts.
- Refunds.
- Cost of goods sold.
- Gross profit.
- Operating expenses.
- Estimated operating profit.

## FR-11: Dashboard and Reporting

The dashboard must provide:

- Today's sales.
- Monthly sales.
- Transaction count.
- Average ticket.
- Best-selling products.
- Best-selling brands.
- Low-stock products.
- Inventory valuation.
- Gross profit.
- Operating expenses.
- Sales by employee.
- Sales by payment method.
- Cash session summaries.

Reports must support date filtering and export to CSV/XLSX/PDF where applicable.

## FR-12: Offline Operation

This is a mandatory system requirement.

The application must support:

- Offline startup after initial setup.
- Local authentication.
- Product catalog access.
- Product search.
- Checkout.
- Payment registration.
- Inventory updates.
- Cash management.
- Customer registration.
- Purchase and inventory operations.
- Sales history.
- Receipt printing.
- Local backup.
- Pending synchronization tracking.

Internet connectivity must never be required to complete a supported local sale.

## FR-13: Synchronization

The system must implement a reliable synchronization mechanism.

Requirements:

- Generate unique event identifiers.
- Persist events in a local Outbox.
- Synchronize events in batches.
- Receive remote events through an Inbox.
- Implement idempotent processing.
- Track synchronization status.
- Retry temporary failures.
- Detect permanent failures.
- Handle schema versions.
- Maintain synchronization cursors.
- Provide conflict reporting.
- Recover interrupted synchronization.

The system must never silently overwrite inventory movements.

## FR-14: Audit Logging

The system must record critical business operations.

Examples:

- User login.
- Product creation.
- Price modification.
- Inventory adjustment.
- Sale completion.
- Sale cancellation.
- Refund.
- Cash opening.
- Cash closing.
- Permission modification.
- Synchronization activity.

Audit records must identify the user, device, operation, timestamp, and relevant entity.

# 8. Business Rules

The following rules are mandatory.

### BR-01: Inventory integrity

Inventory must be calculated from confirmed movements.

### BR-02: Historical sales

Completed sales must preserve the price, cost, SKU, and product description applicable at the time of sale.

### BR-03: Payment integrity

The total of successful payments must match the amount due, considering change and payment-specific rules.

### BR-04: Cash sessions

Cash operations must belong to an open cash session unless explicitly authorized as an administrative correction.

### BR-05: Refunds

Refunds must reference their original sale and items.

### BR-06: Permissions

Restricted operations must be authorized by the backend or local authorization layer, not merely hidden in the interface.

### BR-07: Inventory adjustments

No inventory movement may be silently deleted or overwritten.

### BR-08: Offline consistency

Offline operations must be persisted locally before they are acknowledged as successful.

### BR-09: Synchronization

Repeated delivery of the same event must not create duplicate business transactions.

### BR-10: Financial precision

Financial calculations must use exact decimal or integer-based monetary representations, never floating-point arithmetic.

# 9. Data Model Requirements

Initial domain entities:

| Domain | Entities |
|---|---|
| Identity | User, Role, Permission, UserRole |
| Store | StoreSettings, BusinessProfile |
| Catalog | Product, ProductVariant, Brand, Category, Barcode |
| Inventory | Warehouse, Stock, InventoryMovement, StockCount |
| Sales | Sale, SaleItem, Payment, Refund, RefundItem |
| Customers | Customer, CustomerPreference |
| Suppliers | Supplier |
| Purchases | PurchaseOrder, PurchaseItem, GoodsReceipt |
| Cash | CashRegister, CashSession, CashMovement |
| Finance | Expense, ExpenseCategory, SupplierPayment |
| System | Device, AuditLog, SyncOutbox, SyncInbox, SyncConflict |

All synchronizable entities must use globally unique identifiers.

Historical business records must preserve relevant snapshots.

# 10. Technical Architecture

## Frontend

- React.
- TypeScript.
- Vite.
- Tailwind CSS.
- TanStack Query.
- Zustand.
- React Hook Form.
- Zod.
- pnpm workspaces.

## Desktop

- Tauri 2.
- Rust.
- SQLite.
- Native printing integration.

## Web

- PWA.
- IndexedDB.
- Dexie.js.
- Service Worker.

## Backend

- Java.
- Spring Boot.
- Spring Security.
- Spring Data JPA.
- PostgreSQL.
- Flyway.
- Bean Validation.
- OpenAPI.
- Spring Actuator.

## Infrastructure

- Docker.
- Docker Compose.
- AWS EC2 or equivalent hosting.
- AWS RDS PostgreSQL.
- AWS S3.
- CloudWatch.

## Architecture principles

1. Modular monolith.
2. Separation of concerns.
3. Domain-driven modular organization.
4. Repository and adapter abstractions.
5. Offline-first persistence.
6. Event-based synchronization.
7. Transactional business operations.
8. Explicit authorization.
9. Immutable historical movements.
10. Automated testing.

# 11. Initial Project Structure

```text
perfume-pos/
│
├── frontend/
│   ├── apps/
│   │   ├── web/
│   │   └── desktop/
│   │
│   └── packages/
│       ├── domain/
│       ├── application/
│       ├── infrastructure/
│       ├── shared/
│       └── ui/
│
├── backend/
│   └── src/
│       └── main/
│           ├── java/com/perfumepos/
│           │   ├── common/
│           │   ├── identity/
│           │   ├── store/
│           │   ├── catalog/
│           │   ├── inventory/
│           │   ├── sales/
│           │   ├── customers/
│           │   ├── suppliers/
│           │   ├── purchases/
│           │   ├── cash/
│           │   ├── finance/
│           │   ├── reports/
│           │   └── synchronization/
│           │
│           └── resources/
│               └── db/migration/
│
├── infrastructure/
│   ├── docker/
│   └── deployment/
│
├── docs/
│   ├── PRD.md
│   ├── SYSTEM-ARCHITECTURE.md
│   ├── DATABASE-DESIGN.md
│   ├── OFFLINE-SYNC.md
│   ├── API-SPECIFICATION.md
│   ├── SECURITY.md
│   └── TESTING-STRATEGY.md
│
└── README.md
```

# 12. Development Phases

## Phase 0 — Project Foundation

- Initialize monorepo.
- Configure pnpm.
- Configure React and TypeScript.
- Configure Tauri.
- Initialize Spring Boot.
- Configure PostgreSQL.
- Configure SQLite.
- Configure IndexedDB.
- Establish code conventions.
- Configure development environments.
- Configure CI pipeline.

Deliverable: working development environment.

## Phase 1 — Identity and Store Setup

- User model.
- Authentication.
- Roles.
- Permissions.
- Initial administrator.
- Store configuration.
- Local authentication.

Deliverable: functional application access.

## Phase 2 — Catalog

- Product model.
- Brands.
- Categories.
- Product variants.
- Barcode management.
- Product images.
- Product search.
- Local persistence.

Deliverable: functional product catalog.

## Phase 3 — Inventory

- Stock model.
- Inventory movements.
- Initial inventory.
- Adjustments.
- Stock validation.
- Inventory history.
- Low-stock alerts.

Deliverable: functional inventory management.

## Phase 4 — Sales Engine

- Sale model.
- Sale items.
- Payment model.
- Cart.
- Discounts.
- Checkout.
- Transaction management.
- Inventory deduction.
- Receipt generation.

Deliverable: complete local checkout.

## Phase 5 — Cash Management

- Cash registers.
- Cash sessions.
- Opening balance.
- Cash movements.
- Cash closing.
- Cash reconciliation.

Deliverable: functional cash management.

## Phase 6 — Purchases and Suppliers

- Supplier management.
- Purchase orders.
- Goods receipts.
- Inventory integration.
- Supplier payments.

Deliverable: purchasing operations.

## Phase 7 — Customers and Finance

- Customer management.
- Purchase history.
- Expenses.
- Financial reports.
- Profit calculations.

Deliverable: business management.

## Phase 8 — Synchronization

- Outbox.
- Inbox.
- Event processing.
- Idempotency.
- Synchronization API.
- Conflict management.
- Retry mechanisms.

Deliverable: reliable local-to-cloud synchronization.

## Phase 9 — Production Readiness

- PWA offline support.
- Desktop packaging.
- Printer integration.
- Barcode scanner testing.
- Automated backups.
- Security testing.
- Recovery testing.
- Deployment.

Deliverable: production-ready MVP.

# 13. Testing Requirements

The system must include:

- Unit tests.
- Integration tests.
- API tests.
- End-to-end tests.
- Offline tests.
- Synchronization tests.
- Security tests.
- Recovery tests.

Mandatory test scenarios:

1. Complete a sale without Internet.
2. Sell the last available product.
3. Attempt to sell an unavailable product.
4. Interrupt a sale transaction.
5. Interrupt synchronization.
6. Synchronize the same event repeatedly.
7. Process a partial refund.
8. Close a cash session with a difference.
9. Restore a local backup.
10. Upgrade the local database with pending events.

# 14. MVP Definition

The MVP must include:

- Authentication.
- Store configuration.
- Product catalog.
- Product variants.
- Inventory.
- Sales.
- Payments.
- Receipt generation.
- Cash sessions.
- Basic reports.
- Offline persistence.
- Desktop application.
- Web PWA.
- Synchronization.
- Audit logs.
- Backups.

The following features are deferred:

- CFDI electronic invoicing.
- Multi-branch management.
- E-commerce.
- Loyalty programs.
- Advanced CRM.
- Bank terminal integrations.
- Advanced accounting.
- Mobile native applications.

# 15. Acceptance Criteria

The MVP is accepted only when:

1. An administrator can initialize the store.
2. Users can authenticate.
3. Authorized users can manage products.
4. Product variants have independent stock.
5. Inventory movements are auditable.
6. Cashiers can complete sales offline.
7. Sales update inventory transactionally.
8. Payments are correctly recorded.
9. Mixed payments work correctly.
10. Receipts can be generated and printed.
11. Cash sessions can be opened and closed.
12. Basic sales and inventory reports are available.
13. Local backups can be created and restored.
14. Synchronization does not create duplicate sales.
15. Interrupted synchronization can recover.
16. The desktop and web applications share the same business rules.
17. Critical operations are covered by automated tests.

# 16. Initial Architecture Decisions

The following decisions are established for implementation:

| Decision | Selected approach |
|---|---|
| Product architecture | Modular monolith |
| Frontend | React + TypeScript |
| Desktop | Tauri 2 |
| Web | PWA |
| Desktop persistence | SQLite |
| Web persistence | IndexedDB |
| Backend | Spring Boot |
| Central database | PostgreSQL |
| Migrations | Flyway |
| Synchronization | Outbox / Inbox |
| Identifiers | UUID |
| Currency | MXN |
| Initial business scope | Single store |
| Connectivity | Offline-first |
| Fiscal integration | Deferred |

# 17. Open Decisions and Assumptions

The following items must be validated before their respective implementations.

| Topic | Initial assumption |
|---|---|
| Simultaneous terminals | One authoritative local writer initially |
| Multiple computers | Requires local network architecture decision |
| Product types | Flexible catalog supporting all fragrance types |
| Taxes | Configurable tax model |
| Payment processing | Manual payment registration |
| Printer | ESC/POS-compatible thermal printer |
| Barcode reader | USB HID-compatible reader |
| Store configuration | Single initial store |
| Offline authentication | Previously provisioned local accounts |
| Cloud synchronization | Optional for local operation |

These assumptions must not be interpreted as finalized business requirements.

# 18. Definition of Done

A feature is considered complete only when:

- Requirements are implemented.
- Business rules are respected.
- Validation is implemented.
- Authorization is enforced.
- Error handling is implemented.
- Unit tests are included.
- Integration tests are included when applicable.
- Database migrations are included when necessary.
- Documentation is updated.
- Acceptance criteria are satisfied.
- No known critical regression remains.

# 19. Development Principles

The development process must follow these principles:

1. Implement one bounded feature at a time.
2. Avoid unnecessary abstractions.
3. Keep domain logic independent of frameworks.
4. Do not duplicate business rules between frontend and backend.
5. Do not expose persistence details to UI components.
6. Never silently discard local data.
7. Never overwrite historical financial information.
8. Never modify inventory without recording a movement.
9. Use database migrations for schema changes.
10. Require tests for critical business operations.
11. Keep GitHub Issues focused and independently verifiable.
12. Do not implement deferred features without updating the PRD.

# 20. Initial Development Objective

The first development milestone is not to build the complete POS interface.

It is to establish a stable technical foundation.

The first milestone must deliver:

- A working monorepo.
- A functional React application.
- A functional Tauri desktop application.
- A working Spring Boot backend.
- PostgreSQL configuration.
- SQLite local persistence.
- IndexedDB local persistence.
- A shared domain/application layer.
- Initial database migrations.
- Automated development checks.

Once this foundation is verified, development can proceed into identity, catalog, inventory, and sales.

**End of document.**
