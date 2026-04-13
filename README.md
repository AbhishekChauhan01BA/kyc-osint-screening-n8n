# kyc-osint-screening-n8n
AI-assisted KYC/OSINT screening workflow in n8n Cloud using Gemini, Serper, and Google Docs

# KYC OSINT Screening Workflow | n8n Cloud + Gemini

![n8n Cloud](https://img.shields.io/badge/n8n-Cloud-orange)
![LLM](https://img.shields.io/badge/LLM-Google%20Gemini-blue)
![Search](https://img.shields.io/badge/Search-Serper-success)
![Reporting](https://img.shields.io/badge/Reporting-Google%20Docs-informational)
![Status](https://img.shields.io/badge/Status-Portfolio%20Project-brightgreen)
![Human Review](https://img.shields.io/badge/Decision-Human%20in%20the%20Loop-important)

AI-assisted **KYC / OSINT screening workflow** built in **n8n Cloud** using **Webhook intake**, **Google Gemini**, **Serper web search**, structured findings normalization, AI-driven report drafting, and **Google Docs reviewer report generation**.

This workflow is designed to support a reviewer in understanding:

- what was searched
- what was found
- how strong the match is
- what the risk signals are
- what still needs manual verification

It **does not make the final compliance decision**.  
The final decision remains with a **human analyst**.

---

## Table of Contents

- [Overview](#overview)
- [Business Goal](#business-goal)
- [Key Features](#key-features)
- [Workflow Architecture](#workflow-architecture)
- [Architecture Diagram](#architecture-diagram)
- [Node Inventory](#node-inventory)
- [AI Roles](#ai-roles)
- [Search Categories](#search-categories)
- [Report Output](#report-output)
- [How to Run](#how-to-run)
- [Example Input](#example-input)
- [Example Output](#example-output)
- [Tech Stack](#tech-stack)
- [Design Principles](#design-principles)
- [Known Limitations](#known-limitations)
- [Future Enhancements](#future-enhancements)
- [Security Notes](#security-notes)
- [Author](#author)

---

## Overview

This project demonstrates how to orchestrate an end-to-end **OSINT screening workflow** in **n8n Cloud** for portfolio use.

The workflow combines:

- structured request intake
- validation and normalization
- AI-assisted search planning
- online web-search execution
- standardized findings extraction
- AI-assisted findings analysis
- reviewer-oriented report generation
- final structured API response

It is intended as a practical demonstration of:

- workflow automation
- AI orchestration
- KYC / OSINT review logic
- structured reporting for analyst support

---

## Business Goal

The objective is to build a workflow that can take a screening request for an **individual** or **organization** and produce a structured review output that helps a reviewer decide:

- whether the subject identity is reasonably resolved
- whether there are any indications of:
  - PEP / public office exposure
  - sanctions / watchlist exposure
  - offshore / financial integrity concerns
  - adverse media
  - corporate links / ownership connections
  - scam / fraud / misconduct signals
- whether the results are strong, weak, ambiguous, or require manual verification

---

## Key Features

- **Webhook intake** for API-style workflow triggering
- **Input normalization** for clean downstream processing
- **Validation layer** for required fields
- **Gemini-based Search Planner Agent**
- **Gemini-based Findings Analyst Agent**
- **Dynamic search queue generation**
- **Serper web search integration**
- **Normalized findings model**
- **Case-level findings aggregation**
- **OSINT review report draft generation**
- **Word-report mapping structure**
- **Google Doc report creation and update**
- **Final webhook response**
- **Human-in-the-loop final decision control**

---

## Workflow Architecture

The workflow is divided into seven layers:

### 1. Intake
Receives JSON screening requests through a webhook.

### 2. Validation
Normalizes and validates the incoming payload before any search or AI processing starts.

### 3. AI Search Planning
Uses a Gemini-powered AI agent to:
- prioritize identity resolution
- generate focused search queries
- structure the search plan by category
- reduce noisy search expansion

### 4. Search Collection
Builds a ranked search queue and executes web searches using Serper.

### 5. Findings Normalization
Transforms raw search responses into a standardized finding schema.

### 6. Findings Analysis
Uses a second Gemini-powered AI agent to analyze aggregated findings and draft a structured OSINT report.

### 7. Reporting & Response
Builds a report mapping object, generates a reviewer-friendly Google Doc, and returns the final API response.

---

## Architecture Diagram

```text
Webhook Intake
   │
   ▼
Normalize Request
   │
   ▼
Validate Required Inputs
   ├── Invalid ──> Build Error Response ──> Respond to Webhook (400)
   │
   └── Valid
        │
        ▼
Prepare Screening Context
        │
        ▼
Build Search Inputs
        │
        ▼
AI Search Planner Agent (Gemini)
        │
        ▼
Parse Search Plan
        │
        ▼
Build Execution Queue
        │
        ▼
Expand Queue Items
        │
        ▼
Web Search API (Serper)
        │
        ▼
Normalize Findings
        │
        ▼
Aggregate Findings For Analysis
        │
        ▼
AI Findings Analyst Agent (Gemini)
        │
        ▼
Parse Report Draft
        │
        ▼
Build Word Mapping
        │
        ▼
Build Document Text
        │
        ├──> Create Google Doc
        │        │
        │        ▼
        │   Merge Doc Metadata + Text
        │        │
        │        ▼
        │   Update Google Doc
        │
        ▼
Build Final Response
        │
        ▼
Respond to Webhook (200)
