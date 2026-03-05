# Phase 2: Stakeholder Pain Point Assessment

**Purpose:** Understand what the current system fails to do from the perspective of the people who use it every day. The codebase tells you what the system does. The users tell you what it doesn't do.

## Interview Structure

For each user role identified across all applications, ask:

1. Walk me through your typical day using this system. What do you do first? What takes the longest?
2. What's the most frustrating thing about the system?
3. What workarounds have you invented? (Excel exports, manual tracking, sticky notes, separate tools?)
4. What information do you need that requires looking at multiple screens or asking someone else?
5. What would you change if you could change one thing?
6. What do your customers/partners complain about?
7. What reports or data views do you wish existed?
8. Are there tasks you avoid because the system makes them too painful?
9. What happens when the system goes down? How do you work?
10. If I could give you a magic feature that doesn't exist today, what would it be?

## Pain Point Prioritization

Rate each pain point by:
- **Business impact** (time wasted, revenue lost, risk created, customer satisfaction)
- **Frequency** (daily, weekly, monthly, occasional)
- **Number of users affected**
- **Current workaround quality** (none, poor, adequate)

Categorize preliminary solution: architecture fix, new feature, buy component, process change.

## Cross-Reference with Phase 1

- Map pain points to specific code/architecture findings
- Identify pain points invisible in the code (workflow issues, missing features, UX problems)
- Identify code issues from Phase 1 that users haven't noticed yet (security, performance time bombs)

## Deliverable

**PAIN_POINTS.md** — prioritized matrix organized by user role, business impact, and preliminary solution category.

## Human Checkpoint

Users review their own pain points for accuracy. Management reviews for business priority alignment. New pain points often surface here — seeing others' pain points triggers "oh yeah, we also have this problem."
