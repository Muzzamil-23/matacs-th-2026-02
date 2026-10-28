## 1. AI vs. Rules

- **AI:** Extract structured information from proposals, such as price, experience, timelines, warranties, and licensing details, using supporting evidence from the original documents.
- **Plain code:** Validate extracted data, enforce mandatory requirements, check budget and timeline constraints, and calculate scores and rankings using administrator-configured criteria.
- **Human review:** Resolve missing or conflicting information and make the final procurement decision.
- **Why this separation:** AI is suited to interpreting unstructured documents, while code ensures eligibility checks and calculations are consistent, repeatable, and auditable. Keeping evidence for extracted facts helps reviewers verify results, and uncertain cases are flagged rather than decided through unsupported AI assumptions.

## 2. Consistency and Explainability

## 2. Consistency and Explainability

- **Consistency:** Apply the same configured criteria, scoring rubrics, and weights to the same validated proposal information so repeated evaluations produce consistent results.
- **Scoring:** Assign each criterion a score from 0–100 and calculate the weighted total: `(Experience × 0.30) + (Price × 0.25) + (Timeline × 0.15) + (Warranties × 0.15) + (Team & Approach × 0.15)`.
  - **Experience (30%):** Award 20 points per qualifying commercial installation of at least 100 kW completed within the last five years, capped at 100.
  - **Price (25%):** Calculate `(lowest qualifying price / proposal price) × 100` using comparable total prices and the configured budget policy.
  - **Timeline (15%):** Use a fixed, administrator-configured scale that rewards shorter, supported completion times within the 16-week limit.
  - **Warranties (15%):** Average the panel warranty and workmanship warranty scores, using separate configurable duration scales.
  - **Team & Approach (15%):** Average four equally weighted indicators: named project lead, crew, safety plan, and monitoring plan. Each receives 100 when adequately supported or 0 when explicitly confirmed absent.
- **Scoring safeguards:** These rubrics are proposed design choices, not values specified by the assessment. Administrators can configure them. Missing or ambiguous information is not automatically treated as a failure or assigned an invented value. If a defensible score cannot be calculated, flag the proposal and leave its overall score null until resolved.
- **Explainability:** Show each proposal's overall score, rank where applicable, criterion-level results, and a brief reason supported by evidence from the original proposal.
- **Evaluation history:** Record the criteria, weights, scoring rubrics, proposal information, and results used for each evaluation. If an administrator changes the configuration, preserve previous results and create a new evaluation.
- **Reliability:** Validate extracted information and allow reviewers to correct errors before evaluation, reducing the risk of extraction mistakes affecting rankings.

## 3. Human in the Loop

- **Review points:** A reviewer checks proposals flagged for missing or conflicting information, uncertain eligibility, or possible extraction errors. The reviewer can verify evidence, correct extracted facts, request clarification, and confirm or override a result with a reason.
- **Final decision:** Reviewers examine the ranked proposals and their explanations before making the final procurement decision. The ranking supports the decision but does not automatically award the contract.
- **Audit log:** Record the proposal ID, extracted information and supporting evidence, eligibility results, scores and ranking, criteria and weights used, reviewer corrections or overrides with reasons, and the final decision with the responsible reviewer and timestamp.
- **Traceability:** Preserve the original evaluation alongside any revised evaluation so changes can be understood and audited later.

## 4. Changing Criteria After Evaluation

- Store evaluation criteria, mandatory requirements, and weights as administrator-managed configuration rather than hard-coding them into the application.
- Validate changes before saving, including checking that weights total 100% and that the configuration is complete and valid.
- When criteria or weights change, create a new evaluation using the updated configuration and the validated proposal information. Recalculate affected eligibility results, scores, and rankings as necessary.
- Preserve previous evaluation results and record which configuration was used for each evaluation, so reviewers can understand and compare changes.
- If a new or modified requirement needs information that was not previously extracted, update the extraction process and obtain the missing evidence before reevaluating affected proposals.
- Only proposals that satisfy mandatory requirements and have defensible scores enter the ranked list. Confirmed mandatory failures are marked ineligible, while unresolved requirements are flagged for review. Budget and timeline violations follow the explicitly configured feasibility policy.

## 5. Risks and Safeguards

- **Incorrect extraction:** AI may misread prices, warranties, or licence details. Retain supporting evidence, validate extracted values, and allow reviewers to correct errors before evaluation.
- **Hallucinated information:** AI may invent missing details. Require evidence from the proposal and represent unsupported or missing information as unknown rather than guessing.
- **Prompt injection and manipulation:** A proposal may contain instructions telling AI to favour a bidder. Treat document contents as untrusted data, ignore embedded instructions, and apply eligibility checks and scoring through separate rules and code.
- **Bias and inconsistent evaluation:** AI may interpret similar proposals differently. Use consistent extraction fields, common evaluation criteria, and the same configured weights and rules for all bidders.
- **Incorrect or unfair rankings:** Missing information or incorrect configuration may affect results. Validate inputs and configuration, flag uncertain cases, and require human review before the final procurement decision.
- **Cost and performance:** Processing long documents with AI may be expensive or slow. Extract only relevant information, avoid unnecessary repeated processing, and reuse validated results where appropriate.