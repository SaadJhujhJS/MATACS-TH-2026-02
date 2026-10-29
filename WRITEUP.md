## 1. AI vs. rules

**AI is used for two things only:**
- Extracting facts from free text (licence, price, weeks, warranty years, named lead, safety plan, and so on).
- Optionally rewording an already-calculated score card into one sentence.

**Plain code or rules handle everything else:**
- File checks, OCR routing and hashing.
- Loading the admin's criteria.
- Checking that quotes exist in the source.
- Pass/fail gating.
- Scoring formulas, weights, ranking and tie-breaks.

**Why the line is here:** language models are good at reading and weak at repeatable arithmetic. A decision that affects a contract has to be repeatable and auditable, so nothing that decides is left to the model.

## 2. Consistency and explainability

- The model runs at temperature 0, with a fixed JSON schema and a pinned model and prompt version.
- Extracted facts are stored. Re-ranking reuses them, so scoring is pure maths. Same proposals plus same criteria version gives the same ranking, every time.
- If a fresh extraction is forced, it runs twice and any field where the runs differ is flagged for a person.
- Every proposal gets a score card: criterion, extracted value, exact quote, page, formula and points. A reviewer can trace any rank back to evidence in about a minute.
- The tool says "stated", not "verified". Claims such as the licence are checked by a person on the official register for the top candidates.

## 3. Human in the loop

Four checkpoints:
1. The reviewer confirms the category and criteria version before the run.
2. The reviewer resolves flagged items: low confidence, missing or conditional answers.
3. The reviewer reviews every ineligible and flagged proposal, because a wrong disqualification is the costliest error.
4. The reviewer makes the final award decision. They can confirm or override the ranking, and an override needs a written reason.

**Audit log:** user, timestamps, file hashes, criteria version, model and prompt versions, raw extractions, quotes, flags, scores, rank, overrides with reasons, and the final decision.

## 4. Changing criteria

Criteria are versioned and each result is tied to the version that produced it, so an edit never silently changes past results.
- **Weights or formulas changed:** the facts are still valid. Only scoring re-runs, which is cheap and deterministic. The old and new rankings are shown side by side.
- **Criterion added:** only the new field is extracted for existing proposals.
- **Criterion removed or redefined:** affected fields are re-extracted or re-gated.
- Old runs stay in history, marked superseded, so audits still work. The reviewer must re-confirm before using a new run.

## 5. Risks and guards

| Risk | Guard |
|---|---|
| Wrong extraction | Verbatim quote check, type checks, confidence flags, review queue. |
| Made-up facts | "Not stated" is a valid answer. A value without a quote is rejected. |
| Bias | The scoring step uses facts, not company names. No criterion uses size, location or writing style. |
| Manipulation (prompt injection, marketing claims) | Proposal text is data, not instructions. Suspicious text is logged. Marketing claims are ignored and only numbers and stated facts count. |
| Unverified claims | Output is labelled "stated". The winner's licence and insurance are checked by a person. |
| Wrong disqualification | Ambiguous cases are flagged, not failed. Humans review all ineligible bids. |
| Cost | Cheaper model for extraction, page limits, caching, no re-extraction on weight changes. |
| Over-reliance | Output is labelled a recommendation. Flagged items appear prominently. Overrides are easy and logged. |