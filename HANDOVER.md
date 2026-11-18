# Handover: 06 Assignment (HR-Onboarding Assistant × Privacy Failure)

**Last updated:** 2026-11-18, session 2. **Status:** S0–S9 done; **only team review left (see section 6).**

## 1. How to Use This File

Start a new Claude Code session in this repo and paste:

```text
Read HANDOVER.md first, then backlog.md, work/protocol.md, work/challenges.md
and the files in work/. Follow the working prompt in HANDOVER.md section 2
and its conventions in section 5. Continue with the next action in section 6.
Briefly confirm the current status before you start.
```

## 2. Working Prompt (the team's agreed instructions)

```text
Guide our team through the assignment in 06-session-handout.md, step by step.

Our case
- System: HR-onboarding assistant (helps new hires with tasks and policies;
  uses conversation state, HR documents, task integration and sensitive
  employee data)
- Failure: Privacy or permission failure (the assistant shows information
  the user should not have access to)

Goal: a minimal, precise 3-slide presentation
1. Set the scene: what the system normally does, what went wrong for the user,
   the impact, and our immediate containment.
2. Our thinking: at least 3 distinct, ranked explanations, and the one check
   that would best narrow them down.
3. Recommendation: what we do next, how we'd know it helped, who owns it,
   when we'd roll back, and what evidence would change our mind.
Keep each slide to a few short bullets or one simple diagram, with short
speaker notes. Save it as work/slides.md.

Setup and follow-up
- backlog.md lists every step as a task with acceptance criteria and status.
  Update it after each step.
- README.md covers only our project.
- If we continue in a new chat, first read HANDOVER.md, backlog.md,
  work/protocol.md and work/challenges.md, then pick up at the first
  unfinished task.

Background material
Use the course files in this repository as background, and say which file
an idea comes from:
- 01-anatomy-of-a-genai-system.md: system components (Steps 1, 3)
- 02-context-retrieval-memory-and-adaptation.md: retrieval, permission
  filters, session memory (Steps 3, 4)
- 03-tools-workflows-and-trust-boundaries.md: trust boundaries, tool
  permissions (Steps 3, 4)
- 04-diagnose-ai-system-failures.md: building and testing hypotheses (Steps 4, 5)
- 05-evaluation-observability-and-governance.md: evaluation, monitoring,
  ownership (Steps 6, 7)

How we get there
- Work through the handout's steps 1–8 in order, using the templates
  (system-map, diagnostic-register, recovery-plan). Fill in every section
  briefly; the filled-in copies are in work/.
- For each step: explain what it needs in a sentence, ask us 2–4 focused
  questions, help us write it up, and say which slide it feeds. Wait for our
  OK before moving on.
- After each step, record our decisions in work/protocol.md (the step, what
  we decided, why, and any alternative we rejected) and update backlog.md.
- Note critical challenges in work/challenges.md; we review them at the end.
- We do the thinking. You suggest, challenge and point out gaps.

Key points along the way
- Step 5: switch roles and act as the handout's skeptical review panel.
  Critique everything, including your own earlier suggestions, and log the
  points in the AI Challenge Note.
- Step 6: 3–5 actions across contain / investigate / correct / prevent,
  scored on the handout's five dimensions. Consider non-ML fixes (permissions,
  workflow, review) first.
- End with a check: do the slides answer their questions, do they match
  the templates, and is every challenge in work/challenges.md resolved?

Ground rules
- Label facts, synthetic details, assumptions and unknowns clearly. Never
  invent architecture or evidence. Anything we make up is marked **synthetic**
  and stays the same from then on.
- Use only synthetic data, never real employee data.
- Never call a root cause verified. We only have synthetic evidence.

Context: 3 people working together, about 1–2 hours in total, in English,
no optional extensions. Keep us moving and tell us if a step runs long.
```

## 3. Status Snapshot

| ID | Task | Status | Output |
|---|---|---|---|
| S0 | Setup | Done | [backlog.md](backlog.md), [README.md](README.md), [work/](work/) |
| S1 | Frame the system | Done | [work/system-map.md](work/system-map.md) › System Context |
| S2 | Failure + frozen case facts | Done | [work/diagnostic-register.md](work/diagnostic-register.md) › Incident Frame, Evidence Available |
| S3 | Architecture & trust-boundary map | Done | [work/system-map.md](work/system-map.md) (all other sections) |
| S4 | Competing hypotheses | Done | work/diagnostic-register.md › Competing Hypotheses, Next-Best Evidence, Current Diagnosis |
| S5 | LLM skeptical-panel challenge | Done | AI Challenge Note |
| S6 | Recovery plan | Done | [work/recovery-plan.md](work/recovery-plan.md) |
| S7 | Proof of recovery | Done | work/recovery-plan.md |
| S8 | 3 slides | Done | [work/slides.md](work/slides.md) |
| S9 | Final check + review challenges | Done | [work/challenges.md](work/challenges.md) |

Detailed acceptance criteria: [backlog.md](backlog.md). All decisions and rejected options: [work/protocol.md](work/protocol.md).

## 4. Case at a Glance

All details are **synthetic** and frozen. Don't change them.

- **System (S1):** a read-only assistant for new hires (first ~30 days). It answers HR policy questions and shows the user's **own** onboarding checklist. Owner: HR product owner (People-Tech). Affected: new hires, employees whose data is exposed, HR, the DPO. **Never:** return anything the user isn't authorized to see under HR access rules.
- **Failure (S2):** Sam K. (Junior Data Analyst, day 4) asked *"What is the salary band for my role?"* The assistant replied with the band (€52–64k) **plus colleague Jordan M.'s pay (€61,500 + 8% bonus)** and cited the HR-only document `Compensation_Overview_Analytics_2026.pdf`.
  - When: 2026-11-12; Sam reported it with a screenshot on 2026-11-13.
  - Recent change: prompt + model update on 2026-11-05.
  - Severity: high (possible GDPR breach; the DPO assesses).
  - Containment: remove the compensation docs, keep policy Q&A running, notify the DPO.
  - Unknown: whether it reproduces, wider exposure, how the doc became retrievable, where permissions are really enforced, what the logs contain.
- **Architecture assumptions (S3, team choices):** role filter at retrieval; separate public-policy and HR-only indexes; per-session chat history only; basic request logs (whether retrieved passages and filter decisions are logged is unknown).
- **Trust boundaries (S3):** TB1 login → role · TB2 ingestion → index (labels/routing) · TB3 retrieval filter · TB4 task system (own ID) · TB5 retrieved text → LLM context.
- **Evidence requests (S3):** re-run as a test account; request trace; the doc's label and index; diff of the 11-05 release; Sam's resolved SSO role.

## 5. Conventions Used So Far

- Each step: one sentence on what it needs, then **AskUserQuestion with 2–4 options** (the recommended one first, with a short trade-off). The team asked for proposed options rather than open questions in S1–S3.
- After the team answers: fill in the work/ template, label everything (**K** known, **A** assumption, **U** unknown, **synthetic**), add rows to work/protocol.md (Date | Step | Decision | Why | Rejected alternative), and update the status table and checkboxes in backlog.md.
- Challenges go to work/challenges.md (C1–C5 so far) and are reviewed in S9, not solved immediately.
- Cite lessons 01–05 by file name. Correct your own earlier claims openly (e.g. the GDPR 72-hour wording in C4).
- Don't commit unless the team asks.

## 6. Next Action: Team Review Before Presenting

All steps are done. The main deliverable is [work/slides.md](work/slides.md).
Open items for the team:

1. Read the 3 slides aloud once (target: about 1 minute per slide) and cut anything you don't need.
2. Confirm or change the **proposed** statuses in [work/challenges.md](work/challenges.md).
3. Replace "Team decision" with a real one-sentence reason for P4/P5 in the
   AI Challenge Note ([work/diagnostic-register.md](work/diagnostic-register.md)).
4. Optional: answer the handout's reflection questions.

## 7. Git Note

All work is committed and pushed to `main`.
