# Prioritized Recovery Plan

> Case details are **synthetic**. Hypotheses H1–H3: see
> [diagnostic-register.md](diagnostic-register.md). No cause is verified.

## Decision

- **Recommendation:** **Contain + Investigate.** Keep the assistant running for
  policy Q&A and remove its access to all HR-only documents. Correct only after
  the evidence confirms the cause.
- **Decision owner:** HR product owner (People-Tech), with the DPO for the
  breach assessment.
- **Reason in one sentence:** All three hypotheses have Low confidence, and
  cutting off HR-only documents stops the leak path under each of them while
  keeping the service useful.
- **Evidence that would change the recommendation:** A leak in the test set
  *after* HR-only documents are cut off (points to a cache or other data path
  → pause the assistant). The trace showing a wrong role (H1) widens the fix to
  all new-hire accounts.

## Actions

| Priority | Horizon | Action | Linked cause or unknown | Impact | Effort | Risk reduction | Confidence | Reversibility | Owner |
|---|---|---|---|---|---|---|---|---|---|
| 1 | Contain | Disconnect all HR-only documents from the assistant; keep policy Q&A; DPO assesses the breach | H1–H3 (all) | High | Low | Medium | High | High | HR product owner + DPO |
| 2 | Investigate | Within 48 h: pull the trace of Sam's request; check SSO role (H1), filter config (H2), doc label/index (H3) | Which of H1–H3 | Medium | Low | Low (alone) | High | High | People-Tech engineering lead |
| 3 | Correct | Fix only the confirmed boundary: role mapping, retrieval filter, or doc label. No model change. | Confirmed cause | High | Medium | High | Low until #2 is done | Medium | Owner of the confirmed boundary (IT / People-Tech / HR content) |
| 4 | Prevent | Zero-leak test set before every release; log role, source index and filter per request | Recurrence; observability gap | High | Medium | High | Medium | High | HR product owner |

## Options Considered

| Option | Why it could help | Why not first? | Reversible? |
|---|---|---|---|
| Workflow or non-ML change: cut HR-only docs, HR reviews labels at upload | Stops the leak immediately; cheap | **Chosen** as containment; HR upload review only covers H3 | Yes |
| Small architecture correction: fix role mapping / filter / label | Removes the actual cause | Needs the trace first; we don't know which boundary | Mostly |
| Larger model, data, or platform change: stronger model, fine-tuning | Might follow privacy instructions better | **Rejected:** a model can't enforce permissions (lesson 03: "a system prompt is an instruction, not an authorization boundary") | Costly |

## Proof of Recovery

| Level | Measure or test | Baseline | Pass or guardrail threshold | Owner |
|---|---|---|---|---|
| User or workflow | Public salary band still answered for new hires | Answered (with leak) | ≥ 95% of policy test questions answered correctly | HR product owner |
| Retrieval or context | Restricted passages retrieved for new-hire roles (from trace/logs) | ≥ 1 (Sam's case) | 0 | People-Tech engineering |
| Generation or action | Zero-leak test set: Sam's original question + 20 restricted-data probes across roles (salary, contract, health, performance, private contacts, HR-only docs) | 1 leak observed | **0 leaks** | People-Tech engineering |
| Operations | Role, source index and filter logged per request | Unknown | 100% of requests | People-Tech engineering |
| Risk | Privacy incidents reported by users or found in log review | 1 | 0; DPO review weekly for 4 weeks | DPO |

## Release and Rollback

- **Release scope:** Re-enable HR-only documents only for roles that may see
  them, and only after the fix passes the zero-leak test set.
- **Monitoring required:** Weekly DPO log review for 4 weeks; zero-leak test set
  in every release pipeline.
- **Rollback or shutdown trigger:** Any single leak in tests or production →
  disconnect HR data again; a leak *without* HR-only docs connected → pause the
  assistant.
- **Fallback experience:** The HR inbox for questions the assistant can't answer.
- **Remaining risk and authorized risk owner:** A possible answer cache (H4) or
  prompt-only control (H5) is not yet ruled out; the trace will show both.
  Risk accepted by the HR product owner, with the DPO's assessment.
