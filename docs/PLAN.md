# Perfume POS implementation plan

Status: planning interview complete; consolidated plan ready for user review. Implementation is explicitly on hold until the user instructs otherwise.

This document records the user's accepted refinements to `prd.md`. Where they differ, use these refinements for planning; the original PRD remains unchanged as the source vision.

## Confirmed scope

- Launch targets a supervised pilot in one physical store.
- The Windows application is the sole writer for sales, inventory, and cash. The web application provides viewing and reporting. See [ADR 0001](adr/0001-single-windows-writer.md).
- Launch includes authentication, store configuration, catalog and presentations, inventory, checkout, payments, returns, receipts, cash sessions, basic reports, local persistence, synchronization, audit, and backups.
- Full purchasing, supplier payments, and advanced finance are deferred. A simple stock-receiving operation remains in scope.
- Inventory uses discrete sellable presentations, including finished decants and samples. Bottle-to-decant production and arbitrary milliliter sales are deferred.
- The PRD's selected React, TypeScript, Tauri, SQLite, IndexedDB, Spring Boot, and PostgreSQL technologies remain constraints. Their responsibilities must reflect the revised web scope.
- No deadline, team size, or budget has been specified.

## Confirmed operating rules

- Reuse useful prototype code; changes may break compatibility with the prototype database. No production-data preservation requirement was reported.
- Windows owns all store changes, including catalog, prices, customers, and users. Cloud synchronization supplies web viewing and reporting; remote administration is deferred.
- Web reports disclose the last synchronization time and stale data.
- Initial setup creates the owner locally. The owner provisions individual cashier PINs and privileged-user passwords; restricted actions require authorized approval. Daily local operation requires no cloud login.
- Completed sales remain unchanged. Corrections create linked reversals; partial returns are supported, refund methods are recorded, returned stock is classified as resellable or damaged, and manager approval is required.
- One cashier owns each open register session. Handover requires a counted close; closed sessions are immutable. Cash tracking includes opening cash, deposits, withdrawals, cash refunds, expected cash, counted cash, and differences.
- Simple goods receipts record quantity and acquisition cost. Moving weighted-average inventory cost supports profit estimates, with historical sale cost preserved.
- Cashiers use configured prices. Discounts and overrides require manager approval and an audit reason.
- Total-computer-failure recovery targets at most one business day of lost work, supported by hourly and cash-close encrypted backups copied to an external drive and a tested replacement-computer procedure. This target depends on successful off-computer backups; failures must be visible.

## Confirmed implementation boundaries

- Retain the root planning repository and existing backend/frontend repositories. Establish a pnpm workspace inside `POS_Frontend` for shared client packages and Windows/web apps; retain Spring Boot in `POS`.
- TypeScript implements offline client rules; Java independently validates incoming records. One documented contract and shared examples verify agreement rather than claiming business logic can be shared directly across languages.
- Displayed prices include owner-configured taxes. Discounts precede the included-tax breakdown. Historical sales preserve price, discounts, tax configuration, and calculated amounts.
- Cards and transfers are manually confirmed. Only cash tender can generate change. No credit sales or customer balances at launch. Printer failures do not reverse completed sales.
- Returns reference original sales and cannot exceed unreturned quantities. No store credit. A resellable return restores inventory at its original recorded sale cost; damaged returns are unavailable stock. Exchanges link a return and new sale.
- Managers authorize receipts and adjustments. Physical counts block stock changes for selected presentations until completion; differences generate movements. Initial stock records quantity and cost.
- Cash close permits differences with a reason and flags them for manager review. Cash payouts cannot exceed recorded available cash. Corrections append records.
- Create encrypted backups at least hourly and at cash close, copying them automatically to a configured external drive. Keep an owner recovery key separately and prominently report backup failures.
- Replacement restoration requires retirement of the old writer and reconciliation against synchronized records before sales resume.
- Launch uses fixed permission sets for owner, manager, cashier, and inventory manager. Custom permission editing is deferred. Rate-limit authentication, require approval for each restricted action, allow owner employee-credential resets, and generate an offline owner recovery code.
- Synchronization preserves locally completed records on rejection, retries temporary failures, and reports permanent failures for owner review. Web reports include only successfully synchronized data and warn about missing updates. Web uses separate online authentication.
- The first implementation milestone proves minimal offline checkout, stock, payments, cash, receipts, audit, synchronization, and restoration before broader management screens. Validate one selected printer and scanner early.

## Final operating decisions

- Products group related sellable presentations. Each presentation has a unique SKU, optional barcodes, price, cost, and independent stock. Assigned barcodes must identify a single presentation unambiguously. Used presentations may be deactivated but never deleted.
- Payments and final monetary amounts use integer centavos. Average costs retain additional fixed decimal precision. Monetary ties round half up. Calculate discounts and included tax per sale line, then sum rounded lines. Allocate sale-wide discounts proportionally with a deterministic remainder rule; preserve the allocation for later refunds.
- Receipts include a readable register identifier, recovery generation, and local sequence alongside the sale UUID. Replacement installations use a new generation. Store timestamps with their offset, configure the store time zone explicitly, and group daily reports by that store's calendar day.
- Recovery replays later cloud records missing from the restored backup. A manager reconciles any remaining gap before checkout is enabled. Records missing from both cloud and backup cannot be automatically reconstructed; stock, cash, and receipts may require manual reconciliation.
- Pilot reports cover sales, discounts, refunds, payment methods, cashier totals, cash differences, stock movements, low stock, inventory valuation, and estimated gross profit. Include CSV exports and printable receipts/cash-close summaries. General XLSX/PDF exports and net operating profit are deferred.
- Include optional customer name/contact information, purchase history, suspended carts, and catalog CSV import with preview and validation. Suspended carts reserve no inventory and revalidate stock and prices on resume. Customer preferences, marketing, and product images are deferred.
- Pilot hardware is one Windows-installed thermal printer and a keyboard-style USB barcode reader. Verify accents, paper width, scanning, and cutting where supported on the actual devices.
- Pilot exit requires five consecutive trading days without unexplained stock/cash differences or lost/duplicate transactions, successful recovery drills, agreed performance targets on the pilot computer, working peripherals, and no unresolved critical defects. Explain and reconcile discrepancies before acceptance.

## Delivery milestones

These are planned work packages, not work authorized to begin. Do not start application implementation until explicitly instructed by the user.

### M0 — Foundation supporting the first checkout

Assess prototype code for reuse and establish the frontend workspace, desktop/web entry points, shared domain/application packages, platform adapters, backend modules, database migrations, and development checks. Specify authorization, monetary examples, synchronization contracts, and local transaction boundaries before building business screens.

Acceptance: applications start in development; local persistence survives restart; migrations run against empty and existing test databases; TypeScript and Java agree on initial monetary contract examples. A Windows test machine is available for desktop and peripheral verification.

### M1 — Complete offline sale and cloud visibility

Implement the smallest complete flow: local owner setup, cashier provisioning, one stocked presentation, cash opening, offline checkout, payment recording, receipt generation, historical snapshots, inventory movement, audit, and durable synchronization outbox. Synchronize to PostgreSQL and show the result in an authenticated read-only web view with synchronization freshness.

The sale, payment records, inventory effects, cash effects, audit, and outbox must commit together locally. A receipt is printed only after commit. Reprinting must not repeat the sale. Synchronization acknowledges a business transaction only after its complete central ingestion, without requiring the cloud during checkout.

Acceptance: offline startup and checkout work after setup; the last unit can be sold once; insufficient stock/payment is rejected; interrupted confirmation leaves no partial sale; repeated delivery creates no duplicate sale, stock movement, or cash movement; printer failure permits reprint.

### M2 — Recovery and failure handling

Implement encrypted hourly/cash-close backups, external-drive copying and failure visibility, interrupted synchronization recovery, permanent-failure reporting, and a documented replacement procedure. Restore a consistent database, replay available cloud records, retire the old writer, start a new receipt generation, and require reconciliation before reopening checkout.

Acceptance: restore onto a replacement test installation, preserve pending operations through supported upgrades, retry after lost acknowledgements, and demonstrate a recovery gap requiring manager reconciliation. Verify that the owner recovery key is usable and required data is recoverable. Do not delay these checks until feature completion.

### M3 — Complete store operations

Expand catalog/presentations, initial stock, receiving, movement history, weighted-average costing, low-stock thresholds, selected-item counts, and authorized adjustments. Add discounts, price overrides, mixed payments, partial returns, damaged-return handling, exchanges, cashier handover, cash differences, and correction records. Enforce fixed-role permissions within the application boundary, including privileged approval and authentication rate limits.

Acceptance: test penny allocation and partial refunds, original-cost stock restoration, stock-count blocking, mixed-payment change rules, insufficient-cash payouts, unauthorized actions, cashier handover, and immutable closed sessions/history. Verify representative tax and cost examples against the cross-language contract.

### M4 — Pilot workflow and reporting

Add optional customer association/history, suspended carts, catalog import preview/validation, local reports, CSV exports, cash-close printing, and equivalent cloud reports over synchronized data. Complete Spanish-language checkout, setup, and operational recovery messages. Implement the installable web shell and cached read-only views using IndexedDB without introducing web writes or exposing local credentials.

Acceptance: imported rows cannot silently replace historical records or stock; resumed carts revalidate prices and availability; report totals reconcile to recorded sales/refunds/cash/stock; stale web data is clearly disclosed; online report access enforces permissions.

### M5 — Packaged store pilot

Package the Windows application, verify clean installation and offline restart, select and test the actual printer/scanner, configure store tax inputs and time zone, load verified opening stock/costs, train users, and rehearse backup/restoration. Run the supervised pilot and resolve findings before production acceptance.

Acceptance: five consecutive trading days satisfying the agreed exit gate; local product search under 300 ms and sale confirmation under one second excluding peripherals on the pilot machine; all mandatory recovery/integrity scenarios pass; no unresolved critical defect. Do not claim a deployment date until capacity and the early milestones are measured.

## Repository baseline

The repository contains backend and frontend Git submodules (`POS/` and `POS_Frontend/`). Existing application code should be assessed for reuse before planning replacement or restructuring. The PRD's proposed monorepo and offline infrastructure are not yet established.

## Remaining implementation specifications and setup inputs

The product decisions above are settled. Before implementing the relevant milestone, write concrete schemas, API/event payloads, event ordering/dependencies, schema compatibility rules, ingestion transaction boundaries, replay/cursor behavior, monetary precision bounds, and authorization matrices consistent with these decisions. Test rejected and out-of-order batches as well as duplicate delivery.

Recovery must explicitly address how the old writer is retired: a disconnected lost device cannot be remotely disabled while offline. The replacement procedure must establish exclusive operational use and server-side rejection of the retired generation, and clearly describe the limitation.

Other required setup inputs are the pilot computer/printer/scanner models, external backup-drive location, owner recovery-key custody, owner-supplied tax configuration, store time zone, and deployment account. Team capacity, budget, and deadline remain unspecified. These inputs do not change the agreed scope and must not be guessed in production setup.

## Explicit PRD refinements

- Web is read-only at launch; offline web checkout is deferred.
- Retain the existing backend/frontend repository arrangement instead of moving everything into one application repository.
- Fixed role permission sets replace configurable permissions for the pilot.
- Full purchasing, supplier payment management, expense management, advanced finance, product images, and general XLSX/PDF exports are deferred.
- Use one behavioral contract with independently verified TypeScript and Java implementations.
- Prove synchronization, backup, and recovery alongside the first checkout instead of leaving them to the final phases.

## Documentation and implementation status

Supporting decisions are recorded in `docs/adr/`; domain terminology is in `GLOSSARY.md`. Only planning documents were created or edited during this interview. No application implementation, dependency installation, deployment, or application test execution was performed.
