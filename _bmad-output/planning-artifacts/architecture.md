---
stepsCompleted: [1, 2, 3, 4, 5, 6, 7, 8]
inputDocuments:
  - '_bmad-output/planning-artifacts/prd.md'
  - '_bmad-output/brainstorming/brainstorming-session-20260328-193241.md'
workflowType: 'architecture'
lastStep: 8
status: 'complete'
completedAt: '2026-03-28'
project_name: 'math-app'
initialized: '2026-03-28'
---
# Architecture Decision Document

_This document builds collaboratively through step-by-step discovery. Sections are appended as we work through each architectural decision together._

## Project Context Analysis

### Requirements Overview

**Functional Requirements:**

- 64 functional requirements are defined across capability areas: learning flow, problem generation, recovery support, progression, story world, AI content operations, AI support operations, data reliability, safety/privacy, and AI routing optimization.
- Architectural implication: modular boundaries are required for learning engine, content pipeline, progression engine, model routing, and policy guardrails.

**Non-Functional Requirements:**

- 20 non-functional requirements emphasize performance, reliability, online behavior, child safety/privacy, and AI routing quality.
- Architectural implication: policy-first and failure-safe design is required, including deterministic routing, moderation gates, and resilient persistence.

### Scale & Complexity

- Primary domain: desktop edtech app with AI-assisted generation and support.
- Complexity level: medium-to-high due to AI orchestration, safety controls, and progression integrity.
- Estimated architectural components: 8 to 12 core modules/services.

### Technical Constraints & Dependencies

- Windows-first desktop runtime and online-only learner sessions.
- Local Ollama qwen3 as default model dependency.
- Cloud model fallback dependency via ChatGPT/Grok.
- Mandatory moderation and curriculum-fit checks before learner delivery.
- Persistent local checkpoint state for progress continuity.

### Cross-Cutting Concerns Identified

- Child-safety policy enforcement for all generated outputs.
- Routing governance with confidence gates, retry logic, and budget controls.
- Observability and auditability for routing decisions.
- Progression consistency guarantees across retries and reconnects.
- UX continuity during connectivity or model transitions.

## Starter Template Evaluation

### Primary Technology Domain

Desktop app with React + TypeScript UI, SQLite persistence, and local-first LLM routing.

### Starter Options Considered

1. Tauri 2 + React + TypeScript (recommended)
2. Electron Forge + Vite + TypeScript

### Selected Starter: Tauri 2 + React + TypeScript

**Rationale for Selection:**

- Lightweight footprint while preserving required desktop capabilities.
- Strong security and permission model suitable for child-focused educational software.
- Clean support path for SQLite and local-first model routing.

**Initialization Command:**

```bash
npm create tauri-app@latest
```

**Architectural Decisions Provided by Starter:**

**Language & Runtime:** TypeScript frontend + Rust host runtime via Tauri.

**Styling Solution:** React-compatible baseline styling, expandable with design system decisions.

**Build Tooling:** Modern frontend bundling and Tauri packaging pipeline.

**Testing Foundation:** Frontend and integration testing can be added without conflicting starter assumptions.

**Code Organization:** Clear frontend and host-layer split (`src` + `src-tauri`).

**Development Experience:** Fast desktop dev loop with Tauri dev workflow.

## Core Architectural Decisions

### Decision Priority Analysis

**Critical Decisions (Block Implementation):**

- Desktop shell foundation: Tauri 2 + React + TypeScript
- Local-first AI routing: Ollama qwen3 default, cloud fallback only when needed
- SQLite persistence and startup migration strategy
- Frontend state model and app-core communication contracts

**Important Decisions (Shape Architecture):**

- Fallback provider strategy: OpenAI + Grok failover
- Online-only runtime behavior with reconnect-safe state handling
- Child-safety policy enforcement before learner-facing output

**Deferred Decisions (Post-MVP):**

- Multi-profile account architecture
- Auto-update pipeline
- Cross-platform packaging expansion beyond Windows-first MVP

### Data Architecture

- Database: SQLite as primary local persistence store.
- Migration strategy: Tauri SQL migrations run at app startup.
- Validation strategy: typed schema validation at app boundary before persistence.
- Checkpoint model: explicit save points after answer submit and progress update.

### Authentication & Security

- MVP auth model: no account login; single local learner profile.
- Data protection: local data minimization and safe storage controls.
- Content safety: mandatory moderation and age-appropriateness gates before delivery.
- Cloud boundary: fallback requests only through controlled routing policy with audit logs.

### API & Communication Patterns

- UI-core communication: Tauri `invoke` commands with typed request/response contracts.
- Error model: standardized error payloads for local and cloud paths.
- Routing flow: local attempt -> optional local retry -> cloud escalation on policy trigger.
- Provider strategy: OpenAI + Grok failover for cloud fallback.

### Frontend Architecture

- State management: Zustand for global/session state.
- Data flow: deterministic action-based updates for score/mastery consistency.
- Feature domains: learning flow, progression, AI routing status, and support diagnostics.
- UX continuity: explicit connectivity and model-route status messaging.

### Infrastructure & Deployment

- Runtime target: Windows-first desktop release.
- Packaging: Tauri app packaging pipeline from starter foundation.
- Online operation: cloud-dependent features require active connectivity; blocked actions clearly surfaced.
- Observability: routing and fallback event logs persisted for debugging and budget governance.

### Decision Impact Analysis

**Implementation Sequence:**

1. Initialize starter and persistence/migration foundations.
2. Implement typed command contracts between UI and app core.
3. Build local-first routing engine with retry/escalation policy.
4. Integrate cloud failover providers with budget controls.
5. Implement progression, safety gates, and observability hooks.

**Cross-Component Dependencies:**

- Routing policy depends on persistence (budget/checkpoints) and validation schemas.
- Progression consistency depends on state management and command error standards.
- Safety and moderation affects content generation, support responses, and learner feedback paths.

## Implementation Patterns & Consistency Rules

### Pattern Categories Defined

Critical conflict points were identified across naming, structure, response formats, state updates, and routing observability.

### Naming Patterns

**Database Naming Conventions:**

- SQLite tables and columns use snake_case.
- Foreign keys use `<entity>_id` format.

**API Naming Conventions (Tauri Commands):**

- Command names use `module.action` format.
- Example: `router.route_request`, `progress.save_checkpoint`.

**Code Naming Conventions:**

- TypeScript variables/functions use camelCase.
- React component symbols use PascalCase.
- Files use kebab-case.

### Structure Patterns

**Project Organization:**

- Domain-oriented organization: learning, progress, routing, support.
- One Zustand store slice per domain.
- Shared helpers in dedicated utilities modules.

**File Structure Patterns:**

- Component files: PascalCase component name in kebab-case file path.
- Service and utility files: kebab-case.
- Config and schema files colocated with owning domain module.

### Format Patterns

**API Response Formats:**

- Success: `{ ok: true, data: ... }`
- Error: `{ ok: false, error: { code, message, details? } }`

**Data Exchange Formats:**

- Date/time values use ISO-8601 UTC strings.
- JSON field naming in TypeScript surfaces uses camelCase.

### Communication Patterns

**Event and Routing Patterns:**

- Routing command outputs include deterministic route metadata.
- Standard routing log payload:
  `{ timestamp, request_type, route, confidence, fallback_reason, token_cost_estimate }`

**State Management Patterns:**

- Async state uses explicit status enum: `idle | loading | success | error`.
- State transitions are action-driven and deterministic.

### Process Patterns

**Error Handling Patterns:**

- User-facing errors are child-safe and non-technical.
- Internal logs include technical details and trace context.

**Loading State Patterns:**

- Every async operation must expose visible state transitions in UI.
- Retry and fallback transitions must be represented explicitly.

### Enforcement Guidelines

**All AI Agents MUST:**

- Follow the naming and response format conventions exactly.
- Use defined domain boundaries and state slice rules.
- Emit routing logs with required schema fields.

**Pattern Enforcement:**

- Code review and automated lint/check rules validate naming and response shapes.
- Contract tests validate command response envelope consistency.

### Pattern Examples

**Good Examples:**

- `router.route_request` returns `{ ok: true, data: { route: 'local' } }`
- `progress.save_checkpoint` writes `session_id`, `mastery_score`, `updated_at`

**Anti-Patterns:**

- Mixing snake_case and camelCase in the same TypeScript API surface.
- Returning raw errors without `{ ok: false, error: ... }` wrapper.
- Emitting routing events without fallback reason or confidence fields.

## Project Structure & Boundaries

### Complete Project Directory Structure

```text
math-app/
├── README.md
├── package.json
├── tsconfig.json
├── vite.config.ts
├── .env.example
├── .gitignore
├── src/
│   ├── app/
│   │   ├── app-shell.tsx
│   │   ├── routes.tsx
│   │   └── providers.tsx
│   ├── features/
│   │   ├── learning/
│   │   │   ├── components/
│   │   │   ├── store/
│   │   │   ├── services/
│   │   │   └── types.ts
│   │   ├── progress/
│   │   │   ├── components/
│   │   │   ├── store/
│   │   │   ├── services/
│   │   │   └── types.ts
│   │   ├── routing/
│   │   │   ├── components/
│   │   │   ├── store/
│   │   │   ├── services/
│   │   │   └── types.ts
│   │   └── support/
│   │       ├── components/
│   │       ├── store/
│   │       ├── services/
│   │       └── types.ts
│   ├── shared/
│   │   ├── components/
│   │   ├── schemas/
│   │   ├── utils/
│   │   ├── constants/
│   │   └── api-contracts/
│   ├── assets/
│   │   ├── images/
│   │   ├── sounds/
│   │   └── story-panels/
│   └── main.tsx
├── src-tauri/
│   ├── Cargo.toml
│   ├── tauri.conf.json
│   └── src/
│       ├── main.rs
│       ├── commands/
│       │   ├── learning.rs
│       │   ├── progress.rs
│       │   ├── router.rs
│       │   └── support.rs
│       ├── domain/
│       │   ├── learning/
│       │   ├── progress/
│       │   ├── routing/
│       │   └── support/
│       ├── db/
│       │   ├── migrations/
│       │   ├── repositories/
│       │   └── connection.rs
│       ├── ai/
│       │   ├── ollama_client.rs
│       │   ├── cloud_openai.rs
│       │   ├── cloud_grok.rs
│       │   ├── router_policy.rs
│       │   └── quality_gate.rs
│       ├── safety/
│       │   ├── moderation.rs
│       │   └── policy.rs
│       ├── logging/
│       │   ├── audit.rs
│       │   └── tracing.rs
│       └── errors/
│           └── app_error.rs
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
└── docs/
    └── architecture-notes.md
```

### Architectural Boundaries

**API Boundaries:**

- UI communicates with core through Tauri `invoke` contracts only.
- Commands are domain-scoped: learning, progress, router, support.

**Component Boundaries:**

- Feature folders own their local components, state slice, and services.
- Shared UI primitives and schema utilities are centralized under `src/shared`.

**Service Boundaries:**

- AI routing and moderation responsibilities are isolated in `src-tauri/ai` and `src-tauri/safety`.
- Persistence responsibilities are isolated in `src-tauri/db/repositories`.

**Data Boundaries:**

- SQLite is the source of truth for checkpoints and learner progression.
- External provider responses must pass policy and quality gates before persistence or display.

### Requirements to Structure Mapping

**Feature/FR Mapping:**

- Learning flow FRs map to `src/features/learning` and `src-tauri/commands/learning.rs`.
- Progression FRs map to `src/features/progress` and `src-tauri/commands/progress.rs`.
- AI routing/cost FRs (FR56-FR64) map to `src/features/routing`, `src-tauri/ai`, and `src-tauri/commands/router.rs`.
- Support FRs map to `src/features/support` and `src-tauri/commands/support.rs`.

**Cross-Cutting Concerns:**

- Safety/privacy controls map to `src-tauri/safety` and schema validation in `src/shared/schemas`.
- Observability and routing auditability map to `src-tauri/logging`.

### Integration Points

**Internal Communication:**

- Frontend stores call typed service wrappers that call Tauri commands.
- Commands dispatch domain services and repositories.

**External Integrations:**

- Local model integration: `ollama_client.rs`.
- Cloud fallback providers: `cloud_openai.rs` and `cloud_grok.rs`.

**Data Flow:**

- Request -> router policy -> local attempt -> optional retry -> cloud fallback -> moderation/quality -> persistence -> UI response.

### File Organization Patterns

**Configuration Files:** root-level runtime/build configuration plus `src-tauri/tauri.conf.json`.

**Source Organization:** domain-first under `src/features` and `src-tauri/domain`.

**Test Organization:** unit, integration, and e2e test tiers under `/tests`.

**Asset Organization:** static visual/audio/story assets in `src/assets`.

### Development Workflow Integration

**Development Server Structure:** frontend and Tauri host run in synchronized dev loop.

**Build Process Structure:** frontend bundle + Tauri packaging pipeline.

**Deployment Structure:** Windows-first artifact packaging, with extension path for macOS/Linux.

## Architecture Validation Results

### Coherence Validation ✅

**Decision Compatibility:**

- Tauri 2 + React + TypeScript + SQLite is a compatible stack for a lightweight desktop app with local persistence and typed UI-core contracts.
- Local-first model routing (Ollama qwen3) with cloud fallback (ChatGPT/Grok) is consistent with cost-control and reliability goals.
- No contradictory architecture decisions were identified.

**Pattern Consistency:**

- Naming, response envelope, state transition, and routing log patterns align with the selected stack.
- Error handling and moderation gates are consistent across local and cloud paths.

**Structure Alignment:**

- The project structure supports all major domains: learning, progress, routing, support, safety, AI providers, and persistence.
- Boundaries are clearly separated between UI (`src`) and host/core (`src-tauri`).

### Requirements Coverage Validation ✅

**Epic/Feature Coverage:**

- Story-based learning, problem generation, answer checking, scoring, progression, and rewards are covered by defined modules and commands.
- Token optimization requirements (local Ollama first, cloud fallback) are explicitly covered by routing architecture.

**Functional Requirements Coverage:**

- Functional requirement categories are mapped to learning, progress, routing, support, safety, database, and logging modules.
- Cross-cutting functional requirements are addressed through policy, moderation, and quality gates.

**Non-Functional Requirements Coverage:**

- Performance, reliability, online behavior, and safety/privacy are addressed with migrations, deterministic flows, retry/fallback logic, and policy checks.
- Observability is supported through routing and audit logging.

### Implementation Readiness Validation ✅

**Decision Completeness:**

- Critical architecture choices are documented and actionable.
- Clear command and data contract patterns are defined.

**Structure Completeness:**

- Directory structure and ownership boundaries are explicit and implementation-ready.
- Integration points are identified from UI through commands, domain services, data, and providers.

**Pattern Completeness:**

- Conventions for naming, payloads, errors, async states, and logs are sufficient to prevent implementation drift.

### Gap Analysis Results

**Critical Gaps:**

- None identified.

**Important Gaps:**

- Add explicit model selection thresholds (confidence, latency, token limits) as config constants during implementation.
- Add a short regeneration policy for "new story/problem every time" behavior to enforce age-appropriate variation.

**Nice-to-Have Gaps:**

- Add an ADR index for future architecture changes.
- Add a mastery-tuning rubric for progression balancing.

### Validation Issues Addressed

- Confirmed local-first routing remains default and cloud is fallback-only under policy triggers.
- Confirmed Windows-first and online-only constraints are applied consistently throughout the architecture.

### Architecture Completeness Checklist

**✅ Requirements Analysis**

- [x] Project context thoroughly analyzed
- [x] Scale and complexity assessed
- [x] Technical constraints identified
- [x] Cross-cutting concerns mapped

**✅ Architectural Decisions**

- [x] Critical decisions documented with versions
- [x] Technology stack fully specified
- [x] Integration patterns defined
- [x] Performance considerations addressed

**✅ Implementation Patterns**

- [x] Naming conventions established
- [x] Structure patterns defined
- [x] Communication patterns specified
- [x] Process patterns documented

**✅ Project Structure**

- [x] Complete directory structure defined
- [x] Component boundaries established
- [x] Integration points mapped
- [x] Requirements to structure mapping complete

### Architecture Readiness Assessment

**Overall Status:** READY FOR IMPLEMENTATION

**Confidence Level:** High based on validation results

**Key Strengths:**

- Clear modular boundaries and consistent command contracts
- Local-first routing strategy aligned to token-cost goals
- Safety and observability treated as first-class architecture concerns

**Areas for Future Enhancement:**

- Fine-tune adaptive routing thresholds using usage telemetry
- Expand offline-capable behavior in a future release if product direction changes

### Implementation Handoff

**AI Agent Guidelines:**

- Follow all architecture decisions exactly as documented
- Apply implementation patterns consistently across all components
- Respect project structure boundaries and ownership
- Use this document as the single source of technical truth

**First Implementation Priority:**

- Initialize the Tauri starter and SQLite migrations
