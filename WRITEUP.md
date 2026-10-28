## 1. AI vs. Rules

- **AI:** Extract structured information from proposals, such as price, experience, timelines, warranties, and licensing details, using supporting evidence from the original documents.
- **Plain code:** Validate extracted data, enforce mandatory requirements, check budget and timeline constraints, and calculate scores and rankings using administrator-configured criteria.
- **Human review:** Resolve missing or conflicting information and make the final procurement decision.
- **Why this separation:** AI is suited to interpreting unstructured documents, while code ensures eligibility checks and calculations are consistent, repeatable, and auditable. Keeping evidence for extracted facts helps reviewers verify results, and uncertain cases are flagged rather than decided through unsupported AI assumptions.

## 2. Consistency and Explainability

- **Consistency:** Apply the same configured criteria and weights to the same validated proposal information so repeated evaluations produce consistent results.
- **Explainability:** Show each proposal's overall score, rank, results for individual criteria, and a brief reason supported by evidence from the original proposal.
- **Evaluation history:** Record the criteria, weights, proposal information, and results used for each evaluation. If an administrator changes the criteria or weights, preserve the previous results and create a new evaluation.
- **Reliability:** Validate extracted information and allow reviewers to correct errors before evaluation. This reduces the risk of AI extraction mistakes affecting rankings.

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

## 5. Risks and Safeguards

- **Incorrect extraction:** AI may misread prices, warranties, or licence details. Retain supporting evidence, validate extracted values, and allow reviewers to correct errors before evaluation.
- **Hallucinated information:** AI may invent missing details. Require evidence from the proposal and represent unsupported or missing information as unknown rather than guessing.
- **Prompt injection and manipulation:** A proposal may contain instructions telling AI to favour a bidder. Treat document contents as untrusted data, ignore embedded instructions, and apply eligibility checks and scoring through separate rules and code.
- **Bias and inconsistent evaluation:** AI may interpret similar proposals differently. Use consistent extraction fields, common evaluation criteria, and the same configured weights and rules for all bidders.
- **Incorrect or unfair rankings:** Missing information or incorrect configuration may affect results. Validate inputs and configuration, flag uncertain cases, and require human review before the final procurement decision.
- **Cost and performance:** Processing long documents with AI may be expensive or slow. Extract only relevant information, avoid unnecessary repeated processing, and reuse validated results where appropriate.