# Phase 3: Target Architecture Design — Design

## Approach

Design the system the organization SHOULD have, not a 1:1 replica of what exists. Use the pain points (Phase 2) as the primary driver — the architecture should solve real problems, not just look modern.

## Step Zero: Rationalize Before You Design

Before drawing a single architecture diagram, step back and question the current landscape:
- Are the current application boundaries the right ones? Three apps sharing a database and serving the same users might be one app that was split for historical/technical reasons that no longer apply.
- Are there applications or major features that should be retired? If nobody uses it, or a purchased product does it better, don't modernize it — eliminate it.
- Are there workflows that exist because of old technical limitations? Manual re-entry between systems, batch processes that should be real-time, approval chains that no longer serve a purpose.
- Does the application map match how the business actually operates today, or how it operated when the software was written?

This rationalization prevents the most expensive mistake: spending months faithfully rebuilding something that shouldn't exist. The output is a rationalized view of what the organization actually needs, which becomes the input to architecture design.

## Modern Capabilities: Problem-First, Not Tool-First

The capabilities opportunity scan (REQ-8) is powerful but dangerous. The risk is tool-driven solutioning — "we have Redis, everything looks like a caching problem." The guardrail is simple: every opportunity must trace back to a specific pain point from Phase 2 or a measurable business outcome. If you can't point to the problem it solves, it doesn't belong in the plan.

When an opportunity does trace back to a real problem, red team it: would a simpler approach work? Does the team have the skills to operate this? Is the complexity justified by the impact? A Redis working set cache that eliminates a 30-second page load for 50 daily users is worth it. A Kafka event bus for a system that processes 100 messages a day is not.

## Design Principles

1. **Start with the business domains, not the technology.** Module boundaries should reflect how the business works, not how the code is currently organized.

2. **Right-size the architecture.** Don't propose microservices for a 3-person team. Don't propose a monolith for a 50-person org with independent teams. Match the architecture to the organization's size, skills, and operational capacity.

3. **Leverage existing infrastructure.** If Phase 0 revealed underutilized hardware, design to use it. Don't propose buying new servers when existing ones are at 20% utilization.

4. **Compliance drives constraints, not preferences.** If ITAR says data stays on-prem, data stays on-prem. If PCI-DSS requires network segmentation, design for it. These aren't negotiable.

5. **Every technology choice needs a "why not the alternative" explanation.** Don't just say "use PostgreSQL." Say "PostgreSQL over SQL Server because [licensing, features, portability] and over MySQL because [JSON support, full-text search, extensions]."

## Architecture Decision Records

For each major decision, document:
- **Decision:** What was decided
- **Context:** What factors influenced the decision
- **Alternatives considered:** What else was evaluated
- **Rationale:** Why this option was chosen
- **Red Team critique:** Counter-arguments, risks, and failure modes for this choice
- **Mitigations:** How the identified risks are addressed
- **Consequences:** What this decision enables and constrains
- **Reversibility:** How hard is it to change this decision later
- **Classification:** **[Tactical]** (addresses immediate pain, reduces risk now) or **[Strategic]** (invests in future capability, enables long-term goals)

## Red Teaming

Every major recommendation must be challenged before it's finalized. For each decision:
1. Present the recommendation with rationale
2. Present at least one credible alternative
3. Argue against your own recommendation — what could go wrong? What assumptions might be wrong?
4. Document mitigations for the risks identified
5. Let the human decide — they may see risks the AI doesn't

This isn't bureaucracy. It's how you catch "we chose microservices because it sounded modern" before it becomes a 6-month mistake.

## Cross-Application Impact

For multi-app modernizations, every architecture decision must be evaluated for cross-app impact:
- If App A moves to PostgreSQL but App B still reads from App A's SQL Server database, what's the bridge?
- If App A gets a new API but App C sends SOAP requests to App A, who changes?
- If shared authentication moves to Keycloak, every app that touches auth is affected simultaneously

Document these dependencies explicitly and design the migration sequence to minimize breakage.

## Diagram Requirements

Include at minimum:
- High-level system architecture (all components and their relationships)
- Data flow diagram (how data moves through the system)
- Deployment architecture (what runs where)
- Network architecture (if on-prem or hybrid)
