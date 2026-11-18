# When an HR Assistant Shares Too Much

*A case study for AI Product Managers · 10 minutes · all names, numbers and documents are invented*
*Early text draft. The final deck is [presentation_final.pptx](presentation_final.pptx) / [presentation_final.html](presentation_final.html).*

---

## Intro · Who We Are (~1 min)

- **Our company:** Northwind People GmbH, a tech company in Berlin with about 1,200 employees *(fictional)*
- **Our team:** 3 AI Product Managers who own the AI assistants used by HR and employees
- **The case:** our onboarding assistant showed a new employee a colleague's salary
- **Agenda:** 01 Set the scene · 02 Our thinking · 03 Our recommendation

---

## Slide 1 · Set the Scene (~2.5 min)

**What does the system normally do, and what problem did the user run into?**

### What the assistant normally does
- It helps new employees in their first weeks at the company
- It answers questions about HR rules, such as holidays or salary ranges
- It shows each person **their own** onboarding to-do list
- It can only read information. It cannot change anything.

### What went wrong
- A new employee, four days into the job, asked:
  *"What is the salary range for my role?"*
- The assistant gave the range, **and also a colleague's exact salary and bonus**
- It even named its source: a document that **only HR should see**

### Why it matters
- Private pay data reached the wrong person
- It may be a data protection breach (our data protection officer is checking)
- Employees may stop trusting the assistant and HR

### What we did first
- We blocked the assistant from all HR-only documents
- The assistant stays online for normal HR questions
- We secured the system logs, which record every question and answer

> **Speaker notes:** The new employee reported this the next day with a screenshot. That screenshot is the only hard evidence we have. We also know that the AI model and its instructions were updated one week earlier. That is a lead, but not proof. Our first step was to stop further harm without switching off the whole service. → *So how could this happen?*

---

## Slide 2 · Our Thinking (~4 min)

**What are the most plausible explanations, and what would we check to narrow them down?**

### How we approached it
1. **Facts first:** we only treated the screenshot as fact. Everything else is an assumption.
2. **Follow the path:** we drew the route a question takes, from the user through the system to the answer.
3. **Find the checkpoints:** at each point where private data could slip through, we asked: *"Could the check have failed here?"*

### Our three most plausible explanations

| | What could have gone wrong | In simple words |
|---|---|---|
| **A** | The system thought the new employee **was an HR employee** | The access check worked, but with wrong information about the person |
| **B** | The access check **was skipped** for HR-only documents | The lock was there but not used |
| **C** | The salary document **was stored as "public"** | The document was in the wrong drawer |

All three are **equally uncertain**: we have only one example.

### What we check in our logs
The system logs every request. The log entry for this exact question should show:
- who the system **thought the user was** → A
- whether the **access check ran** → B
- **where** the document came from → C

**One log entry tells all three apart.**
We also search the logs for **other answers** that used HR-only documents, to find out whether anyone else was affected.

> **Speaker notes:** We also asked an AI to review our thinking critically. It suggested two more ideas: the assistant reused an answer given to someone else, or privacy was protected only by written instructions to the AI. We kept both as backups, and the same log entry would show them too. We deliberately did not blame the recent AI update without evidence. → *So what do we do now?*

---

## Slide 3 · Our Recommendation (~2.5 min)

**What would we do next, and how would we know it helped?**

### Our recommendation
**Keep the assistant running safely, find the real cause, then make the smallest fix.**

### Next steps
1. **Now:** keep HR-only documents blocked; the data protection officer reviews the case
2. **Within 2 days:** analyse the logs: check A, B and C for this request, and search for other leaks
3. **Then:** fix only what the evidence shows (the user's role, the access check, or the document's label)
4. **For the future:** test for data leaks before every update; make sure the logs record every access decision

### What we will *not* do
**Switch to a "smarter" AI model.** An AI model is not a lock. Access rules must be enforced by the system, not by instructions to the AI.

### How we will know it helped
- We ask the original question again, plus **20 test questions about private data**, and see **zero leaks**
- New employees still get normal answers, such as the public salary range
- The logs show no HR-only document in any answer to a new employee
- The HR product owner is responsible; the data protection officer reviews the logs weekly for 4 weeks
- **If a single leak appears again:** block HR data again, or pause the assistant

> **Speaker notes:** Our main lesson as product managers: a privacy failure in an AI product is usually a permission or data problem, not an AI problem. The fix belongs in access rules and testing, not in a bigger model. What would change our mind: if data still leaks while HR documents are blocked, the cause lies elsewhere and we would pause the assistant.
