# AI Proposal Triage Design

**Candidate:** Muhammad Muzzamil Siddiqui  
**Assessment Reference:** MATACS-TH-2026-02  
**Assessment:** AI Proposal Triage Design  
**Time Spent:** Approximately 6 hours and 50 minutes, including breaks between work sessions.

## Overview

This repository contains my design for an AI-assisted contractor proposal evaluation system. The design separates AI-based information extraction from deterministic backend validation, eligibility checks, scoring, and ranking, with human review for uncertain cases and final procurement decisions.

## Repository Contents

- [Design Write-up](WRITEUP.md) — Architecture decisions, scoring approach, human oversight, risks, and handling changing requirements.
- [AI Extraction Prompt](PROMPTS.md) — The extraction prompt, expected output structure, backend processing approach, and design rationale.
- [Expected Ranking](ranking.json) — Expected ranking results, proposal statuses, scores where defensible, and explanations.
- [System Workflow Diagram](diagram/system-workflow.png) — End-to-end process from administrator configuration and proposal upload to evaluation and human review.
- [AI Extraction and Validation Diagram](diagram/ai_extraction_validation.png) — Details of the extraction, validation, and human-review process.

## AI Tool Usage

I used ChatGPT to discuss the system design, refine the extraction prompt, and develop a proposed scoring approach. I reviewed its suggestions rather than accepting them automatically: for example, I disagreed with marking Proposal B ineligible based only on the missing evidence of the bidder's own licence, and chose to flag it for human verification because the proposal does not establish whether the bidder holds that licence. I also checked the proposed ranking against the documented scoring rules and corrected inconsistencies during the process.

## Design Principles

- Extract facts with supporting evidence; do not invent missing information.
- Keep eligibility decisions, scoring, and ranking in deterministic backend logic.
- Treat proposal documents as untrusted input and ignore embedded instructions attempting to manipulate evaluation.
- Flag uncertainty for human review and preserve an auditable evaluation history.
- Keep evaluation criteria, scoring rubrics, and weights administrator-configurable.


### Optional Prototype

The prototype demonstrates AI-assisted extraction using the assessment's proposal extraction prompt and the Gemini API. It reads a local proposal text file, requests structured JSON containing extracted facts, evidence, and uncertainty statuses, performs basic response validation, and saves the result locally.

**Run locally:**

1. Install the dependencies listed in `prototype/requirements.txt`.
2. Set `GEMINI_API_KEY` in `prototype/.env`.
3. Place a proposal text file in `prototype/samples/`.
4. From the `prototype/` directory, run:

   `python run_extraction.py samples/A.txt`

The prototype demonstrates extraction only. It does not independently verify contractor claims or make eligibility, scoring, or ranking decisions. Sample documents and generated extraction outputs are excluded from the repository.

