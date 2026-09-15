# Statement of Purpose and Project Scope

**Project:** Price–Storage–Advisory Intelligence System for Jharkhand
**Course:** CS F/U 407 – Artificial Intelligence
**Team:** Team 18
**Phase:** Phase 1 – Source Discovery and Knowledge Base Design

---

## 1. Problem We Are Trying to Address

Agricultural price information, public storage capacity and weather based
advisories for Jharkhand are published by different agencies, in different
formats, and on portals that do not talk to each other. A mandi price for
paddy sits in a national open data API, the depot capacity that decides
whether that paddy can be held back sits in a live HTML dashboard with no
export button, and the agromet advisory that may have influenced the sowing
or harvest decision sits inside a weekly PDF bulletin.

Because these three things are never brought together, a fairly basic
question stays hard to answer. If prices in a district fall sharply, was
there storage available nearby that would have allowed produce to be held,
and had an advisory been issued in the days before that movement?

This project builds a knowledge base that makes those questions answerable
for Jharkhand, with every stored number traceable back to the document and
page it came from.

## 2. Objective

To design and build an agriculture information system for Jharkhand that
pulls price, storage and advisory information out of heterogeneous official
sources and stores it in a single queryable database with full provenance.

## 3. Why This Framing

Three reasons drove the choice of price, storage and advisory as the three
pillars rather than the more common production and yield framing.

**Modality spread.** These three domains happen to cover every document type
the course expects us to handle. Prices come as clean CSV and API output,
storage comes only as HTML dashboards, and advisories come as PDFs, some with
a text layer and some scanned. We are not manufacturing difficulty, the
sources genuinely arrive this way.

**A real join.** All three publish at district level and all three carry a
date. That gives us an honest common key instead of a forced one.

**An analysis worth doing.** Price movement, storage availability and
advisory timing are actually causally related in the field, so the
spatio-temporal relationships we report in Phase 3 will mean something rather
than being a correlation exercise.

## 4. Scope of the Knowledge Base

The knowledge base is organised around five entities.

| Entity | What it holds |
|---|---|
| Location | State, district and market identity, plus name normalisation |
| Prices | Daily mandi arrivals and min, max and modal prices by commodity |
| Storage | Public storage and warehousing capacity by facility and agency |
| Advisories | District level agromet bulletins and departmental notices |
| Source | Source name, URL, document, page number, modality and collection date |

Prices, storage and advisories are joined on **District + Date**.

## 5. In Scope for Phase 1

- Discovering and manually verifying official agriculture data sources for Jharkhand
- Recording modality, update frequency, language and licence for each source
- Documenting what each source can and cannot give us
- Designing the initial schema and writing down the reasoning behind it
- Proposing the extraction agent and the tools it will call

## 6. Explicitly Out of Scope

- Any record level or beneficiary level farmer data, in line with the course ethics guidelines
- Raster map interpretation, spatial data is used only where it is available in vector or tabular form
- Inter-state comparison, which will be revisited only if Phase 3 time permits
- Model fine-tuning, which belongs to Phase 2 after the baseline and eval set exist

## 7. Questions the System Should Eventually Answer

1. What was the modal price of a given commodity in a given district on a given date?
2. Which districts hold the most public storage capacity per unit of arrivals?
3. How does the price of one commodity vary across districts on the same day?
4. Do prices move in the week following an agromet advisory?
5. Which source, document and page did a particular figure come from?

Questions 2 and 3 are spatial, 1 and 4 are temporal, and 5 is a provenance
query, which lines up with what the Phase 3 gate asks for.

## 8. Known Constraints Going In

- No verifiable state run warehousing or cold chain portal exists for Jharkhand, so storage coverage is limited to FCI and WDRA registered facilities and is not total state capacity.
- FCI depot data is a live snapshot with no historical archive, so our storage time series can only begin from our first collection date.
- District name spellings are inconsistent across sources, for example Saraikela Kharsawan, and will need a normalisation table.
- Advisory PDFs are partly scanned, so OCR quality will cap advisory extraction accuracy.

These are recorded as limitations rather than hidden, and the dropped sources
are retained in the inventory as documented data gaps.
