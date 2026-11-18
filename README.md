# AI System Triage: HR-Onboarding Assistant Privacy Failure

Our team's work on the **06 Session Handout** ([06-session-handout.md](06-session-handout.md)):
diagnose one failing GenAI system and defend a prioritized recovery plan.

## Our Case

| | |
|---|---|
| **System** | HR-onboarding assistant: guides new hires through tasks and policies. Uses conversation state, HR documents, task integration and sensitive employee data. |
| **Failure** | Privacy or permission failure: the assistant shows information the user should not have access to. |
| **Team** | 3 people working together on every step |

## Deliverables

| Deliverable | File | Role |
|---|---|---|
| **Presentation (HTML)** | [work/presentation.html](work/presentation.html) | **Main deliverable**: open in a browser, ← → to navigate, N for notes |
| Slide content (Markdown) | [work/slides.md](work/slides.md) | Text and speaker notes of the presentation |
| System & trust-boundary map | [work/system-map.md](work/system-map.md) | Feeds slides 1–2 |
| Diagnostic register | [work/diagnostic-register.md](work/diagnostic-register.md) | Feeds slides 1–2 |
| Prioritized recovery plan | [work/recovery-plan.md](work/recovery-plan.md) | Feeds slide 3 |
| Decision protocol | [work/protocol.md](work/protocol.md) | What we decided and why |
| Open challenges | [work/challenges.md](work/challenges.md) | Critical points to resolve in the final check (S9) |
| Backlog | [backlog.md](backlog.md) | All steps as tasks with status |
| Handover | [HANDOVER.md](HANDOVER.md) | Start here in a new session |

The slides answer three questions:
1. **Set the scene:** what does the system normally do, and what went wrong?
2. **Our thinking:** what are the most plausible explanations, and what would we check?
3. **Recommendation:** what do we do next, and how would we know it helped?

## How to Continue

1. Read [HANDOVER.md](HANDOVER.md): current status, working prompt and the exact next action.
1. Open [backlog.md](backlog.md) and find the first task that is not **Done**.
2. Read [work/protocol.md](work/protocol.md) for the decisions made so far, and [work/challenges.md](work/challenges.md) for open challenges.
3. In a new AI chat, ask it to read both files plus the handout, then continue from that task.

## Ground Rules

- Label everything as **fact**, **synthetic**, **assumption** or **unknown**. Synthetic details stay stable once set.
- Use synthetic data only, never real employee data.
- Never claim a root cause is verified. We only have synthetic evidence.
- The AI suggests and challenges; the team decides.

## Background Material

| File | Useful for |
|---|---|
| [01 - Anatomy of a GenAI System](01-anatomy-of-a-genai-system.md) | System components (S1, S3) |
| [02 - Context, Retrieval, Memory, and Adaptation](02-context-retrieval-memory-and-adaptation.md) | Retrieval, permission filters, session memory (S3, S4) |
| [03 - Tools, Workflows, and Trust Boundaries](03-tools-workflows-and-trust-boundaries.md) | Trust boundaries, tool permissions (S3, S4) |
| [04 - Diagnose AI System Failures](04-diagnose-ai-system-failures.md) | Hypotheses and tests (S4, S5) |
| [05 - Evaluation, Observability, and Governance](05-evaluation-observability-and-governance.md) | Evaluation, monitoring, ownership (S6, S7) |
| [templates/](templates/) | Blank originals of the working templates |

The original course README is in git history (commit `070a1ea`).
