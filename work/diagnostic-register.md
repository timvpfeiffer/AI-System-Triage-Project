# Diagnostic Register

## Incident Frame

> All case details below are **synthetic** (invented for this exercise) and frozen.
> Names, numbers and documents are fictional.

- **Observed symptom:** A new hire asked for the salary band of their role. The
  assistant answered with a named colleague's actual pay and cited an HR-only
  document.
- **Reproducible example:**
  - *Request (new hire "Sam K.", Junior Data Analyst, day 4):*
    "What is the salary band for my role?"
  - *Response:* "The band for Junior Data Analyst is €52–64k. For reference,
    Jordan M. in your team earns €61,500 plus an 8% bonus."
    Source cited: `Compensation_Overview_Analytics_2026.pdf` (HR-only).
  - *Expected:* The published salary band only, or a referral to HR, with **no
    individual employee's pay** and no HR-only document cited.
  - *Reproducible?* **Unknown**. It hasn't been re-tested yet.
- **Affected users or workflow:** Directly: Sam K. (saw the data) and Jordan M.
  (data exposed). Possibly other new hires who asked similar questions, which
  is **unknown**.
- **Impact and severity:** **High.** This is personal compensation data shown
  to an unauthorized employee, so it is a potential GDPR personal-data breach
  (the DPO must assess it). It damages trust in the assistant and in HR.
- **First observed:** Answer given on 2026-11-12. Sam reported it to HR with a
  screenshot on 2026-11-13.
- **Recent relevant changes:** On 2026-11-05, the system prompt and model
  version were updated. *(Known change. It is a lead, not a proven cause.)*
- **Immediate containment:** Remove the compensation documents from the
  assistant's knowledge base. Keep the assistant running for policy Q&A and
  task view. Notify the DPO the same day. If the breach is notifiable, GDPR
  gives 72 hours from awareness to inform the supervisory authority
  *(the DPO verifies this; not a legal conclusion by us)*.
- **Facts frozen for this case:**
  1. The request and response above (screenshot)
  2. Expected behaviour: published band only, no individual's data
  3. Affected: Sam K. saw Jordan M.'s pay; any wider exposure is unknown
  4. When: 2026-11-12 (reported 2026-11-13)
  5. Recent change: prompt + model update on 2026-11-05
- **Explicitly unknown:** Whether the failure reproduces; how many other users
  saw restricted data; how the HR-only document became retrievable; whether
  the assistant ever enforced document permissions, and where (retrieval
  filter, prompt instruction or something else); whether logs of the exchange
  exist.

## Evidence Available

| Evidence | Source | What it shows | Limitation | Observed, inferred, or verified? |
|---|---|---|---|---|
| Screenshot of the exchange | Sam K. via HR | Restricted salary data plus a citation of an HR-only doc reached the user | One case; no system internals | Observed (**synthetic**) |
| Change record: prompt + model update, 2026-11-05 | Release notes (People-Tech) | Something changed one week before the incident | Timing only; no link to the cause shown | Observed (**synthetic**) |

Do not mark a cause as verified unless the available evidence distinguishes it
from credible alternatives. Use **synthetic** for invented exercise evidence.

## Competing Hypotheses

| Rank | Hypothesis and component | Why it fits | Evidence against it | Next discriminating test | Confidence |
|---|---|---|---|---|---|
| 1 |  |  |  |  | Low / Medium / High |
| 2 |  |  |  |  | Low / Medium / High |
| 3 |  |  |  |  | Low / Medium / High |

## Next-Best Evidence

- **The next observation or test we would request:**
- **Why it best separates the leading hypotheses:**
- **Who can provide or run it:**
- **Decision that depends on it:**

## AI Challenge Note

| LLM suggestion | How it was checked | Accepted, edited, or rejected | Reason |
|---|---|---|---|
|  |  |  |  |

## Current Diagnosis

- **Verified cause, if any:**
- **Best-supported hypothesis if not verified:**
- **Important uncertainty:**
- **Evidence that would change the diagnosis:**
