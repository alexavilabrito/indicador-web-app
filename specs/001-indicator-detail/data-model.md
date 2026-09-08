# Data Model: Indicator Detail

## Indicator

Represents one constitution-supported economic indicator.

| Field | Type | Required | Validation |
| --- | --- | --- | --- |
| `code` | string | Yes | One of `uf`, `ivp`, `dolar`, `dolar_intercambio`, `euro`, `ipc`, `utm`, `imacec`, `tpm`, `libra_cobre`, `tasa_desempleo`, `bitcoin` |
| `name` | string | Yes | Non-empty display name |
| `unit` | IndicatorUnit | Yes | Matches constitutional unit for the code |
| `unitCategory` | enum | Yes | `money`, `percentage`, `unit_of_account`, `commodity`, or `cryptoasset` |
| `source` | SourceInfo | Yes | Identifies `mindicador.cl` |
| `isSupported` | boolean | Yes | False only for invalid-code state |

## IndicatorUnit

Represents the unit published by the source and used in display.

| Field | Type | Required | Validation |
| --- | --- | --- | --- |
| `label` | string | Yes | `Pesos`, `Porcentaje`, or `Dolar` according to the indicator |
| `displaySymbol` | string | No | Present only when unambiguous for UI display |
| `locale` | string | Yes | `es-CL` |

## SourceInfo

Represents provenance metadata.

| Field | Type | Required | Validation |
| --- | --- | --- | --- |
| `name` | string | Yes | Must identify `mindicador.cl` |
| `url` | string | Yes | Valid HTTPS URL for source reference |
| `retrievedAt` | datetime | Yes | ISO timestamp of consultation or cache retrieval |

## IndicatorObservation

Represents one published observation for an indicator.

| Field | Type | Required | Validation |
| --- | --- | --- | --- |
| `indicatorCode` | string | Yes | Must match parent indicator code |
| `effectiveDate` | date | Yes | Valid calendar date from source observation |
| `value` | decimal | Yes | Finite numeric value; no binary floating-point truth |
| `unit` | IndicatorUnit | Yes | Must match parent indicator unit |
| `source` | SourceInfo | Yes | Must identify provider provenance |
| `quality` | enum | Yes | `valid`, `invalid_value`, `missing_value`, or `incomplete_payload` |

## Variation

Represents change from the latest valid observation to the immediately previous valid observation.

| Field | Type | Required | Validation |
| --- | --- | --- | --- |
| `status` | enum | Yes | `available`, `not_enough_data`, or `invalid_data` |
| `absolute` | decimal | Conditional | Required only when status is `available` |
| `percentage` | decimal | Conditional | Required only when status is `available`; calculated from previous valid value |
| `direction` | enum | Conditional | `up`, `down`, or `unchanged` when status is `available` |
| `baselineDate` | date | Conditional | Effective date of previous observation when status is `available` |
| `latestDate` | date | Conditional | Effective date of latest observation when status is `available` |

## Freshness

Represents whether displayed data is current or stale.

| Field | Type | Required | Validation |
| --- | --- | --- | --- |
| `status` | enum | Yes | `fresh`, `stale`, or `unknown` |
| `lastSuccessfulRefreshAt` | datetime | Conditional | Required when cached data exists |
| `ageLabel` | string | Conditional | Required when status is `stale` |
| `reason` | enum | Conditional | `provider_unavailable`, `timeout`, `incomplete_payload`, or `policy_expired` |

## IndicatorDetail

Represents the consolidated data needed by the detail view.

| Field | Type | Required | Validation |
| --- | --- | --- | --- |
| `state` | DetailState | Yes | Determines which UI state is rendered |
| `indicator` | Indicator | Conditional | Required for success, stale data, and absence-of-series states |
| `latestObservation` | IndicatorObservation | Conditional | Required when a valid latest value exists |
| `previousObservation` | IndicatorObservation | No | Used only for variation calculation |
| `variation` | Variation | Yes | Must explain unavailable variation when it cannot be calculated |
| `recentSeries` | IndicatorObservation[] | Yes | Sorted ascending by effective date; no generated missing dates |
| `freshness` | Freshness | Yes | Must distinguish fresh and stale display |
| `message` | string | Conditional | Required for non-success states |

## DetailState

Represents the user-visible state of the detail view.

| State | Meaning | Required user-facing behavior |
| --- | --- | --- |
| `loading` | Data request in progress | Show orientation for requested indicator |
| `success` | Valid current detail and optional series available | Show detail, variation when available, and table |
| `no_series` | Indicator exists but recent series is absent or empty | Show available indicator metadata and absence message |
| `invalid_indicator` | Code is outside the supported list | Explain invalid indicator and provide return path to dashboard |
| `stale_data` | Cached prior valid data is displayed | Label stale status, age, and source clearly |
| `service_unavailable` | No fresh or cached valid detail can be shown | Explain unavailability and avoid partial complete presentation |

## Relationships

- `IndicatorDetail` has one `Indicator` when the requested code is supported.
- `IndicatorDetail` has zero or one `latestObservation`.
- `IndicatorDetail` has zero or one `previousObservation`.
- `IndicatorDetail` has zero or many `recentSeries` observations.
- `Variation` is derived from `latestObservation` and `previousObservation`; it is not provider
  data.
- `Freshness` is derived from retrieval and cache metadata; it must be shown when stale.

## State Transitions

```text
loading -> success
loading -> no_series
loading -> invalid_indicator
loading -> stale_data
loading -> service_unavailable
stale_data -> success
stale_data -> service_unavailable
service_unavailable -> loading
invalid_indicator -> loading
```

## Validation Rules

- Indicator code validation happens before any provider lookup.
- Observations with missing date, missing value, non-numeric value, or mismatched unit are marked
  invalid and excluded from latest-value and variation calculations.
- Recent series sorting uses effective date ascending.
- Missing calendar dates are not filled.
- Variation requires two valid observations and a previous value suitable for percentage change.
- Presentation rounding must not mutate stored decimal values.
