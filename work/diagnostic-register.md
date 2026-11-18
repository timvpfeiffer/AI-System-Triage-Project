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
- **Immediate containment:** Remove the assistant's access to **all HR-only
  documents** (widened in S6 from compensation docs only). Keep the assistant
  running for policy Q&A and task view. Notify the DPO the same day. If the breach is notifiable, GDPR
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
| 1 | **H1: Identity / role mapping (TB1).** Sam's account resolved to a role with HR or compensation access, e.g. a wrong SSO group or new-hire provisioning template. *(team rank)* | A correctly working filter would *correctly* allow an HR-only doc if the role says "HR". This explains the citation without any retrieval bug. | No identity or provisioning change is known (the only known change is prompt + model). Day-4 accounts usually have minimal rights, and a wrong role would probably show up in other HR tools too. | Look up Sam's resolved role/groups at request time (SSO); re-run the question as a fresh standard new-hire test account. | Low (one screenshot only) |
| 2 | **H2: Retrieval permission filter not applied (TB3).** The role filter is missing, bypassed or not applied to the HR-only index. One possibility: the 2026-11-05 release changed which indexes are queried. *(team rank)* | The HR-only doc was retrieved; which index it came from is **unknown** (corrected in S5, P3). The timing fits a release one week earlier. | The filter is our *assumption*, not known. The release is described as prompt + model, which may not touch retrieval config (C2). | Diff of the 2026-11-05 release (orchestration/retrieval config); request trace: indexes queried + filter parameters; re-run as a test new-hire account. | Low (one screenshot only) |
| 3 | **H3: Ingestion / labelling error (TB2).** `Compensation_Overview_Analytics_2026.pdf` (synthetic) was labelled or routed as public and sits in the public policy index. *(team rank)* | With a wrong label, even a correct role filter lets the doc through. Mislabelling is a common, simple failure. | No recent document upload is known (the team rejected "new doc batch" in S2). The file name makes its sensitivity obvious, so a misroute is less likely to go unnoticed. | Check the doc's index location, access label and ingestion record (date, uploader). | Low (one screenshot only) |

All three sit at different trust boundaries, so they are materially different
(handout §4).

**Also considered (from the S5 panel review, not in the top 3 by team
decision):** H4 cross-user state via a response cache (TB6); H5 permission
enforced only by a prompt instruction, weakened by the 11-05 update (TB5). The
same trace would reveal both. Lesson 04's "sensitive logs" and "unsafe tool
output" were set aside: they don't explain data reaching Sam.

## Next-Best Evidence

- **The next observation or test we would request:** The trace/log of Sam's
  2026-11-12 request, showing (a) Sam's resolved role, (b) which index each
  retrieved passage came from, (c) which filter was applied, (d) whether the
  answer was a cache hit, (e) the prompt + model version used. If logs don't hold
  this (unknown, see S3), fall back to three quick checks: Sam's SSO role,
  the doc's index + label, and a re-run as a fresh new-hire test account.
- **Why it best separates the leading hypotheses:** One record answers all three:
  wrong role → H1; doc retrieved from the HR-only index under a correct new-hire
  role → H2; doc retrieved from the public index → H3. It also checks the
  "also considered" causes (cache hit → H4; only a prompt rule → H5).
- **Who can provide or run it:** People-Tech engineering (trace, release diff,
  index); IT / identity team (SSO role); HR content owner (doc label).
- **Decision that depends on it:** Where to correct, and how wide to contain.
  H1 → fix provisioning and audit all new-hire roles. H2 → keep restricted mode
  or roll back the release. H3 → audit the labels of all HR-only docs.

## AI Challenge Note

Panel review on 2026-11-18 (AI acting as the handout §5 skeptical review panel).

| LLM suggestion | How it was checked | Accepted, edited, or rejected | Reason |
|---|---|---|---|
| P1: Missed cause: cross-user state via response cache | Checked against lesson 04's privacy-leak row ("cross-user state") and our map (no cache shown) | **Accepted**, then moved to "also considered" | Plausible and distinct; team limited the main set to 3 |
| P2: Prompt-only control (TB5) was excluded only by the S3 filter assumption, yet it best fits the known change | Checked against the S3 assumption (marked A, unverified) and lesson 03 | **Accepted**, then moved to "also considered" | Our exclusion rested on an assumption; team limited the main set to 3 |
| P3: H2 wording "so the HR-only index was searched" overstates an inference (AI's own earlier wording) | Compared with the frozen facts: only the citation is observed | **Accepted** → H2 wording fixed | Source index is exactly what H3 asks |
| P4: Containment only covers H2/H3; disconnect the HR-only index, clear caches, check Sam's role | Compared the containment action with H1/H4 | **Rejected** in S5, **partly adopted** in S6 | In S6 the team cut off all HR-only docs (covers H1); cache clearing not adopted (C7) |
| P5: "Jordan M. *in your team*" suggests a team/profile data path missing from the map | Checked the system map: no such path shown | **Rejected** | Team decision; not added to the map |

## Current Diagnosis

- **Verified cause, if any:** None. We only have one synthetic screenshot.
- **Best-supported hypothesis if not verified:** No hypothesis is clearly
  better supported yet. The team ranks H1 (role mapping) first, then H2 and H3.
- **Important uncertainty:** Whether the failure reproduces, whether logs
  contain retrieval details, and how many other users were exposed.
- **Evidence that would change the diagnosis:** The request trace or the three
  fallback checks above. A re-run that leaks under a clean new-hire account
  would weaken H1 and point to H2/H3.
