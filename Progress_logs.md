# Project Progress Log

## 8 September 2026

### Initial Setup
- Selected Jharkhand as the state for the agriculture information system.
- Registered as Team 18.
- Created the GitHub repository `jharkhand-agriculture-ai-agent`.
- Invited the Instructor and TA as collaborators.
- Declared team roles: Source Lead, Data Engineering Lead, Model Lead, Evaluation Lead.
- Created the initial project README.

### Current Phase
Phase 1 – Source Discovery and Knowledge Base Design

## 10–12 September 2026

### Source Discovery
- Defined the project scope as a Price–Storage–Advisory Intelligence System for Jharkhand.
- Compiled an initial list of roughly 24 candidate agriculture data sources across seven
  categories: monitoring, production, storage, logistics, prices, trade, and advisories.
- Ran a manual verification pass, opening every candidate URL rather than relying on the
  compiled list alone.
- Found that a substantial number of candidate entries pointed to portals that do not exist,
  including one that referenced a Nepalese research body rather than an Indian institution.
- Classified each source as verified, corrected, or dropped.
- Retained dropped sources in the inventory as documented data gaps instead of deleting them.

## 13–14 September 2026

### Schema and Extraction Planning
- Recorded modality, update frequency, language, and limitations for each surviving source.
- Identified the main source modalities in use:
  - Structured CSV and API data (data.gov.in mandi dataset)
  - HTML dashboards with no export function (FCI Depot Online, e-NAM)
  - Text-layer PDF bulletins (IMD agromet advisories)
  - Unstructured and partly scanned PDF notices (Directorate of Agriculture)
- Defined the initial knowledge-base schema around location, prices, storage, advisories,
  and provenance.
- Chose District + Date as the primary join key connecting the three data tables.
- Drafted the proposed extraction and agent/tool workflow.
- Noted key limitations: no verifiable state warehousing or cold-chain portal exists for
  Jharkhand, FCI data is a live snapshot with no historical archive, and district name
  spellings vary across sources.
- Uploaded project documentation and the source inventory to GitHub.

### Next Tasks
- Complete a final click-through check of every URL before submission.
- Finalize the Phase 1 source inventory.
- Prepare the Phase 1 presentation.
- Refine the proposed agent and tool architecture for the presentation.
- Prepare for the Phase 1 review.
