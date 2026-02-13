# Master Plan: HouseCall — Appropriate Followup as a Service

> This document is the implementation plan for the HouseCall service.
> It is being built iteratively. See git history for research notes and decision rationale.

---

## 1. Domain Analysis

### 1.1 Core Abstraction

Every use case follows the same lifecycle:

```
Intake → Triage → Active Monitoring → Resolution
```

The fundamental unit is a **Case**. A Case represents a situation that needs monitoring and followup over time. Every use case — from a child's medical concern to a data center installation — is a Case with domain-specific context.

### 1.2 Core Domain Entities

| Entity | Description |
|---|---|
| **Case** | A situation being monitored. Has a lifecycle (open → active → resolved → closed). Contains all context, history, and relationships. |
| **Party** | A person or organization involved in a case. Has a role (initiator, subject, coordinator, third-party). Has contact preferences and information boundaries. |
| **Workstream** | A parallel track of work within a case. Cases can have multiple concurrent workstreams (e.g., "fix car" + "pick up kids"). |
| **FollowUp** | A scheduled future action — a call, a check-in, a deadline. Has a trigger time and can be dynamically rescheduled. |
| **Escalation** | A state change in urgency. Has trigger criteria and a target (person or action). Can go up or down. |
| **Action** | Something the service does on behalf of a user — contacting a third party, scheduling an appointment, sending a notification. |
| **Artifact** | A generated output — a summary, a dashboard, a comparison matrix, a report. |
| **AuditEntry** | An immutable record of something that happened — a call, a decision, an action taken. |

### 1.3 Case Lifecycle

```
         ┌──────────┐
         │  INTAKE   │  Gather information, assess urgency
         └────┬──────┘
              │
         ┌────▼──────┐
         │  TRIAGE    │  Determine urgency, set initial plan
         └────┬──────┘
              │
         ┌────▼──────┐
         │  ACTIVE    │◄──── Most time spent here
         │            │  Monitor, follow up, escalate/de-escalate
         │            │  Manage workstreams, coordinate parties
         └────┬──────┘
              │
         ┌────▼──────┐
         │ RESOLVING  │  Confirm resolution, check for loose ends
         └────┬──────┘
              │
         ┌────▼──────┐
         │  CLOSED    │  Archive, final summary
         └──────────┘
```

### 1.4 Cross-Cutting Concerns Identified from Use Cases

| Concern | Use Cases Where It Appears |
|---|---|
| Time-critical parallel workstreams | UC2, UC4, UC5, UC9, UC10 |
| Emotional/anxiety awareness | UC1, UC3, UC7, UC11, UC12 |
| Third-party coordination (calling businesses, services) | UC2, UC4, UC5, UC6, UC8, UC9, UC10 |
| Dynamic frequency adjustment of followups | UC1, UC3, UC7 |
| Information boundaries between parties | UC2, UC3, UC6, UC8, UC11 |
| Financial/insurance coordination | UC4, UC10, UC12 |
| Domain-specific knowledge (medical, travel, construction) | UC1, UC5, UC6, UC7, UC12 |
| Autonomy respect (presenting options, not deciding) | UC3, UC7, UC11, UC12 |
| Pattern detection over time (recurring falls, budget trends) | UC3, UC6, UC7 |
| Handoff to professionals | UC1, UC7, UC11 |

---

## 2. Technology Stack

> **Design Principle:** Choose boring technology. Maximize ecosystem maturity, testing tool support, and hiring pool. Avoid novelty where reliability is needed.

### 2.1 Language & Runtime

**TypeScript on Node.js**

Rationale:
- Strong static typing catches a class of bugs at compile time
- Enormous ecosystem of testing, linting, and CI tools
- async/await is a natural fit for a service that manages many concurrent cases with scheduled callbacks
- Shared language for API, business logic, and any web dashboard
- Large talent pool for future contributors

### 2.2 Framework

**Fastify** for the HTTP/API layer

Rationale:
- Schema-based request/response validation (integrates with JSON Schema / TypeBox)
- Plugin architecture for clean separation of concerns
- Excellent TypeScript support
- High performance where it matters (status dashboards, webhook receivers)
- Simpler and more testable than Express

### 2.3 Database

**PostgreSQL** with **Prisma** ORM

Rationale:
- PostgreSQL is the most reliable open-source relational database
- Cases are inherently relational: parties, workstreams, followups, actions all reference cases
- JSONB columns for domain-specific context that varies per use case
- Prisma provides type-safe database access that integrates with TypeScript
- Excellent migration tooling for schema evolution
- PostgreSQL's `pg_cron` or application-level scheduling for followup triggers

### 2.4 Scheduling / Job Queue

**BullMQ** (Redis-backed)

Rationale:
- FollowUps are essentially delayed jobs with dynamic rescheduling
- BullMQ supports delayed jobs, repeatable jobs, and job prioritization
- Redis provides the performance needed for high-frequency scheduling operations
- Mature, well-tested library with good TypeScript support
- Supports job events for audit logging

### 2.5 Voice / Telephony

**Placeholder — see Open Questions**

The use cases describe phone calls as the primary interaction channel. The telephony integration is a major architectural decision that needs further discussion.

### 2.6 Testing Stack

| Layer | Tool | Purpose |
|---|---|---|
| Unit Tests | **Vitest** | Fast, TypeScript-native, compatible with Jest API |
| Integration Tests | **Vitest** + **Testcontainers** | Spin up real PostgreSQL and Redis in containers |
| Property Tests | **fast-check** | Generate random inputs to find edge cases in business logic |
| E2E / API Tests | **Vitest** + **Supertest** (or Fastify's `inject`) | Full HTTP request/response testing |
| Contract Tests | **Pact** | Verify API contracts between services |
| Mutation Tests | **Stryker** | Verify test suite quality by mutating source code |
| Load Tests | **k6** | Verify performance under load |

### 2.7 Static Analysis & Code Quality

| Tool | Purpose |
|---|---|
| **TypeScript** (strict mode) | Type checking — the first line of defense |
| **ESLint** (with typescript-eslint) | Linting and code style |
| **Prettier** | Consistent formatting |
| **Biome** | Fast linting and formatting (complementary/alternative) |
| **knip** | Dead code / unused export detection |
| **madge** | Circular dependency detection |
| **publint** | Package publishing correctness (if publishing packages) |
| **Dependency Cruiser** | Architectural boundary enforcement |
| **npm audit / Snyk** | Vulnerability scanning |
| **OSSF Scorecard** | Supply chain security |

### 2.8 Infrastructure

| Concern | Tool |
|---|---|
| Containerization | **Docker** + **Docker Compose** (local dev) |
| CI/CD | **GitHub Actions** |
| Package Management | **pnpm** (strict, fast, disk-efficient) |
| Monorepo Management | **Turborepo** (if multi-package) |
| Schema Validation | **Zod** (runtime) + **TypeBox** (Fastify schemas) |

---

## 3. Project Structure

```
housecall/
├── .github/
│   └── workflows/
│       ├── pr-quick.yml          # Tier 1: Fast checks on every PR
│       ├── dev-merge.yml         # Tier 2: Full checks before merging to dev
│       ├── main-merge.yml        # Tier 3: Complete validation before main
│       └── nightly.yml           # Tier 4: Expensive checks (mutation, load)
├── src/
│   ├── domain/                   # Pure business logic — no I/O
│   │   ├── case/
│   │   │   ├── case.ts           # Case entity and value objects
│   │   │   ├── case.test.ts      # Unit tests
│   │   │   ├── case.property.test.ts  # Property-based tests
│   │   │   ├── lifecycle.ts      # State machine for case lifecycle
│   │   │   └── lifecycle.test.ts
│   │   ├── followup/
│   │   │   ├── followup.ts
│   │   │   ├── followup.test.ts
│   │   │   ├── scheduler.ts      # Followup scheduling logic
│   │   │   └── scheduler.test.ts
│   │   ├── escalation/
│   │   │   ├── escalation.ts
│   │   │   ├── escalation.test.ts
│   │   │   ├── rules.ts          # Escalation/de-escalation rules engine
│   │   │   └── rules.test.ts
│   │   ├── party/
│   │   │   ├── party.ts
│   │   │   └── party.test.ts
│   │   ├── workstream/
│   │   │   ├── workstream.ts
│   │   │   └── workstream.test.ts
│   │   ├── triage/
│   │   │   ├── triage.ts         # Urgency assessment logic
│   │   │   └── triage.test.ts
│   │   └── audit/
│   │       ├── audit.ts
│   │       └── audit.test.ts
│   ├── application/              # Use case orchestration — coordinates domain + infra
│   │   ├── intake/
│   │   │   ├── intake-service.ts
│   │   │   └── intake-service.test.ts
│   │   ├── monitoring/
│   │   │   ├── monitoring-service.ts
│   │   │   └── monitoring-service.test.ts
│   │   ├── coordination/
│   │   │   ├── coordination-service.ts
│   │   │   └── coordination-service.test.ts
│   │   └── resolution/
│   │       ├── resolution-service.ts
│   │       └── resolution-service.test.ts
│   ├── infrastructure/           # External concerns — database, queue, telephony
│   │   ├── database/
│   │   │   ├── prisma/
│   │   │   │   └── schema.prisma
│   │   │   ├── repositories/
│   │   │   │   ├── case-repository.ts
│   │   │   │   └── case-repository.integration.test.ts
│   │   │   └── migrations/
│   │   ├── queue/
│   │   │   ├── followup-queue.ts
│   │   │   └── followup-queue.integration.test.ts
│   │   ├── telephony/
│   │   │   ├── telephony-adapter.ts    # Adapter interface
│   │   │   ├── mock-telephony.ts       # For testing
│   │   │   └── telephony.integration.test.ts
│   │   └── notifications/
│   │       ├── notification-adapter.ts
│   │       └── notification.integration.test.ts
│   └── api/                      # HTTP layer — routes, controllers
│       ├── routes/
│       │   ├── case-routes.ts
│       │   ├── case-routes.test.ts
│       │   ├── webhook-routes.ts
│       │   └── webhook-routes.test.ts
│       ├── middleware/
│       └── server.ts
├── tests/
│   ├── e2e/                      # End-to-end scenario tests
│   │   ├── medical-concern.e2e.test.ts
│   │   ├── data-center.e2e.test.ts
│   │   ├── elderly-fall.e2e.test.ts
│   │   ├── water-leak.e2e.test.ts
│   │   ├── travel-disruption.e2e.test.ts
│   │   ├── renovation.e2e.test.ts
│   │   ├── medication-change.e2e.test.ts
│   │   ├── hiring-pipeline.e2e.test.ts
│   │   ├── event-crisis.e2e.test.ts
│   │   ├── vehicle-breakdown.e2e.test.ts
│   │   ├── academic-crisis.e2e.test.ts
│   │   └── pet-emergency.e2e.test.ts
│   ├── integration/
│   │   └── ...
│   ├── load/
│   │   └── scenarios.k6.js
│   └── fixtures/                 # Shared test data
│       └── ...
├── docker-compose.yml
├── tsconfig.json
├── vitest.config.ts
├── .eslintrc.cjs
├── .prettierrc
└── package.json
```

---

## 4. CI/CD Pipeline — Tiered Validation

### Tier 1: PR Quick Checks (every push to a PR branch)

**Goal:** Fast feedback. Must complete in under 3 minutes.

```yaml
# .github/workflows/pr-quick.yml
triggers: pull_request (to dev branch)
```

| Check | Tool | Time Target |
|---|---|---|
| TypeScript compilation | `tsc --noEmit` | < 30s |
| Linting | `eslint` | < 30s |
| Formatting | `prettier --check` | < 10s |
| Dead code detection | `knip` | < 15s |
| Circular dependency check | `madge --circular` | < 10s |
| Unit tests | `vitest run --project unit` | < 60s |
| Property tests | `vitest run --project property` | < 30s |
| Build check | `tsc --build` | < 30s |

### Tier 2: Dev Branch Merge Gate (PR to dev, after Tier 1 passes)

**Goal:** Comprehensive correctness. Must complete in under 10 minutes.

```yaml
# .github/workflows/dev-merge.yml
triggers: pull_request (to dev branch), requires Tier 1 pass
```

| Check | Tool | Time Target |
|---|---|---|
| Everything from Tier 1 | — | — |
| Integration tests | Vitest + Testcontainers | < 5 min |
| API / E2E tests | Vitest + Fastify inject | < 3 min |
| Dependency vulnerability scan | `npm audit` / Snyk | < 30s |
| Test coverage threshold | Vitest coverage (≥ 90% line, ≥ 85% branch) | < 30s |
| Schema validation | Prisma validate | < 10s |

### Tier 3: Main Branch Merge Gate (PR from dev to main)

**Goal:** Production readiness. Can take up to 30 minutes.

```yaml
# .github/workflows/main-merge.yml
triggers: pull_request (to main branch), requires Tier 2 pass
```

| Check | Tool | Time Target |
|---|---|---|
| Everything from Tier 2 | — | — |
| Mutation testing | Stryker | < 20 min |
| Contract tests | Pact | < 5 min |
| Architectural boundary enforcement | Dependency Cruiser | < 1 min |
| License compliance | license-checker | < 30s |
| Docker build verification | docker build | < 3 min |
| Database migration dry-run | Prisma migrate diff | < 1 min |

### Tier 4: Nightly / Scheduled (not blocking merges)

**Goal:** Deep validation and trend tracking.

```yaml
# .github/workflows/nightly.yml
triggers: schedule (daily at 2 AM UTC)
```

| Check | Tool |
|---|---|
| Load / performance tests | k6 |
| Full mutation testing (higher thresholds) | Stryker |
| OSSF Scorecard | scorecard-action |
| Dependency freshness report | npm outdated |
| Test flakiness detection | Re-run test suite 5x |
| Security audit (deep scan) | Snyk / Trivy |

---

## 5. TDD Approach

### 5.1 Development Workflow

Every feature follows this cycle:

```
1. Write a failing test (Red)
2. Write the minimum code to pass (Green)
3. Refactor while keeping tests green (Refactor)
4. Commit
```

The test comes first. Always.

### 5.2 Test Layering Strategy

```
                    ┌─────────────┐
                    │   E2E Tests  │  Few, slow, high confidence
                    │  (Scenarios) │  Test full use case flows
                    └──────┬──────┘
                           │
                    ┌──────▼──────┐
                    │ Integration  │  Moderate count, moderate speed
                    │   Tests      │  Test real DB, queue, adapters
                    └──────┬──────┘
                           │
                    ┌──────▼──────┐
                    │  Property    │  Moderate count, fast
                    │   Tests      │  Find edge cases in logic
                    └──────┬──────┘
                           │
                    ┌──────▼──────┐
                    │  Unit Tests  │  Many, very fast
                    │              │  Test domain logic in isolation
                    └─────────────┘
```

### 5.3 What Gets Tested Where

| Layer | What It Tests | Dependencies |
|---|---|---|
| **Unit** | Domain entities, value objects, state machines, rules engines, pure functions | None (no I/O, no DB, no network) |
| **Property** | Invariants of domain logic (e.g., "a case can never go from CLOSED to INTAKE") | None |
| **Integration** | Repositories, queues, adapters — real infrastructure in containers | Testcontainers (PostgreSQL, Redis) |
| **E2E / Scenario** | Full use case flows from API request to database state | Full application stack in containers |
| **Contract** | API shape between services | Pact broker |
| **Mutation** | Quality of the test suite itself | Stryker |
| **Load** | Performance under concurrent case load | k6 + staged environment |

---

## 6. Comprehensive Test List

This is the full list of tests that will be written to drive out the implementation. Tests are organized by domain area and level.

### 6.1 Case Lifecycle (Unit Tests)

These tests verify the state machine governing case progression.

| # | Test | Rationale |
|---|---|---|
| U-CL-001 | A new case starts in INTAKE state | Every case begins at intake |
| U-CL-002 | A case in INTAKE can transition to TRIAGE | Normal flow after information gathered |
| U-CL-003 | A case in TRIAGE can transition to ACTIVE | Normal flow after urgency assessed |
| U-CL-004 | A case in ACTIVE can transition to RESOLVING | When resolution conditions are met |
| U-CL-005 | A case in RESOLVING can transition to CLOSED | When user confirms resolution |
| U-CL-006 | A case in RESOLVING can transition back to ACTIVE | Resolution was premature, new issue arose |
| U-CL-007 | A case in CLOSED cannot transition to any other state | Closed is terminal |
| U-CL-008 | A case in INTAKE cannot skip to ACTIVE | Must go through triage |
| U-CL-009 | A case in INTAKE cannot skip to RESOLVING | Must go through triage and active |
| U-CL-010 | A case in TRIAGE cannot skip to CLOSED | Must go through active and resolving |
| U-CL-011 | Every state transition is recorded as an AuditEntry | Full history preserved |
| U-CL-012 | A case records the timestamp of each state transition | For timeline reconstruction |
| U-CL-013 | A case in ACTIVE can transition to TRIAGE (re-triage on scope change) | UC2 Variant C: scope change mid-recovery |
| U-CL-014 | A case stores the reason for each state transition | Audit trail |
| U-CL-015 | Multiple rapid state transitions are recorded in order | Consistency under rapid updates |

### 6.2 Case Lifecycle (Property Tests)

| # | Test | Rationale |
|---|---|---|
| P-CL-001 | No sequence of valid transitions can reach CLOSED without passing through ACTIVE | Business rule: you can't close what was never worked on |
| P-CL-002 | No sequence of valid transitions can reach CLOSED from INTAKE directly | Must go through full lifecycle |
| P-CL-003 | The number of audit entries is always ≥ the number of state transitions | Every transition is audited |
| P-CL-004 | State transition timestamps are always monotonically increasing | Time only moves forward |
| P-CL-005 | For any valid sequence of transitions, the case can always eventually reach CLOSED | No dead-end states (liveness) |

### 6.3 Intake & Triage (Unit Tests)

| # | Test | Rationale |
|---|---|---|
| U-IT-001 | Intake creates a new case with the provided initial context | Basic creation |
| U-IT-002 | Intake requires at minimum one party (the initiator) | Can't have a case with no one involved |
| U-IT-003 | Intake records all gathered information as structured data | Information is queryable |
| U-IT-004 | Triage assigns an urgency level (CRITICAL, HIGH, MEDIUM, LOW) | UC1: emergency vs. wait-until-morning |
| U-IT-005 | Triage CRITICAL urgency triggers immediate escalation action | UC1 Variant C, UC7 Variant D |
| U-IT-006 | Triage generates an initial followup schedule based on urgency | Higher urgency = more frequent followups |
| U-IT-007 | Triage CRITICAL generates followups within 1 hour | Emergency frequency |
| U-IT-008 | Triage LOW generates followups at 24-hour intervals initially | Low-urgency frequency |
| U-IT-009 | Triage produces a clear initial recommendation | UC1: "no emergency, comfort measures" |
| U-IT-010 | Intake with multiple concerns creates multiple workstreams | UC10: car + daycare |
| U-IT-011 | Intake identifies all parties mentioned and creates Party records | UC2: team lead, boss, crew |
| U-IT-012 | Triage detects time-critical constraints and flags them | UC10: daycare closes at 6 PM |
| U-IT-013 | Intake rejects creation of a case with no descriptive context | Guard against empty cases |
| U-IT-014 | Triage can be re-run if new information changes the assessment | UC1 Variant C: symptoms worsen |

### 6.4 FollowUp Scheduling (Unit Tests)

| # | Test | Rationale |
|---|---|---|
| U-FS-001 | A followup is created with a scheduled time | Basic creation |
| U-FS-002 | A followup can be rescheduled to a new time | Dynamic adjustment |
| U-FS-003 | A followup can be canceled | No longer needed |
| U-FS-004 | A completed followup records its outcome | Audit trail |
| U-FS-005 | Followup frequency decreases when trajectory is IMPROVING | UC1 Variant A, UC3, UC7 |
| U-FS-006 | Followup frequency increases when trajectory is WORSENING | UC1 Variant C, UC3 Variant B |
| U-FS-007 | Followup frequency stays constant when trajectory is STABLE | UC1 Variant B |
| U-FS-008 | A followup that is not completed by its deadline triggers an alert | Missed followups are a problem |
| U-FS-009 | Followups respect party time preferences (no 3 AM calls unless CRITICAL) | UC1 exception: 2 AM is OK because the party initiated then |
| U-FS-010 | A case's followup schedule can be viewed as a timeline | For dashboard display |
| U-FS-011 | Completing a followup can trigger creation of the next followup | Continuous monitoring chain |
| U-FS-012 | A followup can be associated with a specific workstream | UC10: car followup vs. daycare followup |
| U-FS-013 | The maximum followup frequency is bounded (no more than once per 15 minutes) | Prevent runaway scheduling |
| U-FS-014 | The minimum followup frequency for an active case is bounded (at least once per 7 days) | Prevent forgotten cases |
| U-FS-015 | Dynamic rescheduling preserves the history of previous schedule | Audit trail for why timing changed |

### 6.5 FollowUp Scheduling (Property Tests)

| # | Test | Rationale |
|---|---|---|
| P-FS-001 | For any series of trajectory updates, followup intervals are always within bounds [15min, 7days] | Invariant: bounded scheduling |
| P-FS-002 | Followup intervals decrease monotonically while trajectory is consistently WORSENING | Frequency increases predictably |
| P-FS-003 | Followup intervals increase monotonically while trajectory is consistently IMPROVING | Frequency decreases predictably |
| P-FS-004 | A canceled followup never triggers a subsequent followup | Clean cancellation |
| P-FS-005 | The total number of scheduled followups for a case is always finite | No infinite scheduling loops |

### 6.6 Escalation & De-escalation (Unit Tests)

| # | Test | Rationale |
|---|---|---|
| U-ES-001 | An escalation is triggered when defined criteria are met | Basic escalation |
| U-ES-002 | An escalation targets a specific party or action | UC1 Variant C: advise ER, notify pediatrician |
| U-ES-003 | An escalation changes the case urgency level | From MEDIUM to HIGH, for example |
| U-ES-004 | A de-escalation reduces the case urgency level | UC1 Variant A: symptoms resolved |
| U-ES-005 | An escalation is recorded as an AuditEntry | Audit trail |
| U-ES-006 | An escalation triggered by symptom worsening increases followup frequency | Linked to followup scheduling |
| U-ES-007 | A de-escalation triggered by improvement decreases followup frequency | Linked to followup scheduling |
| U-ES-008 | An escalation can create a new workstream | UC4 Variant D: mold discovered |
| U-ES-009 | An escalation can notify multiple parties simultaneously | UC10: notify driver + daycare + backup |
| U-ES-010 | Escalation rules can be domain-specific | Medical vs. construction vs. travel |
| U-ES-011 | An escalation that cannot reach its target party triggers a secondary escalation path | UC10 Variant B: no backup available |
| U-ES-012 | Multiple escalation criteria met simultaneously are all recorded | Complete audit |
| U-ES-013 | De-escalation cannot reduce urgency below LOW | Floor on urgency |
| U-ES-014 | Escalation above CRITICAL is not possible (CRITICAL is the ceiling) | Ceiling on urgency |
| U-ES-015 | Escalation criteria can include time-based triggers (e.g., no response in 48h) | UC6 Variant C: contractor goes silent |

### 6.7 Escalation (Property Tests)

| # | Test | Rationale |
|---|---|---|
| P-ES-001 | Urgency is always within the defined enum range [LOW, MEDIUM, HIGH, CRITICAL] | Invariant |
| P-ES-002 | For any sequence of escalation/de-escalation events, urgency never exceeds CRITICAL or drops below LOW | Bounded |
| P-ES-003 | Every escalation event has a corresponding audit entry | No silent escalations |

### 6.8 Party & Multi-Party Coordination (Unit Tests)

| # | Test | Rationale |
|---|---|---|
| U-PC-001 | A party can be added to a case with a specific role | Basic party management |
| U-PC-002 | A party has configurable information visibility | UC2: boss sees different info than crew |
| U-PC-003 | A notification respects the party's information boundary | UC6: contractor doesn't see homeowner's budget concerns |
| U-PC-004 | A party can have multiple contact methods | Phone, email, dashboard |
| U-PC-005 | A party's preferred contact method is used first | User preferences |
| U-PC-006 | A party can be marked as unreachable after failed contact attempts | UC6 Variant C: contractor goes silent |
| U-PC-007 | A party marked unreachable triggers an escalation | UC6 Variant C |
| U-PC-008 | Party contact attempts are logged as AuditEntries | Accountability |
| U-PC-009 | A case must always have at least one party | Invariant |
| U-PC-010 | A party can be the subject of a case without being the initiator | UC3: elderly parent is subject, adult child is initiator |
| U-PC-011 | Removing the last party from a case is rejected | Guard against orphaned cases |
| U-PC-012 | A party can opt out of further contact | UC3 Variant D: parent refuses help |
| U-PC-013 | When a party opts out, the initiator is notified | UC3 Variant D |
| U-PC-014 | Information summaries are tailored to the party's role | UC2: team gets priorities, boss gets progress |

### 6.9 Workstream Management (Unit Tests)

| # | Test | Rationale |
|---|---|---|
| U-WS-001 | A workstream can be created within a case | Basic creation |
| U-WS-002 | A case can have multiple concurrent workstreams | UC10: car + daycare |
| U-WS-003 | A workstream has its own lifecycle (active, blocked, completed) | Independent tracking |
| U-WS-004 | Completing all workstreams can trigger case resolution check | When all tracks are done, maybe the case is done |
| U-WS-005 | A workstream can be blocked by an external dependency | UC4: waiting for plumber |
| U-WS-006 | A blocked workstream triggers proactive alternatives search | UC4 Variant B: no plumber for 4 hours |
| U-WS-007 | A workstream can have its own followup schedule | UC10: car repair followups vs. daycare followups |
| U-WS-008 | Workstreams can have dependencies on each other | UC5: can't adjust hotel until flight is rebooked |
| U-WS-009 | A workstream can be created mid-case (not just at intake) | UC4 Variant D: mold found during repair |
| U-WS-010 | Workstream status changes are recorded as AuditEntries | Audit trail |

### 6.10 Artifact Generation (Unit Tests)

| # | Test | Rationale |
|---|---|---|
| U-AG-001 | A case summary can be generated at any point in the case lifecycle | UC7: summary for doctor visit |
| U-AG-002 | A summary includes all relevant events in chronological order | Timeline view |
| U-AG-003 | A dashboard artifact can be created for ongoing status tracking | UC2: status dashboard |
| U-AG-004 | A comparison matrix can be generated from case data | UC8: candidate comparison |
| U-AG-005 | Artifacts are versioned — each generation creates a new version | History preservation |
| U-AG-006 | An artifact can be scoped to a specific audience/party | UC2: boss dashboard vs. team view |
| U-AG-007 | A summary respects information boundaries | Doesn't leak restricted info |
| U-AG-008 | A post-resolution final summary is generated on case close | UC9: thank-you summary |

### 6.11 Audit Trail (Unit Tests)

| # | Test | Rationale |
|---|---|---|
| U-AT-001 | Every action taken by the service is recorded | Comprehensive audit |
| U-AT-002 | Audit entries are immutable once created | Tamper-proof history |
| U-AT-003 | Audit entries include a timestamp, actor, action type, and details | Structured logging |
| U-AT-004 | Audit entries can be queried by case, by time range, by actor | Reporting flexibility |
| U-AT-005 | The audit trail for a case survives case closure | Post-hoc review |
| U-AT-006 | Audit entries include the before/after state for state changes | Debugging and accountability |

### 6.12 Action Execution (Unit Tests)

| # | Test | Rationale |
|---|---|---|
| U-AE-001 | An action to contact a third party is created with target and purpose | UC4: contact plumber |
| U-AE-002 | An action can succeed or fail | Real-world outcomes |
| U-AE-003 | A failed action triggers retry or alternative action | UC10 Variant B: backup unavailable, try secondary |
| U-AE-004 | Action results are stored and associated with the case | Outcome tracking |
| U-AE-005 | An action can be conditional on case state | Only send notification if urgency is HIGH |
| U-AE-006 | Actions are recorded in the audit trail | Accountability |
| U-AE-007 | An action can spawn a new followup | After calling plumber, follow up to confirm arrival |
| U-AE-008 | Concurrent actions on different workstreams execute independently | UC10: car tow + daycare call in parallel |

### 6.13 Pattern Detection (Unit Tests)

| # | Test | Rationale |
|---|---|---|
| U-PD-001 | Detecting a recurring pattern generates a proactive recommendation | UC3 Variant C: recurring falls |
| U-PD-002 | Budget trend analysis detects overrun trajectory | UC6 Variant B |
| U-PD-003 | Symptom trajectory analysis detects worsening trend | UC1 Variant C, UC7 Variant B |
| U-PD-004 | Contact failure pattern detection (repeated unreachable) | UC6 Variant C |
| U-PD-005 | Pattern detection threshold is configurable per domain | Different sensitivity for medical vs. budget |

### 6.14 Resolution Detection (Unit Tests)

| # | Test | Rationale |
|---|---|---|
| U-RD-001 | Resolution is suggested when all workstreams are complete | Automatic detection |
| U-RD-002 | Resolution requires user confirmation | Service doesn't close unilaterally |
| U-RD-003 | Resolution checks for loose ends (open followups, pending actions) | UC8: notify declined candidates |
| U-RD-004 | A case with open followups cannot be resolved until they are addressed | No premature closure |
| U-RD-005 | Resolution generates a final summary artifact | UC2: archives dashboard |
| U-RD-006 | Resolution records the resolution reason | Why this case is done |
| U-RD-007 | A case that has been resolved can be referenced by future cases | Historical context |

### 6.15 Infrastructure — Repository (Integration Tests)

| # | Test | Rationale |
|---|---|---|
| I-DB-001 | A case can be persisted and retrieved | Basic CRUD |
| I-DB-002 | Case state transitions are persisted atomically | Consistency |
| I-DB-003 | A case with multiple parties is persisted with all relationships intact | Relational integrity |
| I-DB-004 | A case with multiple workstreams is persisted correctly | Relational integrity |
| I-DB-005 | Audit entries are persisted in order | Ordering guarantee |
| I-DB-006 | Querying cases by state returns correct results | Filtering |
| I-DB-007 | Querying cases by party returns correct results | Filtering |
| I-DB-008 | Querying cases by urgency returns correct results | Filtering |
| I-DB-009 | Concurrent updates to different cases do not interfere | Isolation |
| I-DB-010 | Concurrent updates to the same case are serialized correctly | Consistency under contention |
| I-DB-011 | Domain-specific context (JSONB) can be queried | PostgreSQL JSONB queries |
| I-DB-012 | Database migrations run forward cleanly on empty database | Fresh setup |
| I-DB-013 | Database migrations run forward cleanly on populated database | Upgrade path |

### 6.16 Infrastructure — Job Queue (Integration Tests)

| # | Test | Rationale |
|---|---|---|
| I-JQ-001 | A followup job is enqueued with correct delay | Basic scheduling |
| I-JQ-002 | A followup job executes at approximately the correct time | Timing accuracy |
| I-JQ-003 | A canceled followup job does not execute | Clean cancellation |
| I-JQ-004 | A rescheduled followup job executes at the new time, not the old | Schedule update |
| I-JQ-005 | A failed followup job is retried with backoff | Resilience |
| I-JQ-006 | Job execution is idempotent (re-processing a followup does not duplicate actions) | At-least-once delivery safety |
| I-JQ-007 | The queue survives Redis restart (persistence) | Durability |
| I-JQ-008 | Multiple workers can process different case followups concurrently | Scalability |
| I-JQ-009 | A followup job for a CLOSED case is discarded | Stale job cleanup |

### 6.17 Infrastructure — Telephony Adapter (Integration Tests)

| # | Test | Rationale |
|---|---|---|
| I-TE-001 | An outbound call can be initiated | Basic telephony |
| I-TE-002 | Call status updates (ringing, answered, completed, failed) are captured | State tracking |
| I-TE-003 | A failed call attempt is logged and triggers retry or alternative | Resilience |
| I-TE-004 | Call recordings/transcripts are associated with the correct case | Data linkage |
| I-TE-005 | Inbound calls are routed to the correct case handler | UC1 Variant C: parent calls back |
| I-TE-006 | Telephony adapter failures are isolated from core business logic | Adapter pattern |

### 6.18 API / E2E Scenario Tests

Each use case and its variants are tested as end-to-end scenarios. These tests simulate the full timeline described in the use cases.

#### UC1: Child's Nighttime Medical Concern

| # | Test | Scenario |
|---|---|---|
| E2E-UC1-000 | Happy path: intake → triage → followup → resolution | Full lifecycle |
| E2E-UC1-A | Variant A: symptoms resolve by morning, case closes | Quick resolution |
| E2E-UC1-B | Variant B: symptoms persist, doctor visit, 48h followup, close | Extended monitoring |
| E2E-UC1-C | Variant C: symptoms worsen overnight, ER escalation, post-ER followup | Emergency escalation |
| E2E-UC1-D | Variant D: anxious parent, telehealth handoff, resume standard followup | Professional handoff |

#### UC2: Data Center Installation Behind Schedule

| # | Test | Scenario |
|---|---|---|
| E2E-UC2-000 | Happy path: setup coordination, 2-hour status cycles, completion | Full lifecycle |
| E2E-UC2-A | Variant A: smooth recovery in 48h | Straightforward completion |
| E2E-UC2-B | Variant B: key person unavailable, shift rearrangement | Staffing disruption |
| E2E-UC2-C | Variant C: scope change mid-recovery, re-triage | Scope management |
| E2E-UC2-D | Variant D: escalating frustration, communication adjustment | Social/emotional awareness |

#### UC3: Elderly Parent Fall at Home

| # | Test | Scenario |
|---|---|---|
| E2E-UC3-000 | Happy path: assessment, monitoring, recovery | Full lifecycle |
| E2E-UC3-A | Variant A: parent minimizes symptoms, inconsistency detection, escalation | Detecting minimization |
| E2E-UC3-B | Variant B: delayed serious symptoms, medical transport, followthrough | Delayed escalation |
| E2E-UC3-C | Variant C: recurring falls, pattern detection, safety recommendations | Pattern recognition |
| E2E-UC3-D | Variant D: parent refuses help, autonomy respect, initiator notification | Consent handling |

#### UC4: Home Water Leak Emergency

| # | Test | Scenario |
|---|---|---|
| E2E-UC4-000 | Happy path: assess, find plumber, repair, close | Full lifecycle |
| E2E-UC4-A | Variant A: quick fix, next-day verification, close | Simple resolution |
| E2E-UC4-B | Variant B: serious burst, no plumber for hours, insurance claim, multi-day | Complex escalation |
| E2E-UC4-C | Variant C: renter situation, landlord coordination, documentation | Third-party involvement |
| E2E-UC4-D | Variant D: mold discovered, new workstream, expanded scope | Mid-case scope expansion |

#### UC5: International Travel Disruption

| # | Test | Scenario |
|---|---|---|
| E2E-UC5-000 | Happy path: rebooking, hotel, notification, arrival | Full lifecycle |
| E2E-UC5-A | Variant A: smooth rebooking within 2 hours | Quick resolution |
| E2E-UC5-B | Variant B: multi-day delay, hotel, schedule rearrangement | Extended disruption |
| E2E-UC5-C | Variant C: lost luggage compounds problem, parallel workstream | Multiple issues |
| E2E-UC5-D | Variant D: medical issue during layover, fitness-to-fly assessment | Emergency within disruption |

#### UC6: Contractor Home Renovation Oversight

| # | Test | Scenario |
|---|---|---|
| E2E-UC6-000 | Happy path: daily updates, weekly summaries, completion | Full lifecycle |
| E2E-UC6-A | Variant A: on time and on budget | Smooth project |
| E2E-UC6-B | Variant B: budget overrun detected early, course correction | Financial pattern detection |
| E2E-UC6-C | Variant C: contractor goes silent for 48h, escalation | Unreachable party |
| E2E-UC6-D | Variant D: quality concern reported, documentation, resolution tracking | User-reported issue |

#### UC7: Chronic Condition Medication Change

| # | Test | Scenario |
|---|---|---|
| E2E-UC7-000 | Happy path: daily → less frequent monitoring, doctor summary | Full lifecycle |
| E2E-UC7-A | Variant A: medication works, positive summary, close | Straightforward |
| E2E-UC7-B | Variant B: side effects emerge, urgent appointment, dose change, reset | Adverse reaction |
| E2E-UC7-C | Variant C: patient forgets medication, adherence support | Compliance issue |
| E2E-UC7-D | Variant D: emergency symptom (chest pain), immediate 911 advice | Life-threatening escalation |

#### UC8: Job Candidate Pipeline Management

| # | Test | Scenario |
|---|---|---|
| E2E-UC8-000 | Happy path: applications → interviews → offer → acceptance | Full lifecycle |
| E2E-UC8-A | Variant A: clear top candidate, accelerated process | Fast-track |
| E2E-UC8-B | Variant B: top candidate drops out, pipeline adjustment | Loss recovery |
| E2E-UC8-C | Variant C: owner can't decide between two, structured comparison | Decision support |
| E2E-UC8-D | Variant D: candidate has concerns, address and follow up | Objection handling |

#### UC9: Community Event Crisis Response

| # | Test | Scenario |
|---|---|---|
| E2E-UC9-000 | Happy path: weather watch, decision, execute plan, post-event | Full lifecycle |
| E2E-UC9-A | Variant A: weather clears, proceed as planned | Stand-down |
| E2E-UC9-B | Variant B: backup venue falls through, alternative search | Cascading failure |
| E2E-UC9-C | Variant C: event postponed, full rescheduling cycle | Postponement |
| E2E-UC9-D | Variant D: partial execution, split indoor/outdoor | Hybrid response |

#### UC10: Vehicle Breakdown and Repair Coordination

| # | Test | Scenario |
|---|---|---|
| E2E-UC10-000 | Happy path: parallel workstreams (car + daycare), both resolved | Full lifecycle |
| E2E-UC10-A | Variant A: quick roadside fix, backup stood down | Simple resolution |
| E2E-UC10-B | Variant B: no backup for daycare, cascading alternatives | Exhausted options |
| E2E-UC10-C | Variant C: major repair, rental car, multi-day tracking | Extended repair |
| E2E-UC10-D | Variant D: caused by prior bad repair, insurance claim | Legal/insurance complexity |

#### UC11: Student Academic Crisis

| # | Test | Scenario |
|---|---|---|
| E2E-UC11-000 | Happy path: assessment, options presented, plan executed | Full lifecycle |
| E2E-UC11-A | Variant A: professor offers recovery path, follow through | Positive resolution |
| E2E-UC11-B | Variant B: withdrawal is best, financial/academic impact analysis | Difficult choice |
| E2E-UC11-C | Variant C: deeper mental health issues surface, counseling referral | Professional handoff |
| E2E-UC11-D | Variant D: financial aid at risk, full-GPA analysis, comprehensive plan | High-stakes analysis |

#### UC12: Pet Emergency After-Hours

| # | Test | Scenario |
|---|---|---|
| E2E-UC12-000 | Happy path: assessment, recommendation, monitoring, resolution | Full lifecycle |
| E2E-UC12-A | Variant A: low risk, overnight monitoring, morning vet confirm | Home monitoring |
| E2E-UC12-B | Variant B: emergency vet visit needed, locate clinic, follow up | Emergency action |
| E2E-UC12-C | Variant C: owner can't afford vet, financial options, informed decision | Financial constraint |
| E2E-UC12-D | Variant D: unknown substance, worst-case assessment | Uncertainty handling |

### 6.19 Cross-Cutting Scenario Tests

These test behaviors that span multiple use cases.

| # | Test | Scenario |
|---|---|---|
| E2E-CC-001 | Multiple concurrent cases for the same user are tracked independently | User has a renovation AND a medical concern |
| E2E-CC-002 | A case with 5+ parties manages all information boundaries correctly | Complex multi-party |
| E2E-CC-003 | A case running for 30+ days adjusts followup frequency appropriately over time | Long-duration case |
| E2E-CC-004 | A case that escalates to CRITICAL and then de-escalates to LOW follows correct frequency changes | Full escalation round-trip |
| E2E-CC-005 | A case with 3+ parallel workstreams resolves correctly when all complete | Complex parallelism |
| E2E-CC-006 | A mid-case scope expansion (new workstream) integrates with existing followup schedule | Dynamic scope |
| E2E-CC-007 | An action that fails and exhausts all retry/alternative options is surfaced to the user clearly | Failure transparency |
| E2E-CC-008 | The audit trail for a complex case (50+ entries) can be retrieved and is complete | Audit at scale |
| E2E-CC-009 | A party opting out of a case does not affect other parties in the same case | Partial opt-out |
| E2E-CC-010 | System handles 100+ active cases simultaneously without degradation | Load baseline |

---

## 7. Implementation Phases

### Phase 1: Foundation (Weeks 1-3)

**Goal:** Core domain model, basic lifecycle, and CI pipeline.

1. Project scaffolding (TypeScript, Vitest, ESLint, Prettier, pnpm)
2. CI Tier 1 (PR quick checks) — operational from day 1
3. Case entity and lifecycle state machine (with tests U-CL-*)
4. Party entity (with tests U-PC-001 through U-PC-005)
5. Audit trail (with tests U-AT-*)
6. FollowUp entity (with tests U-FS-001 through U-FS-004)
7. CI Tier 2 (dev merge gate) — operational by end of phase

### Phase 2: Core Logic (Weeks 4-6)

**Goal:** Triage, escalation, workstreams, and scheduling.

1. Intake and triage logic (with tests U-IT-*)
2. Escalation and de-escalation engine (with tests U-ES-*)
3. Workstream management (with tests U-WS-*)
4. Dynamic followup scheduling (with tests U-FS-005 through U-FS-015)
5. Property tests for all domain logic (P-CL-*, P-FS-*, P-ES-*)

### Phase 3: Infrastructure (Weeks 7-9)

**Goal:** Database, queue, adapters — real I/O.

1. PostgreSQL schema and Prisma setup
2. Repository implementations (with tests I-DB-*)
3. BullMQ job queue for followups (with tests I-JQ-*)
4. Telephony adapter interface + mock (with tests I-TE-*)
5. Notification adapter
6. Docker Compose for local development
7. CI Tier 3 (main merge gate) — operational by end of phase

### Phase 4: Application Services (Weeks 10-12)

**Goal:** Use case orchestration, API, end-to-end scenarios.

1. Intake service
2. Monitoring service
3. Coordination service
4. Resolution service
5. API routes
6. First E2E scenario tests (UC1, UC10 — chosen for breadth of patterns)

### Phase 5: Full Coverage (Weeks 13-16)

**Goal:** All use case E2E tests, pattern detection, artifact generation.

1. All remaining E2E scenario tests
2. Pattern detection logic (with tests U-PD-*)
3. Artifact generation (with tests U-AG-*)
4. Resolution detection (with tests U-RD-*)
5. Cross-cutting scenario tests (E2E-CC-*)
6. CI Tier 4 (nightly) — operational by end of phase

### Phase 6: Hardening (Weeks 17-18)

**Goal:** Mutation testing, load testing, security review.

1. Stryker mutation testing integration
2. k6 load test scenarios
3. Security audit (OWASP top 10, dependency vulnerabilities)
4. Documentation
5. Production deployment readiness

---

## 8. Test Count Summary

| Level | Count |
|---|---|
| Unit Tests | 96 |
| Property Tests | 15 |
| Integration Tests | 28 |
| E2E Scenario Tests | 62 |
| Cross-Cutting Tests | 10 |
| **Total** | **211** |

These are the *specification* tests — the tests we know we need before writing code. Additional tests will emerge during TDD as implementation reveals edge cases.

---

## 9. Open Questions

These must be resolved before or during implementation. Each is tagged with the phase it blocks.

| # | Question | Blocks | Impact |
|---|---|---|---|
| OQ-001 | **Telephony provider:** Which voice/telephony service will be used? (Twilio, Vonage, custom SIP, LLM-based voice agent?) The use cases describe natural phone conversations with intake interviews — this implies conversational AI, not just automated calls. | Phase 3 | Affects telephony adapter design, cost model, and the entire conversational intake engine |
| OQ-002 | **Conversational AI:** How are the intake interviews conducted? Is there an LLM powering the phone conversations? If so, which one, and how is it integrated? | Phase 2 | Core to the service — this is the primary user interface |
| OQ-003 | **Domain knowledge base:** The service needs domain-specific knowledge (medical symptoms, travel procedures, construction norms). Is this LLM-powered, rules-based, or a curated knowledge base? | Phase 2 | Affects triage logic, escalation rules, and recommendations |
| OQ-004 | **Multi-tenancy:** Is this a single-tenant service (one organization) or multi-tenant (many users, many organizations)? | Phase 1 | Affects database schema, authentication, and data isolation |
| OQ-005 | **Authentication & authorization:** How do users authenticate? How are party permissions managed? Is there an API key model, OAuth, or something else? | Phase 3 | Affects API design and security model |
| OQ-006 | **Third-party integrations:** The use cases mention contacting plumbers, airlines, insurance companies. Is this done via phone (through the telephony system), via APIs, or is it a manual action that the service tracks? | Phase 4 | Affects action execution architecture |
| OQ-007 | **Dashboard technology:** UC2 mentions a real-time status dashboard. Is this a web UI? What frontend framework? Or is it a third-party tool integration? | Phase 4 | Affects artifact generation and adds a frontend workstream |
| OQ-008 | **Data retention & privacy:** How long is case data retained? Are there HIPAA implications for medical use cases? GDPR for international users? | Phase 3 | Affects database design, audit trail, and data lifecycle |
| OQ-009 | **Deployment target:** Where will this run? AWS, GCP, Azure, self-hosted? Kubernetes, ECS, bare EC2? | Phase 6 | Affects CI/CD, Docker configuration, and infrastructure-as-code |
| OQ-010 | **Cost model:** Is this a SaaS with per-case pricing, a subscription, or an internal tool? | Phase 5 | Affects multi-tenancy, metering, and reporting |
| OQ-011 | **Offline/degraded mode:** What happens if Redis is down? If PostgreSQL is unavailable? What's the durability guarantee for in-flight followups? | Phase 3 | Affects queue design and failure handling |
| OQ-012 | **Rate of scale:** How many concurrent cases should the system support? 10? 1,000? 100,000? | Phase 3 | Affects database indexing, queue partitioning, and load test targets |
| OQ-013 | **Notification channels:** Beyond phone calls, which channels are supported? SMS? Email? Push notifications? Slack/Teams? | Phase 4 | Affects notification adapter design |
| OQ-014 | **Testing the conversational AI:** If the intake is LLM-powered, how do we test conversation quality? What is the acceptance criteria for a "good" intake interview? | Phase 2 | Affects the entire testing strategy for the intake engine |
| OQ-015 | **Handoff protocol:** When the service hands off to a professional (nurse line, counselor), what does that look like technically? A warm transfer? A summary sent to the professional? | Phase 4 | Affects third-party communication design |
