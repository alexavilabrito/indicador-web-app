# Quickstart: Indicator Detail

## Prerequisites

- Frontend and backend dependencies installed for the repository.
- Redis available for cache and stale-data scenarios.
- `mindicador.cl` reachable for fresh-data scenarios, or test fixtures available for offline
  validation.
- Environment variables configured for the BFF runtime, cache connection, and allowed origin
  policy.

## Setup

```bash
npm install
docker compose up -d redis
```

If the final repository uses package-specific scripts, run the equivalent install command for both
`frontend/` and `backend/`.

## Contract Validation

Validate the BFF response against [contracts/indicator-detail.openapi.yaml](contracts/indicator-detail.openapi.yaml).

Expected outcomes:

- `GET /indicators/uf/detail` returns `state: success` or `state: stale_data` with indicator
  metadata, latest observation, freshness, recent series, and variation when enough data exists.
- `GET /indicators/nope/detail` returns `state: invalid_indicator` and no provider lookup is
  needed.
- Decimal fields are serialized as strings to preserve precision.
- Recent series entries are sorted by `effectiveDate` ascending.

## Unit Validation

Run unit tests for:

- Indicator code allowlist validation.
- Provider payload transformation into the canonical model.
- Chronological sorting without filling missing dates.
- Decimal absolute and percentage variation.
- Variation unavailable states for one observation, invalid values, or missing values.
- Fresh versus stale data metadata.

```bash
npm test
```

## Integration Validation

Run integration tests for the BFF detail flow:

- Successful provider response for each supported indicator category.
- Empty or missing provider series produces `no_series`.
- Provider timeout or unavailable service with cached data produces `stale_data`.
- Provider timeout or unavailable service without cached data produces `service_unavailable`.
- Incomplete payload is observable and never presented as complete success.

```bash
npm run test:integration
```

## Frontend Validation

Run component and page tests for:

- Selecting an indicator from the dashboard opens the detail view.
- Detail view displays name, code, latest value, unit, effective date, source, update timestamp,
  and referential warning.
- Variation direction is communicated by text or iconography, not only color.
- Table headers and rows are navigable with keyboard and understandable by assistive technology.
- Loading, success, no-series, invalid-indicator, stale-data, and service-unavailable states are
  visually distinct.

```bash
npm run test:frontend
```

## End-to-End Validation

Run responsive E2E checks across mobile and desktop viewports:

1. Open the dashboard.
2. Select `uf`.
3. Confirm the detail screen appears in under 2.5 seconds with useful content or an explanatory
   state.
4. Confirm source, effective date, update timestamp, latest value, unit, variation, and accessible
   table are present when data is available.
5. Open an invalid indicator route and confirm the invalid state plus return path to dashboard.
6. Simulate provider unavailability with and without cached data and confirm stale-data and
   service-unavailable states.

```bash
npm run test:e2e
```

## Quality Gates

Before implementation is considered complete:

- Lint and formatting checks pass.
- Type checks pass in strict mode.
- Unit, integration, contract, accessibility, and E2E tests pass.
- Dependency and container vulnerability scans pass or documented exceptions are approved.
- No graph, advanced statistics, year selection, conversion, comparison, authentication, alerts,
  favorites, or export behavior appears in the feature.
