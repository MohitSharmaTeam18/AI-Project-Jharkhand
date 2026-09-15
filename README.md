# Jharkhand Agricultural AI Agent

## Course

CS F/U 407 – Artificial Intelligence

## Team

Team 18

### Members and Roles

| Name | Role(s) |
|---|---|
| Mohit Sharma | Source + data engineering Lead |
| Nikhil Kumar Siani | Model lead |
| Chetan Bugalia | Evaluation Lead |

## State

Jharkhand

## Project Overview

This project aims to develop an intelligent agricultural information system for Jharkhand.

The system will identify and integrate agriculture-related information from heterogeneous sources such as PDFs, webpages, spreadsheets, APIs, and other official data sources.

An agentic AI pipeline will later be developed to extract, validate, store, and query structured agricultural information.

## Current Phase

Phase 1 – Source Discovery and Knowledge Base Design

## Phase 1 Objectives

- Define the purpose and scope of the Jharkhand agriculture knowledge base
- Identify reliable agriculture-related data sources
- Record source URLs and data modalities
- Identify languages used in the sources
- Document source limitations
- Determine what information can be extracted from each source
- Design the initial knowledge-base/database schema
- Identify possible AI agents and tools for data extraction

## Current Status

- State selected: Jharkhand
- Team: Team 18
- GitHub repository created
- Instructor and TA invited as collaborators
- Source discovery: completed and verified (24 candidate sources reviewed, 8 verified, 13 corrected, 7 dropped as unverifiable)
- Initial knowledge base schema: drafted
- Agent and tool plan: drafted

## Repository Structure

```
data/                                    # Collected raw datasets and documents
docs/phase1/                             # Statement of purpose, schema reasoning, agent plan
logs/                                    # Phase-wise progress logs
schema/                                  # Knowledge base schema definition
Jharkhand_Source_Inventory_Phase1.xlsx   # Phase 1 source inventory
README.md
```

## Notes on Source Verification

All candidate sources were checked against live search results before inclusion. Sources are marked
as VERIFIED, CORRECTED (real concept, wrong URL — replaced with the actual portal), or DROPPED
(no verifiable equivalent found). Dropped sources are retained in the inventory as documented data
gaps rather than removed, in line with the Phase 1 requirement to report limitations of current sources.
