# Phase 4: Modernization Plan — Design

## Approach

The plan translates the target architecture (Phase 3) into a sequenced implementation with realistic timelines, costs, and risk management. Every estimate must state its assumptions. Every unknown must be called out.

## Phase Ordering Principles

1. **Security fixes first** — on the existing system, before modernization begins
2. **Foundation next** — auth, database, API skeleton, CI/CD pipeline
3. **Highest-pain-point modules early** — builds credibility, momentum, and stakeholder confidence
4. **Revenue-generating features prioritized** over nice-to-haves
5. **External dependencies early** — anything with long lead times (hardware procurement, license acquisition, third-party API access) starts as soon as possible
6. **Each phase independently valuable** — if the project stops after Phase N, the organization has something useful

## Estimation Approach

Provide two estimates for every phase:
- **AI-assisted (Kiro + spec-driven):** Assumes experienced dev/architect with AI IDE and structured specs
- **Traditional:** Assumes manual development for comparison

Then add logistics overhead. This is where estimates diverge dramatically based on organization size:

**Small organizations (< 50 employees):**
- Hardware procurement and delivery: 2-6 weeks (ordering, shipping, racking, cabling)
- Hardware installation and base configuration: 1-3 weeks (OS, networking, hypervisor, storage)
- Cloud account setup: 1-2 weeks
- License procurement: 1-2 weeks
- Stakeholder approvals: minimal — often the decision-maker is in the room
- Typical logistics multiplier: 1.3-1.5x development time

**Mid-size organizations (50-500 employees):**
- Hardware procurement: 4-8 weeks (budget approval, vendor selection, PO process, delivery)
- Cloud account setup and security review: 2-4 weeks
- Stakeholder approvals: 1-3 weeks per approval gate (more gates than small orgs)
- License procurement: 2-4 weeks (legal review of terms, procurement process)
- Knowledge transfer / onboarding: 1-2 weeks if new team members
- Change management board reviews: 1-2 weeks per change window
- Typical logistics multiplier: 1.5-2x development time

**Large organizations (500+ employees) and enterprises:**
- Hardware procurement: 8-16+ weeks (capital budget cycles, RFP process, vendor evaluation, legal review, PO approval chains)
- Cloud account setup and security review: 4-8 weeks (security architecture review, compliance validation, network integration)
- Stakeholder approvals: 2-4 weeks per gate, with multiple gates (architecture review board, security review, change advisory board, budget committee)
- License procurement: 4-8 weeks (legal, procurement, vendor negotiations, security questionnaires)
- Cross-team coordination: 2-6 weeks (network team, DBA team, security team, infrastructure team — each with their own queue and priorities)
- Environment provisioning: 2-4 weeks per environment (dev, staging, prod — each may require separate approvals)
- Typical logistics multiplier: 2-3x development time

Calendar time = development time × logistics multiplier for organization size

The logistics multiplier is the single most underestimated factor in modernization timelines. A 12-week development effort at a large enterprise can easily become a 9-12 month calendar project once procurement, approvals, cross-team dependencies, and change windows are factored in.

## Cost Model

- Development: (hours × rate) per phase
- Infrastructure: one-time + monthly recurring
- Licensing: annual costs for commercial components
- Eliminated costs: current licensing, maintenance, manual labor that goes away
- Cost of inaction: what the status quo costs per year (maintenance, security risk, lost productivity, missed opportunities)

## Risk Framework

For each risk: Description → Probability (L/M/H) → Impact (L/M/H) → Mitigation → Contingency → Owner
