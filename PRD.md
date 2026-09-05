# Industriehandel Operationssystem

## Product requirements document

| Field | Value |
| --- | --- |
| Status | Baseline approved after grilling |
| Product type | Internal B2B wholesale operations system |
| Primary market context | Germany |
| Default language | German |
| Secondary language | English |
| Currency | EUR |
| Time zone | Europe/Berlin |
| Reference domain | German industrial C-parts distribution |

## 1. Product summary

Industriehandel Operationssystem is a portfolio case study for a fictional German distributor of industrial fasteners and workshop supplies. It covers the work between customer setup, product and stock administration, sales orders, approval, shipment, invoicing, and payment recording.

The domain research draws on publicly documented workflows in German C-parts distribution, including those described by Würth Industrie Service. The product is independent. It is not commissioned, approved, or endorsed by Würth. It must use original branding, original interface design, and synthetic data.

The project is intended to demonstrate Java and Spring enterprise development for German employers in manufacturing, logistics, insurance-adjacent operations, public-sector IT, and the Mittelstand. It should demonstrate sound decisions, observable business behavior, tests that survive refactoring, and professional system documentation.

## 2. Product goals

1. Deliver a working, bilingual order-to-invoice system for one fictional German B2B wholesaler.
2. Prove business rules through behavior-focused tests at appropriate boundaries.
3. Establish a modular base that can accept deeper logistics and procurement features over months of development.
4. Demonstrate familiarity with German business conventions without copying an existing German product.
5. Make the system easy for a recruiter to run locally and understand through seeded data and guided workflows.
6. Record product, architecture, data, security, and testing decisions so the developer can defend them in an interview.

## 3. Non-goals

The baseline is not a complete ERP, accounting package, warehouse-management system, CRM, marketplace, or customer portal. It does not claim legal certification or production readiness for a real company.

The following are out of scope for the baseline:

- Multi-tenancy.
- B2C sales and customer self-service.
- Suppliers, procurement, and purchase orders.
- Multiple warehouses, bins, batches, serial numbers, RFID, and Kanban replenishment.
- EDI, OCI, carrier, banking, and payment-gateway integrations.
- Partial shipments, returns, refunds, and partial credit notes.
- Automatic bank reconciliation.
- Multiple currencies.
- Marketplace and field-sales features.
- Native mobile applications.
- Production email, SMS, and push notifications.
- Advanced analytics, forecasting, and executive dashboards.
- External identity providers and multifactor authentication.
- High availability and VPS deployment.
- Claims of certified GoBD, BFSG, or WCAG compliance.

These items may enter later planning. Their exclusion is not a permanent rejection.

## 4. Users and responsibilities

### 4.1 Sales staff

Sales staff maintain customers and contacts, search the catalog, create draft orders, apply permitted line discounts, submit high-value orders for approval, confirm permitted orders, and inspect fulfilment and invoice status.

### 4.2 Approver

An approver reviews orders above the configured net-value threshold. The approver may approve or reject an order but may not approve an order they created. A rejection requires a reason.

### 4.3 Accounting staff

Accounting staff review draft invoices, issue invoices, cancel issued invoices through a linked corrective document, record payments, inspect overdue accounts, and run authorized financial exports.

### 4.4 Administrator

Administrators manage users, roles, permissions, products, stock adjustments, application settings, job failures, and security events. Public demo users cannot access platform-control functions.

### 4.5 Public demo visitor

A visitor uses restricted Sales, Approver, and Accounting personas. The visitor may complete the baseline business workflow but may not administer accounts or roles, import data, retry jobs, change system settings, reset seed data, or run unbounded exports.

## 5. Product principles

### 5.1 German conventions, original design

The interface defaults to German and uses `de-DE` dates, numbers, addresses, terminology, and formal address with "Sie." English is available throughout. The product should feel familiar because the business conventions are correct, not because its screens imitate another application.

### 5.2 Behavior before implementation detail

Tests describe behavior through public interfaces. They do not test private methods, internal call sequences, or incidental database state when the behavior can be observed through the application interface.

### 5.3 Immutable financial history

Issued invoices and accounting-relevant records are not overwritten or casually deleted. Corrections create linked records that preserve history.

### 5.4 Secure backend authorization

The backend enforces every permission. Hiding a frontend control is a usability measure, not an authorization boundary.

### 5.5 Explainable operations

Important state changes, failures, and conflicts tell the user what happened and what they can do next. Technical logs and business audit records remain separate.

## 6. Delivery strategy

### 6.1 Walking skeleton

The first executable increment connects the chosen frontend, Spring Boot, PostgreSQL, session authentication, migrations, Docker Compose, CI, and test infrastructure. It proves that the main components work together but contains little business depth.

### 6.2 Vertical development

Development proceeds one observable behavior at a time:

1. Write one failing test at the narrowest boundary that proves the behavior.
2. Implement enough behavior to make it pass.
3. Refactor without changing behavior.
4. Add the next behavior based on what the previous cycle revealed.

Browser tests grow with the slices. The first tracer workflow is login, create a customer, and verify that the customer appears. Once all dependent slices exist, a critical browser test covers customer, order, approval where required, shipment, invoice issuance, and payment recording.

### 6.3 Suggested slice order

1. Walking skeleton and authentication.
2. Create, view, edit, archive, and search customers.
3. Create, view, edit, archive, and search products.
4. Adjust stock and inspect stock movements.
5. Create and edit draft orders.
6. Confirm ordinary orders and reserve stock safely.
7. Submit, approve, or reject high-value orders.
8. Create and ship one shipment per order.
9. Generate, review, and issue a PDF invoice.
10. Record payments and derive invoice payment state.
11. Run overdue-invoice and simulated-email jobs.
12. Search, exports, audit views, notifications, and operational controls.
13. Add XRechnung, invoice cancellation, imports, retention controls, and restore evidence.

The prototype can remain incomplete between slices. Every completed slice must work through the real application path and leave the system releasable.

## 7. Prototype boundary

Prototype 1 is complete when the following work together locally:

- German-first and English-optional interface.
- Login, session security, and three default business roles.
- Configurable roles and named permissions.
- B2B customers, addresses, and contacts.
- Product catalog and one-warehouse inventory.
- Stock movements and concurrency-safe reservations.
- Order creation, approval, fulfilment, shipment, and cancellation.
- One shipment per order.
- PDF invoice generation and issuance.
- Manual payment recording.
- Search, filters, pagination, and agreed exports.
- Audit history and in-app notifications.
- Overdue-invoice and simulated-email jobs.
- Docker Compose development environment and synthetic German demo data.
- Database migrations, API documentation, observability, CI, and agreed tests.

The first post-prototype increment contains:

- Valid XRechnung XML with validation tests.
- Full invoice cancellation through a linked `Stornorechnung`.
- Customer and product CSV import.
- Immutable artifact-retention controls.
- A successful backup and restore drill.

## 8. Functional requirements

### 8.1 Authentication and sessions

| ID | Requirement |
| --- | --- |
| AUTH-01 | Users sign in with email and password. |
| AUTH-02 | An administrator creates accounts. Public registration is unavailable. |
| AUTH-03 | The system stores passwords using a current adaptive password hash. |
| AUTH-04 | The browser uses a secure server-side session in an HTTP-only cookie. |
| AUTH-05 | State-changing browser requests receive CSRF protection. |
| AUTH-06 | Repeated failed logins lock the account according to configurable policy. |
| AUTH-07 | A newly created user must change the temporary password at first login. |
| AUTH-08 | Sessions expire after a configurable inactivity period. |
| AUTH-09 | Users can log out of the current session. |
| AUTH-10 | Successful login, failed login, lockout, password change, and logout create security events. |

Password recovery, public registration, multifactor authentication, and full device-session management are outside the baseline.

### 8.2 Roles and permissions

| ID | Requirement |
| --- | --- |
| RBAC-01 | A role contains named permissions. |
| RBAC-02 | A user may hold one or more roles. |
| RBAC-03 | The seed data provides Sales, Approver, Accounting, and Administrator roles. |
| RBAC-04 | Administrators may create, edit, archive, and assign roles. |
| RBAC-05 | The backend checks the required permission for every protected operation. |
| RBAC-06 | Permission changes create an audit event. |
| RBAC-07 | The system prevents removal of the final usable administrator assignment. |

The permission catalog must distinguish read, create, update, archive, approve, issue, cancel, export, adjust stock, and administer operations where relevant.

### 8.3 Localization

| ID | Requirement |
| --- | --- |
| I18N-01 | German is the fallback language. |
| I18N-02 | On first visit, the browser language selects German or English when supported. |
| I18N-03 | An explicit user choice overrides browser language and persists in the user profile. |
| I18N-04 | Public routes may use `/de/` and `/en/` where routing benefits from a stable language. |
| I18N-05 | Labels, validation messages, error summaries, notifications, emails, and generated customer documents support both languages where specified. |
| I18N-06 | German uses `de-DE`; English uses an explicitly selected English locale. |
| I18N-07 | Product names and descriptions store separate German and English values. |

IP geolocation must not choose the language.

### 8.4 Customers and contacts

Each customer contains:

- Immutable internal ID.
- Human-readable number such as `KD-000001`.
- Legal company name.
- German VAT identification number where applicable.
- Billing address.
- One or more shipping addresses.
- One or more contacts with name, work email, and work phone.
- Preferred language.
- Payment terms.
- Active or archived status.
- Restricted internal notes.
- Version value for optimistic locking.

| ID | Requirement |
| --- | --- |
| CUST-01 | Authorized users create, view, update, archive, restore, list, and search customers. |
| CUST-02 | Customer numbers are immutable and never reused. Sequence gaps are allowed. |
| CUST-03 | The system validates German address and VAT-ID shape without claiming tax-authority verification. |
| CUST-04 | A customer needs one billing address before an order can be confirmed. |
| CUST-05 | An archived customer cannot receive a new order. Historical records remain accessible. |
| CUST-06 | A stale edit fails with a conflict rather than overwriting a newer version. |
| CUST-07 | Authorized administrators may deactivate or anonymize contacts when retention policy permits. |
| CUST-08 | Historical issued invoices retain their recipient snapshot after customer or contact changes. |

Sample data must use clearly fictional German companies, plausible addresses, `DE`-shaped test VAT IDs, German phone formats, and visible `Testdaten` marking.

### 8.5 Products

Each product contains:

- Internal ID.
- Manually assigned SKU, for example `DIN933-M8X30`.
- German and English name and description.
- Category.
- Unit of measure.
- Package quantity.
- Net base price in EUR.
- VAT category.
- Reorder threshold.
- Active or archived status.
- Version value for optimistic locking.

| ID | Requirement |
| --- | --- |
| PROD-01 | Authorized users create, view, update, archive, restore, list, and search products. |
| PROD-02 | SKUs are unique, immutable after first business use, and never silently reused. |
| PROD-03 | Archived products remain visible on historical orders and invoices. |
| PROD-04 | Price changes do not alter stored order lines or invoice snapshots. |
| PROD-05 | The default VAT category for industrial C-parts uses the standard German rate configured for the effective period. Rates must not be scattered as code constants. |

### 8.6 Inventory

The baseline has one warehouse. Stock is derived from append-only movements and reservations rather than unexplained quantity edits.

Movement types include opening balance, manual increase, manual decrease, shipment, reversal, and reservation release where modeled as a movement.

| ID | Requirement |
| --- | --- |
| INV-01 | Authorized administrators record a stock adjustment with quantity, reason, and reference. |
| INV-02 | The system exposes physical, reserved, and available quantity. |
| INV-03 | Confirming an order reserves its quantity at commit time. |
| INV-04 | Confirmation fails if available stock is insufficient. Negative available stock is forbidden. |
| INV-05 | Concurrent confirmations cannot reserve the same remaining units twice. |
| INV-06 | Cancelling an eligible order releases its reservation. |
| INV-07 | Shipping consumes the reservation and reduces physical stock. |
| INV-08 | Every adjustment, reservation, release, shipment, and reversal is traceable. |
| INV-09 | Products at or below the reorder threshold create a low-stock condition and notification. |

### 8.7 Orders and approval

Order states are:

```text
DRAFT -> PENDING_APPROVAL -> CONFIRMED -> PROCESSING -> SHIPPED -> COMPLETED
   |             |              |
   +----------> CANCELLED <------+ 
                 |
              REJECTED -> DRAFT
```

The exact transition model must resolve whether rejection returns the same revision to `DRAFT` or creates a revision. The selected rule must preserve rejection history.

Each order contains an immutable number such as `AUF-2026-000001`, customer and address snapshots, line items, net totals, discounts, VAT summary, gross total, state, creator, timestamps, and version.

| ID | Requirement |
| --- | --- |
| ORD-01 | Sales users create and freely edit draft orders. |
| ORD-02 | A line captures product ID, SKU, localized description, quantity, unit, package quantity, base price, discount, final net unit price, VAT rate, and totals. |
| ORD-03 | The user may apply an optional percentage discount per line within their permission. |
| ORD-04 | Monetary totals use documented decimal precision and independent worked examples. |
| ORD-05 | An order at or below the configured net threshold may be confirmed by an authorized sales user. |
| ORD-06 | An order above the default €10,000 net threshold enters `PENDING_APPROVAL`. |
| ORD-07 | An authorized approver may approve or reject but may not approve their own order. |
| ORD-08 | Rejection requires a reason and notifies the creator. |
| ORD-09 | Confirmation reserves stock transactionally. |
| ORD-10 | Only drafts may be edited freely. Correcting a confirmed order requires controlled cancellation and replacement or a documented future revision flow. |
| ORD-11 | Eligible drafts and confirmed orders may be cancelled with a reason. |
| ORD-12 | Every state transition creates an audit event. |
| ORD-13 | Business numbers are concurrency-safe, immutable, never reused, and may contain gaps. |

### 8.8 Shipment

The baseline allows one shipment per order. Shipment numbers use a format such as `LS-2026-000001`.

| ID | Requirement |
| --- | --- |
| SHIP-01 | An authorized user creates a shipment for a confirmed order. |
| SHIP-02 | A shipment may store carrier name and tracking number as free operational references. |
| SHIP-03 | Shipping consumes reserved stock and transitions the order atomically. |
| SHIP-04 | A shipped order cannot be returned to an editable state. |
| SHIP-05 | The shipment preserves the delivery-address snapshot. |

Packing, labels, carrier APIs, partial shipments, delivery confirmation, and returns are excluded.

### 8.9 Invoices

Shipping creates one draft invoice for one order. Accounting reviews and issues it. Invoice numbers use a format such as `RE-2026-000001`.

The issued invoice snapshot must include the fields required by the selected German invoice rules, including supplier and recipient identity and address, supplier tax number or VAT ID, issue date, unique sequential invoice number, supply date, goods and quantities, net consideration, reductions, VAT rates and amounts, and payment terms where applicable.

| ID | Requirement |
| --- | --- |
| BILL-01 | Shipping creates a draft invoice from immutable order and shipment facts. |
| BILL-02 | Accounting may review a draft but cannot silently change commercial facts inherited from the order. |
| BILL-03 | Issuing assigns the final invoice number and creates immutable invoice data and artifacts. |
| BILL-04 | The system generates a German-first PDF and can render the customer-facing document in English. |
| BILL-05 | Each artifact stores its checksum, media type, creation time, invoice reference, and template version. |
| BILL-06 | Ordinary users cannot edit or delete an issued invoice. |
| BILL-07 | Invoice status derives from issuance, due date, payments, and cancellation. |
| BILL-08 | The post-prototype increment generates valid XRechnung XML and validates it against the selected standard version. |
| BILL-09 | The post-prototype increment cancels an invoice through a linked full `Stornorechnung`; it never overwrites the original. |
| BILL-10 | The system stores retention metadata and preserves original structured invoice components. |

The project must state that it is a portfolio implementation, not tax advice or proof of GoBD compliance.

### 8.10 Payments

Invoice payment states are `UNPAID`, `PARTIALLY_PAID`, `PAID`, and `OVERDUE`.

| ID | Requirement |
| --- | --- |
| PAY-01 | Accounting records a payment with date, amount, currency, and reference. |
| PAY-02 | A payment cannot allocate more than the remaining payable amount unless an explicit future overpayment feature exists. |
| PAY-03 | Payment changes use reversal or correction records rather than destructive deletion after posting. |
| PAY-04 | Invoice payment state derives deterministically from valid payments and due date. |
| PAY-05 | Every payment and correction creates an audit event. |

### 8.11 Search and lists

| ID | Requirement |
| --- | --- |
| SEARCH-01 | Search covers customer number, company name, contact, SKU, product name, order number, invoice number, and tracking number. |
| SEARCH-02 | Lists support exact filters for relevant state and date fields. |
| SEARCH-03 | All nontrivial lists use server-side pagination with bounded page sizes. |
| SEARCH-04 | Sort fields come from an allowlist. User input cannot become arbitrary SQL. |
| SEARCH-05 | Results respect record and action permissions. |

PostgreSQL provides baseline search. Elasticsearch and OpenSearch are excluded.

### 8.12 Exports

The baseline provides:

- Customer list CSV.
- Product and stock CSV.
- Filtered order list CSV.
- Filtered invoice list CSV.
- Individual invoice PDF.

| ID | Requirement |
| --- | --- |
| EXP-01 | An export uses the current authorized filter set. |
| EXP-02 | Export permissions are separate from read permissions where data sensitivity warrants it. |
| EXP-03 | Formula-like CSV values are escaped to prevent spreadsheet formula injection. |
| EXP-04 | Export size is bounded and large future exports may move to background processing. |
| EXP-05 | Every export creates an audit event with actor, type, filters, and result count. |

Formatted Excel workbooks, scheduled reports, bulk PDF packages, and accounting integrations are excluded.

### 8.13 Imports

Customer and product CSV imports enter the first post-prototype increment.

| ID | Requirement |
| --- | --- |
| IMP-01 | Users can download a versioned import template. |
| IMP-02 | A dry run validates every row without writing business records. |
| IMP-03 | Errors identify row, field, stable error code, and localized message. |
| IMP-04 | Duplicate detection follows documented business keys. |
| IMP-05 | A file containing invalid rows writes no business records. |
| IMP-06 | A successful import creates an audit summary. |

### 8.14 Notifications

| ID | Requirement |
| --- | --- |
| NOTE-01 | The application creates in-app notifications for approval requests, approvals, rejections, low stock, and overdue invoices. |
| NOTE-02 | A user can mark a notification read or unread. |
| NOTE-03 | Notifications link to an authorized target and reveal no inaccessible record data. |
| NOTE-04 | Simulated outbound email writes to a development mailbox or equivalent local boundary. |

Production email, SMS, push, and configurable notification rules are excluded.

### 8.15 Background jobs

| ID | Requirement |
| --- | --- |
| JOB-01 | A daily job marks eligible unpaid invoices overdue. |
| JOB-02 | An outbox-backed job produces simulated email notifications. |
| JOB-03 | Job runs persist status, attempt count, timestamps, correlation ID, and sanitized errors. |
| JOB-04 | Transient failures retry up to a configured limit. |
| JOB-05 | Exhausted work moves to an inspectable failed state. |
| JOB-06 | Authorized administrators may retry eligible failures. Public demo users may not. |
| JOB-07 | Every handler is idempotent and has a test proving repeated delivery does not duplicate effects. |

### 8.16 Audit and security events

Audit records include actor, timestamp, action, entity type, entity ID, correlation ID, and a safe changed-field summary.

The system audits:

- Authentication and lockout events.
- User, role, and permission changes.
- Order and shipment transitions.
- Invoice issuance and cancellation.
- Payment creation and correction.
- Stock adjustments and reservation events.
- Customer and product archive actions.
- Imports and exports.
- Administrative job actions.

| ID | Requirement |
| --- | --- |
| AUD-01 | Relevant records display an authorized audit timeline. |
| AUD-02 | Administrators can filter audit events by actor, action, entity, and time range. |
| AUD-03 | Selected fields display safe before-and-after values. |
| AUD-04 | Application users cannot edit, delete, or arbitrarily roll back audit records. |
| AUD-05 | Passwords, tokens, session IDs, and full sensitive payloads never enter audit data. |
| AUD-06 | Technical logs and business audit records have separate purposes and retention rules. |

### 8.17 Dashboard

The dashboard contains actionable counts and tables rather than decorative analytics.

- Sales sees drafts, approval work, rejections, and low-stock warnings affecting open work.
- Accounting sees draft, unpaid, and overdue invoices plus recent payments.
- Administrators see failed jobs, inactive or locked users, and recent security events.

Each widget must respect permissions and link to a filtered working list.

### 8.18 Demo mode

| ID | Requirement |
| --- | --- |
| DEMO-01 | Docker Compose can load a versioned set of synthetic German demo data. |
| DEMO-02 | A guided page explains the main workflow and restricted persona credentials. |
| DEMO-03 | A future public demo resets from its seed snapshot every hour and displays the next reset. |
| DEMO-04 | Visitor-created records are tagged by temporary demo session and filtered from other visitors where feasible. This is demo isolation, not product multi-tenancy. |
| DEMO-05 | Public demo roles cannot administer accounts, roles, imports, jobs, settings, resets, or unbounded exports. |
| DEMO-06 | The public demo caps writes and record counts and prevents file uploads. |

Public hosting is outside Prototype 1. Local seed and reset behavior remain in scope so deployment can reuse it later.

## 9. Validation and error behavior

1. Forms display field errors and a form-level summary.
2. A rejected submission preserves safe entered values.
3. The API returns stable machine-readable error codes with localized frontend messages.
4. Errors distinguish invalid input, missing authentication, insufficient permission, missing record, stale version, invalid state, insufficient stock, duplicate business key, and unexpected failure.
5. Conflicts name the affected record and give a corrective action without leaking restricted data.
6. Optimistic locking protects mutable customers, products, draft orders, roles, and configuration.
7. Transactional or atomic database behavior protects stock reservation, shipment, business-number allocation, invoice issuance, and payment allocation.

## 10. Data and calculation rules

### 10.1 Time

- Store instants in UTC.
- Display them in the user time zone, default `Europe/Berlin`.
- Store business dates without a time zone.
- Inject a controllable clock into time-dependent business behavior.

### 10.2 Money

- Use decimal types and an ISO currency code. Do not use binary floating point.
- Support EUR only in the baseline.
- Record the final unit price, discount, VAT rate, and calculated totals on order and invoice snapshots.
- Define precision, intermediate rounding, line rounding, and document-total reconciliation in an architecture decision record.
- Use worked examples from the specification as independent expected test values.

### 10.3 VAT

- Model VAT as effective-dated configuration or another explicit design selected in `SYSTEM_DESIGN.md`.
- Industrial C-parts normally use the standard German VAT category in the sample domain.
- Do not hard-code one rate across all products or historical documents.
- Issued documents retain the applied rate and amount even if configuration changes later.

### 10.4 Quantities

- Store decimal quantities with a unit of measure.
- Validate package constraints according to the selected rule.
- Never derive historical quantities from a changed product definition.

## 11. Non-functional requirements

### 11.1 Capacity targets

The design targets:

- 100 internal users.
- 50 concurrent active users.
- 100,000 customers.
- 50,000 products.
- 1,000,000 orders.
- 2,000,000 stock movements.

Common server-side list and detail requests should complete within 500 ms under documented normal local test conditions. Results must state hardware, data volume, concurrency, cache state, and measurement method. The target is not a production guarantee.

### 11.2 Accessibility

The implemented workflows target WCAG 2.2 AA as an engineering goal. Each affected slice covers keyboard operation, visible focus, semantic labels, error association and summary, contrast, and accessible tables. Automated scans do not justify a certification claim.

### 11.3 Supported clients

The interface is desktop-first and responsive on tablets. It supports current stable Chrome, Firefox, and Edge according to a recorded support policy. Phone-specific workflows and native applications are excluded.

### 11.4 Security

- Follow current OWASP guidance selected during system design.
- Validate input at trust boundaries and encode output for its context.
- Apply least privilege to users, containers, databases, and CI credentials.
- Keep secrets outside source control.
- Use HTTPS-only cookies in hosted environments.
- Rate-limit authentication and future public-demo writes.
- Prevent CSV formula injection and unauthorized direct-object access.
- Scan dependencies and container images, then triage findings under a written policy.
- Record a threat model before exposing the system publicly.

### 11.5 Privacy and retention

- Classify employee and customer-contact personal data.
- Record purpose, access, retention, and deletion or anonymization rules in a matrix.
- Keep invoice retention separate from contact-data erasure decisions.
- Preserve immutable invoice artifacts and structured components under the selected retention policy.
- Do not copy unnecessary personal values into audit or technical logs.

### 11.6 Observability

- Produce structured logs with correlation IDs.
- Expose health and readiness endpoints.
- Record basic request, error, job, and database-pool metrics.
- Make failed jobs inspectable.
- Keep audit data separate from diagnostic logs.
- Sanitize errors before returning them or persisting job details.

### 11.7 Reliability and recovery

- Database transactions preserve business invariants.
- Jobs tolerate repeat delivery.
- Migrations run from a clean database and upgrade the supported previous state.
- PostgreSQL data and immutable invoice artifacts have scripted backup and restore procedures.
- The project records evidence of a successful local restore drill in the first post-prototype increment.

## 12. Test strategy

### 12.1 Test boundaries

- Domain tests cover pricing, VAT, totals, stock rules, reservations, state transitions, approval, and payment allocation.
- Spring integration tests exercise HTTP, security, validation, transactions, persistence, and error responses against real PostgreSQL through Testcontainers.
- Focused repository tests exist only for custom queries, SQL, constraints, locking, and database-specific behavior not adequately covered elsewhere.
- Frontend component tests cover forms, validation, permission-dependent presentation, and state changes.
- Browser tests cover a small set of critical workflows and grow with the vertical slices.
- Mocks represent boundaries outside the system, including email, object storage, carrier services, and controllable time.

### 12.2 Prohibited test patterns

- Testing private methods.
- Verifying internal collaborator calls when public behavior is sufficient.
- Querying the database as a side channel when the public interface can prove the result.
- Recomputing the expected value with the same algorithm as production code.
- Writing the entire imagined test suite before implementing behaviors.
- Replacing PostgreSQL behavior with an incompatible in-memory database for integration tests.
- Making every behavior depend on a slow browser test.

### 12.3 Required critical examples

At minimum, automated tests must cover:

- Concurrent confirmation of orders competing for the final stock.
- Self-approval rejection and high-value approval.
- Stale update rejection.
- Order and invoice monetary totals from independent worked examples.
- Immutability after invoice issuance.
- Payment transitions through unpaid, partial, paid, and overdue states.
- Idempotent background-job delivery.
- Permission denial at the HTTP boundary.
- Language selection and localized validation.
- CSV formula-injection protection.
- The full customer-to-invoice browser workflow once all slices exist.

### 12.4 Definition of done for a slice

A slice is done when:

- Behavior and acceptance examples are documented.
- A failing test exists at the appropriate boundary before the behavior is complete.
- The implementation passes the test and relevant regression suite.
- Authorization, validation, localization, audit behavior, and failure cases are covered where relevant.
- Database migrations work from a clean database.
- API documentation matches the implementation.
- Affected UI passes automated accessibility checks and manual keyboard review.
- CI passes.
- The real path works without hidden mock data.
- Related decision and design records are updated.

## 13. CI requirements

GitHub Actions must run:

1. Formatting and lint checks.
2. Static analysis.
3. Domain tests.
4. Frontend component tests.
5. Spring integration tests with PostgreSQL.
6. Migration validation.
7. OpenAPI generation or drift checks.
8. Automated accessibility checks.
9. Production builds.
10. The small critical browser suite after faster checks pass.
11. Dependency and container scanning.

The security policy selected in `SYSTEM_DESIGN.md` decides which finding severities block CI. A passing workflow must not conceal skipped required tests.

## 14. Local development and demo data

One documented Docker Compose command should start the application dependencies needed for professional local development. Expected services include PostgreSQL, local object storage, and development email capture. Development profiles may keep optional tools out of the normal path.

The repository must provide:

- Versioned database migrations.
- A repeatable synthetic German seed dataset.
- Clearly documented local persona accounts.
- A seed reset procedure.
- Health checks.
- No production secrets.
- A route or document that guides reviewers through the main workflow.

The exact frontend, storage service, mail tool, and Compose topology remain system-design decisions.

## 15. Acceptance journeys

### 15.1 Customer tracer

Given a Sales user is signed in, when they create a valid German business customer, then the application confirms creation and the customer appears through the customer interface and search. The behavior works in German and English and creates the required audit record.

### 15.2 Ordinary order

Given a customer and sufficient stock, when Sales creates and confirms an order below the approval threshold, then the system reserves stock, assigns the correct state, preserves price and customer snapshots, and records the transition.

### 15.3 High-value order

Given an order above €10,000 net, when its creator submits it, then it awaits approval. The creator cannot approve it. A different authorized user can approve it, after which stock is reserved. A rejection requires a reason and notifies the creator.

### 15.4 Competing reservation

Given two eligible orders compete for the same remaining stock, when confirmation occurs concurrently, then at most one reserves the unavailable overlap. The other receives an actionable insufficient-stock conflict. Available stock never becomes negative.

### 15.5 Shipment and invoice

Given a confirmed order, when an authorized user ships it, then reserved stock becomes shipped stock, the order moves to `SHIPPED`, and one draft invoice is created. When Accounting issues the invoice, its data and generated PDF become immutable.

### 15.6 Payment and overdue processing

Given an issued invoice, when Accounting records a partial and then final payment, its derived state changes correctly. Given an unpaid invoice passes its due date, the daily job marks it overdue exactly once and creates the intended notifications.

### 15.7 Full critical browser journey

A browser test signs in through the provided personas and proves customer creation, order creation, approval where required, shipment, invoice issuance, and payment recording through the visible application interface.

## 16. Success criteria

Prototype 1 succeeds when:

- Every in-scope capability works through the integrated local application.
- Critical business invariants have automated behavior-focused tests.
- A clean checkout can start through documented commands.
- A recruiter can follow the guided workflow with synthetic German data.
- German is complete for in-scope screens and English is available.
- The system-design record explains each selected technology and rejected alternative.
- The developer can explain at least the stock-concurrency, immutable-invoice, authorization, localization, and idempotent-job stories in an interview.
- No screen, document, seed record, or README implies affiliation with Würth.

## 17. Risks and open implementation decisions

The product scope is approved. The following remain deliberate system-design decisions:

- Frontend framework and component system.
- Module and package architecture.
- Persistence approach.
- Money, VAT, and rounding implementation.
- Locking strategy for reservations.
- Business-number allocation.
- Session storage.
- Background-job and outbox implementation.
- PDF and XRechnung libraries.
- Object storage.
- Observability stack.
- Exact CI quality gates.
- Local Compose topology and later VPS design.

`SYSTEM_DESIGN.md` is the decision form and artifact register for these choices.

## 18. AI-assisted development statement

The project treats agentic development as an explicit engineering skill. Codex may produce substantial implementation code. The developer owns product requirements, acceptance examples, architecture choices, design documents, reviews, security judgments, and final approval.

Architecture decision records and review evidence should capture options considered, reasons for selection, defects found, changes rejected, and tests designed. Project descriptions and interviews must describe authorship honestly.

## 19. Reference and legal note

Domain inspiration: [Würth Industrie Service public C-parts material](https://www.wuerth-industrie.com/web/de/wuerthindustrie/cteile_management/kanban/kanban_formen/kanbanformen.php).

German invoice and retention references used during product discovery:

- [UStG §14](https://www.gesetze-im-internet.de/ustg_1980/__14.html)
- [UStG §14b](https://www.gesetze-im-internet.de/ustg_1980/__14b.html)
- [AO §147](https://www.gesetze-im-internet.de/ao_1977/__147.html)
- [Federal Ministry of Finance e-invoice FAQ](https://www.bundesfinanzministerium.de/Content/DE/FAQ/e-rechnung.html)

These links support product discovery. This PRD is not legal or tax advice. Requirements must be rechecked before a real deployment because laws, administrative guidance, and technical standards change.
