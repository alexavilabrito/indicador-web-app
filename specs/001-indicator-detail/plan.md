# Implementation Plan: Indicator Detail

**Branch**: `001-indicator-detail` | **Date**: 2026-09-07 | **Spec**: [spec.md](spec.md)

**Input**: Feature specification from `/specs/001-indicator-detail/spec.md`

**Note**: This template is filled in by the `$speckit-plan` command; its definition describes the execution workflow.

## Summary

Build the indicator detail feature so a user can open one supported Chilean economic indicator
from the dashboard and review its current value, source, effective date, recent chronological
series, accessible table, variation against the previous observation, and explicit loading/failure
states. The implementation will use a React/Vite frontend with Material UI, a NestJS BFF as the
only caller of `mindicador.cl`, an anti-corruption adapter that maps provider payloads into a
canonical model, Redis-backed freshness/cache handling, and deterministic tests for decimal
variation, sorting, invalid indicators, stale data, unavailable service, and accessibility.

## Technical Context

**Language/Version**: TypeScript in strict mode; React 19 frontend; NestJS BFF

**Primary Dependencies**: React 19, Vite, Material UI, NestJS, Redis, decimal arithmetic library,
ESLint, Prettier, SonarQube, Trivy, Docker, Jenkins

**Storage**: Redis for cached provider responses and stale-data fallback; PostgreSQL is not
required for this feature because no durable user or domain data is created

**Testing**: Vitest and React Testing Library for frontend behavior; Jest for BFF/domain logic;
Playwright for responsive end-to-end scenarios; accessibility checks in component and E2E flows

**Target Platform**: Responsive web application for mobile, tablet, and desktop browsers; BFF
service deployed as the controlled server-side integration boundary

**Project Type**: Web application with separate frontend and backend/BFF packages

**Performance Goals**: In p75 sessions, users see useful content or an explanatory state within
2.5 seconds after selecting an indicator; the detail route does not download unrelated historical
series or graph libraries

**Constraints**: Data provenance must be visible; all values must preserve unit/source/effective
date; calculations use decimal arithmetic; WCAG 2.2 AA; no graphs, advanced statistics, year
selection, conversion, comparison, authentication, alerts, favorites, or export in this feature

**Scale/Scope**: One detail experience for the 12 constitution-governed indicators, recent series
only, six user-visible states: loading, success, absence of series, invalid indicator, stale data,
and service unavailable

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

- **I. Data fidelity and transparency**: PASS. The plan preserves indicator code, name, value,
  unit, effective date, source, consultation/update timestamp, stale status, and referential
  warning in both model and UI contract.
- **II. API coverage**: PASS. This feature uses the recent-series-by-indicator capability for the
  12 supported indicators and validates indicator codes against the constitutional list.
- **III. Financial semantics**: PASS. Variation is computed with decimal arithmetic from valid
  observations only; no currency conversion or percentage-as-currency behavior is introduced.
- **IV. Conversion limits**: PASS. Conversion is explicitly out of scope for this feature.
- **V. Honest series analysis**: PASS. The feature provides exact recent values and a table, but
  excludes graphs and advanced statistics as required by the spec.
- **VI. Progressive, responsive, accessible UX**: PASS. The design requires mobile-first layout,
  keyboard-accessible table semantics, visible focus, non-color-only variation direction, and
  `es-CL` formatting.
- **VII. Resilience and controlled degradation**: PASS. The BFF encapsulates `mindicador.cl`,
  applies timeout/retry/cache policy, distinguishes stale cached data from fresh data, and avoids
  partial results presented as complete.
- **VIII. Security and privacy**: PASS. Indicator input uses an allowlist; no secrets or personal
  data are exposed; abuse and OWASP controls are part of quality gates.
- **IX. Performance budget**: PASS. The route is scoped to one indicator and excludes graph
  bundles and unrelated historical downloads.
- **X. Testable quality and stable contracts**: PASS. Contracts, canonical data model, unit,
  integration, E2E, accessibility, lint, type, build, and dependency/security checks are planned.
- **XI. Observability**: PASS. The BFF will categorize source health, latency, cache use, stale
  age, transformation failures, invalid indicators, and unavailable service.
- **XII. Simplicity and separation**: PASS. The design separates provider access, normalization,
  domain calculation, BFF contract, and presentation. No unnecessary persistence or extra product
  scope is added.

No constitutional violations require complexity justification.

## Project Structure

### Documentation (this feature)

```text
specs/001-indicator-detail/
├── plan.md
├── research.md
├── data-model.md
├── quickstart.md
├── contracts/
│   └── indicator-detail.openapi.yaml
└── tasks.md
```

### Source Code (repository root)

```text
backend/
├── src/
│   ├── indicators/
│   │   ├── adapters/
│   │   ├── domain/
│   │   ├── dto/
│   │   └── services/
│   └── shared/
└── tests/
    ├── contract/
    ├── integration/
    └── unit/

frontend/
├── src/
│   ├── components/
│   ├── pages/
│   ├── services/
│   └── test/
└── tests/
    ├── integration/
    └── unit/

tests/
└── e2e/
```

**Structure Decision**: Use a web application split into `frontend/`, `backend/`, and root
`tests/e2e/`. The backend owns external provider access and canonical transformation; the
frontend consumes only the BFF detail contract and renders the user-facing states.

## Complexity Tracking

No constitutional violations or exceptional complexity are introduced.
