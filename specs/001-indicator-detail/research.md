# Research: Indicator Detail

## Decision: Use a NestJS BFF as the only integration point with `mindicador.cl`

**Rationale**: The constitution requires the external provider to be encapsulated behind an
adapter and prevents the user experience from coupling directly to provider payloads. A BFF gives
the frontend a stable product contract, centralizes allowlist validation for the 12 indicators,
and keeps timeout, retry, stale-cache, and observability policy in one server-side boundary.

**Alternatives considered**:
- Direct frontend calls to `mindicador.cl`: rejected because it exposes the UI to provider payload
  changes and weakens centralized resilience, security, and observability.
- Static build-time data snapshots: rejected because users need recent values and explicit source
  freshness.

## Decision: Define a canonical indicator detail model before presentation

**Rationale**: The canonical model preserves code, name, unit, source, effective date, last value,
previous observation, recent series, freshness, and detail state independently of provider field
names. This supports contract tests with provider fixtures and prevents incomplete or malformed
provider data from reaching the UI as trustworthy values.

**Alternatives considered**:
- Passing provider payloads through unchanged: rejected because it violates separation of
  responsibilities and makes contract changes leak across the app.
- Normalizing only in the frontend: rejected because stale handling, invalid values, and
  observability belong at the integration boundary.

## Decision: Use decimal arithmetic for variation calculations

**Rationale**: Variation values are financial calculations. Decimal arithmetic avoids binary
floating-point artifacts, keeps absolute and percentage variation reproducible, and satisfies the
constitutional rule that monetary and indicator calculations define precision and rounding.

**Alternatives considered**:
- Native binary floating-point numbers: rejected for calculation truth because small precision
  errors can surface in financial display and tests.
- String-only display with no calculation: rejected because the feature explicitly requires
  absolute and percentage variation when at least two observations exist.

## Decision: Sort recent observations chronologically in the canonical model

**Rationale**: The spec requires chronological presentation even if provider ordering differs. The
adapter/domain layer will preserve original date/value/source semantics while exposing a stable
ascending chronological series to the frontend contract.

**Alternatives considered**:
- Preserve provider order: rejected because it fails the user-facing chronological requirement.
- Fill missing dates in the series: rejected because the constitution forbids silently generating
  values for dates without observations.

## Decision: Use Redis for short-lived cache and stale fallback, not PostgreSQL

**Rationale**: The feature needs resilience and stale-data labeling, not durable user data or
long-term analytical storage. Redis supports recent provider response caching, stale fallback
metadata, and quick invalidation without adding persistent relational schema for this scope.

**Alternatives considered**:
- PostgreSQL persistence: rejected for this feature because no durable user-generated or audited
  dataset is required.
- No cache: rejected because the constitution requires controlled degradation and stale data
  handling when the provider is unavailable.

## Decision: Keep Apache ECharts out of this feature bundle

**Rationale**: The feature explicitly excludes graphs. Deferring chart dependencies protects the
performance budget and avoids implementing future visualization scope prematurely.

**Alternatives considered**:
- Include chart scaffolding now: rejected because it adds unused complexity and violates the
  feature boundary.

## Decision: Validate six user-visible states end to end

**Rationale**: The spec and constitution both require differentiated states for loading, success,
absence of series, invalid indicator, stale data, and service unavailable. These states must be
observable in quickstart, contract, integration, E2E, and accessibility validation.

**Alternatives considered**:
- Generic error state: rejected because it hides domain distinctions and can cause stale or
  partial data to appear complete.
