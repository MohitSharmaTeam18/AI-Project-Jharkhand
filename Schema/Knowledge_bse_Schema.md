# Knowledge Base Schema — Jharkhand Agriculture AI Agent

Phase 1 minimum viable schema. Intentionally frozen and minimal to avoid schema churn in Phase 2.
Designed around three analysis axes required at the Phase 3 gate: **spatial**, **temporal**, **provenance**.

---

## Table: `price_observations`

| Field | Type | Description / source mapping |
|---|---|---|
| id | UUID | Internal record ID |
| commodity | text | Crop/commodity name, normalized against a controlled vocabulary |
| district | text | Jharkhand district (normalized spelling) |
| market_name | text | Mandi/market name, from Agmarknet / e-NAM market master list |
| date | date | Date of price observation |
| price_min | numeric | Minimum price (Rs/quintal) |
| price_max | numeric | Maximum price (Rs/quintal) |
| price_modal | numeric | Modal price (Rs/quintal) |
| source_id | FK → sources.id | Provenance link |
| source_url | text | Direct page/report URL the value was extracted from |

**Sources feeding this table:** Agmarknet, e-NAM, data.gov.in daily mandi price dataset

---

## Table: `storage_facilities`

| Field | Type | Description / source mapping |
|---|---|---|
| id | UUID | Internal record ID |
| facility_name | text | From FCI Depot Online / WDRA registry |
| district | text | Jharkhand district |
| capacity_mt | numeric | Storage capacity in metric tons |
| agency_type | text | FCI / CWC / SWC / Private |
| snapshot_date | date | Date capacity figure was recorded (FCI data is live, not historical) |
| source_id | FK → sources.id | Provenance link |

**Sources feeding this table:** FCI Depot Online System, WDRA registered warehouses

---

## Table: `advisories`

| Field | Type | Description / source mapping |
|---|---|---|
| id | UUID | Internal record ID |
| district | text | District/block the advisory targets |
| issue_date | date | Advisory issue date |
| advisory_type | text | Weather / pest / irrigation / other (tagged during extraction) |
| summary_text | text | Extracted advisory content (agent-parsed from PDF bulletin) |
| source_id | FK → sources.id | Provenance link |

**Sources feeding this table:** IMD Gramin Krishi Mausam Seva (GKMS) agromet bulletins

---

## Table: `sources` (provenance / manifest)

| Field | Type | Description |
|---|---|---|
| id | UUID | Internal record ID |
| source_name | text | e.g. "Agmarknet", "FCI Depot Online" |
| base_url | text | Root portal URL |
| modality | text | PDF / CSV / API / HTML dashboard / GeoTIFF |
| language | text | Language(s) of source content |
| license_status | text | OGL-India / unstated-academic-use-only / other |
| access_date | date | When the team last verified the source was live |
| known_limitation | text | From the source inventory "Limitation" column |

This table satisfies the manifest requirement directly. Every fact in the other three tables must
trace back to a row here — this is what makes the Phase 3 provenance query ("which source and page
did this number come from?") answerable.

---

## Reasoning: Source to Schema Mapping

- **`price_observations`** exists because three independent verified sources (Agmarknet, e-NAM,
  data.gov.in) converge on the same underlying data — price by commodity, market, and date — making
  it the most reliable and cross-checkable table in the system.
- **`storage_facilities`** exists because FCI Depot Online and WDRA are the only two storage-related
  sources that survived verification. The schema reflects what is actually obtainable, not an
  idealized storage model.
- **`advisories`** exists because IMD GKMS bulletins were the one advisory source confirmed real and
  Jharkhand-specific, replacing a fabricated department portal in the original candidate list.
- **`sources`** exists to make provenance queries possible and to serve as the corpus manifest.

## Known Schema Limitations

- Trade data (TRADESTAT) is available at state level but not disaggregated to district, so it is not
  modelled as a table in Phase 1 — revisit in Phase 2 if district mapping becomes feasible.
- District name normalization across sources is an open problem (spellings vary); a mapping table may
  be required in Phase 2.
- Commodity naming is inconsistent across Agmarknet and e-NAM; controlled vocabulary to be finalized.
