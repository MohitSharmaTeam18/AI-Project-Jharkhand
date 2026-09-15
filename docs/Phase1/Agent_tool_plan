# Proposed Agent and Tool Architecture

**Project:** Price–Storage–Advisory Intelligence System for Jharkhand
**Team:** Team 18
**Phase:** Phase 1 (proposed design, to be implemented in Phase 2)

---

## 1. Design Idea

Rather than writing one script per source, we plan a single controller agent
that looks at an incoming document, decides what kind of thing it is, and
calls the right tool for it. That keeps the system open to sources we have
not seen yet, which matters because the Phase 2 gate tests extraction on
unseen documents supplied by the instructor.

The agent is responsible for routing and for deciding when an extraction has
failed. The tools do the actual reading. The schema mapper is deliberately
kept separate from the extractors so that a change in the schema does not
mean rewriting every extractor.

## 2. Pipeline

```
Source document
      |
      v
[ Ingestion ]  -> record URL, collection date, language, licence in the manifest
      |
      v
[ Router Agent ]  -> classify modality: API / CSV / HTML / text PDF / scanned PDF
      |
      v
[ Extraction Tool ]  -> selected by modality
      |
      v
[ Schema Mapper ]  -> map raw fields to Location / Prices / Storage / Advisories
      |
      v
[ Validator ]  -> pass, or flag for review
      |
      v
[ Database ]  -> row written with its provenance record attached
      |
      v
[ Query Agent ]  -> natural language question to SQL to answered result
```

## 3. Tools by Modality

### 3.1 Structured API and CSV

*Source example:* data.gov.in mandi price resource

Tool: HTTP client plus a pandas based tabular loader.
Work done: paginated pull, filter to Jharkhand, type coercion on dates and
prices, deduplication on district, market, commodity and date.
This is the cleanest path and needs no language model in the loop.

### 3.2 HTML Dashboard With No Export

*Source example:* FCI Depot Online capacity listing, e-NAM

Tool: a headless browser driver plus an HTML table parser.
Work done: drive the state and district dropdowns, read the rendered table,
stamp every row with the snapshot date since the portal overwrites rather
than archives. Scraping respects robots.txt and is rate limited, and we
prefer any official download if one appears.

### 3.3 PDF With a Text Layer

*Source example:* IMD GKMS agromet bulletin for Jharkhand

Tools: a PDF text and layout extractor, then an LLM based field extractor.
Work done: pull the text per page, locate the district block, and have the
model return a structured advisory record with issue date, district,
advisory type and a short summary. Page number is carried through so the
provenance query can point back to it.

### 3.4 Scanned or Mixed PDF

*Source example:* Directorate of Agriculture notices and statistical annexures

Tools: OCR, then table structure recovery, then the same field extractor.
Work done: rasterise, OCR, rebuild the table grid, map columns to schema
fields. This is the weakest link and we expect a meaningful failure rate,
which is why anything from this path carries a lower confidence flag.

### 3.5 Supporting Tool: District Normaliser

A lookup table mapping every observed spelling to one canonical district code.
Every extractor calls it before writing. Without this the District + Date join
silently loses rows.

## 4. Validation Layer

Nothing is written to the database until it clears these checks.

| Check | What it catches |
|---|---|
| Required fields present | Partial OCR rows and truncated API responses |
| Type and range check | Prices that are zero, negative or absurdly large, dates outside the collection window |
| Ordering check on prices | Rows where min exceeds modal or modal exceeds max |
| District resolves to a canonical name | Spelling drift and stray header rows read as data |
| Duplicate key check | Repeated pulls of the same district, market, commodity and date |
| Provenance present | Any row that cannot name its source, document and page |

Rows that fail are not discarded. They go to a quarantine table with the
reason, so the failure rate itself becomes something we can report at the
Phase 2 and Phase 3 gates.

A sample of scanned PDF output will additionally be checked by hand against
the original document, and that manual agreement rate becomes our OCR
accuracy estimate.

## 5. Provenance Handling

Every row written to Prices, Storage or Advisories carries a foreign key into
the Source table holding source name, URL, document name, page number,
modality and collection date. This is what makes the Phase 3 provenance query
answerable rather than something we reconstruct afterwards.

## 6. Model Plan

Phase 2 begins with a prompted base model as the baseline, with the eval set
built and held out before any training happens. A LoRA or QLoRA adapter is
then trained on our own extraction examples, mainly the messy PDF cases where
prompting alone is weakest, and the with and without adapter accuracy is
reported on the same held out set. If the adapter does not beat the baseline
we report that honestly rather than tuning the eval set to make it look good.

## 7. Known Risks in This Plan

- Portal structure changes will break the HTML scrapers, so selectors are kept in a config file rather than in code.
- OCR quality on old scanned annexures may make some crop and storage tables unusable, in which case those sources move to the documented gaps list.
- The FCI snapshot limitation means storage is thin as a time series for the whole of Phase 2, so early analysis will lean more on the price and advisory relationship.
