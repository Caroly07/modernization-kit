# Phase 3: Target Architecture Design

Propose the future-state architecture based on Phases 0-2. Expect multiple revision cycles.

## Before Designing: Rationalize the Landscape

- Should these remain separate applications, or should some be consolidated?
- Should any applications or features be retired entirely?
- Are there workflows that exist because of old technical limitations?
- Do current app boundaries match how the business actually works today?
- Document the rationalized view — what the org actually needs vs. what it has.

## Architecture Areas

**1. Application Architecture:** Monolith, modular monolith, or microservices (justify). Module boundaries mapped to business domains. Communication patterns.

**2. Frontend Strategy:** Rendering strategy (SPA, SSR, SSG, hybrid). Framework recommendation. Mobile strategy. Accessibility. For public-facing apps: SEO and AI discoverability drive the tech stack — a site dependent on search traffic cannot be a client-only SPA.

**3. Backend Strategy:** Language/framework. API design. Background jobs. Real-time communication. File storage.

**4. Data Architecture:** Database tech. Schema design. Data access strategy (and WHY). Caching. Search. Data migration approach.

**5. Auth & Authorization:** Identity provider (buy vs. build). Role/permission model. SSO/federation. API auth.

**6. Infrastructure & Deployment:** Container orchestration. CI/CD. Environments. Monitoring/logging/alerting. Secrets management. Edge security.

**7. AI/ML Integration:** Tasks AI could improve. On-prem vs. cloud. Model selection. Inference infrastructure.

**8. Modern Capabilities Scan:** Problem-first check — caching, real-time, messaging, API management, search, AI/ML, MCP/agents, workflow orchestration. For each: pain point it solves, impact, effort, red team critique. If it doesn't trace to a real problem, discard it.

**9. DR & Business Continuity:** RPO/RTO. Backup. Failover. Geographic risk.

**10. Buy vs. Build:** For every component that could be purchased.

**11. Elimination & Preservation Lists.**

**12. Cross-App Impact:** How changes affect other apps. Migration sequence. Temporary integration bridges.

**13. Red Team Review:** Every major recommendation gets counter-arguments, risks, mitigations. Tag [Tactical] or [Strategic].

**14. Testing Strategy:** Tests against legacy behavior. Comprehensive coverage in new system (unit, integration, API, e2e). CI/CD automation. Test data management.

## Deliverable

**TARGET_ARCHITECTURE.md** — diagrams, technology choices, buy vs. build, compliance validation, current vs. target comparison.

## Human Checkpoint

This is the MOST IMPORTANT review. Expect 3-6 revision cycles.
