# Phase 3: Target Architecture Design

**Purpose:** Propose the future-state architecture based on everything learned in Phases 0-2. This is where you make recommendations — and where human pushback is most valuable. Expect multiple revision cycles.

## Before Designing Anything: Rationalize the Landscape

Step back and question whether the current application landscape makes sense:
- Should these remain separate applications, or should some be consolidated?
- Should any applications or major features be retired entirely?
- Are there workflows that exist because of old technical limitations?
- Do current application boundaries match how the business actually works today?
- Are there business processes split across multiple apps that should be unified?

Document the rationalized view — what the organization actually needs vs. what it currently has. This prevents the most expensive mistake: faithfully rebuilding something that shouldn't exist.

## Architecture Areas to Address

### 1. Application Architecture
Monolith, modular monolith, or microservices? (Justify — don't default to microservices.) Module/service boundaries mapped to business domains. Communication patterns. Number of deployable units.

### 2. Frontend Strategy
Number of frontends. Rendering strategy (SPA, SSR, SSG, hybrid) with justification. Framework recommendation. Mobile strategy. Accessibility.

**For public-facing applications:** SEO and AI discoverability drive the tech stack. A site that depends on search traffic cannot be a client-only SPA — this constrains the framework before any other factor. Include: structured data/schema.org, Core Web Vitals, sitemap strategy, AI/agent discoverability (structured data, semantic HTML for AI search engines and autonomous agents).

### 3. Backend Strategy
Language/framework with rationale. API design (REST, GraphQL, both). Background jobs. Real-time communication. File storage.

### 4. Data Architecture
Database technology. Schema design. Data access strategy (and WHY). Caching strategy. Search strategy. Data migration approach.

### 5. Authentication & Authorization
Identity provider (buy vs. build — almost always buy). Internal vs. external users. Role/permission model. SSO/federation. API auth.

### 6. Infrastructure & Deployment
Container orchestration. CI/CD. Environment strategy. Monitoring/logging/alerting. Secrets management. Edge security (CDN, WAF, SSL/TLS).

### 7. AI/ML Integration (if applicable)
Tasks AI could improve (based on pain points). On-prem vs. cloud (compliance constraints). Model selection. Inference infrastructure.

### 8. Modern Capabilities Opportunity Scan
Starting from pain points, systematically check whether modern tools could solve specific problems:
- **Caching/working sets** (Redis, Memcached)
- **Real-time** (SignalR, WebSockets, SSE)
- **Messaging** (RabbitMQ, Redis Streams, Kafka)
- **API management** (Kong, Traefik, gateway)
- **Search** (Typesense, Meilisearch, Elasticsearch, vector search)
- **AI/ML** (vision, NLP, document parsing, anomaly detection)
- **MCP/agent integration** (workflows as AI tools)
- **Workflow orchestration** (Temporal, state machines)

For each: describe the pain point it solves, estimate impact/effort, then red team it — would a simpler approach work? If it doesn't trace to a real problem, discard it.

### 9. DR & Business Continuity
RPO/RTO targets. Backup strategy. Failover architecture. Geographic risk factors.

### 10. Buy vs. Build Analysis
For every component that could be purchased: build estimate, buy options, recommendation with rationale.

### 11. Elimination & Preservation Lists
What disappears and what replaces it. What gets preserved as-is.

### 12. Cross-Application Impact (multi-app)
How changes to each app affect others. Shared databases/APIs/queues creating coupling. Migration sequence. Temporary integration bridges.

### 13. Red Team Review
For every major recommendation: at least one counter-argument, risks, mitigations. Tag as [Tactical] or [Strategic].

### 14. Testing Strategy
- Automated tests against legacy behavior before migration
- Comprehensive test coverage in modernized system (unit, integration, API, e2e)
- Test automation in CI/CD
- Test data management

## Deliverable

**TARGET_ARCHITECTURE.md** — with diagrams, technology choices, buy vs. build decisions, compliance validation, current vs. target comparison tables, and [Tactical]/[Strategic] tags.

## Human Checkpoint

This is the MOST IMPORTANT review. Expect 3-6 revision cycles. Every correction improves the plan.
