# Backlog: HR-Onboarding Assistant × Privacy/Permission Failure

> **Resuming in a new chat?** Read [HANDOVER.md](HANDOVER.md), this file, [work/protocol.md](work/protocol.md) and [work/challenges.md](work/challenges.md) first,
> then continue with the first task that is not **Done**.

Main goal: a minimal, precise 3-slide presentation ([work/slides.md](work/slides.md)),
built from the filled-in templates in [work/](work/).
Assignment: [06-session-handout.md](06-session-handout.md).

| ID | Task | Output | Feeds | Status |
|---|---|---|---|---|
| S0 | Setup | backlog, README, work/ | – | Done |
| S1 | Frame the system | work/system-map.md › System Context | Slide 1 | Done |
| S2 | Failure + frozen case facts | work/diagnostic-register.md › Incident Frame | Slide 1 | Done |
| S3 | Architecture & trust-boundary map, evidence requests | work/system-map.md (rest) | Slides 1–2 | Done |
| S4 | Competing hypotheses | work/diagnostic-register.md | Slide 2 | Done |
| S5 | LLM skeptical-panel challenge | AI Challenge Note | Slide 2 | Done |
| S6 | Recovery plan | work/recovery-plan.md › Decision, Actions, Options | Slide 3 | Done |
| S7 | Proof of recovery & rollback | work/recovery-plan.md › Proof, Release | Slide 3 | Done |
| S8 | Draft the 3 slides | work/slides.md | All | Done |
| S9 | Final check | Review Criteria + Submission Check | All | Done |

---

## S0 · Setup: Done
**As a** team, **we want** a backlog, a decision protocol and a project README **so that** we can resume the work in any new session.
- [x] backlog.md created
- [x] README.md rewritten for our project
- [x] Templates copied into work/, plus work/protocol.md

## S1 · Frame the system (handout §1): Done
**As a** team, **we want** to define who the HR-onboarding assistant serves and what it must never do **so that** the failure has a clear reference point.
- [x] Primary user, task, supported decision, expected outcome
- [x] Unacceptable outcome / "must never do"
- [x] Accountable owner and affected people
- [x] Decisions logged in protocol.md

## S2 · Failure scenario + frozen case facts (handout §2): Done
**As a** team, **we want** a concrete, reproducible privacy/permission failure **so that** every later step works from the same facts.
- [x] User-visible example (failing request + response)
- [x] Expected behaviour, who was affected and how, when, any recent change
- [x] Impact, first containment action, explicit unknowns
- [x] Invented details labelled **synthetic** and kept stable

## S3 · Architecture & trust-boundary map (handout §3): Done
**As a** team, **we want** a map of the request and data flows with the trust boundaries marked **so that** we can see where the leak could happen.
- [x] Mermaid request flow adapted to our system
- [x] Data/knowledge flow, components, trust boundaries & permissions
- [x] Uncertain components labelled as assumptions; architecture unknowns listed
- [x] Evidence we would request (smallest useful set)

## S4 · Competing hypotheses (handout §4): Done
**As a** team, **we want** at least 3 materially different causes, ranked **so that** we don't jump to a single explanation.
- [x] ≥3 distinct hypotheses, each with component, why it fits, evidence against, next test, confidence
- [x] Next-best evidence: the one check that best separates them
- [x] Current diagnosis (no root cause claimed as verified)

## S5 · LLM challenge (handout §5): Done
**As a** team, **we want** a skeptical AI review of our map and register **so that** we find missed causes and weak fixes.
- [x] The AI acts as the skeptical panel, including critique of its own earlier suggestions
- [x] Most important points logged as accepted / edited / rejected in the AI Challenge Note

## S6 · Recovery plan (handout §6): Done
**As a** team, **we want** a prioritized recovery plan **so that** our response follows from the evidence.
- [x] 3–5 actions across contain / investigate / correct / prevent
- [x] Each scored on impact, effort, risk reduction, confidence, reversibility, with an owner
- [x] Non-ML options (permissions, workflow, review) considered first
- [x] Decision + evidence that would change it

## S7 · Proof of recovery (handout §7): Done
**As a** team, **we want** measurable recovery criteria **so that** we know when the incident can be closed.
- [x] Original failure turned into a fixed regression test
- [x] Guardrail thresholds, monitoring owner, rollback/shutdown trigger
- [x] Remaining risk and who is authorized to accept it

## S8 · 3-slide presentation (handout §8): Done
**As a** team, **we want** 3 minimal slides with speaker notes **so that** we can present our decision clearly.
- [x] Slide 1: what the system does, what went wrong, impact, containment
- [x] Slide 2: ≥3 ranked explanations + the check that narrows them down
- [x] Slide 3: next steps, how we'd know it helped, owner, rollback, what would change our mind
- [x] Cut until every bullet is essential

## S9 · Final check: Done
**As a** team, **we want** to verify consistency **so that** all deliverables tell one story.
- [x] Slides answer their three questions
- [x] Slides match the templates; facts / synthetic / assumptions / unknowns are visibly distinct
- [x] Handout Review Criteria and Submission Check passed
- [x] Every item in work/challenges.md marked addressed / accepted risk / rejected
