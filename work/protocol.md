# Decision Protocol

Log of our team decisions, one row per decision. We add rows after each step.
Labels: **fact** (frozen case fact), **synthetic** (invented for the exercise and kept stable), **assumption**, **unknown**.

| Date | Step | Decision | Why | Rejected alternative |
|---|---|---|---|---|
| 2026-11-17 | Setup | Case: HR-onboarding assistant × privacy/permission failure | Team choice | – |
| 2026-11-17 | Setup | Main deliverable: 3 minimal slides; templates as supporting work | Presentation is what we deliver | Full-depth templates first (too slow for 1–2 h) |
| 2026-11-17 | S1 | Primary user: new hire (first ~30 days) | The new hire is the one exposed to data they shouldn't see | HR admin, hiring manager |
| 2026-11-17 | S1 | Scope: answer policy questions + show own onboarding tasks; read-only | Personal data flows through the system, so a leak is plausible; no actions keeps the focus on privacy | Answer only (no personal data in flow); answer + act (adds an unsafe-action angle) |
| 2026-11-17 | S1 | Owner: HR product owner (People-Tech); affected: new hires, exposed employees, HR, DPO | One clear business owner for policy and data decisions | Head of HR (split ownership), IT only (no business owner) |
| 2026-11-17 | S1 | Never do: return anything unauthorized under HR access rules (with concrete examples as test anchors) | Covers all leak types | Narrow salary/contract-only rule (covers too little) |
| 2026-11-17 | S2 | Case: "salary band" question → answer exposes colleague Jordan M.'s pay, citing HR-only doc (synthetic) | Highly sensitive, concrete, testable | Other new hire's tasks (state/scoping); buddy's private details (directory over-fetch) |
| 2026-11-17 | S2 | Known recent change: prompt + model update on 2026-11-05 | Team choice; a realistic lead that must not be treated as the cause | New HR doc batch; retrieval index migration; no known change |
| 2026-11-17 | S2 | Detection: new hire reported it with a screenshot; wider exposure unknown | Realistic; we invent no logs | DPO audit (implies logs exist); Slack post |
| 2026-11-17 | S2 | Containment: remove compensation docs from knowledge base, keep policy Q&A running, notify DPO same day | Fast, reversible, keeps the service usable; GDPR 72 h clock | Pause whole assistant (bigger workflow impact); policy-only mode (config may not exist) |
| 2026-11-17 | S2 | Challenges go to work/challenges.md and are reviewed together in S9 | Keeps momentum; nothing gets lost | Resolve each challenge immediately |
| 2026-11-17 | S3 | Assumption: permissions enforced by a role filter at retrieval | Team choice; common design | Mark as unknown; prompt instruction only |
| 2026-11-17 | S3 | Assumption: separate public-policy and HR-only indexes | Team choice; makes "how did the doc cross over?" the key question | One shared index; unknown |
| 2026-11-17 | S3 | Assumption: per-session chat history only, no long-term memory | Keeps the map simple; cross-user leak stays testable | Persistent profile; unknown |
| 2026-11-17 | S3 | Assumption: basic request logs exist; whether passages/filter decisions are logged is unknown | Makes trace data our key evidence request without inventing it | Full traces; no logs |
| 2026-11-17 | Stop | Session 1 ends after S3; HANDOVER.md created for the next session | Chat is temporary; another teammate may continue | – |
| 2026-11-18 | S4 | Hypotheses: H1 role mapping (TB1) rank 1, H2 retrieval filter not applied (TB3) rank 2, H3 ingestion/labelling (TB2) rank 3 | Team judgment; three different trust boundaries | Prompt-only control (TB5), cross-user session state, sensitive logs, unsafe tool output |
| 2026-11-18 | S4 | Next-best evidence: trace of Sam's request (role, source index, filter); fallback: SSO role + doc label/index + re-run as new-hire test account | One record separates all three hypotheses | Start with the release diff (only tests H2) |
| 2026-11-18 | S4 | Confidence Low for all three; no verified cause | Only one synthetic screenshot as evidence | – |
| 2026-11-18 | S5 | Accept P1: add H4 cross-user state via response cache (TB6) | Plausible, distinct, testable via the same trace | Note only; reject |
| 2026-11-18 | S5 | Accept P2: add H5 prompt-only permission control (TB5) | Our exclusion rested on an unverified S3 assumption; best fit to the known change | Merge into H2; reject |
| 2026-11-18 | S5 | Accept P3: fix the AI's overstated H2 wording | Source index is unknown | – |
| 2026-11-18 | S5 | Reject P4: keep the original containment (remove compensation docs, notify DPO) | Team decision | Disconnect HR-only index + clear cache + check role; pause assistant |
| 2026-11-18 | S5 | Reject P5: don't add a team/profile data path to the map | Team decision | Add as architecture unknown |
| 2026-11-18 | S4/S5 | Keep 3 main hypotheses: H1, H2, H3; H4 + H5 listed as "also considered" | Focus for Slide 2 and the time limit | H1, H2, H5; H2, H3, H5 |
| 2026-11-18 | S6 | Decision: keep the assistant running; remove its access to all HR-only documents | Stops the leak path for H1–H3 while keeping policy Q&A available | Contain + investigate with doc removal only; roll back release; pause assistant |
| 2026-11-18 | S7 | Proof: zero-leak test set (Sam's case + 20 restricted-data probes), 0 leaks, public band still answered | Measurable, repeatable | Original case only; + 2-week canary |
| 2026-11-18 | S6 | Actions: 1 cut HR-only docs + DPO, 2 trace + 3 checks (48 h), 3 fix confirmed boundary only, 4 zero-leak tests per release + per-request logging | Non-ML, smallest, reversible; matches Low confidence | Clear cache; re-run test only; stronger model; HR upload review only |
| 2026-11-18 | S8–S9 | Slides drafted in work/slides.md; final check done; challenge statuses proposed for team confirmation | 30-minute time limit | – |
| 2026-11-18 | S8 | Logs exist and are used for error analysis (this request + search for other affected users) | Team decision; logs are the key evidence source | Logs with unknown contents (S3 assumption) |
| 2026-11-18 | S8 | HTML presentation: intro slide + 3 slides, dark editorial style, chat bubble, checkpoint path, icons, notes + timer | Easy to present, minimalist | Clean light; warm neutral; scrollable page |
| 2026-11-18 | S8 | Version 3 (presentation-v3.html + .pptx) built with the external-case-deck skill: 4-slide outline (Option B), takeaway titles, visual headings and captions, Arial in both formats | Coach's 3-slide format; titles alone tell the argument | Option A: 9 slides, one idea per slide |
