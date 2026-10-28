# AI Proposal Triage — Design Write-up

## 1. AI vs. Rules

- **AI:** Extract structured information from proposals, such as price, experience, timelines, warranties, and licensing details, with supporting evidence from the original documents.
- **Plain code:** Validate extracted data, apply mandatory requirements, check budget and timeline constraints, and calculate scores and rankings using administrator-configured criteria.
- **Human review:** Resolve missing or conflicting information and make the final procurement decision.
- **Why this separation:** AI is suited to interpreting unstructured documents, while code ensures eligibility checks and calculations are consistent, repeatable, and auditable. Evidence helps reviewers verify extracted facts, and uncertain cases are flagged rather than decided through unsupported AI assumptions.

## 2. Consistency and Explainability

- **Consistency:** Apply the same configured criteria, scoring rubrics, and weights to the same validated proposal information so repeated evaluations produce consistent results.
- **Weighted scoring:** Assign each criterion a score from 0–100 and calculate the weighted total:

  `Total = (Experience × 0.30) + (Price × 0.25) + (Timeline × 0.15) + (Warranties × 0.15) + (Team & Approach × 0.15)`

- **Experience (30%):** Award 20 points per qualifying commercial installation of at least 100 kW completed within the last five years, capped at 100. Count only installations supported by sufficient evidence.
- **Price (25%):** Calculate `(lowest qualifying comparable price / proposal price) × 100` using comparable total prices and the configured budget policy.
- **Timeline (15%):** Use a fixed, administrator-configured scale: 8 weeks or less = 100; 9–10 weeks = 90; 11–12 weeks = 80; 13–14 weeks = 70; and 15–16 weeks = 60. Durations exceeding 16 weeks follow the configured feasibility policy. Unknown or unclear timelines are flagged rather than scored.
- **Warranties (15%):** Average the panel warranty and workmanship warranty scores using separate configurable duration scales. The proposed panel warranty scale is 25+ years = 100; 20–24 = 80; 15–19 = 60; 10–14 = 40; and below 10 = 20. The proposed workmanship warranty scale is 12+ years = 100; 10–11 = 80; 7–9 = 60; 4–6 = 40; and below 4 = 20.
- **Team & Approach (15%):** Average four equally weighted indicators: named project lead, crew, safety plan, and monitoring plan. Each receives 100 when adequately supported or 0 when explicitly confirmed absent. Missing or unclear information remains unresolved rather than automatically receiving zero.
- **Scoring safeguards:** These scoring rubrics are proposed design choices, not values specified by the assessment brief. Administrators can configure the criteria, weights, and thresholds. Missing or ambiguous information must not be invented or silently treated as a pass or failure. If unresolved information prevents a defensible overall score, flag the proposal and leave its score `null` until resolved.
- **Explainability:** Show each criterion score, weighted total, rank where applicable, evaluation status, and a concise reason supported by proposal evidence.
- **Evaluation history:** Record the criteria, weights, scoring rubrics, validated proposal information, evidence, and results used for each evaluation. When configuration changes, preserve previous results and create a new evaluation.
- **Reliability:** Validate extracted information before scoring. Reviewers can correct extraction errors, with corrections and overrides recorded in the audit log.

## 3. Human in the Loop

- **Review points:** Reviewers check proposals flagged for missing or conflicting information, uncertain eligibility, or possible extraction errors. They can verify evidence, correct extracted facts, request clarification, and confirm or override results with a recorded reason.
- **Final decision:** Reviewers examine the ranked proposals and their explanations before making the final procurement decision. The ranking supports the decision but does not automatically award the contract.
- **Audit log:** Record the proposal ID, extracted information and evidence, eligibility results, criterion scores and ranking, the configuration used, reviewer corrections or overrides with reasons, and the final decision with the responsible reviewer and timestamp.
- **Traceability:** Preserve the original evaluation alongside revised evaluations so changes can be understood and audited later.

## 4. Changing Criteria After Evaluation

- Store evaluation criteria, mandatory requirements, scoring rubrics, and weights as administrator-managed configuration rather than hard-coding them into the application.
- Validate changes before saving, including checking that weights total 100% and the configuration is complete and valid.
- When criteria or weights change, create a new evaluation using the updated configuration. Recalculate affected eligibility results, scores, and rankings as necessary.
- Preserve previous evaluation results and record which configuration was used for each evaluation so reviewers can understand and compare changes.
- If a new or modified requirement needs information that was not previously extracted, update the extraction schema or prompt and obtain the necessary evidence before reevaluating affected proposals.
- **Eligibility and ranking:** Check mandatory requirements before ranking. Mark confirmed mandatory failures as `ineligible` and unresolved requirements as `flagged` for human review. Only proposals that satisfy mandatory requirements and have defensible scores enter the ranked list. Budget and timeline violations follow the explicitly configured feasibility policy.

## 5. Risks and Safeguards

- **Incorrect extraction:** AI may misread prices, warranties, or licence details. Retain supporting evidence, validate extracted values, and allow reviewers to correct errors before evaluation.
- **Hallucinated information:** AI may invent missing details. Require evidence from the proposal and represent unsupported or missing information as unknown rather than guessing.
- **Prompt injection and manipulation:** A proposal may contain instructions telling the AI to favour a bidder. Treat document contents as untrusted data, ignore embedded instructions, and apply eligibility checks and scoring through separate rules and code. Suspicious instructions alone do not automatically disqualify a proposal.
- **Bias and inconsistent evaluation:** AI may interpret similar proposals differently. Use consistent extraction fields, common evaluation criteria, and the same configured weights and rules for all bidders.
- **Incorrect or unfair rankings:** Missing information or incorrect configuration may affect results. Validate inputs and configuration, flag uncertain cases, and require human review before the final procurement decision.
- **Cost and performance:** Processing long documents with AI may be expensive or slow. Extract only relevant information, avoid unnecessary repeated processing, and reuse validated extraction results where appropriate.