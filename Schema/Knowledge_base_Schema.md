# Knowledge Base Schema

## Project
Price–Storage–Advisory Intelligence System for Jharkhand

## Purpose
The knowledge base will connect mandi price information with storage capacity
and agricultural advisories across districts and time in Jharkhand.

## 1. Location

Fields:
- state
- district
- market_name

## 2. Prices

Fields:
- date
- district
- market_name
- commodity
- price_min
- price_max
- price_modal

Source example:
`Agmarknet daily price export`

## 3. Storage

Fields:
- district
- facility_name
- agency_type
- capacity_mt
- snapshot_date

Source example:
`FCI Depot Online capacity listing`

## 4. Advisories

Fields:
- issue_date
- district
- advisory_type
- summary_text

Source example:
`IMD GKMS agromet bulletin (PDF)`

## 5. Source / Provenance

Fields:
- source_name
- source_url
- document_name
- page_number
- modality

## Main Relationship

Prices, storage, and advisories will primarily be connected using:

District + Date

Example:

Mandi Prices 2025
        |
        | District + Date
        v
Storage Capacity / Agromet Advisories

## Initial Query Types

The knowledge base should eventually support questions such as:

- What was the modal price of paddy in a particular district on a given date?
- Which districts have the highest storage capacity?
- How did prices for a commodity vary across districts?
- Do prices shift after an agromet advisory is issued?
- Which source and page contain a particular price or capacity figure?

## Known Limitations

- Storage data covers only FCI and WDRA facilities, not total state capacity.
- FCI data is a live snapshot with no historical archive, so storage time series
  begins from our first collection date.
- District name spellings vary across sources and will need normalization.
