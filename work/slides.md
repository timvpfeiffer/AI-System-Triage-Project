# HR-Onboarding Assistant: Privacy Incident

*All case details are synthetic.*

---

## Slide 1 · What Is Broken

**System:** read-only assistant for new hires. It answers HR policy questions and shows their own onboarding tasks.

**What happened (2026-11-12):**
- New hire asked: *"What is the salary band for my role?"*
- Answer included **a colleague's actual salary + bonus** and cited an **HR-only** document

**Impact:** High. Personal pay data exposed; possible GDPR breach (the DPO assesses).

**Contained:** the assistant keeps running for policy Q&A; **all HR-only documents are cut off**.

**Unknown:** whether it reproduces, who else was affected, how the document became reachable.

> **Speaker notes:** The new hire reported it with a screenshot the next day. That screenshot is our only evidence. The only known change is a prompt + model update on 2026-11-05. We treat it as a lead, not as the cause.

---

## Slide 2 · Why We Think It Is Broken

| # | Hypothesis | Where |
|---|---|---|
| 1 | Wrong role: new hire treated as HR, so the filter correctly let it through | Login → role |
| 2 | Permission filter not applied to HR-only documents | Retrieval |
| 3 | Document labelled/stored as public | Ingestion |

All **Low confidence**. Also considered: answer cache, prompt-only control.

**Next check:** the trace of the failing request (role · source index · filter). **One record separates all three.**

> **Speaker notes:** The ranking is our team judgment; no evidence favours one yet. The architecture (role filter, separate indexes) is assumed, not known. An AI review panel added the cache and prompt-only ideas; we kept them as alternatives the same trace will check.

---

## Slide 3 · What We Recommend

**Decision: Contain + Investigate.** No model change.

1. **Contain:** cut HR-only documents; DPO assesses
2. **Investigate (48 h):** trace + check role, filter, document label
3. **Correct:** fix only the confirmed boundary
4. **Prevent:** zero-leak test set in every release; log role, index and filter

**Proof:** Sam's question + 20 restricted-data probes → **0 leaks**; public band still answered.
**Owner:** HR product owner (+ DPO). **Rollback:** any leak → cut HR data again or pause.
**Would change our mind:** a leak with HR-only documents disconnected → pause the assistant.

> **Speaker notes:** We rejected "switch to a stronger model": a model can't enforce permissions. The fix is a permission or data fix, not an AI fix.
