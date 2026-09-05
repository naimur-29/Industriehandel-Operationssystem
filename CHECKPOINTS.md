# Development checkpoints

This is the project's working register for planning, implementation, review, and delivery. Keep accepted work here as history. Add future checkpoint sets to this file when their scope is agreed.

| Reference | Purpose |
| --- | --- |
| [PRD](./PRD.md) | Product scope and behavior requirements |
| [System design](./SYSTEM_DESIGN.md) | Architecture decisions and design evidence |
| [Git workflow](./SYSTEM_DESIGN.md#28-git-and-change-management) | Branches, commits, reviews, merges, and releases |
| [Engineering references](./SYSTEM_DESIGN.md#29-engineering-practices-and-german-context) | German context, source guidance, and project conventions |

## Current work

| Field | Value |
| --- | --- |
| Current milestone | [M-01. Customer tracer](#m-01-customer-tracer) |
| First checkpoint to review | [CP-00. Review the project Git policy](#cp-00) |
| Milestone state | Planned |
| Current review record | `docs/reviews/customer-tracer.md`, created in CP-01 |
| Next milestone | Not planned yet |

Update this table when work advances. Checkpoint checkboxes are the acceptance record; the table is only a navigation aid.

## Navigation

- [Working rules](#working-rules)
- [Git lifecycle for every checkpoint](#git-lifecycle-for-every-checkpoint)
- [Adding and changing checkpoints](#adding-and-changing-checkpoints)
- [M-01. Customer tracer](#m-01-customer-tracer)
- [Reusable checkpoint template](#reusable-checkpoint-template)
- [Plan change log](#plan-change-log)

## Working rules

### Review size and completion

Each checkpoint is one change the human can review in 15 to 45 minutes, with 60 minutes as the limit. Implementation and study time are separate. Split a task before coding if its decisions, files, or acceptance cases would exceed that review budget.

Codex completes one checkpoint and stops for review. Only the human marks its checkbox after accepting the work. Passing tests alone do not mean acceptance. Study topics should prepare the reviewer to explain the changed code or decision, not require finishing an entire course.

Use the following status words alongside the checkbox:

| Status | Checkbox | Meaning |
| --- | --- | --- |
| Planned | Unchecked | Work has not started |
| In progress | Unchecked | Codex is implementing or addressing review findings |
| Ready for review | Unchecked | Evidence is available; human acceptance is pending |
| Blocked | Unchecked | Record the blocker and the next action |
| Accepted | Checked | Human review is complete; record reviewer and date |
| Superseded | Unchecked | Record replacement IDs; this work was not completed |

Acceptance and merge are different events. Record branch, reviewed commit, pull request, merge state, and final merge commit in the evidence entry. Do not start dependent work from an unmerged predecessor without an explicitly recorded dependency plan.

### Tests and evidence

For behavior changes, work through one example at a time:

1. Write a failing test at the narrowest useful public boundary.
2. Run it and record why it failed.
3. Implement enough behavior to pass it.
4. Refactor and run the relevant checks again.
5. Record the result and present the diff for human review.

Use real PostgreSQL through Testcontainers for database behavior. Test observable results, not private methods or internal call sequences. Documentation and configuration need direct checks such as a build, Compose validation, or clean startup; do not invent unit tests for static files.

Keep the full evidence in the milestone's review record and link its checkpoint entry from **Evidence**. Include acceptance examples, changed files, verification commands and results, red/green evidence where relevant, human findings, and unresolved limitations. Record reviewer, acceptance date, and Git references. Before CP-01 creates the review file, retain bootstrap evidence in its PR and transfer the links afterward.

Keep secrets and session values out of evidence. Update affected diagrams, API contracts, glossary terms, and requirement mappings in the same change. Final milestone reviews reconcile these records; they do not replace incremental maintenance.

### Execution order

Follow the displayed order within the active milestone. Earlier checkpoints must be accepted and merged, unless marked superseded with accepted replacements. Prerequisite fields name additional references and important dependencies; they do not silently authorize skipping earlier work.

An intermediate checkpoint can leave a feature incomplete, but it must preserve the behavior already delivered. The milestone acceptance checkpoint proves the integrated business workflow.

## Git lifecycle for every checkpoint

Use the [system design Git policy](./SYSTEM_DESIGN.md#28-git-and-change-management) throughout development, including documentation changes.

| Step | Required action |
| --- | --- |
| Start | Inspect status and current branch. Preserve unrelated work. Start a short-lived branch from current `main`, such as `feat/cp-31-create-customer`. |
| Implement | Keep the diff within the checkpoint. Commit coherent changes with the checkpoint ID. Record TDD evidence without requiring broken commits on `main`. |
| Prepare review | Run applicable checks. Review the diff for unrelated files and secrets. Prepare a PR with scope, test results, and the evidence link. |
| Human review | The human inspects code and evidence. Address findings and rerun affected checks. Record acceptance against the reviewed commit. |
| Merge | Merge only after human acceptance and required checks on the final revision. Follow the accepted merge policy and record the resulting commit. |
| Continue | Update this register, sync `main`, and start the next branch. Remove merged task branches after confirming the work is retained. |

The human may mark acceptance in the PR branch before merge. If only the checkbox and review metadata change afterward, record that fact; code or behavior changes require another review. Add the final merge link to the review record in the next documentation update rather than trying to write a commit's own hash into itself.

Suggested session instruction:

> Complete checkpoint CP-NN only. Read its prerequisites and the Git policy. Use TDD for behavior changes. Record verification and Git evidence. Stop for a decision if an unresolved choice changes the scope. Leave acceptance to the human and present the diff for review.

## Adding and changing checkpoints

- Give each milestone a stable ID such as `M-02`, an observable finish line, explicit exclusions, a review-record path, and a final acceptance checkpoint.
- Add new milestones before the reusable template. Update navigation and the milestone register. Plan only the next agreed scope; do not preselect future technologies.
- Keep checkpoint IDs globally unique. Preserve CP-01 through CP-50. The next ordinary ID is CP-51. CP-00 and its suffixes establish the workflow before the original sequence.
- Never renumber existing IDs or change their anchors. When splitting CP-27, use CP-27a and CP-27b beside the original, mark the uncompleted parent superseded, and link both replacements. Do not reuse retired IDs.
- State dependencies explicitly when adding work out of numerical order. Display order governs execution, not the numeric value alone.
- Keep accepted scope and evidence intact. Add a corrective checkpoint for a later defect or changed requirement. Do not rewrite history to imply the original behavior was different.
- Carry deferred requirements into the next planning discussion. Deferral does not mean completion or permission to expand the current milestone.
- Add a dated plan-change entry for scope, dependency, or workflow changes. Formatting-only edits need no new milestone.

## Milestone register

| ID | Outcome | Checkpoints | State |
| --- | --- | --- | --- |
| [M-01](#m-01-customer-tracer) | Sign in, create a customer, and find it again | CP-00, CP-00a, CP-00b, then CP-01 through CP-50 | Planned |

## M-01. Customer tracer

The starting point is the documentation-only repository. The finish line comes from PRD section 15.1. A seeded Sales user signs in, creates a valid synthetic German business customer, and finds it through the customer list and search.

The workflow supports German and English, backend permission checks, customer creation history, and a browser test against the real application. Persist named permissions and role assignments without building their administration screens.

| Boundary | Scope |
| --- | --- |
| Included | Git workflow, initial design decisions, integrated local setup, session authentication, customer creation, read-only detail/history, list, search, localization, and verification |
| Deferred | Customer editing, archiving, restoration, contact anonymization, account and role administration screens, and temporary-password onboarding |
| Later product work | Products, inventory, orders, invoices, exports, and public hosting |
| Dependencies not yet needed | Object storage and email infrastructure |
| Slice record | `docs/slices/customer-tracer.md`, created in CP-01 |
| Review record | `docs/reviews/customer-tracer.md`, created in CP-01 |

| Phase | Checkpoints | Review outcome |
| --- | --- | --- |
| 0. Git workflow | [CP-00](#cp-00) through [CP-00b](#cp-00b) | Agreed contribution and recovery process |
| 1. Initial decisions | [CP-01](#cp-01) through [CP-09](#cp-09) | Enough design evidence to start implementation |
| 2. Integrated setup | [CP-10](#cp-10) through [CP-18](#cp-18) | Frontend, backend, PostgreSQL, migrations, Compose, and initial CI work together |
| 3. Authentication | [CP-19](#cp-19) through [CP-27](#cp-27) | Real sessions and language selection |
| 4. Customer creation | [CP-28](#cp-28) through [CP-37](#cp-37) | Validated, authorized, audited creation |
| 5. Customer discovery | [CP-38](#cp-38) through [CP-41](#cp-41) | List, search, detail, and creation history |
| 6. Slice acceptance | [CP-42](#cp-42) through [CP-50](#cp-50) | Browser evidence, operational checks, documentation, and human acceptance |

### Phase 0. Establish the Git workflow

<a id="cp-00"></a>

#### CP-00. Review the project Git policy

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** Read design workbook sections 28 and 29.

**Study before starting.** Working trees, commits, branches, pull requests, and review ownership.

##### Work

- Review D-020 with the human and record the accepted workflow or changes.
- Confirm the solo-maintainer review arrangement, default branch, commit style, and who may push and merge.
- Keep repository protection as pending until verified.

##### Acceptance checks

- The branch-to-review-to-merge path is clear.
- No fictional independent reviewer or unverified repository setting appears in the record.

---

<a id="cp-00a"></a>

#### CP-00a. Add repository contribution conventions

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-00.

**Study before starting.** Diffs, ignored files, line endings, and pull request templates.

##### Work

- Add a short `CONTRIBUTING.md` linked to the policy and this register.
- Add a pull request template with checkpoint ID, behavior, verification, and review evidence.
- Set `.editorconfig`, `.gitattributes`, and focused ignore rules for the chosen repository conventions.

##### Acceptance checks

- Review the diff for accidental generated files or secrets.
- Check that the template distinguishes human acceptance from CI success.
- Do not require application checks before the application exists.

---

<a id="cp-00b"></a>

#### CP-00b. Verify remote review controls and repository recovery

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-00a and access to the project hosting settings.

**Study before starting.** Branch protection, required status checks, maintainer permissions, and Git backups.

##### Work

- Inspect the actual hosting capabilities and record them.
- Configure the agreed main-branch protections when authorized.
- Add CI requirements only when their workflows exist.
- Document a separate repository backup location and verify a disposable restore or clone from that backup.

##### Acceptance checks

- Record actual protection settings and backup recovery evidence.
- If remote access is unavailable, keep this checkpoint blocked and name the missing access.
- A local workflow may continue only for checkpoints whose dependencies the human explicitly updates; do not claim remote checks passed.

---

### Phase 1. Decide enough to start

<a id="cp-01"></a>

#### CP-01. Confirm and record the slice boundary

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-00 through CP-00b. Read PRD sections 6, 12, and 15.1 and this plan.

**Study before starting.** Vertical slices, acceptance examples, and the difference between checkpoint acceptance and slice completion.

##### Work

- Create `docs/slices/customer-tracer.md` and `docs/reviews/customer-tracer.md`.
- Record the finish line above, excluded work, and literal success and failure examples.
- Link them from the design workbook.
- Record the project owner and reviewer in its project record.

##### Acceptance checks

- The reviewer can identify exactly what the final demo will do.
- Use the workbook's separate gates for starting and completing the walking skeleton.

---

<a id="cp-02"></a>

#### CP-02. Name the first slice's business concepts

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-01.

**Study before starting.** Domain glossaries and the distinction between a customer, address, contact, user, role, and permission.

##### Work

- Create a small German and English glossary for this slice.
- Agree customer-facing terms and code names.
- Record who can review the German wording and any review still pending.

##### Acceptance checks

- Every term used in the slice examples has one clear definition.
- Do not write the glossary for orders or billing yet.

---

<a id="cp-03"></a>

#### CP-03. Draw the system boundary and set first-slice quality examples

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-02.

**Study before starting.** System context diagrams and measurable quality scenarios.

##### Work

- Add an editable context diagram showing users and the application boundary.
- Write brief scenarios for permission denial, database unavailability, language switching, and a bounded customer list.
- Link the PRD's larger capacity targets as later measurement work.

##### Acceptance checks

- PostgreSQL is an internal dependency, not a business actor.
- The examples say what a user observes and do not claim unmeasured performance.

---

<a id="cp-04"></a>

#### CP-04. Choose a provisional frontend approach

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-03; design workbook sections 3 and 4.

**Study before starting.** Client routing, form state, component libraries, and accessible form controls.

##### Work

- Compare the credible frontend options against this slice.
- Record the human's trial choice in D-001 and a minimal component approach in D-002.
- Name the tracer evidence needed to accept the trial.
- Defer a full visual system.

##### Acceptance checks

- The choice has a reason, costs, and a revisit condition.
- Do not claim the workbook's tracer evaluation has passed before the tracer exists.
- Verify compatibility and licenses against primary documentation when choosing actual packages.

---

<a id="cp-05"></a>

#### CP-05. Choose backend organization and persistence

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-03; design workbook sections 5, 6, and 8.

**Study before starting.** Feature packages, transaction boundaries, persistence mapping, and module dependencies.

##### Work

- Settle D-003, D-004, and D-005 for identity and customers.
- Record how customer creation will call audit recording without coupling unrelated internals.
- Create a short ADR and a small module sketch.

##### Acceptance checks

- The reviewer can locate a future customer HTTP handler, business operation, and database mapping.
- Do not generate empty modules for every future feature.

---

<a id="cp-06"></a>

#### CP-06. Choose runtime, migration, and local startup tools

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-04 and CP-05; design workbook section 20.

**Study before starting.** Build wrappers, dependency lockfiles, database migrations, and Compose networking.

##### Work

- Record compatible Java, Spring Boot, PostgreSQL, frontend runtime, build tool, and migration tool versions.
- Set initial D-006 database naming rules before the first migration.
- Decide D-017, host ports, environment variable names, and the development request route.
- Draw the container diagram.
- Keep dependency startup and full-stack startup explicit.

##### Acceptance checks

- Versions have current primary-source compatibility evidence.
- The diagram explains how the browser reaches the backend and how the backend reaches PostgreSQL.
- No secrets enter version control.

---

<a id="cp-07"></a>

#### CP-07. Decide test tools and incremental CI gates

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-06; design workbook sections 18 and 21.

**Study before starting.** Domain, HTTP integration, component, and browser test boundaries.

##### Work

- Record D-015 and D-018.
- Choose test tools, local command names, and the security finding policy.
- Map each required PRD CI check to the checkpoint that will introduce it.
- Define browser support and test-data isolation.

##### Acceptance checks

- Real PostgreSQL integration tests are required.
- Empty suites do not masquerade as passing coverage.
- Note which gates need application code before they can run.

---

<a id="cp-08"></a>

#### CP-08. Decide first-slice permissions and session behavior

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-05 through CP-07; PRD AUTH and RBAC requirements.

**Study before starting.** Server-side sessions, password hashing, CSRF, session fixation, and authorization.

##### Work

- Record D-010 and a permission matrix for customer create, list, search, and detail.
- Choose local persona assignments, session timeout, cookie flags per environment, login throttling and lockout rules, and logout behavior.
- Define authentication endpoint paths, generic login errors, and the initial error contract before implementing login.
- Temporary-password onboarding remains later account-administration work.

##### Acceptance checks

- Specify a seeded user without customer access for the denial tests added in CP-20.
- Hosted cookie requirements and local development behavior are distinct and deliberate.
- No browser-stored authentication tokens are introduced.

---

<a id="cp-09"></a>

#### CP-09. Trace sensitive data and document initial threats

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-08.

**Study before starting.** Trust boundaries, direct-object access, log redaction, and personal-data minimization.

##### Work

- Draw focused login and customer-contact data flows.
- Start the threat model and risk register with mitigations and planned verification.
- Define safe fields for audit records and technical logs and a provisional local test-data retention rule.

##### Acceptance checks

- Passwords, session identifiers, and full customer payloads cannot be copied into logs or review evidence.
- Every material first-slice threat has an owner and a check.

---

### Phase 2. Connect the application components

<a id="cp-10"></a>

#### CP-10. Create the smallest backend that builds and starts

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-06 through CP-09 accepted.

**Study before starting.** Build lifecycle, Spring configuration, and environment profiles.

##### Work

- Generate the backend with the selected wrapper and only current dependencies.
- Add ignored local configuration and a safe environment example.
- Document build and startup commands.

##### Acceptance checks

- The wrapper builds the application and it starts.
- Review dependency purpose and configuration loading.
- No customer endpoints yet.

---

<a id="cp-11"></a>

#### CP-11. Start PostgreSQL with Compose

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-10.

**Study before starting.** Container volumes, health checks, and host versus container connection addresses.

##### Work

- Add the PostgreSQL service, a persistent development volume, health check, and local credential configuration.
- Document startup and shutdown without deleting data.

##### Acceptance checks

- Compose configuration validates, PostgreSQL becomes healthy, and data survives a restart.
- Bind local database access deliberately; do not expose it unnecessarily.

---

<a id="cp-12"></a>

#### CP-12. Connect migrations to the backend

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-11.

**Study before starting.** Migration history, checksums, and schema ownership.

##### Work

- Configure the selected migration runner against PostgreSQL.
- Add the smallest deliberate initial migration.
- Disable automatic ORM schema mutation if an ORM is used.

##### Acceptance checks

- Startup migrates a fresh disposable database; another startup does not reapply the migration.
- Document who runs migrations and how failures stop startup.

---

<a id="cp-13"></a>

#### CP-13. Prove database connectivity through an integration test

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-12.

**Study before starting.** Testcontainers lifecycle and application readiness.

##### Work

- Add a real PostgreSQL integration test for database-dependent readiness using the selected health mechanism.
- Run migrations in that environment.
- Implement the configuration needed to pass.

##### Acceptance checks

- Record a meaningful failure before the fix and a passing test afterward.
- The test cannot silently connect to the developer's database.
- Public health output contains no connection details.

---

<a id="cp-14"></a>

#### CP-14. Create the frontend and render one page

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-04, CP-06, and CP-13.

**Study before starting.** Frontend build scripts, routing, and component tests.

##### Work

- Initialize the selected frontend with a lockfile.
- Add one accessible application page and a behavior test that finds its heading.
- Configure formatting and type checking where applicable.

##### Acceptance checks

- The component test, type checks, and production build pass.
- The page has original, minimal styling and no fake business dashboard.

---

<a id="cp-15"></a>

#### CP-15. Make a browser request reach the real backend

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-14.

**Study before starting.** Same-origin requests, development proxies, and browser network errors.

##### Work

- Configure the agreed request route and add a minimal connection state to the development page.
- Test visible success and backend-unavailable behavior without disclosing internal errors.

##### Acceptance checks

- The browser reaches the running backend and displays an actionable failure when it is stopped.
- Review origin handling before authentication arrives.

---

<a id="cp-16"></a>

#### CP-16. Package the backend for full-stack Compose startup

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-15.

**Study before starting.** Multi-stage builds, container users, and runtime configuration.

##### Work

- Add the backend image and Compose service.
- Use the container database address and the established migration path.
- Keep build credentials and development secrets out of image layers.

##### Acceptance checks

- The containerized backend becomes ready against Compose PostgreSQL.
- Review image contents, runtime user, and startup failure behavior.

---

<a id="cp-17"></a>

#### CP-17. Package and route the frontend in Compose

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-16.

**Study before starting.** Static asset serving or the selected frontend runtime, reverse proxying, and readiness ordering.

##### Work

- Add the frontend runtime and browser-to-backend route for full-stack startup.
- Update the container diagram and README commands.

##### Acceptance checks

- One documented Compose command starts the integrated application.
- Refreshing the page works.
- Verify both the chosen development path and full-container path if the topology provides both.

---

<a id="cp-18"></a>

#### CP-18. Run the initial automated checks in CI

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-17.

**Study before starting.** GitHub Actions jobs, least-privilege tokens, and dependency caching.

##### Work

- Add CI for existing formatting, static checks, builds, integration tests, and migration validation.
- Use the same named commands as local development.

##### Acceptance checks

- Review a successful workflow run when repository access permits.
- Otherwise record the external CI run as pending and leave this checkpoint open.
- No required check is silently skipped.

---

### Phase 3. Sign in through the real application

<a id="cp-19"></a>

#### CP-19. Design identity persistence and create its migration

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-08, CP-09, and CP-18.

**Study before starting.** User-role relationships, unique constraints, and password hashes.

##### Work

- Extend the D-006 naming rules from CP-06 with identity IDs and constraint conventions.
- Add the identity ERD, data dictionary entries, and migration for users, roles, named permissions, and assignments.
- Add selected session storage tables if needed.

##### Acceptance checks

- Migration checks pass.
- Constraints prevent duplicate identities and duplicate assignments.
- The schema supports several roles per user without adding administration screens.

---

<a id="cp-20"></a>

#### CP-20. Seed local personas repeatably

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-19.

**Study before starting.** Development profiles, deterministic fixtures, and adaptive password hashing.

##### Work

- Add explicitly local seed users and the PRD's default roles.
- Include Sales and a persona denied customer access.
- Keep fixed test credentials confined to local/test configuration and document them.

##### Acceptance checks

- Repeating the seed creates no duplicates or unexpected credential resets.
- Passwords are hashed.
- Normal application startup does not automatically enable demo credentials.

---

<a id="cp-21"></a>

#### CP-21. Authenticate with a server-side session

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-20.

**Study before starting.** Security filters, session fixation protection, and credential errors.

##### Work

- Write HTTP tests for valid and invalid login, then implement login and a minimal current-user response.
- Use the approved session strategy and cookie settings.

##### Acceptance checks

- A valid login creates a usable session; invalid credentials return the agreed generic error.
- Verify session identifier rotation on authentication without logging the identifier.

---

<a id="cp-22"></a>

#### CP-22. Protect writes and end sessions correctly

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-21.

**Study before starting.** CSRF token exchange, logout invalidation, and inactivity expiry.

##### Work

- Add HTTP behavior tests and configuration for CSRF-protected state changes, logout, and session expiry.
- Use controllable time or bounded test configuration rather than long sleeps.

##### Acceptance checks

- Missing or invalid CSRF tokens fail, valid requests succeed, and expired or logged-out sessions cannot access protected behavior.
- Review cookie flags in the local configuration.

---

<a id="cp-23"></a>

#### CP-23. Enforce the agreed failed-login policy

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-22.

**Study before starting.** Account lockout, request throttling, and enumeration risks.

##### Work

- Add focused examples for failed attempts, the configured limit, and permitted recovery.
- Implement the policy from CP-08, including the agreed throttling boundary.

##### Acceptance checks

- The boundary cases pass without real-time waits.
- Responses do not reveal whether an account exists.
- Limits are configuration, not scattered constants.

---

<a id="cp-24"></a>

#### CP-24. Record authentication security events safely

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-23.

**Study before starting.** Security events versus business audit records and structured-log redaction.

##### Work

- Persist the login, failed-login, lockout, and logout events implemented so far.
- Add a minimal authorized application query for verification and future operational use.
- Use safe event fields from CP-09.

##### Acceptance checks

- Tests observe the events through that query boundary and prove unauthorized access is denied.
- Inspect sample records for credential or session leakage.

---

<a id="cp-25"></a>

#### CP-25. Build the sign-in form

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-24.

**Study before starting.** Accessible labels, safe form state, and CSRF-aware HTTP clients.

##### Work

- Use component tests to drive the email/password form, pending state, and generic failure message.
- Connect it to the real login endpoint and route successful login to the application page.

##### Acceptance checks

- Valid and invalid login work in the browser.
- Keyboard submission works.
- Password values do not enter persistent browser storage or error evidence.

---

<a id="cp-26"></a>

#### CP-26. Handle signed-out and expired-session navigation

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-25.

**Study before starting.** Route guards, authentication state, and the limits of frontend permission checks.

##### Work

- Add visible logout and session-expiry handling.
- Distinguish an unauthenticated response from a permission denial.
- Ensure safe navigation returns the user to sign-in when appropriate.

##### Acceptance checks

- Component tests and a short browser smoke test cover login, page refresh, and logout.
- Backend protection still works when frontend navigation is bypassed.

---

<a id="cp-27"></a>

#### CP-27. Select and persist the interface language

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-26; PRD I18N-01 through I18N-06.

**Study before starting.** Locale fallback, browser preferences, translation keys, and profile settings.

##### Work

- Decide the explicit English locale.
- Implement German fallback, supported browser-language detection, and a user language choice stored in the profile.
- Translate the existing sign-in and application controls.

##### Acceptance checks

- Tests cover German, English, an unsupported browser language, and a saved choice overriding browser preference after a new login.
- Review keyboard access to the selector and localized messages.

---

### Phase 4. Create one customer safely

<a id="cp-28"></a>

#### CP-28. Decide the customer creation contract

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-27; PRD customer requirements and design workbook section 7.

**Study before starting.** Request validation, DTOs, stable error codes, and API contracts.

##### Work

- Settle the first slice's fields, required versus optional values, address and contact cardinality, payment terms representation, preferred language, and restricted-note permissions.
- Decide API paths, error format, bounded pagination, sort allowlist, and OpenAPI ownership.
- Record literal valid and invalid examples.

##### Acceptance checks

- Every creation field has a rule and a reason.
- Address and VAT-ID checks validate shape only.
- Keep order-confirmation rules out of customer creation unless independently required here.

---

<a id="cp-29"></a>

#### CP-29. Model and migrate customer storage

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-28.

**Study before starting.** Foreign keys, embedded values, child records, and optimistic versions.

##### Work

- Extend the customer ERD and data dictionary.
- Add customer, address, and contact storage with required constraints, active/archive representation, and version.
- Store instants and business dates using the agreed distinct types.

##### Acceptance checks

- Fresh and previous-schema migration checks pass.
- Cardinalities match CP-28.
- Adding an archive field does not mean archive behavior is complete.

---

<a id="cp-30"></a>

#### CP-30. Allocate immutable customer numbers

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-29; design workbook section 13.

**Study before starting.** Database sequences or locked counters, concurrency, and rollback gaps.

##### Work

- Decide the customer-number portion of D-009.
- Test and implement the chosen allocation mechanism for numbers such as `KD-000001`.
- Keep order and invoice numbering decisions deferred.

##### Acceptance checks

- A focused PostgreSQL test proves concurrent allocation does not duplicate numbers.
- Record that gaps are allowed and numbers are never reused.
- Review the database constraint as well as the application behavior.

---

<a id="cp-31"></a>

#### CP-31. Create and retrieve a minimal valid customer through HTTP

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-30.

**Study before starting.** Application transactions, permission checks, and response mapping.

##### Work

- Drive creation and retrieval through HTTP integration tests using CP-28's smallest valid example.
- Require the appropriate permission on both operations.
- Return the assigned customer number through the public interface.

##### Acceptance checks

- An authenticated Sales user creates and retrieves the customer.
- Anonymous and unauthorized requests fail.
- Tests read the result through HTTP instead of querying tables as a side channel.

---

<a id="cp-32"></a>

#### CP-32. Validate customer identity and address fields

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-31.

**Study before starting.** Boundary validation, nested field paths, and error summaries.

##### Work

- Add one failing example at a time for company identity, VAT-ID shape, required fields, address shape, and configured length limits.
- Implement stable field error codes and paths.

##### Acceptance checks

- Invalid submissions create no customer.
- Tests include literal inputs at relevant boundaries.
- Messages do not suggest tax-authority verification.

---

<a id="cp-33"></a>

#### CP-33. Handle contacts and optional customer values

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-32.

**Study before starting.** Collection validation, normalization, and permission-dependent fields.

##### Work

- Complete the agreed contact and shipping-address collections, language, payment terms, and optional notes.
- Validate their rules and enforce restricted-note permissions on both writes and reads.

##### Acceptance checks

- Public-interface tests cover the agreed cardinalities, invalid child values, and restricted-field denial.
- No partial customer remains after a rejected request.

---

<a id="cp-34"></a>

#### CP-34. Audit customer creation in the same transaction

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-33.

**Study before starting.** Atomicity, append-only audit events, and safe change summaries.

##### Work

- Add the customer-created audit event and a minimal authorized customer-history query.
- Include actor, time, entity reference, action, and correlation ID.
- Introduce request correlation here so the audit event has a real request identifier.
- Test that customer creation and its audit event succeed or fail together.

##### Acceptance checks

- A created customer has one visible creation event.
- Failed creation has no successful event.
- The application offers no audit update/delete operation, and unauthorized history access fails.

---

<a id="cp-35"></a>

#### CP-35. Build the customer form's identity and address controls

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-34.

**Study before starting.** Form labels, field-error association, and reusable input components.

##### Work

- Build the first part of the form with component tests for company identity and the agreed address inputs.
- Use the settled component approach and translated labels.
- Keep submission disabled until the full required form exists.

##### Acceptance checks

- Tests use labels and visible errors rather than component internals.
- Tab order, required-field instructions, and German text fit the layout.

---

<a id="cp-36"></a>

#### CP-36. Complete the form's contacts and optional fields

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-35.

**Study before starting.** Accessible repeated field groups and permission-aware presentation.

##### Work

- Add the remaining fields from CP-28, including collection controls where required.
- Translate labels and validation messages.
- Hide unavailable actions while retaining backend checks.

##### Acceptance checks

- Component tests cover adding and removing allowed repeated values and preserving entered values after validation errors.
- English and German layouts remain usable.

---

<a id="cp-37"></a>

#### CP-37. Submit the customer form to the real API

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-36.

**Study before starting.** Submission lifecycle, error mapping, and uncertain network outcomes.

##### Work

- Connect creation to the backend.
- Show the assigned customer number on success.
- Map backend field errors into the form and summary, preserve safe inputs, and prevent accidental repeat clicks while a request is pending.
- Do not automatically retry a creation request after an ambiguous network failure.

##### Acceptance checks

- Successful creation, rejected input, denied permission, and connection failure produce understandable outcomes.
- Review focus movement to the error summary and success message announcement.

---

### Phase 5. Find the customer again

<a id="cp-38"></a>

#### CP-38. List customers through a bounded API

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-37.

**Study before starting.** Server-side pagination, stable ordering, and query authorization.

##### Work

- Test and implement customer listing with the agreed page strategy, a maximum page size, stable default ordering, and an allowlist for sorting.
- Return only fields needed by the list.

##### Acceptance checks

- Multiple pages have predictable results.
- Invalid bounds and sort fields receive defined errors.
- Unauthenticated and unauthorized access fail.

---

<a id="cp-39"></a>

#### CP-39. Search customers by number, company, and contact

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-38.

**Study before starting.** Parameterized queries, case handling, joins, and duplicate results.

##### Work

- Decide matching semantics and implement PostgreSQL-backed search for the customer fields named in SEARCH-01.
- Apply search before pagination.
- Add only indexes justified by the actual query.

##### Acceptance checks

- Tests cover matching and nonmatching results, literal special characters, and a customer with several matching contacts appearing once.
- Restricted data does not leak through search responses.

---

<a id="cp-40"></a>

#### CP-40. Display and search the real customer list

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-39.

**Study before starting.** Accessible tables, loading states, pagination controls, and stale request responses.

##### Work

- Build the list with number, company, and approved summary fields.
- Connect search and pagination.
- Add translated loading, empty, no-match, and failure states.
- Show the newly created customer through a refreshed real query.

##### Acceptance checks

- Component tests cover list states and search/page changes.
- A late response cannot replace newer search results.
- Keyboard users can operate every control.

---

<a id="cp-41"></a>

#### CP-41. Open the created customer's details and history

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-40.

**Study before starting.** Detail routing, missing-record handling, and authorized history views.

##### Work

- Link list rows to a minimal read-only customer detail view and creation history.
- Use the existing APIs.
- Render only authorized values with German and English labels.

##### Acceptance checks

- The reviewer can create, find, and inspect the same customer number.
- Missing records and denied access have safe messages.
- No edit or archive controls imply unfinished behavior exists.

---

### Phase 6. Prove and hand over the slice

<a id="cp-42"></a>

#### CP-42. Prove the customer tracer in a browser test

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-41.

**Study before starting.** Browser test isolation, accessible locators, and observable assertions.

##### Work

- Extend the login smoke test into login, customer creation, list/search discovery, and detail/history inspection through the visible application.
- Run the journey in German and English using isolated synthetic records.

##### Acceptance checks

- Tests pass against the real frontend, backend, and PostgreSQL.
- No mocked customer API, private-method assertion, or database lookup substitutes for user-visible proof.
- Failures retain useful traces without passwords.

---

<a id="cp-43"></a>

#### CP-43. Review accessibility and language behavior

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-42.

**Study before starting.** Keyboard navigation, focus visibility, automated accessibility limits, and locale formatting.

##### Work

- Add automated accessibility checks for sign-in, form errors, list, and detail.
- Perform a manual keyboard review and verify language persistence, error wording, and any displayed dates or numbers.
- Fix small findings here; split substantial fixes into new checkpoints.

##### Acceptance checks

- Record actual findings and retest results, including the agreed browser support checks.
- Do not claim WCAG certification from a scan.

---

<a id="cp-44"></a>

#### CP-44. Add safe request diagnostics for this workflow

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-43; design workbook section 19.

**Study before starting.** Correlation IDs, structured logs, health versus readiness, and basic metrics.

##### Work

- Record the first-slice D-016 choice.
- Verify the request correlation introduced in CP-34 across existing audit events, add sanitized unexpected-error responses, and expose the agreed health and basic request/database metrics to authorized operational access.

##### Acceptance checks

- A failed request can be traced without logging passwords, session IDs, or full contact values.
- Tests check safe error output.
- No monitoring dashboard is required for this slice.

---

<a id="cp-45"></a>

#### CP-45. Make API documentation match the implemented endpoints

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-44.

**Study before starting.** OpenAPI schemas, authentication documentation, and contract drift.

##### Work

- Complete the selected OpenAPI generation or validation path for login/session behavior, language preference, customer creation/read/list/search, and history access as applicable.
- Document field errors, permission failures, and pagination.

##### Acceptance checks

- A generated or validated contract matches the real responses.
- Examples contain only synthetic values.
- The drift check fails on a deliberate local mismatch and passes after restoring consistency.

---

<a id="cp-46"></a>

#### CP-46. Add remaining first-slice CI checks and security scans

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-45.

**Study before starting.** CI artifacts, vulnerability triage, and required checks.

##### Work

- Extend CP-18 with component tests, the critical browser suite, accessibility checks, OpenAPI validation, and dependency/container scans under the chosen policy.
- Reconcile every PRD CI category with the existing code and record any category that has no applicable code yet.

##### Acceptance checks

- CI runs the actual workflow and reports failures visibly.
- Security exceptions have reasons and expiry.
- A required test is not skipped to obtain a green status.

---

<a id="cp-47"></a>

#### CP-47. Add a safe local seed reset command

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-46.

**Study before starting.** Destructive command scope, seed versions, and reproducible environments.

##### Work

- Add a documented reset procedure restricted to the local demo database.
- Restore the versioned personas and clearly marked synthetic customers.
- Require explicit targeting so it cannot accidentally use a different configured database.

##### Acceptance checks

- Run reset against disposable local data, repeat it, and verify the same usable starting state through the application.
- No public hourly reset or visitor isolation is implemented here.

---

<a id="cp-48"></a>

#### CP-48. Verify startup from a clean checkout and write the demo guide

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-47.

**Study before starting.** Reproducibility and prerequisite documentation.

##### Work

- Test the documented startup with fresh disposable volumes and no untracked local configuration.
- Update README with prerequisites, commands, persona access, reset instructions, troubleshooting, and the exact customer tracer steps.

##### Acceptance checks

- A reviewer can follow the guide and complete the tracer.
- Record commands and results.
- Check that synthetic data says `Testdaten` and no wording implies company affiliation.

---

<a id="cp-49"></a>

#### CP-49. Reconcile the design records with the working slice

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-48.

**Study before starting.** Requirement traceability and evidence-based architecture decisions.

##### Work

- Map the implemented portions of AUTH, RBAC, I18N, CUST, AUD, SEARCH, and DEMO requirements to tests and manual evidence.
- Label partially fulfilled requirements explicitly.
- Update diagrams, the data dictionary, risks, and the design register.
- Accept or revise the frontend trial using the completed tracer evidence.

##### Acceptance checks

- Documentation describes the code that exists.
- No whole requirement is marked complete merely because this slice covers part of it.
- Record later work without expanding this checkpoint into its implementation.

---

<a id="cp-50"></a>

#### CP-50. Accept the first vertical slice

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** CP-00, CP-00a, CP-00b, and CP-01 through CP-49 accepted, including any checkpoints added to split oversized changes.

**Study before starting.** Review the slice's acceptance examples and unresolved risks.

##### Work

- The human runs the customer tracer, reviews CI and the accumulated review record, and checks the slice definition of done against PRD section 12.4.
- Confirm backend denial behavior, language switching, customer creation history, and fresh-start evidence.
- Record acceptance date and reviewer.

##### Acceptance checks

- All agreed first-slice behaviors have evidence and no blocking findings remain.
- If something fails, add a focused corrective checkpoint and keep this box open.
- Close milestone M-01 here.
- Add the next agreed checkpoint set to this register using the extension rules above.

---

## Reusable checkpoint template

Copy this into the relevant milestone. Replace every placeholder, use a stable anchor, and write specific acceptance checks. Keep the checkbox unchecked until human acceptance.

```markdown
<a id="cp-nn"></a>

#### CP-NN. One concrete outcome

- [ ] Human review accepted.

**Status:** Planned

**Evidence:** Not recorded.

**Prerequisites.** Accepted checkpoint IDs, decisions, and source requirements.

**Study before starting.** Topics needed to understand this change.

##### Work

- State the bounded change and the behavior it adds.
- State decisions that must be settled before implementation.

##### Acceptance checks

- Name an observable result and the command or inspection that proves it.
- Name the failure case or boundary that matters.

---
```

## Plan change log

| Date | Change | Reason |
| --- | --- | --- |
| 2026-09-05 | Created the first customer-tracer plan, CP-01 through CP-50 | Limit the first checkpoint set to one integrated vertical slice |
| 2026-09-05 | Converted the file into a project-wide register; added CP-00 through CP-00b and the Git lifecycle | Preserve checkpoint history and support future milestone sets with reviewable Git changes |
