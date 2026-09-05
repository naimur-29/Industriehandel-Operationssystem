# Industriehandel Operationssystem

## System design workbook

This file is both a decision form and the register for the design documents produced during development. Complete it gradually. Do not choose every option before the first slice. Make a decision when a slice needs it, record why, and link the evidence.

Checkboxes record choices:

- `[ ]` not selected
- `[x]` selected
- `[~]` trial in progress
- `[!]` rejected after trial

Do not select incompatible options unless the decision explicitly describes how they coexist.

## 1. Project record

| Field | Entry |
| --- | --- |
| Product owner | `[fill in]` |
| Lead developer and reviewer | `[fill in]` |
| AI implementation tools | Codex `[add versions or working policy]` |
| Repository | `[link]` |
| PRD | [PRD.md](./PRD.md) |
| Current milestone | `[fill in]` |
| Last architecture review | `[YYYY-MM-DD]` |
| Current production status | Local development only |

### Decision rule

For every material choice, record:

1. The requirement or problem.
2. The options actually considered.
3. The chosen option.
4. The reason it fits this project.
5. Costs and risks accepted.
6. Evidence from a spike, test, measurement, or primary documentation.
7. The trigger that would justify revisiting it.

Use an architecture decision record when the explanation no longer fits comfortably in the table.

## 2. Decision register

| ID | Decision | Status | Selected option | ADR | Date | Owner |
| --- | --- | --- | --- | --- | --- | --- |
| D-001 | Frontend framework | Proposed | `[fill in]` | `[link]` | | |
| D-002 | UI component and design system | Proposed | `[fill in]` | `[link]` | | |
| D-003 | Application module architecture | Proposed | `[fill in]` | `[link]` | | |
| D-004 | Backend package structure | Proposed | `[fill in]` | `[link]` | | |
| D-005 | Persistence approach | Proposed | `[fill in]` | `[link]` | | |
| D-006 | Database naming and ID policy | Proposed | `[fill in]` | `[link]` | | |
| D-007 | Money, VAT, and rounding | Proposed | `[fill in]` | `[link]` | | |
| D-008 | Stock concurrency | Proposed | `[fill in]` | `[link]` | | |
| D-009 | Business-number allocation | Proposed | `[fill in]` | `[link]` | | |
| D-010 | Session storage | Proposed | `[fill in]` | `[link]` | | |
| D-011 | Background jobs and outbox | Proposed | `[fill in]` | `[link]` | | |
| D-012 | Invoice PDF | Proposed | `[fill in]` | `[link]` | | |
| D-013 | XRechnung | Proposed | `[fill in]` | `[link]` | | |
| D-014 | Object storage | Proposed | `[fill in]` | `[link]` | | |
| D-015 | Testing toolchain | Proposed | `[fill in]` | `[link]` | | |
| D-016 | Observability | Proposed | `[fill in]` | `[link]` | | |
| D-017 | Local development topology | Proposed | `[fill in]` | `[link]` | | |
| D-018 | CI quality gates | Proposed | `[fill in]` | `[link]` | | |
| D-019 | Later VPS deployment | Deferred | `[fill in later]` | `[link]` | | |
| D-020 | Git and change management | Proposed for CP-00 review | Short-lived checkpoint branches and reviewed PRs | [Policy](#28-git-and-change-management) | 2026-09-05 | Project owner |

## 3. Frontend framework

The application needs dense CRUD screens, bilingual text, accessible forms and tables, permission-aware actions, and a maintainable testing setup.

### Option A: React with TypeScript

- [ ] Select

Why it fits: React has a broad employment market, mature routing, data-fetching, form, table, accessibility, and browser-testing choices. It is easy to find examples and reviewers. This is useful for a portfolio aimed at a range of German employers.

Costs: the team must choose and govern more supporting libraries. Poor choices can scatter server state, form state, and UI state across several patterns.

Choose it when: employer recognition, ecosystem depth, and long-term library availability matter more than a compact framework experience.

Verify before selection: implement the customer tracer with routing, a translated validated form, an accessible paginated table, session expiry handling, and one component test.

### Option B: Svelte with TypeScript

- [ ] Select

Why it fits: Svelte can produce concise components and a small amount of framework ceremony. It suits an individual developer who wants to read generated code carefully.

Costs: the enterprise component and hiring ecosystem is smaller than React's. Library integration patterns may have fewer established answers in a mixed-experience team.

Choose it when: implementation clarity and developer control matter more than matching the largest frontend job market.

Verify before selection: build the same customer tracer used for React and compare code volume, accessibility behavior, test clarity, and table support.

### Option C: server-rendered Spring MVC with progressive enhancement

- [ ] Select

Why it fits: one deployable application, simple session security, fewer cross-origin and client-state problems, and a strong fit for conventional internal CRUD screens.

Costs: it does not meet the original React-or-Svelte portfolio goal and may provide less evidence of modern frontend application work. Highly interactive tables and workflows need deliberate progressive enhancement.

Choose it when: operational simplicity becomes more important than a separate frontend portfolio.

### Selection record

Chosen option: `[fill in]`

Reason: `[fill in with project evidence]`

Rejected options and why: `[fill in]`

## 4. UI component and design system

The visual system should be original, German-first, keyboard-friendly, restrained, and suited to dense operational work. Do not reproduce Würth branding or layouts.

### Option A: accessible unstyled primitives plus project-owned tokens

- [ ] Select

Examples depend on the chosen framework. Use a maintained headless library where it solves difficult interaction behavior, then own typography, spacing, colors, tables, and forms.

Fit: maximum visual originality and explicit accessibility decisions. It creates strong design-system evidence.

Cost: the most design and QA work. Complex widgets still require careful accessibility testing.

### Option B: enterprise component library with a custom theme

- [ ] Select

Examples include a mature framework-compatible library with data grids, dialogs, form controls, and accessibility documentation.

Fit: fastest path to consistent CRUD screens and dense tables.

Cost: large grids may introduce licensing, bundle size, theme constraints, or framework lock-in. A lightly themed default can look like every other demo.

### Option C: utility CSS plus a curated accessible component layer

- [ ] Select

Fit: balances original styling with reusable implementation. Tokens can remain explicit while utility classes accelerate layout work.

Cost: generated code can accumulate long class strings and inconsistent compositions unless components own repeated patterns.

### Required design-system record

Attach or link:

- [ ] Color tokens for light mode, statuses, focus, and charts if later added: `[link]`
- [ ] Typography and density scale: `[link]`
- [ ] Spacing, radius, border, and elevation tokens: `[link]`
- [ ] Form patterns and error summary: `[link]`
- [ ] Table, filter, empty, loading, and pagination patterns: `[link]`
- [ ] German and English content guidelines: `[link]`
- [ ] Keyboard and screen-reader test notes: `[link]`

Chosen option and reason: `[fill in]`

## 5. Application architecture

Prototype 1 should remain one deployable backend and one database. Distributed services would add operational cost without a demonstrated need.

### Option A: modular monolith with feature modules

- [ ] Select

Organize the backend around customers, catalog, inventory, orders, fulfilment, billing, payments, identity, audit, and notifications. Each module owns its application behavior and persistence access. Cross-module calls use explicit public contracts or events.

Fit: supports vertical slices, keeps one transaction boundary where needed, and creates visible module boundaries that can deepen over time.

Cost: boundaries rely on discipline and architecture tests. Shared JPA relationships can quietly couple modules.

### Option B: conventional layered monolith

- [ ] Select

Organize controllers, services, repositories, and entities into broad technical layers.

Fit: familiar to many Spring teams and easy to start.

Cost: one behavior spreads across global packages. Domain boundaries become harder to see as the project grows.

### Option C: ports and adapters inside feature modules

- [ ] Select

Each feature keeps domain and application code independent from web, persistence, jobs, and storage adapters.

Fit: provides clear test boundaries and protects rules from infrastructure code.

Cost: interface and mapping overhead can become ceremony for simple CRUD. Apply it to meaningful boundaries, not every class.

### Recommended evaluation

Compare options using customer creation, concurrent stock reservation, and invoice issuance. They represent simple CRUD, a transactional invariant, and an external artifact boundary.

Chosen shape: `[fill in]`

Module map: `[link to component or module diagram]`

Boundary enforcement: `[ArchUnit, Spring Modulith, review rule, or other choice]`

## 6. Backend package structure

### Option A: package by feature

- [ ] Select

Example top level: `customer`, `catalog`, `inventory`, `order`, `billing`, `identity`.

Best when: vertical ownership and module comprehension matter.

### Option B: package by layer

- [ ] Select

Example top level: `controller`, `service`, `repository`, `entity`.

Best when: the codebase stays small and the team strongly values conventional discovery.

Risk here: the planned depth makes global technical layers likely to become crowded.

### Option C: feature modules with internal layers

- [ ] Select

Example: `order.api`, `order.application`, `order.domain`, `order.infrastructure`.

Best when: the project needs explicit feature ownership and separation of rules from adapters.

Risk: nesting can exceed the complexity of a CRUD-only module. Allow simpler modules to use fewer internal packages.

Chosen structure and exceptions: `[fill in]`

## 7. REST API design

### Option A: resource-oriented REST with explicit action endpoints

- [ ] Select

Use ordinary resource endpoints for CRUD and named commands such as `/orders/{id}/confirm`, `/orders/{id}/approve`, and `/invoices/{id}/issue` for guarded transitions.

Fit: state changes remain visible business operations instead of ambiguous partial updates.

### Option B: pure resource mutation through PATCH

- [ ] Select

Clients request state changes by patching resource fields.

Fit: uniform HTTP vocabulary.

Cost: authorization, reasons, transition commands, and idempotency can become unclear.

### Option C: command-oriented application API

- [ ] Select

Expose commands and task-specific responses rather than general resources.

Fit: matches rich workflows and can make intent explicit.

Cost: less conventional for CRUD clients and may duplicate query models.

### Required API choices

- Base path: `/api/v1` `[confirm or replace]`
- Error format: `[RFC 9457 Problem Details or project format]`
- Pagination: `[cursor, offset, or hybrid]`
- Optimistic lock transport: `[ETag/If-Match, version field, or other]`
- Idempotency keys: `[operations requiring them]`
- OpenAPI source: `[code-first, contract-first, or hybrid]`
- Deprecation policy: `[fill in]`

API guidelines and OpenAPI document: `[links]`

## 8. Persistence approach

### Option A: Spring Data JPA and Hibernate

- [ ] Select

Fit: conventional Spring enterprise stack, productive CRUD, transactions, optimistic locking, and a large knowledge base.

Cost: implicit fetching, cascades, and dirty checking can obscure SQL and module boundaries. Complex reports and locking queries require close review.

### Option B: jOOQ

- [ ] Select

Fit: explicit SQL, generated schema types, strong support for PostgreSQL features, reporting queries, atomic updates, and query review.

Cost: more mapping and SQL ownership for ordinary CRUD. Code generation must fit migrations and CI.

### Option C: mixed JPA and jOOQ

- [ ] Select

Use JPA for transactional aggregates and ordinary CRUD. Use jOOQ for search, reporting, exports, and database-specific concurrency operations.

Fit: each tool handles the work it explains well.

Cost: two persistence models, transaction integration, conventions, and team knowledge must remain explicit. Do not implement the same write path twice.

Chosen approach: `[fill in]`

SQL visibility and query-review policy: `[fill in]`

## 9. Database design system

### 9.1 Naming

#### Option A: lower snake case, singular tables

- [ ] Select

Example: `sales_order`, `invoice_line`, `stock_movement`.

#### Option B: lower snake case, plural tables

- [ ] Select

Example: `sales_orders`, `invoice_lines`, `stock_movements`.

#### Option C: bounded-context prefixes or schemas

- [ ] Select

Example: `billing.invoice` or `billing_invoice`.

Useful when database ownership must visibly follow modules. PostgreSQL schemas add permission and migration considerations.

Record table, column, constraint, index, enum, and foreign-key naming rules: `[link or text]`

### 9.2 Internal identifiers

#### Option A: UUID generated by the application or database

- [ ] Select

Fit: opaque IDs and independent creation. Random UUID versions can enlarge indexes.

#### Option B: time-ordered UUID or ULID

- [ ] Select

Fit: opaque and more index-friendly. Confirm library, database, and sorting behavior before selection.

#### Option C: database bigint identity

- [ ] Select

Fit: compact indexes and simple local generation.

Cost: predictable IDs must never substitute for authorization, and distributed creation is harder if later needed.

Business numbers remain separate from internal IDs under every option.

Chosen ID policy: `[fill in]`

### 9.3 Schema rules to decide

- [ ] Every table has an explicit primary key.
- [ ] Foreign keys express real ownership and reference rules.
- [ ] Required invariants have database constraints where possible.
- [ ] Money uses documented precision and scale.
- [ ] Instants and business dates use distinct types.
- [ ] Mutable roots include an optimistic version.
- [ ] Archive state has a consistent representation.
- [ ] Issued document snapshots do not point only to mutable master data.
- [ ] Audit and technical log storage remain separate.
- [ ] Indexes follow measured query patterns rather than every column.

Data standards document: `[link]`

## 10. ERD development

An ERD records stored facts, ownership, cardinality, optionality, keys, and constraints. It should not become a screenshot generated once and forgotten.

### Professional workflow

1. Start with conceptual entities and relationships from the current slice.
2. Add logical attributes, identifiers, cardinalities, and ownership.
3. Convert the logical model into PostgreSQL tables, types, keys, constraints, and indexes.
4. Check every relationship against create, archive, correction, and retention behavior.
5. Compare the ERD with migrations in review.
6. Update it in the same change that alters persisted structure.

### Required views

- [ ] Context-level domain model: `[attach or link]`
- [ ] Customer and identity ERD: `[attach or link]`
- [ ] Catalog and inventory ERD: `[attach or link]`
- [ ] Order and fulfilment ERD: `[attach or link]`
- [ ] Billing and payment ERD: `[attach or link]`
- [ ] Audit, jobs, and notifications ERD: `[attach or link]`
- [ ] Combined physical ERD: `[attach or link]`

### ERD review checklist

- [ ] Names match the glossary.
- [ ] Cardinalities and optional relationships are explicit.
- [ ] Delete and archive behavior is recorded.
- [ ] Historical snapshots are distinguishable from live references.
- [ ] Unique constraints match business rules.
- [ ] Stock and financial invariants are visible or linked to constraint documentation.
- [ ] Personal data is marked for the retention matrix.
- [ ] Indexes have a query or constraint reason.

## 11. Money, VAT, and rounding

### Option A: project value objects with decimal storage

- [ ] Select

Create explicit `Money`, `Quantity`, `VatRate`, and related domain types. Persist their values through deliberate mappings.

Fit: rules become hard to bypass and tests read in business terms.

Cost: mapping and serialization need discipline.

### Option B: library-based money types

- [ ] Select

Use a maintained Java money implementation where it integrates cleanly with persistence and JSON.

Fit: standardized currency behavior and less custom low-level code.

Cost: VAT, line rounding, and invoice reconciliation remain project rules. A money library does not decide them.

### Option C: decimal fields governed by application services

- [ ] Select

Fit: simplest implementation.

Cost: scale, currency, and rounding rules can drift between modules. This is acceptable only with strict conventions and tests.

### Required calculation record

- Rounding mode: `[fill in]`
- Unit-price scale: `[fill in]`
- Quantity scale: `[fill in]`
- Line-total rule: `[fill in]`
- VAT calculation level: `[line or document, with reason]`
- Reconciliation rule: `[fill in]`
- Effective-dated VAT model: `[fill in]`
- Independent worked examples: `[link]`

Have a German tax professional review these rules before real commercial use.

## 12. Stock concurrency

### Option A: pessimistic row locking

- [ ] Select

Lock the stock balance rows while checking and reserving.

Fit: direct mental model and strong serialization for scarce stock.

Cost: lock ordering, contention, and transaction duration matter. Test deadlocks and timeouts.

### Option B: atomic conditional update

- [ ] Select

Reserve with an update whose predicate requires enough available quantity, then verify one row changed.

Fit: short, database-enforced critical section with good concurrency.

Cost: becomes harder if reservation spans many products and all lines must succeed together. The transaction and rollback behavior need focused tests.

### Option C: optimistic version with retry

- [ ] Select

Update a versioned balance and retry conflicts under a strict policy.

Fit: low contention and consistent use of optimistic locking.

Cost: retries under scarcity may waste work and complicate multi-line orders.

### Selection evidence

- [ ] Concurrent integration test for the last available units.
- [ ] Multi-line rollback test.
- [ ] Deadlock or retry policy documented.
- [ ] Measured result under representative contention.

Chosen approach: `[fill in]`

Interview story and evidence: `[link]`

## 13. Business-number allocation

### Option A: PostgreSQL sequences

- [ ] Select

Fit: concurrency-safe and fast. Gaps are expected and allowed.

### Option B: locked counter per document type and year

- [ ] Select

Fit: explicit formatting and reset policy.

Cost: counter rows can contend. Rollbacks and gaps still require a stated rule.

### Option C: allocate at irreversible business transition

- [ ] Select

Drafts use internal IDs. Final order or invoice numbers are allocated only on confirmation or issuance.

Fit: avoids consuming final numbers for abandoned drafts. Usually combined with A or B.

Record formats, time-zone boundary, gap policy, migration behavior, and concurrency tests: `[link]`

## 14. Authentication and session storage

The PRD selects browser sessions rather than browser-stored JWTs.

### Option A: application-memory sessions

- [ ] Select for early local development only

Fit: minimal setup for one process.

Cost: sessions disappear on restart and cannot support multiple backend replicas.

### Option B: JDBC-backed sessions in PostgreSQL

- [ ] Select

Fit: one existing durable service and familiar Spring Session integration.

Cost: adds session traffic and cleanup work to the business database.

### Option C: Redis-backed sessions

- [ ] Select

Fit: expiry and session workload fit Redis well and permit later multiple replicas.

Cost: adds an operational dependency that Prototype 1 may not need.

Record cookie flags, CSRF strategy, inactivity timeout, lockout rules, password policy, and session cleanup: `[link]`

Later OpenID Connect migration note: `[link]`

## 15. Background jobs and outbox

### Option A: Spring scheduling plus database tables

- [ ] Select

Fit: few jobs, one deployment, direct transactional outbox, and low operational cost.

Cost: retries, leases, visibility, and concurrency control must be implemented or supplied by a focused library.

### Option B: Quartz with JDBC job store

- [ ] Select

Fit: persistent schedules, calendars, retries, and operational controls.

Cost: larger model and configuration than two baseline jobs may need.

### Option C: dedicated database-backed job library

- [ ] Select

Fit: persistent execution, retries, dashboards, and idempotent handler conventions without a broker.

Cost: library-specific tables and behavior. Confirm maintenance, licensing, Spring compatibility, and failure semantics.

Message brokers are a later option only when an integration or load requirement justifies them.

Record outbox transaction boundary, polling or delivery method, idempotency key, lease behavior, retries, and failure handling: `[link]`

## 16. PDF and XRechnung

Library names and versions change. Confirm current maintenance, licenses, PDF features, EN 16931 support, and validation behavior against primary documentation before choosing.

### PDF option A: HTML and CSS template rendered to PDF

- [ ] Select

Fit: designers can work with familiar markup and preview the invoice in a browser.

Cost: renderer CSS support, fonts, pagination, and deterministic output need verification.

### PDF option B: programmatic PDF library

- [ ] Select

Fit: precise document control and fewer browser-rendering differences.

Cost: layout code can become verbose and harder to review visually.

### PDF option C: reporting template engine

- [ ] Select

Fit: mature business-report concepts, parameters, tables, and subreports.

Cost: template tooling and runtime complexity may exceed one invoice design.

### XRechnung option A: generate EN 16931 XML through a maintained library

- [ ] Select

Fit: domain code maps invoice snapshots into a known model.

Cost: library release support must match the selected German standard version.

### XRechnung option B: generate XML from owned templates and schema-bound types

- [ ] Select

Fit: full control and transparent mapping.

Cost: the project owns more standard detail and upgrade work.

### XRechnung option C: external conversion service

- [ ] Select later only

Fit: delegates standard maintenance.

Cost: privacy, availability, pricing, vendor dependency, and test isolation. It is unnecessary for the local baseline.

### Required document evidence

- [ ] Invoice field-to-domain mapping: `[link]`
- [ ] PDF visual regression samples: `[link]`
- [ ] Font and license record: `[link]`
- [ ] XRechnung version decision: `[link]`
- [ ] Official validator output and automated validation test: `[link]`
- [ ] Artifact checksum and template-version design: `[link]`

## 17. Object storage

### Option A: filesystem behind a storage port

- [ ] Select

Fit: few moving pieces for local development.

Cost: later VPS backup, permissions, atomic writes, and migration to object storage need care.

### Option B: local S3-compatible service

- [ ] Select

Fit: exercises object-storage semantics locally and supports checksums and immutable artifacts.

Cost: another container and credential set.

### Option C: PostgreSQL binary storage

- [ ] Select

Fit: transactional ownership and one backup system.

Cost: database growth, backup size, and artifact streaming. Large binary data does not belong here without measurements.

Record object key policy, metadata, immutability, checksum verification, orphan cleanup, backup, and restore: `[link]`

## 18. Test toolchain

The behavior boundaries in the PRD are fixed. Libraries remain choices.

### Backend option A: JUnit, AssertJ, Spring Boot Test, Testcontainers

- [ ] Select

Fit: conventional Java and Spring stack with real PostgreSQL integration.

### Backend option B: add an architecture-test library

- [ ] Select with A

Fit: enforces feature-module dependencies and prevents accidental package coupling.

### Backend option C: add property-based testing for selected rules

- [ ] Select with A

Fit: valuable for money invariants, allocation, state-machine constraints, and generated edge cases. It supplements worked examples rather than replacing them.

### Frontend component option A: framework-native testing library

- [ ] Select

Test accessible behavior through roles, labels, visible state, and user events.

### Browser option A: Playwright

- [ ] Select

Fit: multi-browser support, tracing, strong waiting behavior, and API utilities.

### Browser option B: Cypress

- [ ] Select

Fit: interactive debugging and a mature web-testing workflow.

### Browser option C: Selenium-based stack

- [ ] Select

Fit: common in established Java organizations and broad WebDriver compatibility.

Cost: more synchronization and driver concerns for a new project than newer browser-native tools may require.

### Test record

- Test pyramid or portfolio diagram: `[link]`
- Worked business examples: `[link]`
- Naming and fixture policy: `[link]`
- Test-data builders and seed boundary: `[link]`
- Flaky-test policy: `[link]`
- Coverage policy and why: `[link]`
- Browser workflow inventory: `[link]`

## 19. Observability

### Option A: Spring Actuator, structured logs, and Micrometer only

- [ ] Select

Fit: exposes standard health and metrics without requiring a full local monitoring stack.

### Option B: add Prometheus and Grafana locally through an optional profile

- [ ] Select

Fit: visible metrics and dashboard practice. Keep it optional so ordinary development stays light.

### Option C: add OpenTelemetry and a local trace backend

- [ ] Select

Fit: traces requests through HTTP, database calls, storage, and background jobs.

Cost: one modular monolith may not need distributed tracing. Add it to answer a debugging or future integration need, not for a logo list.

Record log schema, correlation propagation, redaction, metric names, alert candidates, health semantics, and retention: `[link]`

## 20. Local development topology

### Option A: frontend and backend run on the host, dependencies in Compose

- [ ] Select

Fit: fast reload and debugging. Compose starts PostgreSQL, storage, and development mail.

Cost: host toolchain versions need management.

### Option B: all services run in Docker Compose

- [ ] Select

Fit: consistent startup and closer container testing.

Cost: slower rebuild loops and more debugger setup.

### Option C: hybrid profiles

- [ ] Select

Default to dependencies in containers and application processes on the host. Provide a full-container profile for clean-checkout and release checks.

Fit: preserves fast development and proves container packaging.

### Required local-development record

- [ ] Prerequisite versions: `[link]`
- [ ] One-command dependency startup: `[command]`
- [ ] One-command full-stack startup: `[command]`
- [ ] Migration and seed commands: `[commands]`
- [ ] Reset command: `[command]`
- [ ] Health verification: `[command or link]`
- [ ] Debugging instructions: `[link]`
- [ ] Resource requirements: `[fill in]`

Chosen topology: `[fill in]`

## 21. CI and code quality

### Option A: one required GitHub Actions workflow with staged jobs

- [ ] Select

Fit: one visible gate with parallel fast checks and dependent integration or browser jobs.

### Option B: separate required workflows by concern

- [ ] Select

Fit: independent ownership and reruns for frontend, backend, security, and browser suites.

Cost: required-check configuration and cross-workflow dependencies need care.

### Option C: reusable workflows and local task runner

- [ ] Select with A or B

Fit: CI invokes the same named tasks developers run locally, reducing shell duplication.

### Gate form

- [ ] Formatting
- [ ] Frontend lint and type checks
- [ ] Java static analysis
- [ ] Domain tests
- [ ] Frontend component tests
- [ ] Spring integration tests with Testcontainers
- [ ] Migration validation
- [ ] OpenAPI drift check
- [ ] Accessibility automation
- [ ] Production frontend and backend builds
- [ ] Critical browser suite
- [ ] Dependency scan
- [ ] Container scan
- [ ] License policy check

Blocking vulnerability policy: `[fill in severity, exploitability, exception owner, and expiry]`

Branch protection and review policy: [Git and change management](#28-git-and-change-management). Record verified hosting settings during CP-00b and extend required checks when CI jobs exist.

## 22. Later VPS deployment

Do not implement this for Prototype 1. Complete the record when local behavior is satisfactory.

### Option A: Docker Compose on one VPS

- [ ] Consider later

Fit: lowest operational cost and easiest migration from local containers.

Cost: one failure domain and maintenance downtime.

### Option B: managed PostgreSQL plus application containers on one VPS

- [ ] Consider later

Fit: delegates database durability and upgrades.

Cost: higher monthly cost and network dependency.

### Option C: small managed container platform

- [ ] Consider later

Fit: managed ingress, certificates, rollouts, and logs.

Cost: platform limits and pricing may outweigh the needs of a portfolio demo.

Before public access, document DNS, TLS, reverse proxy, secrets, backups, restore, log rotation, resource limits, rate limits, demo reset, monitoring, patching, and incident response.

Chosen later option: `[deferred]`

## 23. Design-document register

Store editable source beside rendered output where practical. Prefer text-based formats that reviewers can diff. Images and PDFs should link back to their source.

| Artifact | When required | Status | Source | Rendered copy | Last reviewed |
| --- | --- | --- | --- | --- | --- |
| Product glossary, DE and EN | Before walking skeleton | Not started | `[link]` | `[link]` | |
| System context diagram | Before walking skeleton | Not started | `[link]` | `[link]` | |
| User and permission matrix | Before authentication slice | Not started | `[link]` | `[link]` | |
| Quality-attribute scenarios | Before architecture selection | Not started | `[link]` | `[link]` | |
| Initial threat model | Before authentication slice | Not started | `[link]` | `[link]` | |
| Domain model | Updated per domain slice | Not started | `[link]` | `[link]` | |
| ERD set | Updated per persistence slice | Not started | `[link]` | `[link]` | |
| API contract | Updated per HTTP slice | Not started | `[link]` | `[link]` | |
| Order state diagram | Before order transitions | Not started | `[link]` | `[link]` | |
| Invoice state diagram | Before invoice issuance | Not started | `[link]` | `[link]` | |
| Sequence diagram, stock race | Before reservation implementation | Not started | `[link]` | `[link]` | |
| Sequence diagram, invoice issue | Before artifact implementation | Not started | `[link]` | `[link]` | |
| DFD and trust boundaries | Before handling personal data | Not started | `[link]` | `[link]` | |
| Data dictionary | Maintained with schema | Not started | `[link]` | `[link]` | |
| Retention and deletion matrix | Before invoice issuance | Not started | `[link]` | `[link]` | |
| Test strategy and examples | Before first business slice | Not started | `[link]` | `[link]` | |
| Requirement-to-test traceability | Maintained per slice | Not started | `[link]` | `[link]` | |
| Deployment diagram | Before full Compose setup | Not started | `[link]` | `[link]` | |
| Backup and restore runbook | Post-prototype increment | Not started | `[link]` | `[link]` | |
| Operational runbook | Before public hosting | Deferred | `[link]` | `[link]` | |
| Risk register | Start before walking skeleton | Not started | `[link]` | `[link]` | |
| ADR index | Maintained per decision | Not started | `[link]` | `[link]` | |
| AI-assisted review record | Maintained per slice | Not started | `[link]` | `[link]` | |

## 24. How to produce the documents

### 24.1 Product glossary

Record one preferred German term, one English term, definition, aliases to avoid, and the code name for every business concept. Ask a domain-aware German speaker to review customer-facing terms. Do not translate identifiers mechanically after code has spread them.

Attachment: `[link]`

### 24.2 System context diagram

Show the operations system as one box, its user roles, and external systems or actors. For Prototype 1, PostgreSQL is an internal container, not an external actor. Email capture and object storage may appear as external dependencies at the container level.

Review questions:

- Who uses the system?
- What data crosses its boundary?
- Which dependencies can fail independently?
- Which integrations are real, simulated, or deferred?

Attachment: `[link]`

### 24.3 Container and component diagrams

Use a C4-style hierarchy if it helps. The container diagram should show browser frontend, Spring application, PostgreSQL, object storage, and mail capture. Component diagrams should exist only for modules with non-obvious collaboration, such as inventory reservation or billing.

Attachments: `[links]`

### 24.4 Data-flow diagrams

Use DFDs to follow personal, authentication, invoice, and export data across trust boundaries. Label stores, processes, external actors, transport protection, and where data appears in logs or backups.

Create focused DFDs for:

- Login and session creation.
- Customer-contact data.
- Invoice generation and artifact storage.
- CSV export.
- Future public demo reset.

Do not use a DFD as a second component diagram. Its purpose is data movement and trust.

Attachments: `[links]`

### 24.5 State-transition diagrams

For orders, invoices, jobs, and imports, record legal states, allowed transitions, initiating permission, guards, side effects, audit event, and failure behavior. Generate tests from the transition table without making the test calculate its own expected result.

Attachments: `[links]`

### 24.6 Sequence diagrams

Create them only when timing, transaction boundaries, concurrency, retries, or several components make prose ambiguous. Required candidates are concurrent stock confirmation, invoice issue and artifact persistence, and outbox delivery.

Each sequence should mark:

- Transaction start and commit.
- Locks or conditional updates.
- External boundary calls.
- Retry and idempotency points.
- User-visible success and failure.

Attachments: `[links]`

### 24.7 Threat model

Start with assets, actors, entry points, trust boundaries, and abuse cases. Review authentication, authorization, object access, CSV injection, stored text, invoice artifacts, imports, exports, demo accounts, job controls, logs, secrets, backups, and dependency supply chain.

For every material threat, record likelihood, impact, mitigation, verification, owner, and accepted residual risk. Revisit it before public hosting.

Attachment: `[link]`

### 24.8 Permission matrix

Rows are operations, not pages. Columns are default roles. Record read, create, edit, archive, confirm, approve, ship, issue, cancel, pay, export, adjust, and administer permissions. Include ownership rules and prohibited combinations such as self-approval.

Attachment: `[link]`

### 24.9 Data dictionary

For each field, record German and English business name, meaning, type, unit or format, nullability, validation, source, sensitivity, retention class, mutability, and example. Link the owning requirement and database column.

Attachment: `[link]`

### 24.10 Retention and deletion matrix

For each data class, record purpose, legal or operational basis to verify, retention trigger, duration, archive behavior, deletion or anonymization method, backup treatment, and approving role. Keep invoice artifacts separate from mutable customer contacts.

Attachment: `[link]`

### 24.11 Quality-attribute scenarios

Write measurable scenarios with source, stimulus, environment, affected component, response, and measure. At minimum cover concurrent stock reservation, stale edits, list performance at target volume, job redelivery, backup restoration, permission denial, and German-English switching.

Attachment: `[link]`

### 24.12 Traceability map

Map PRD requirement IDs to acceptance examples, implementation slice, automated tests, API operations, schema elements where helpful, and status. Traceability should expose missing behavior, not become a claim that every line of code maps to a requirement.

Attachment: `[link]`

### 24.13 Architecture decision records

Use one short file per durable choice:

```markdown
# ADR-NNN: Decision title

## Status

Proposed, accepted, superseded, or rejected.

## Context

What requirement, constraint, or observed problem requires a choice?

## Options considered

What credible options were examined?

## Decision

What was selected?

## Consequences

What improves, what becomes harder, and what operational cost is accepted?

## Evidence

Links to a spike, test, measurement, primary documentation, or incident.

## Revisit when

What measurable condition would justify reopening the choice?
```

ADR directory: `[link]`

### 24.14 AI-assisted review record

For each slice, record:

- Requirements and examples supplied to Codex.
- Files or behaviors Codex produced.
- Human review performed.
- Defects, unsafe assumptions, and missing cases found.
- Changes rejected or rewritten and why.
- Tests added from independent examples.
- Commands and results used to verify the slice.
- Remaining risks accepted by the developer.

This record demonstrates agentic development skill through evidence rather than slogans.

Attachment: `[link]`

## 25. Slice design sheet

Copy this section for each vertical slice.

### Slice `[number and name]`

Business outcome: `[observable result]`

PRD requirements: `[IDs]`

Actor and permission: `[fill in]`

Preconditions: `[fill in]`

Worked examples with literal expected results: `[link or text]`

Public interface under test: `[domain API, HTTP API, component, or browser]`

First failing test: `[link]`

Database change and ERD update: `[link or none]`

API change: `[link or none]`

Authorization and abuse cases: `[fill in]`

Localization impact: `[fill in]`

Accessibility checks: `[fill in]`

Audit and observability impact: `[fill in]`

Failure and recovery behavior: `[fill in]`

AI-assisted review record: `[link]`

Verification commands and results: `[fill in]`

Accepted residual risks: `[fill in]`

Completion date and reviewer: `[fill in]`

## 26. Professional review gates

### Before walking skeleton

- [ ] Glossary reviewed.
- [ ] Context and container diagrams reviewed.
- [ ] Initial permission matrix reviewed.
- [ ] Threat model started.
- [ ] Frontend, backend module, persistence, session, test, and local-topology decisions recorded.
- [ ] Git workflow reviewed and actual repository controls recorded.

### At walking-skeleton completion

- [ ] CI proves a minimal integrated path.
- [ ] Documented local startup connects frontend, backend, PostgreSQL, and migrations.

### Before each checkpoint merges

- [ ] Checkpoint scope, review evidence, and PR are linked.
- [ ] The human has accepted the reviewed implementation revision.
- [ ] Applicable checks pass on the final revision; new behavior changes receive another review.
- [ ] Existing behavior still works and incomplete features are not presented as finished.
- [ ] Affected design records are updated in the same change.

### Before each slice is accepted

- [ ] Slice sheet complete.
- [ ] Acceptance examples do not reproduce the production algorithm.
- [ ] Tests observe behavior at the narrowest useful boundary.
- [ ] Authorization checked in the backend.
- [ ] Schema, API, diagrams, and traceability updated where affected.
- [ ] German and English behavior reviewed.
- [ ] Accessibility and failure behavior reviewed.
- [ ] Codex output received human review.
- [ ] CI passes without unexplained skips.

### Before Prototype 1 is declared complete

- [ ] Every in-scope PRD item has evidence.
- [ ] Full critical browser journey passes.
- [ ] Concurrency and idempotency scenarios pass against PostgreSQL.
- [ ] Fresh local setup and seed procedure pass.
- [ ] No real company data, branding, or implied endorsement remains.
- [ ] Security findings follow the recorded policy.
- [ ] Architecture and risk records match the implemented system.
- [ ] Guided recruiter workflow is usable.

### Before public VPS deployment

- [ ] Threat model updated for public access.
- [ ] Restricted demo permissions verified.
- [ ] Reset isolation and abuse limits tested.
- [ ] DNS, TLS, secrets, patching, backups, restore, logging, monitoring, and incident procedures documented.
- [ ] No local or test credentials grant platform control.
- [ ] Legal and privacy requirements rechecked against current official sources.

## 27. Current decisions inherited from the PRD

These product decisions are already made. Do not reopen them as technology preferences:

- [x] Single-company B2B industrial C-parts distributor.
- [x] One warehouse.
- [x] German default with English option and browser-language detection.
- [x] EUR and `Europe/Berlin` defaults.
- [x] Configurable roles with default business roles.
- [x] Server-side browser sessions.
- [x] PostgreSQL-backed search for the baseline.
- [x] Immutable issued-invoice snapshots and artifacts.
- [x] PDF first, XRechnung in the next increment.
- [x] Real PostgreSQL integration tests through Testcontainers.
- [x] Browser tests grow with vertical slices.
- [x] Docker Compose supports professional local development.
- [x] VPS deployment remains deferred.

If implementation evidence forces one of these to change, update the PRD and create an ADR. Do not let the code silently make the decision.

## 28. Git and change management

### 28.1 Scope and decision status

D-020 proposes the following workflow for human review in CP-00. It covers code, tests, migrations, configuration, and documentation throughout the project. Repository settings remain unverified until CP-00b records them. Writing this policy does not configure hosting or authorize a release.

The project has one human maintainer reviewing Codex's work and no maintained production release lines. Use short-lived branches from `main`, one checkpoint per pull request, and squash merging as the proposed default. This follows the branch-and-review approach described in [GitHub flow](https://docs.github.com/en/get-started/using-github/github-flow). The checkpoint granularity and merge strategy are project choices.

| Option | Decision rationale |
| --- | --- |
| Short-lived branches and reviewed PRs | Proposed. Fits small changes and frequent human review. |
| Long-lived `develop` and release branches | Defer. Introduces integration work without a current need to maintain several released versions. |
| Direct implementation commits to `main` | Reject for normal development. Removes the separate review step this project requires. |

Revisit the choice when several developers regularly work together, deployment needs stabilization periods, or supported releases need independent fixes. Application module architecture and Git branching are separate decisions.

### 28.2 Branches and commits

| Item | Project convention |
| --- | --- |
| Default branch | `main`; keep existing delivered behavior working |
| Task branch | `<type>/cp-<id>-<short-description>`, for example `feat/cp-31-create-customer` |
| Types | `feat`, `fix`, `docs`, `test`, `refactor`, `build`, `ci`, or `chore` |
| Commit subject | `<type>: <concrete change> [CP-NN]` |
| Example | `feat: create customer through HTTP [CP-31]` |
| PR title | `CP-31: Create and retrieve a customer` |
| Language | English code identifiers, commits, PRs, and engineering documents; German-first product text with English translations |
| Merge | Squash the accepted checkpoint into one understandable change on `main` |

Use UTF-8 and repository-managed line-ending and editor conventions. Commit wrappers, migrations, and dependency lockfiles. Exclude credentials, generated build output, local databases, IDE user state, and test traces containing sensitive values. Do not rewrite unrelated files just to apply formatting.

Inspect the working tree before switching branches or staging. Preserve existing user changes and stage only the checkpoint's files or hunks. Do not reset, discard, or stash unrelated work without an explicit reason and authorization.

Commit coherent work during implementation. Record the failing and passing TDD runs in review evidence; separate failing-test commits are optional. Never merge a deliberately broken state to prove TDD. Push task branches under the agreed session authorization so remote review and backup can occur.

### 28.3 Pull requests and human review

A PR contains the checkpoint ID, concrete behavior change, requirement references, verification commands and results, the review-record link, and relevant limitations. Include screenshots only when they help inspect UI behavior. Keep unrelated cleanup in another checkpoint.

Codex prepares and checks the change. The human reviews the diff, expected outcomes, test quality, affected permissions, migrations, and documentation. Codex cannot approve its own output on the human's behalf. Mark the checkpoint accepted only after the human accepts it and record the reviewed commit.

GitHub does not allow PR authors to approve their own PRs. If Codex operates through the maintainer's account, record that maintainer's review explicitly in the review record or PR discussion. Do not require an unavailable second account or describe self-review as independent peer review. When a second human joins, require their approving review and configure stale approval dismissal. See [GitHub's review rules](https://docs.github.com/en/pull-requests/how-tos/review-pull-requests/approving-a-pull-request-with-required-reviews).

### 28.4 Repository controls and integration

Configure the following where the actual hosting plan supports them. Record unavailable controls and the manual procedure used instead; never imply that a written rule is enforced by the server.

- Require PRs for changes to `main` and prevent force pushes and deletion of that branch.
- Require applicable CI checks after their jobs exist. Run application checks on PRs and on the integrated `main` revision.
- Require review discussions to be resolved before merge.
- Use the solo-maintainer arrangement above until independent review is available. Do not set an impossible approval requirement and then routinely bypass it.
- Keep repository and CI permissions limited to their work. Protect deployment credentials separately when deployment enters scope.

Before merge, incorporate current `main` when necessary, resolve conflicts, and rerun affected checks. Avoid rewriting published branch history by default. Never force-push a shared branch as routine cleanup. A code change after acceptance requires human review again; checkbox and evidence-only updates must be identified as such.

After acceptance and passing checks, the authorized maintainer squash-merges the PR. Record the merged commit and PR in the review record, update the checkpoint register, sync local `main`, and delete the merged task branch once its work is retained. Integration does not itself authorize deployment.

During bootstrap, use direct documentation checks until CI exists. Record local evidence honestly. External CI or protection verification stays pending if access is unavailable. Once a gate is configured, a failing gate blocks merge; do not bypass it to make progress appear complete.

### 28.5 Corrections, releases, and recovery

Correct an accepted change in a new checkpoint and PR. Use a revert commit when undoing an integrated change; preserve history and link the reason. Database changes need a forward migration or a separately reviewed recovery procedure. Reverting application code does not undo persisted data.

Tag accepted runnable milestones with annotated tags, starting with `v0.1.0` for the customer tracer if no release has used that version. Record delivered behavior and known limitations in release notes. Do not tag every checkpoint or overwrite published tags. Select a public version compatibility policy before an external API or deployment depends on it.

Include repository recovery in the development process. Record the owner, independent backup location, backup schedule, and a tested restore procedure in CP-00b. Back up all required refs and separately account for PR discussions, issues, settings, and any large-file storage; an ordinary clone does not preserve all hosting metadata. Keep application database and invoice-artifact recovery in their separately planned milestones.

### 28.6 Control record

| Control | Current evidence |
| --- | --- |
| D-020 human acceptance | Pending CP-00 |
| Contribution and PR templates | Pending CP-00a |
| Hosting capabilities and default-branch protection | Pending CP-00b |
| Review arrangement and merge authorization | Pending CP-00 |
| Repository backup and recovery evidence | Pending CP-00b |
| Initial required CI checks | Pending CP-18 |
| Full first-slice CI checks | Pending CP-46 |

## 29. Engineering practices and German context

### 29.1 How to use German references

There is no single national Git branching model or application architecture to select. This project records the source, applicable scope, and evidence for each practice. A preference such as squash merging is a project convention, not a German standard or a legal obligation.

The BSI's CON.8 software-development guidance addresses development testing and controlled, traceable, reversible source changes, including version-control backups. Use these concerns to review this project's process. See the [BSI CON.8 reference, edition 2021](https://www.bsi.bund.de/SharedDocs/Downloads/DE/BSI/Grundschutz/IT-GS-Kompendium_Einzel_PDFs_2021/03_CON_Konzepte_und_Vorgehensweisen/CON_8_Software_Entwicklung_Edition_2021.pdf?__blob=publicationFile&v=2) and the [BSI edition 2023 publication page](https://www.bsi.bund.de/SharedDocs/Downloads/DE/BSI/Grundschutz/IT-GS-Kompendium_Einzel_PDFs_2023/03_CON_Konzepte_und_Vorgehensweisen/CON_8_Software_Entwicklung_Edition_2023.html). These dated references are engineering input, not evidence that the project satisfies the current complete IT-Grundschutz framework.

Reference review date: 2026-09-05. Recheck applicable editions and requirements before making compliance claims or preparing real commercial use. The PRD's existing legal, privacy, invoice, and accessibility verification requirements still apply. Do not expand this portfolio into a certification project without a separate scope decision.

### 29.2 Architecture documentation with arc42

Use arc42 as a review structure for the existing documents. Its template covers goals, constraints, context, solution strategy, building blocks, runtime and deployment views, cross-cutting concepts, decisions, quality, risks, and terminology. It allows adaptation to the project. See the [official German arc42 overview](https://arc42.de/overview/).

Keep one authoritative record for each topic. Link existing material instead of creating a second architecture book with duplicate content.

| arc42 topic | Project record |
| --- | --- |
| Goals and constraints | PRD sections 1 through 7 and 11; this workbook's project record |
| Context and boundaries | Context diagrams and DFDs in the artifact register |
| Solution strategy and building blocks | Application architecture, package structure, and module map |
| Runtime behavior | Slice examples, transition tables, and sequence diagrams |
| Deployment | Local topology and container diagram; VPS view remains deferred |
| Cross-cutting concepts | Authentication, permissions, localization, errors, persistence, audit, and observability |
| Architecture decisions | D-001 onward and linked ADRs |
| Quality requirements | PRD targets and measurable quality scenarios |
| Risks and technical debt | Risk register with owners and revisit conditions |
| Glossary | German and English domain terms with agreed code names |

Update these records as behavior changes. Use a diagram when relationships or timing need one, and keep its editable source in Git. Record missing evidence as pending rather than drawing a complete system that does not exist yet.

### 29.3 Practices carried through every milestone

| Concern | Project practice | Evidence |
| --- | --- | --- |
| Requirements | Link each checkpoint to agreed behavior and distinguish partial coverage | Slice record and requirement-to-test map |
| Architecture | Record credible options, tradeoffs, and reasons when a slice needs a choice | ADRs and updated diagrams |
| Implementation | Use behavior-driven TDD examples, negative cases, and relevant integration tests | Red/green record and CI results |
| Review | A human reviews Codex output before acceptance; independent review is recorded only when it occurs | Reviewed commit, findings, and reviewer identity |
| Change control | Use the Git lifecycle in section 28 for every checkpoint | Branch, PR, merge commit, and recovery record |
| Security and privacy | Review trust boundaries, sensitive data, access controls, dependencies, and retention as applicable | Threat model, scans, and data dictionary |
| German business behavior | Use the PRD's German wording, EUR, locale, time-zone, and synthetic-data rules | Worked examples and interface review |
| Delivery | Prove clean startup and state limitations before calling a milestone complete | Demo guide and acceptance evidence |

These are this project's selected working practices. Framework choices remain open until their decision checkpoints; neither German context nor arc42 selects React, JPA, microservices, or a particular branch naming scheme.
