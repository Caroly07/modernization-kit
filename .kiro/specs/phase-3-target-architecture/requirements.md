# Phase 3: Target Architecture Design — Requirements

## Overview
Propose the future-state architecture based on everything learned in Phases 0-2. This is where recommendations are made — and where human pushback is most valuable. Expect multiple revision cycles.

## Requirements

### REQ-0: Application Landscape Rationalization
Before designing anything, step back and question whether the current application landscape makes sense. Using everything learned in Phases 0-2:
- [ ] Should these remain separate applications, or should some be consolidated? (Three apps sharing the same database and serving the same users may be one app with three entry points)
- [ ] Should any applications or major components be retired entirely? (Features nobody uses, workflows that exist because of old technical limitations, processes that a purchased product handles better)
- [ ] Are there entire workflows that should be eliminated rather than modernized? (Manual processes that exist because the old system couldn't automate them, approval chains that no longer serve a purpose)
- [ ] Does the current application boundary map to how the business actually works, or to how the technology was organized 15 years ago?
- [ ] Are there business processes split across multiple applications that should be unified?
- [ ] Document the rationalized application landscape — what the organization actually needs vs. what it currently has

This step prevents the most expensive modernization mistake: faithfully rebuilding something that shouldn't exist.

### REQ-1: Application Architecture
- [ ] Recommend architecture pattern (monolith, modular monolith, microservices) with justification
- [ ] Define module/service boundaries mapped to business domains
- [ ] Design inter-module communication (REST, gRPC, message queue, shared DB)
- [ ] Specify number of deployable units
- [ ] Tag each major recommendation as **[Tactical]** or **[Strategic]**

### REQ-2: Frontend Strategy
- [ ] Determine number of frontends needed (public vs. internal often differ)
- [ ] Recommend rendering strategy per frontend (SPA, SSR, SSG, hybrid) with justification
- [ ] Recommend framework with rationale
- [ ] Define mobile strategy (responsive, PWA, native)
- [ ] Address accessibility requirements
- [ ] For public-facing applications: SEO and AI discoverability strategy — this drives tech stack decisions, not the other way around
  - SEO requirements constrain framework and rendering choices. A site that depends on search traffic cannot be a client-only SPA — this alone may determine the frontend framework (Next.js, Nuxt, Astro, etc. over pure React/Vue/Angular SPA)
  - Rendering approach: SSR for dynamic content that needs crawlability, SSG for content that changes infrequently, hybrid for sites with both
  - Structured data / schema.org markup for rich search results
  - Core Web Vitals optimization (LCP, INP, CLS) — these are ranking factors
  - Sitemap and robots.txt strategy
  - AI and agent discoverability — structured data, clean semantic HTML, and machine-readable content that AI search engines (Google AI Overviews, Perplexity, ChatGPT search) and autonomous agents can parse and act on. This is the next frontier of SEO — optimize for machines that read and summarize, not just index.
  - If the current site depends on SEO for traffic/revenue, SEO is an architectural constraint that drives the tech stack. Document this explicitly — it narrows the framework options before any other factor.

### REQ-3: Backend Strategy
- [ ] Recommend language and framework with rationale
- [ ] Design API approach (REST, GraphQL, or both)
- [ ] Plan background job processing
- [ ] Address real-time communication needs
- [ ] Plan file storage and document management

### REQ-4: Data Architecture
- [ ] Recommend database technology with rationale
- [ ] Design schema approach (single DB with schemas, multiple DBs, polyglot)
- [ ] Define data access strategy (ORM, micro-ORM, raw SQL, stored procs — and WHY each)
- [ ] Design caching strategy (what, how, invalidation)
- [ ] Plan search strategy (DB full-text, dedicated engine, vector search)
- [ ] Outline data migration approach

### REQ-5: Authentication & Authorization
- [ ] Recommend identity provider (buy vs. build — almost always buy)
- [ ] Design internal vs. external user handling
- [ ] Define role and permission model
- [ ] Address SSO/federation requirements
- [ ] Plan API authentication for external consumers

### REQ-6: Infrastructure & Deployment
- [ ] Design container orchestration approach
- [ ] Design CI/CD pipeline
- [ ] Plan environment strategy (dev, staging, production)
- [ ] Specify monitoring, logging, and alerting stack
- [ ] Plan secrets management
- [ ] Design edge security (CDN, WAF, DDoS, SSL/TLS)

### REQ-7: AI/ML Integration Assessment
- [ ] Identify tasks where AI could improve workflows (based on pain points)
- [ ] Determine on-prem vs. cloud AI (based on compliance constraints)
- [ ] Recommend model selection approach (small open models vs. cloud APIs)
- [ ] Plan inference infrastructure if on-prem
- [ ] Design embedding and vector search strategy if applicable

### REQ-8: Modern Capabilities Opportunity Scan
Starting from the pain points (Phase 2), systematically evaluate whether modern tools and practices could solve specific problems better than a traditional implementation. This is problem-first, not tool-first — every opportunity must trace back to a real pain point or measurable business outcome.

For each of the following capability categories, check whether any Phase 2 pain points or Phase 1 findings suggest a fit:
- [ ] **Caching / working sets** (Redis, Memcached) — large datasets loaded repeatedly? Slow queries that return the same data? Users competing for the same records?
- [ ] **Real-time communication** (SignalR, WebSockets, SSE) — users refreshing to see updates? Collaboration workflows? Live dashboards?
- [ ] **Event-driven architecture / messaging** (RabbitMQ, Redis Streams, Kafka) — tightly coupled processes that should be async? Long-running operations blocking users? Cross-system notifications?
- [ ] **API management** (Kong, Traefik, API gateway) — multiple consumers of the same data? Partner/third-party integration needs? Rate limiting, versioning, or monetization opportunities?
- [ ] **Full-text / semantic search** (Typesense, Meilisearch, Elasticsearch, vector search) — users can't find what they need? Keyword search returning irrelevant results? Unstructured document corpus?
- [ ] **AI/ML beyond basic automation** (computer vision, NLP, document parsing, anomaly detection, recommendations) — manual classification tasks? Document intake workflows? Pattern recognition in data?
- [ ] **MCP / agent integration** — workflows that could be exposed as tools for AI agents? Data that external systems or AI assistants should be able to query programmatically?
- [ ] **Workflow orchestration** (Temporal, custom state machines) — complex multi-step processes with branching logic? Approval chains? Long-running sagas?
- [ ] **Edge computing / CDN** — geographically distributed users? Static content that could be cached at the edge? Latency-sensitive operations?

For each identified opportunity:
- [ ] Describe the specific pain point or business outcome it addresses
- [ ] Estimate the impact (time saved, revenue enabled, risk reduced)
- [ ] Estimate the additional effort to implement
- [ ] Red team it: is this solving a real problem or adding complexity for its own sake? Would a simpler approach work? Does the team have the skills to operate this?
- [ ] Classify as [Tactical] or [Strategic]

If an opportunity doesn't trace back to a specific pain point or measurable outcome, it doesn't belong in the plan.

### REQ-8: Disaster Recovery & Business Continuity
- [ ] Define RPO and RTO targets
- [ ] Design backup strategy
- [ ] Plan failover architecture
- [ ] Address geographic/environmental risk factors

### REQ-9: Buy vs. Build Analysis
- [ ] For every component that could be purchased, provide build estimate, buy options, and recommendation
- [ ] Validate against organization's open-source policy (from Phase 0)

### REQ-10: Elimination & Preservation Lists
- [ ] List every current component that disappears and what replaces it
- [ ] List anything preserved as-is

### REQ-11: Cross-Application Impact Analysis
- [ ] For multi-app modernizations: document how changes to each application affect other applications in the landscape
- [ ] Identify shared databases, APIs, file drops, or message queues that create coupling between applications
- [ ] Define the migration sequence considering inter-app dependencies (which app must move first?)
- [ ] Document temporary integration bridges needed during phased migration (e.g., API facades over legacy databases)

### REQ-12: Red Team Review
- [ ] For every major architecture recommendation, present at least one counter-argument or alternative approach
- [ ] Document risks and failure modes for each major decision
- [ ] Include mitigations for each identified risk
- [ ] Flag decisions where the team lacks direct experience — these carry higher execution risk

### REQ-13: Testing Strategy
AI-assisted development has eliminated the traditional excuse for skipping tests. Comprehensive automated testing is now a baseline expectation for any modernization effort, not a nice-to-have.
- [ ] Plan for generating automated tests against legacy behavior before migration (capture what "correct" looks like before changing anything)
- [ ] Define validation approach (how to prove new system produces correct results)
- [ ] Plan for side-by-side output comparison where feasible
- [ ] Require comprehensive test coverage in the modernized system:
  - Unit tests for all business logic (AI can generate these from specs and code)
  - Integration tests for data access, external service calls, and cross-module communication
  - API tests for every endpoint (contract testing, response validation, error handling)
  - End-to-end tests for critical user workflows
- [ ] Define test automation in CI/CD — tests run on every commit, not as an afterthought
- [ ] Plan test data management (seeding, fixtures, anonymized production data)
- [ ] Estimate test generation effort — with AI-assisted development, generating tests is dramatically faster than it was manually, making comprehensive coverage realistic even on tight timelines

## Deliverable
A **TARGET_ARCHITECTURE.md** with diagrams, technology choices, buy vs. build decisions, compliance validation, and comparison tables (current vs. target).

## Human Checkpoint
This is the MOST IMPORTANT review. Domain expert and stakeholders challenge every recommendation. Expect 3-6 revision cycles. Every correction improves the plan.
