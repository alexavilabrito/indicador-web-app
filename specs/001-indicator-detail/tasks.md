# Tasks: Indicator Detail

**Input**: Design documents from `/specs/001-indicator-detail/`

**Prerequisites**: [plan.md](plan.md), [spec.md](spec.md), [research.md](research.md), [data-model.md](data-model.md), [contracts/](contracts/), [quickstart.md](quickstart.md)

**Tests**: Included because the constitution and plan require contract, unit, integration, E2E, accessibility, lint, type, build, and security gates.

**Organization**: Tasks are grouped by user story to enable independent implementation and testing of each story.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: Can run in parallel (different files, no dependencies)
- **[Story]**: Which user story this task belongs to (US1, US2, US3, US4)
- Include exact file paths in descriptions

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Initialize the split frontend/backend project and shared quality tooling.

- [ ] T001 Create root workspace scripts and package metadata in `package.json`
- [ ] T002 Create backend NestJS package metadata and scripts in `backend/package.json`
- [ ] T003 Create frontend React/Vite package metadata and scripts in `frontend/package.json`
- [ ] T004 [P] Configure TypeScript strict mode for backend in `backend/tsconfig.json`
- [ ] T005 [P] Configure TypeScript strict mode for frontend in `frontend/tsconfig.json`
- [ ] T006 [P] Configure shared ESLint and Prettier rules in `eslint.config.js` and `.prettierrc.json`
- [ ] T007 [P] Configure Redis service for local validation in `docker-compose.yml`
- [ ] T008 [P] Configure CI quality gates in `Jenkinsfile`
- [ ] T009 [P] Document required runtime variables in `.env.example`

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Establish the canonical domain, BFF boundary, cache, routing, and frontend shell required by all stories.

**Critical**: No user story work can begin until this phase is complete.

- [ ] T010 Create backend indicator module skeleton in `backend/src/indicators/indicators.module.ts`
- [ ] T011 [P] Create backend application bootstrap and global validation setup in `backend/src/main.ts`
- [ ] T012 [P] Create backend configuration loader for provider/cache settings in `backend/src/shared/config/app-config.ts`
- [ ] T013 [P] Create backend indicator allowlist and unit catalog in `backend/src/indicators/domain/indicator-catalog.ts`
- [ ] T014 [P] Create backend canonical domain types from data-model.md in `backend/src/indicators/domain/indicator-detail.types.ts`
- [ ] T015 Create backend Redis cache abstraction in `backend/src/indicators/services/indicator-cache.service.ts`
- [ ] T016 Create backend provider HTTP client with timeout and retry policy in `backend/src/indicators/adapters/mindicador-client.ts`
- [ ] T017 Create backend provider-to-canonical mapper shell in `backend/src/indicators/adapters/mindicador-detail.mapper.ts`
- [ ] T018 [P] Create backend structured error categories in `backend/src/indicators/domain/indicator-errors.ts`
- [ ] T019 [P] Create frontend application bootstrap in `frontend/src/main.tsx`
- [ ] T020 [P] Create frontend routing shell with dashboard and indicator detail routes in `frontend/src/app/App.tsx`
- [ ] T021 [P] Create frontend BFF contract types matching OpenAPI schemas in `frontend/src/services/indicator-detail.types.ts`
- [ ] T022 [P] Create frontend API client wrapper for indicator detail responses in `frontend/src/services/indicator-detail.client.ts`

**Checkpoint**: Foundation ready; user story implementation can now begin.

---

## Phase 3: User Story 1 - Consultar detalle del indicador (Priority: P1) MVP

**Goal**: Users can open a supported indicator from the dashboard and see its main detail, provenance, dates, and referential warning.

**Independent Test**: Select any valid indicator from the dashboard and verify name, code, latest value, unit, effective date, source, update timestamp, and referential warning.

### Tests for User Story 1

- [ ] T023 [P] [US1] Add backend contract test for successful `GET /indicators/{code}/detail` metadata in `backend/tests/contract/indicator-detail-success.contract.spec.ts`
- [ ] T024 [P] [US1] Add backend mapper unit test for preserving name, code, value, unit, effective date, and source in `backend/tests/unit/mindicador-detail.mapper.spec.ts`
- [ ] T025 [P] [US1] Add frontend page test for valid indicator detail metadata rendering in `frontend/src/pages/IndicatorDetailPage.test.tsx`
- [ ] T026 [P] [US1] Add Playwright dashboard-to-detail happy path test in `tests/e2e/indicator-detail.spec.ts`

### Implementation for User Story 1

- [ ] T027 [US1] Implement provider payload mapping for latest indicator detail in `backend/src/indicators/adapters/mindicador-detail.mapper.ts`
- [ ] T028 [US1] Implement indicator detail orchestration service for fresh successful data in `backend/src/indicators/services/indicator-detail.service.ts`
- [ ] T029 [US1] Implement BFF controller route `GET /indicators/:code/detail` in `backend/src/indicators/indicators.controller.ts`
- [ ] T030 [US1] Implement frontend dashboard indicator selection links in `frontend/src/pages/DashboardPage.tsx`
- [ ] T031 [US1] Implement frontend detail page data loading for valid indicators in `frontend/src/pages/IndicatorDetailPage.tsx`
- [ ] T032 [US1] Implement detail summary component with value, unit, effective date, source, update timestamp, and warning in `frontend/src/components/indicator-detail/IndicatorDetailSummary.tsx`
- [ ] T033 [US1] Add `es-CL` number and date formatting utilities in `frontend/src/services/formatters.ts`

**Checkpoint**: User Story 1 is functional and independently testable.

---

## Phase 4: User Story 2 - Revisar variacion reciente (Priority: P2)

**Goal**: Users can understand absolute and percentage variation against the previous valid observation.

**Independent Test**: Open an indicator with at least two valid observations and verify absolute variation, percentage variation, baseline date, latest date, and non-color-only direction.

### Tests for User Story 2

- [ ] T034 [P] [US2] Add backend unit tests for decimal absolute and percentage variation in `backend/tests/unit/indicator-variation.spec.ts`
- [ ] T035 [P] [US2] Add backend unit tests for unavailable variation states in `backend/tests/unit/indicator-variation-unavailable.spec.ts`
- [ ] T036 [P] [US2] Add frontend variation rendering test for up, down, unchanged, and unavailable states in `frontend/src/components/indicator-detail/IndicatorVariation.test.tsx`

### Implementation for User Story 2

- [ ] T037 [US2] Implement decimal variation calculation domain service in `backend/src/indicators/domain/indicator-variation.service.ts`
- [ ] T038 [US2] Integrate variation calculation into detail orchestration in `backend/src/indicators/services/indicator-detail.service.ts`
- [ ] T039 [US2] Extend BFF detail response DTO with variation fields in `backend/src/indicators/dto/indicator-detail-response.dto.ts`
- [ ] T040 [US2] Implement frontend variation component with text/icon direction indicators in `frontend/src/components/indicator-detail/IndicatorVariation.tsx`
- [ ] T041 [US2] Render variation status and unavailable explanations in `frontend/src/pages/IndicatorDetailPage.tsx`

**Checkpoint**: User Stories 1 and 2 work independently.

---

## Phase 5: User Story 3 - Explorar serie reciente accesible (Priority: P3)

**Goal**: Users can inspect the recent series in chronological order through an accessible table.

**Independent Test**: Open an indicator with recent series and verify the table lists every visible observation with date and value, sorted chronologically and navigable by keyboard.

### Tests for User Story 3

- [ ] T042 [P] [US3] Add backend unit test for chronological sorting without filling missing dates in `backend/tests/unit/indicator-series-sorting.spec.ts`
- [ ] T043 [P] [US3] Add backend contract test for recent series array shape in `backend/tests/contract/indicator-detail-series.contract.spec.ts`
- [ ] T044 [P] [US3] Add frontend accessibility test for table headers and keyboard navigation in `frontend/src/components/indicator-detail/IndicatorSeriesTable.test.tsx`
- [ ] T045 [P] [US3] Add Playwright table inspection scenario in `tests/e2e/indicator-detail-series.spec.ts`

### Implementation for User Story 3

- [ ] T046 [US3] Implement recent series validation and chronological sorting in `backend/src/indicators/domain/indicator-series.service.ts`
- [ ] T047 [US3] Integrate recent series sorting into detail orchestration in `backend/src/indicators/services/indicator-detail.service.ts`
- [ ] T048 [US3] Implement accessible recent series table component in `frontend/src/components/indicator-detail/IndicatorSeriesTable.tsx`
- [ ] T049 [US3] Render no-generated-dates recent series table in `frontend/src/pages/IndicatorDetailPage.tsx`

**Checkpoint**: User Stories 1, 2, and 3 work independently.

---

## Phase 6: User Story 4 - Entender estados de carga y falla (Priority: P4)

**Goal**: Users can distinguish loading, success, no-series, invalid-indicator, stale-data, and service-unavailable states.

**Independent Test**: Force each required state and verify differentiated messaging, stale labeling, age display, and dashboard return path where required.

### Tests for User Story 4

- [ ] T050 [P] [US4] Add backend integration tests for invalid indicator and no-series states in `backend/tests/integration/indicator-detail-states.spec.ts`
- [ ] T051 [P] [US4] Add backend integration tests for stale-data and service-unavailable cache behavior in `backend/tests/integration/indicator-detail-resilience.spec.ts`
- [ ] T052 [P] [US4] Add frontend state rendering tests for all six detail states in `frontend/src/pages/IndicatorDetailStates.test.tsx`
- [ ] T053 [P] [US4] Add Playwright unavailable and invalid indicator scenarios in `tests/e2e/indicator-detail-states.spec.ts`

### Implementation for User Story 4

- [ ] T054 [US4] Implement invalid indicator guard before provider lookup in `backend/src/indicators/services/indicator-detail.service.ts`
- [ ] T055 [US4] Implement no-series state mapping in `backend/src/indicators/services/indicator-detail.service.ts`
- [ ] T056 [US4] Implement stale cache fallback and age metadata in `backend/src/indicators/services/indicator-cache.service.ts`
- [ ] T057 [US4] Implement service-unavailable response mapping in `backend/src/indicators/services/indicator-detail.service.ts`
- [ ] T058 [US4] Implement frontend loading, no-series, invalid, stale, and unavailable state components in `frontend/src/components/indicator-detail/IndicatorDetailStates.tsx`
- [ ] T059 [US4] Add dashboard return actions for non-success states in `frontend/src/pages/IndicatorDetailPage.tsx`
- [ ] T060 [US4] Add backend observability events for provider health, cache usage, stale age, and transformation failures in `backend/src/indicators/services/indicator-detail.service.ts`

**Checkpoint**: All user stories are independently functional.

---

## Phase 7: Polish & Cross-Cutting Concerns

**Purpose**: Validate quality gates and prevent scope creep beyond the approved feature.

- [ ] T061 [P] Verify no graph, advanced statistics, year selection, conversion, comparison, authentication, alerts, favorites, or export behavior exists in `frontend/src/pages/IndicatorDetailPage.tsx`
- [ ] T062 [P] Add OpenAPI contract validation script in `backend/tests/contract/validate-openapi-contract.spec.ts`
- [ ] T063 [P] Add accessibility audit coverage for detail page states in `tests/e2e/indicator-detail-a11y.spec.ts`
- [ ] T064 Run lint, formatting, type, unit, integration, contract, E2E, and accessibility checks from `package.json`
- [ ] T065 Run dependency and container vulnerability checks documented by CI in `Jenkinsfile`
- [ ] T066 Update implementation notes and validation evidence in `specs/001-indicator-detail/quickstart.md`

---

## Dependencies & Execution Order

### Phase Dependencies

- Setup (Phase 1) has no dependencies and can start immediately.
- Foundational (Phase 2) depends on Setup completion and blocks all user stories.
- User Story phases depend on Foundational completion.
- Polish depends on all desired user stories being complete.

### User Story Dependencies

- US1 (P1) can start after Foundational and is the MVP scope.
- US2 (P2) can start after Foundational but is easiest after US1 response shape exists.
- US3 (P3) can start after Foundational but is easiest after US1 response shape exists.
- US4 (P4) can start after Foundational and integrates with US1, US2, and US3 states.

### Within Each User Story

- Tests precede implementation tasks.
- Domain/model tasks precede service orchestration.
- Service orchestration precedes controller and UI integration.
- Each checkpoint requires running the story's independent test criteria before continuing.

### Parallel Opportunities

- T004-T009 can run in parallel after T001-T003 ownership is clear.
- T011-T014 and T018-T022 can run in parallel because they create separate foundational files.
- Test tasks within each user story can run in parallel.
- US2 and US3 can proceed in parallel after the foundational phase and the US1 contract shape are stable.
- Polish audit tasks T061-T063 can run in parallel.

---

## Parallel Example: User Story 1

```bash
Task: "T023 Add backend contract test for successful metadata in backend/tests/contract/indicator-detail-success.contract.spec.ts"
Task: "T024 Add backend mapper unit test in backend/tests/unit/mindicador-detail.mapper.spec.ts"
Task: "T025 Add frontend page test in frontend/src/pages/IndicatorDetailPage.test.tsx"
Task: "T026 Add Playwright happy path in tests/e2e/indicator-detail.spec.ts"
```

## Parallel Example: User Story 2

```bash
Task: "T034 Add backend decimal variation tests in backend/tests/unit/indicator-variation.spec.ts"
Task: "T035 Add backend unavailable variation tests in backend/tests/unit/indicator-variation-unavailable.spec.ts"
Task: "T036 Add frontend variation rendering tests in frontend/src/components/indicator-detail/IndicatorVariation.test.tsx"
```

## Parallel Example: User Story 3

```bash
Task: "T042 Add backend sorting tests in backend/tests/unit/indicator-series-sorting.spec.ts"
Task: "T043 Add backend series contract tests in backend/tests/contract/indicator-detail-series.contract.spec.ts"
Task: "T044 Add frontend table accessibility tests in frontend/src/components/indicator-detail/IndicatorSeriesTable.test.tsx"
Task: "T045 Add Playwright table scenario in tests/e2e/indicator-detail-series.spec.ts"
```

## Parallel Example: User Story 4

```bash
Task: "T050 Add invalid/no-series integration tests in backend/tests/integration/indicator-detail-states.spec.ts"
Task: "T051 Add stale/unavailable integration tests in backend/tests/integration/indicator-detail-resilience.spec.ts"
Task: "T052 Add frontend state tests in frontend/src/pages/IndicatorDetailStates.test.tsx"
Task: "T053 Add Playwright state scenarios in tests/e2e/indicator-detail-states.spec.ts"
```

---

## Implementation Strategy

### MVP First (User Story 1 Only)

1. Complete Phase 1 setup.
2. Complete Phase 2 foundational domain, BFF, cache, routing, and client shells.
3. Complete Phase 3 User Story 1.
4. Stop and validate that a user can open a valid indicator detail from the dashboard.

### Incremental Delivery

1. Deliver US1 for basic detail and provenance.
2. Add US2 for variation.
3. Add US3 for accessible recent series.
4. Add US4 for full state and resilience coverage.
5. Run Phase 7 quality gates before implementation is considered complete.

### Parallel Team Strategy

1. One developer owns backend domain/BFF tasks while another owns frontend shell tasks during Phase 2.
2. After US1 contract shape stabilizes, US2 variation and US3 table work can proceed in parallel.
3. US4 resilience and state work integrates after the core success response is stable.
