# Application Modernization Methodology
## AI-Assisted Legacy System Transformation — A Practitioner's Framework

*Developed by [Burly Mingo LLC](https://burlymingo.com) using [Kiro IDE](https://kiro.dev) + Claude Opus 4.6*

### What This Is

A structured, multi-phase methodology for modernizing legacy applications using AI-assisted development. Designed to be run in an AI IDE (Kiro, Cursor, or equivalent) where the AI has direct access to the codebase and a domain expert is actively participating.

This is not a single prompt. It's a process that produces real engineering deliverables — architecture assessments, modernization plans, business cases, and supporting documents — through iterative collaboration between an AI assistant and a human who knows the business.

### What This Is Not

- Not a "paste this prompt and get a modernization plan" magic trick
- Not a replacement for domain expertise — the human's corrections are the most valuable part
- Not a 1:1 code translation tool — the goal is holistic modernization, not just changing the language
- Not tied to any specific source or target technology stack

### Prerequisites

1. **Codebase access.** The AI needs to read the actual source code, configuration files, database schemas, and deployment artifacts. An AI IDE with workspace access is the ideal environment — [Kiro](https://kiro.dev) with its spec-driven workflow is what this methodology was built for and tested with. If the codebase is too large for direct context, see the "Scaling" section for vector DB approaches.
2. **A domain expert.** Someone who understands the business processes the software supports. Ideally the original developer or a senior user. This person reviews every deliverable and corrects the AI's assumptions. Without this person, the output will be generic and potentially wrong.
3. **A capable model.** This methodology was developed with Claude Opus 4.6. The codebase analysis, architecture design, and plan generation require a model that can reason about complex systems across large contexts. Lesser models will produce lesser plans.
4. **Stakeholder access.** The domain expert may not know everything — infrastructure details, licensing costs, compliance requirements, and business priorities may require input from IT, finance, legal, or management.
5. **Time for iteration.** Each phase involves AI analysis → human review → correction → revision. Plan for multiple sessions per phase. The engagement that produced this methodology spanned 12+ collaborative sessions.

### Core Principles

These are non-negotiable and should guide every decision:

- **Modernize, don't translate.** Changing VB.NET to C# is not modernization. Rethinking the architecture, data model, user experience, and operational model is modernization. Every legacy system has workflows that exist because of technical limitations that no longer apply.
- **Containers by default, cloud/on-prem agnostic — but not dogma.** The default recommendation is containerized deployment that runs identically in a closet, a colo, AWS, Azure, or GCP. Docker containers orchestrated by Kubernetes (K3s for small deployments, managed K8s for larger ones). However, if the organization is committed to a specific cloud and cloud-native services (Azure Service Bus, AWS RDS, GCP Cloud Run) make their life genuinely easier, that's a valid choice. Ask the question, respect the answer, and document the trade-off (convenience vs. portability). The goal is informed decisions, not ideology.
- **Buy vs. build at every decision point.** For every component, explicitly ask: is there an existing product that does this better than we could build it? Auth (Keycloak, Auth0, Okta), CMS (Strapi, Contentful), PDF generation (Gotenberg), monitoring (Grafana, Datadog), analytics (Metabase, Looker) — don't build what you can deploy. But know your audience: many enterprises have procurement policies that require vendor support contracts, SLAs, SOC 2 reports, and security attestations that open-source projects can't provide. "Use Keycloak" is great advice for a startup; a Fortune 500 might need Okta because their security team requires a vendor on the phone at 3am. Ask about the organization's open-source policy and procurement requirements early — it changes the buy vs. build calculus significantly.
- **AI-assisted, spec-driven development is the baseline.** Timeline and cost estimates assume an experienced developer/architect working with [Kiro IDE](https://kiro.dev) using spec-driven development — structured requirements, design, and implementation tasks, not "vibe coding." This is a critical distinction: an experienced dev with Kiro specs producing production code is easily 3-5x faster than traditional development for CRUD operations, API endpoints, data migration scripts, and UI scaffolding. An intern vibe-coding with Copilot is producing 3-5x more bugs faster. The methodology assumes the former. If the organization plans to use traditional development without AI assistance, multiply timeline estimates by 2.5-4x. If they plan to use AI without spec-driven workflow or without experienced developers reviewing output, the timeline may not change but the quality and maintainability of the output will suffer dramatically.
- **New languages and frameworks are not the barrier they once were.** AI IDEs have fundamentally changed the cost of adopting a new tech stack. A team experienced in C# can produce idiomatic TypeScript or Go with AI assistance in ways that weren't practical five years ago. Don't let "we don't know React" block the right architecture decision — factor in AI-assisted ramp-up when evaluating tech stack options. That said, all else being equal, prefer the team's existing expertise. If a Microsoft shop is choosing between C# and Python and there's no functional difference, go with C#. Existing knowledge, tooling, licensing, and operational experience have real value. The principle is: don't let unfamiliarity veto the right choice, but don't ignore familiarity when the options are equivalent.
- **Comprehensive automated testing is a baseline, not a luxury.** AI-assisted development makes generating unit tests, integration tests, and API tests dramatically faster than manual test writing. Every modernization effort should include full test coverage as a standard deliverable — unit tests for business logic, integration tests for data access and services, API contract tests for every endpoint, and end-to-end tests for critical workflows. There is no longer a credible excuse for shipping without tests.
- **Honest about unknowns.** Every plan has gaps. Call them out explicitly rather than papering over them with assumptions. An "Open Questions" section is mandatory.
- **Security is Phase 0, not Phase Last.** Identify and fix critical vulnerabilities in the existing system before starting modernization work.


---

## The Process

### Phase 0: Discovery Interview

**Purpose:** Understand the organization, its industry, its constraints, and its goals before touching a single line of code. The answers to these questions fundamentally change the architecture, technology choices, compliance requirements, and risk profile of the modernization.

**How to use:** Present these questions to the domain expert and stakeholders. The AI should ask them conversationally, not as a checklist. Follow-up questions based on answers are expected and encouraged. Some answers will trigger additional questions from specific domains (e.g., if the answer to "What industry?" is "healthcare," the AI should ask HIPAA-specific questions).

**Prompt for the AI assistant:**

```
I'm going to help you build a comprehensive modernization plan for your legacy application(s). Before I look at any code, I need to understand your organization, your constraints, and what you're trying to achieve. I'll ask questions in groups — answer what you can, and say "I don't know" for anything you're unsure about. That's valuable information too.

Let's start with the big picture, then get specific.
```

#### Question Group 1: Organization & Industry

```
ORGANIZATION & INDUSTRY

1. What does your organization do? (One paragraph — what's the core business?)
2. What industry are you in?
3. Approximately how many employees? How many are IT/engineering?
4. Where are you physically located? (Matters for DR, latency, regulations)
5. Do you have existing cloud accounts (AWS, Azure, GCP)? Which ones?
6. Is there a preference or mandate for cloud vs. on-premises?
7. What's the approximate annual IT budget? (Even a range helps — $50K? $500K? $5M?)
```

**Why these matter:** Location affects disaster recovery planning (hurricane zone? earthquake zone? flood plain?). Industry determines the regulatory framework. Cloud accounts and preferences constrain infrastructure choices. Budget determines what's realistic.

#### Question Group 2: Regulatory & Compliance Environment

```
REGULATORY & COMPLIANCE

Based on your industry, which of these apply? (Say yes, no, or "I don't know")

- HIPAA (healthcare data)
- PCI-DSS (payment card data)
- SOX (financial reporting controls)
- ITAR/EAR (defense/export-controlled articles)
- FERPA (student education records)
- GLBA (financial institution customer data)
- CMMC/DFARS/NIST 800-171 (defense contractor cybersecurity)
- FedRAMP (selling cloud services to federal government)
- GDPR (EU personal data)
- CCPA/CPRA (California consumer privacy)
- NRC regulations (nuclear)
- FDA 21 CFR Part 11 (pharma/medical device electronic records)
- SOC 2 (service organization controls)
- State-specific gaming regulations (casinos)
- Other: ___

Do you have a dedicated compliance officer or team?
Have you had a compliance audit in the last 2 years? Any findings?
Are there data residency requirements (data must stay in a specific country/region)?
Is any of your data classified (government classification levels)?
```

**Why these matter:** Compliance requirements can eliminate entire categories of technology choices. ITAR may prohibit cloud LLM APIs. HIPAA requires specific encryption and access controls. PCI-DSS has network segmentation requirements. Gaming regulations may require specific audit trail capabilities. These aren't nice-to-know — they're architecture-defining constraints.

**AI behavior:** Based on the answers, the AI should note which compliance frameworks apply and carry them forward into every subsequent phase. Every technology choice, data flow, and deployment decision must be validated against the applicable frameworks.

#### Question Group 3: Current Application Landscape

```
CURRENT APPLICATIONS

1. How many applications are we modernizing? List each one with:
   - Name
   - What it does (one sentence)
   - Who uses it (internal employees, external customers, both, other systems)
   - Approximate age (when was it originally built?)
   - Current framework/language (e.g., .NET 4.5 WebForms VB.NET, Java 8 Spring, PHP 5.6, COBOL, etc.)
   - Database(s) it uses
   - How it's deployed today (IIS, Tomcat, Apache, mainframe, etc.)

2. Do these applications talk to each other? How? (SOAP, REST, shared database, file drops, message queues, manual re-entry?)

3. Are there other systems that depend on these applications? (Reporting tools, ETL jobs, third-party integrations, mobile apps, partner feeds?)

4. Are there systems these applications depend on? (Active Directory, email servers, file shares, external APIs, mainframe systems?)
```

#### Question Group 4: Current Pain Points

```
CURRENT PAIN POINTS

This is the most important section. Be specific and honest.

1. What are the top 3-5 things that frustrate users of these applications daily?
2. What manual workarounds exist because the system can't do something?
3. What data do people export to Excel because the system can't filter/sort/report the way they need?
4. What processes require looking at multiple screens or switching between applications?
5. What breaks most often? What's the most recent outage or data issue?
6. How is the application deployed today? (CI/CD pipeline, manual file copy, FTP, drag-and-drop, etc.)
7. Can you roll back a bad deployment? How long does it take?
8. What's the current backup situation? Are backups tested?
9. Has the application been hacked, breached, or had a security incident?
10. What features have users been asking for that the current system can't support?
```

#### Question Group 5: People & Knowledge

```
PEOPLE & KNOWLEDGE

1. Who built the original system? Are they still available?
2. How many developers currently maintain it?
3. Is there documentation? (Be honest — "no" is a common and valid answer)
4. What institutional knowledge exists only in people's heads?
5. What's the team's experience with modern frameworks? (React, .NET Core, containers, cloud, etc.)
6. Is the organization open to AI-assisted development? (AI IDE, code generation, etc.)
7. Who are the key stakeholders who need to approve this project?
8. Is there executive sponsorship for modernization?
```

#### Question Group 6: Existing Infrastructure & Hardware

```
EXISTING INFRASTRUCTURE

This section is critical and often overlooked. Organizations — especially those with on-prem 
data centers (or "server closets") — frequently have significant compute, storage, and 
networking capacity that's underutilized. Discovering what already exists can dramatically 
change the architecture and cost picture.

COMPUTE
1. What servers do you currently have? (Make, model, CPU, RAM for each)
2. Are you running a hypervisor? (VMware, Hyper-V, Proxmox, bare metal?)
3. What's the current CPU/RAM utilization on your servers? (Often shockingly low)
4. Do you have any GPU hardware anywhere? (Workstations, servers, even gaming PCs?)
5. Are there decommissioned or underutilized servers sitting around?

STORAGE
1. What storage infrastructure exists? (SAN, NAS, local disks, NFS shares?)
2. What's the total capacity vs. used capacity?
3. What's the backup storage situation? (Tape, disk-to-disk, cloud backup?)
4. Is there a separate backup appliance? (Veeam, Commvault, Datto, etc.)

NETWORKING
1. What's the internet bandwidth? (Upload AND download — upload matters for cloud replication)
2. Is there a load balancer? (F5, HAProxy, Kemp, Nginx?) — if so, this replaces part of the ingress story
3. Is there a firewall appliance? (Palo Alto, Fortinet, pfSense?)
4. Is there a VPN for remote access?
5. What's the internal network speed? (1Gbps? 10Gbps?)
6. Is there a DMZ or network segmentation?

LICENSING
1. What software licenses are you currently paying for? (SQL Server, Windows Server, VMware, Office 365, etc.)
2. Are any of these under Enterprise Agreements that bundle additional products?
3. What are the renewal dates and annual costs?
4. Are there licenses you're paying for but not fully using?

CLOUD
1. Do you have existing cloud accounts? (AWS, Azure, GCP?)
2. What's currently running in the cloud? (Even small things — a single VM, a storage bucket, Azure Functions?)
3. What's the monthly cloud spend?
4. Are there cloud credits or commitments (reserved instances, savings plans)?
```

**Why this matters enormously:** In a real engagement, we initially designed for new hardware procurement. But if someone already has a half-empty VMware cluster with 128GB of unused RAM, a NetApp with 50TB free, and an F5 doing load balancing for their current apps, the infrastructure story changes completely. The F5 might replace Traefik. The existing storage might eliminate the need for a new backup solution. The unused compute might run the K3s cluster without buying anything. An underutilized GPU in a workstation might handle the AI inference workload. Every piece of existing infrastructure that can be repurposed is money not spent and time not waiting for procurement.

#### Question Group 7: Goals & Constraints

```
GOALS & CONSTRAINTS

1. What's driving the modernization? (End of support, security concerns, can't hire developers, performance problems, new feature requirements, competitive pressure, compliance mandate, other?)
2. What does success look like? (Faster feature delivery? Lower maintenance cost? Better security? New capabilities? All of the above?)
3. What's the timeline expectation? (3 months? 12 months? 24 months? "As fast as possible"?)
4. What's the budget range? (Even approximate — are we talking $50K, $250K, $1M, $5M+?)
5. Can the business tolerate downtime during cutover? How much?
6. Are there any technology mandates or restrictions? ("Must use Azure," "No open source," "Must keep Oracle," etc.)
7. Are there any upcoming deadlines that affect this? (Contract renewals, compliance audits, end-of-support dates, fiscal year boundaries?)
```

**Deliverable from Phase 0:** A "Discovery Summary" document that captures all answers, identifies the applicable regulatory frameworks, lists known constraints, and flags areas where more information is needed. This document is the foundation for everything that follows.

**Human checkpoint:** Review the Discovery Summary. Correct any misunderstandings. Fill in gaps. This is where the domain expert's knowledge is most critical — the AI can ask good questions, but only the human knows the real answers.


---

### Phase 1: Codebase Analysis

**Purpose:** The AI reads the actual source code and produces a technical assessment of each application. This is where having the codebase in the AI IDE's workspace is essential.

**Prompt for the AI assistant:**

```
I have access to the codebase for [application name(s)] in this workspace. Using the Discovery Summary from Phase 0 as context, I'm going to perform a comprehensive technical assessment of each application.

For each application, I will analyze and document:

ARCHITECTURE & STRUCTURE
- Overall architecture pattern (monolith, n-tier, microservices, etc.)
- Project structure and organization
- Entry points and request flow
- Inter-application communication patterns
- Third-party dependencies and their versions/status (EOL, deprecated, current)

CODE QUALITY & PATTERNS
- Language version and framework version
- Design patterns in use (or absence thereof)
- Code organization (separation of concerns, layering)
- Error handling patterns (or lack thereof)
- Logging and observability
- Configuration management (hardcoded values, config files, environment variables)

DATA LAYER
- Database technology and version
- Number of databases and their relationships
- ORM or data access pattern (raw SQL, stored procedures, ORM, micro-ORM)
- Schema complexity (table count, relationship density, stored proc count)
- Data access patterns (are stored procs simple CRUD wrappers or complex business logic?)
- Cross-database dependencies (linked servers, cross-DB queries, ETL jobs)
- Connection string management and credential handling

AUTHENTICATION & AUTHORIZATION
- Auth mechanism (custom, framework-provided, third-party)
- Password storage (hashed? salted? plain text?)
- Session management
- Role/permission model
- External identity provider integration (AD, LDAP, OAuth, SAML)

SECURITY POSTURE
- Known vulnerabilities in dependencies (check framework/library versions against known CVEs)
- Input validation patterns
- SQL injection exposure
- XSS exposure
- CSRF protection
- Encryption (at rest, in transit)
- Secrets management
- Compliance-relevant findings (based on the regulatory frameworks identified in Phase 0)

DEPLOYMENT & OPERATIONS
- Build process
- Deployment mechanism
- Environment configuration (dev, staging, production)
- Monitoring and alerting
- Backup and recovery
- CI/CD pipeline (or absence thereof)

BUSINESS LOGIC LOCATION
- Where does business logic live? (Application code, stored procedures, database triggers, configuration tables, external rules engines, Excel spreadsheets?)
- How much business logic is in stored procedures vs. application code?
- Are there "black box" components (compiled DLLs, undocumented services, vendor-provided modules)?

I will produce an ASSESSMENT.md document for each application with findings organized by these categories, severity ratings for issues found, and specific evidence (file paths, code snippets, configuration values) for every finding.

I will NOT make recommendations yet — that comes in Phase 3. This phase is purely diagnostic.

7 R's CLASSIFICATION
For each application (or major component), I will classify it using the 7 R's framework:
- Retain — keep as-is for now (acceptable risk, low pain, other priorities)
- Retire — decommission (no longer needed, replaced by another system)
- Rehost — move to new infrastructure without code changes (lift and shift)
- Replatform — minor changes to run on modern infrastructure (e.g., containerize without rewriting)
- Refactor — restructure code without changing external behavior (improve maintainability)
- Rearchitect — redesign the application architecture (new patterns, new data model)
- Rebuild — rewrite from scratch using modern stack

Each classification will be tagged as [Tactical] (fix the bleeding, reduce immediate risk) or [Strategic] (invest in the future, enable new capabilities), with justification based on the codebase findings and business context from Phase 0.

This is a diagnostic classification, not a design recommendation — it answers "what kind of modernization does this need?" and frames the scope for Phase 3.
```

**Scaling note:** If the codebase is too large to read directly (millions of lines, hundreds of projects), consider:
1. **Prioritized analysis** — start with entry points (controllers, pages, API endpoints), configuration files, data access layer, and authentication. These tell you 80% of what you need to know.
2. **Vector DB approach** — chunk the codebase into files/functions, embed them, and use semantic search to find relevant code when analyzing specific concerns. This works well for "find all places where SQL is constructed dynamically" or "find all authentication checks."
3. **Static analysis tools first** — run SonarQube, Snyk, or equivalent to get a machine-readable inventory of issues, then feed that to the AI for interpretation and prioritization.

**Deliverable from Phase 1:** An `ASSESSMENT.md` per application. Factual findings with evidence. No recommendations yet.

**For multi-application engagements:** After all per-app assessments are complete, produce a `CONSOLIDATED_ASSESSMENT.md` that analyzes the application landscape as a whole. This is where the real insights emerge. Map shared data sources — which apps read/write the same databases and tables. Identify data ownership conflicts, overlapping business logic implemented differently in different apps, hidden coupling (App A's stored proc called by App B, App C reading App A's audit tables), and data consistency issues (the same customer represented differently across three systems). Document the "blast radius" of schema changes — if you alter a table, which apps break?

The consolidated view almost always reveals things invisible in per-app assessments: migration sequencing constraints (you can't touch the shared database until all consuming apps are ready), consolidation opportunities (three apps maintaining separate customer records should share one source of truth), and risk amplification (a change that looks safe for one app breaks two others). When multiple applications share a data source, it fundamentally changes the migration strategy, timeline, and risk profile.

**Human checkpoint:** The domain expert reviews each assessment. Key questions:
- "Is this finding accurate? Or is there context I'm missing?"
- "You found X — is that intentional or a bug?"
- "You didn't mention Y — that's actually a major issue, here's why..."
- "This component you flagged as a black box — here's what it actually does..."

The corrections from this review are critical. The AI will miss things that only someone who's worked with the system daily would know. It will also flag things that look like problems but are intentional design decisions. Both types of corrections improve the plan.

---

### Phase 2: Stakeholder Pain Point Assessment

**Purpose:** Before designing the target architecture, understand what the current system fails to do from the perspective of the people who use it every day. The codebase tells you what the system does. The users tell you what it doesn't do.

**Prompt for the AI assistant:**

```
Based on the codebase assessment and the initial pain points from the Discovery Interview, I need to conduct a deeper pain point assessment. This should be done as a structured conversation with actual users of each application — not just the technical team.

For each user role (e.g., trader, warehouse staff, finance, customer, admin), ask:

1. Walk me through your typical day using this system. What do you do first? What takes the longest?
2. What's the most frustrating thing about the system?
3. What workarounds have you invented? (Excel exports, manual tracking, sticky notes, separate tools?)
4. What information do you need that requires looking at multiple screens or asking someone else?
5. What would you change if you could change one thing?
6. What do your customers/partners complain about?
7. What reports or data views do you wish existed?
8. Are there tasks you avoid because the system makes them too painful?
9. What happens when [the system] goes down? How do you work?
10. If I could give you a magic feature that doesn't exist today, what would it be?

Compile the answers into a PAIN POINTS document organized by:
- User role
- Pain point description
- Business impact (time wasted, revenue lost, risk created, customer satisfaction affected)
- Frequency (daily, weekly, monthly, occasional)
- Current workaround (if any)
- Preliminary solution category (architecture fix, new feature, buy vs. build, process change)
```

**Why this phase exists separately:** In the engagement that produced this methodology, the most valuable insights came from the domain expert describing real workflows — a 1,500-line sourcing page, a printed-barcode-in-photo warehouse workflow, a personal-email org-hopping problem. None of these were visible in the code alone. The code showed a Repeater control loading a DataSet. The human explained why that was a nightmare for users working large requests. That context changed the entire data architecture (Redis working set cache, SignalR real-time collaboration).

**Deliverable from Phase 2:** A prioritized pain point matrix that feeds directly into the target architecture design. Pain points that affect daily work for many users get higher priority than edge cases.

**Human checkpoint:** Users review their own pain points for accuracy. Management reviews for business priority alignment. This is also where new pain points surface that nobody mentioned in the initial interview — seeing other people's pain points triggers "oh yeah, we also have this problem..."


---

### Phase 3: Target Architecture Design

**Purpose:** Propose the future-state architecture based on everything learned in Phases 0-2. This is where the AI makes recommendations — and where the human pushback is most valuable.

**Prompt for the AI assistant:**

```
Using the Discovery Summary (Phase 0), Codebase Assessments (Phase 1), and Pain Point Matrix (Phase 2), design a target architecture for the modernized system.

BEFORE DESIGNING ANYTHING — RATIONALIZE THE LANDSCAPE:
Step back and question whether the current application landscape makes sense:
- Should these remain separate applications, or should some be consolidated?
- Should any applications or major features be retired entirely?
- Are there workflows that exist because of old technical limitations that no longer apply?
- Do the current application boundaries match how the business actually works today?
Produce a rationalized view of what the organization actually needs. This frames everything that follows.

IMPORTANT CONSTRAINTS:
- Containerized deployment (Docker + Kubernetes/K3s). The architecture must run identically on-premises and in any major cloud.
- This is NOT a 1:1 translation. Rethink the architecture based on what the system SHOULD be, not what it currently is.
- Every component choice must include a Buy vs. Build analysis.
- All technology choices must be validated against the compliance frameworks identified in Phase 0.
- Assume AI-assisted development unless the organization has explicitly ruled it out.

For the target architecture, address each of these areas:

1. APPLICATION ARCHITECTURE
   - Monolith, modular monolith, or microservices? (Justify the choice — don't default to microservices because it sounds modern. A modular monolith is often the right answer for small-to-mid organizations.)
   - How many deployable units?
   - How do they communicate? (REST, gRPC, message queue, shared database?)
   - What are the module/service boundaries? (Map to business domains, not technical layers)

2. FRONTEND STRATEGY
   - How many frontends? (Public-facing vs. internal often have different requirements)
   - SPA, SSR, SSG, or hybrid? (Justify based on SEO needs, interactivity requirements, user base)
   - Framework recommendation with rationale
   - Mobile strategy (responsive web, PWA, native app?)
   - Accessibility requirements
   - For public-facing applications — SEO and AI discoverability drive the tech stack:
     - SEO requirements constrain framework and rendering choices. A site that depends on search traffic cannot be a client-only SPA — this alone may determine the frontend framework (Next.js, Nuxt, Astro, etc. over pure React/Vue/Angular SPA)
     - SSR for dynamic content that needs crawlability, SSG for content that changes infrequently, hybrid for sites with both
     - Structured data / schema.org markup for rich search results
     - Core Web Vitals optimization (LCP, INP, CLS) — these are ranking factors
     - AI and agent discoverability — structured data, clean semantic HTML, and machine-readable content that AI search engines (Google AI Overviews, Perplexity, ChatGPT search) and autonomous agents can parse and act on. This is the next frontier of SEO.
     - If the current site depends on SEO for traffic or revenue, SEO is an architectural constraint that drives the tech stack. Document this explicitly.

3. BACKEND STRATEGY
   - Language and framework recommendation with rationale
   - API design (REST, GraphQL, or both?)
   - Background job processing
   - Real-time communication needs (WebSockets, SSE?)
   - File storage and document management

4. DATA ARCHITECTURE
   - Database technology recommendation with rationale
   - Schema design approach (single DB with schemas, multiple DBs, polyglot persistence?)
   - Data access strategy (ORM, micro-ORM, raw SQL, stored procedures — and WHY)
   - Caching strategy (what needs caching, what technology, what invalidation pattern?)
   - Search strategy (database full-text search, dedicated search engine, vector search?)
   - Data migration approach (how does data move from old to new?)

5. AUTHENTICATION & AUTHORIZATION
   - Identity provider recommendation (Buy vs. Build — almost always Buy)
   - Internal vs. external user handling
   - Role and permission model
   - SSO/federation requirements
   - API authentication for external consumers

6. INFRASTRUCTURE & DEPLOYMENT
   - Container orchestration (K3s, K8s, ECS, etc.)
   - CI/CD pipeline design
   - Environment strategy (dev, staging, production)
   - Monitoring, logging, and alerting stack
   - Secrets management
   - SSL/TLS and edge security (CDN, WAF, DDoS protection)
   - DNS and domain strategy

7. AI/ML INTEGRATION (if applicable)
   - What tasks could AI improve? (Document parsing, classification, search, recommendations, anomaly detection, content generation?)
   - On-premises vs. cloud AI (based on compliance constraints from Phase 0)
   - Model selection (small open models vs. cloud APIs — justify based on task complexity and data sensitivity)
   - Inference infrastructure (if on-prem)
   - Embedding and vector search strategy

8. MODERN CAPABILITIES OPPORTUNITY SCAN
   Starting from the pain points (Phase 2), systematically check whether modern tools and practices could solve specific problems better than a traditional implementation. This is problem-first, not tool-first — every opportunity must trace back to a real pain point or measurable business outcome.
   
   Check each category against the pain points:
   - Caching / working sets (Redis, Memcached) — large datasets loaded repeatedly? Slow queries? Users competing for the same records?
   - Real-time communication (SignalR, WebSockets, SSE) — users refreshing to see updates? Collaboration workflows? Live dashboards?
   - Event-driven / messaging (RabbitMQ, Redis Streams, Kafka) — tightly coupled processes that should be async? Long-running operations blocking users?
   - API management (Kong, Traefik, API gateway) — multiple consumers? Partner integrations? Rate limiting or monetization?
   - Full-text / semantic search (Typesense, Meilisearch, Elasticsearch, vector search) — users can't find what they need? Unstructured document corpus?
   - MCP / agent integration — workflows that could be exposed as tools for AI agents? Data that external systems should query programmatically?
   - Workflow orchestration (Temporal, state machines) — complex multi-step processes? Approval chains? Long-running sagas?
   
   For each opportunity: describe the pain point it solves, estimate impact and effort, then red team it — would a simpler approach work? Does the team have the skills? Is the complexity justified? If it doesn't trace to a real problem, discard it.

9. DISASTER RECOVERY & BUSINESS CONTINUITY
   - RPO and RTO targets (based on business impact analysis)
   - Backup strategy
   - Failover architecture
   - Geographic risk factors (natural disasters, power grid reliability)

10. BUY VS. BUILD ANALYSIS
   For every component that could be purchased or deployed as an existing product, provide:
   - Component name and function
   - Build estimate (effort, ongoing maintenance)
   - Buy options (product name, licensing model, cost)
   - Recommendation with rationale
   
   Common buy candidates:
   - Authentication/Identity (Keycloak, Auth0, Okta, Azure AD B2C)
   - CMS (Strapi, Directus, Contentful, Sanity)
   - PDF generation (Gotenberg, wkhtmltopdf, commercial libraries)
   - Email delivery (SES, SendGrid, Postmark)
   - Monitoring (Grafana Cloud, Datadog, New Relic)
   - Analytics/BI (Metabase, Superset, Redash, Looker)
   - Search (Typesense, Meilisearch, Elasticsearch, Algolia)
   - Background jobs (Hangfire, Quartz, Celery, Bull)
   - Message queue (RabbitMQ, Redis Streams, SQS, Kafka)
   - Object storage (MinIO, S3, Azure Blob)
   - Log aggregation (Loki, ELK, Splunk)

11. WHAT GETS ELIMINATED
    List every current component, technology, or practice that disappears in the new architecture and what replaces it.

12. WHAT GETS PRESERVED
    List anything from the current system that should be kept as-is (working integrations, external services, data that doesn't need migration).

13. CROSS-APPLICATION IMPACT (for multi-app modernizations)
    - How do changes to each application affect other applications in the landscape?
    - What shared databases, APIs, file drops, or message queues create coupling?
    - What's the migration sequence considering inter-app dependencies?
    - What temporary integration bridges are needed during phased migration?

14. RED TEAM REVIEW
    For every major architecture recommendation:
    - Present at least one credible alternative approach
    - Argue against your own recommendation — what could go wrong? What assumptions might be wrong?
    - Document mitigations for the risks identified
    - Tag each recommendation as [Tactical] (addresses immediate pain) or [Strategic] (invests in future capability)

Present the architecture with:
- ASCII or text-based architecture diagrams
- A technology stack table with justification for each choice
- A comparison table: current state vs. target state for each major concern
- Explicit callouts for every compliance-relevant decision
- Tactical vs. Strategic tags on every major recommendation
```

**Human checkpoint:** This is the most important review point. The domain expert and stakeholders should challenge every recommendation:
- "Why this framework and not that one?"
- "You're proposing microservices but we only have 2 developers — is that realistic?"
- "You didn't account for [specific workflow] — here's why that changes things..."
- "We tried [technology X] before and it didn't work because..."
- "Our team has deep experience in [Y] — shouldn't we leverage that?"

**Expect multiple revision cycles.** The first draft of the target architecture is a starting point for discussion, not a final answer. The plan that produced this methodology went through at least 6 major revisions based on stakeholder feedback, and each revision made it substantially better.

**Deliverable from Phase 3:** A Target Architecture document with diagrams, technology choices, buy vs. build decisions, and compliance validation. This becomes the technical foundation for the modernization plan.


---

### Phase 4: Modernization Plan

**Purpose:** Turn the target architecture into an actionable, phased implementation plan with timelines, cost estimates, risk assessment, and explicit unknowns.

**Prompt for the AI assistant:**

```
Using the Target Architecture (Phase 3), the Pain Point Matrix (Phase 2), and the Codebase Assessments (Phase 1), create a detailed modernization plan.

The plan must include:

1. MIGRATION PHASES
   For each phase:
   - What gets built (specific modules, features, infrastructure)
   - Dependencies (what must be done before this phase can start)
   - Data migration required for this phase
   - Deliverable (what's usable at the end of this phase — each phase should deliver working functionality, not just infrastructure)
   - Estimated effort (in weeks, with assumptions stated)
   - Risk factors specific to this phase
   - Human checkpoint / validation criteria

   Phase ordering principles:
   - Security fixes first (Phase 0) — on the existing system, before modernization begins
   - Foundation infrastructure next (auth, database, API skeleton, CI/CD)
   - Highest-pain-point modules early (build credibility and momentum)
   - Modules with external dependencies or long lead times started early
   - Revenue-generating or cost-saving features prioritized over nice-to-haves
   - Each phase should be independently valuable — if the project stops after Phase N, the organization still has something useful

2. DATA MIGRATION STRATEGY
   - Per-database or per-schema migration plan
   - Identity/auth migration (this is always the hardest — plan accordingly)
   - Data transformation requirements (schema changes, type conversions, encoding)
   - Validation approach (how do you verify the migrated data is correct?)
   - Rollback plan (what if the migration has errors?)
   - ETL/job replacement plan (what scheduled jobs exist today and what replaces them?)

3. CUTOVER STRATEGY
   - Build-then-cutover vs. parallel operation vs. strangler fig pattern (justify the choice)
   - Can any components cut over independently?
   - Downtime requirements and mitigation
   - DNS/routing changes
   - Rollback procedure if cutover fails
   - Post-cutover monitoring plan

4. TIMELINE
   - Development time estimates (with AI-assisted development)
   - Development time estimates (traditional, for comparison)
   - Logistics overhead — scaled to organization size. This is the most underestimated factor in modernization timelines:
     - Small orgs (< 50 employees): hardware procurement and setup (2-6 weeks ordering + 1-3 weeks install/config), cloud/license setup (1-2 weeks each). Fewer approval gates but physical infrastructure work takes real time. Typical multiplier: 1.3-1.5x development time.
     - Mid-size orgs (50-500 employees): procurement processes, approval gates, change management boards, legal review of license terms (2-4 weeks each). Typical multiplier: 1.5-2x development time.
     - Large orgs / enterprise (500+ employees): capital budget cycles, RFP processes, architecture review boards, security reviews, cross-team coordination (network, DBA, security, infrastructure — each with their own queue), environment provisioning approvals, change advisory boards. Individual items take 4-16 weeks. Typical multiplier: 2-3x development time.
   - Realistic calendar timeline (development time × logistics multiplier)
   - Key milestones and decision points
   - Long-lead-time items that should start immediately (hardware orders, license negotiations, security architecture reviews)

5. COST ESTIMATE
   - Development cost (labor)
   - Infrastructure cost (hardware, cloud, licensing)
   - Ongoing operational cost (hosting, licensing, maintenance)
   - Cost of doing nothing (what does it cost to maintain the status quo for another 1, 3, 5 years?)
   - Licensing costs eliminated by modernization

6. RISK ASSESSMENT
   For each identified risk:
   - Description
   - Probability (low/medium/high)
   - Impact (low/medium/high)
   - Mitigation strategy
   - Contingency plan

   Common risks to always assess:
   - Key person dependency (what if the lead developer leaves?)
   - Scope creep (how do you manage feature requests during migration?)
   - Data migration errors (how do you detect and recover?)
   - Integration breakage (what external systems might break?)
   - Performance regression (new system slower than old for specific workflows?)
   - User adoption resistance (people hate change — how do you manage it?)
   - Vendor/technology risk (what if a chosen technology is abandoned?)
   - Timeline overrun (what's the impact of 50% schedule slip?)
   - Budget overrun (what's the contingency?)

7. BEYOND 1:1 — VALUE-ADD FEATURES
   Based on the pain points, industry analysis, and modern capabilities, identify features that go beyond replacing what exists today:
   - AI-powered automation of manual workflows
   - Analytics and business intelligence that didn't exist before
   - Customer-facing improvements (self-service, real-time updates, mobile access)
   - Integration opportunities (APIs, partner connections, marketplace presence)
   - Competitive advantages enabled by the new architecture
   
   For each value-add feature:
   - What it does
   - Why it matters (business impact)
   - Estimated additional effort
   - Which phase it fits into
   - Dependencies

   These features often have ROI that dwarfs the modernization cost itself. Don't treat them as optional extras — they're the reason modernization is worth doing.

8. OPEN QUESTIONS
   Everything you don't know and didn't guess. For each:
   - The question
   - What you assumed in the absence of an answer
   - Why the real answer matters
   - How to get the answer
   - What changes in the plan if the assumption is wrong

9. KEY DESIGN DECISIONS
   A summary of every major architectural decision with:
   - The decision
   - The alternatives considered
   - Why this option was chosen
   - What would change the decision
```

**Human checkpoint:** The domain expert and stakeholders review the full plan. Key questions:
- "Are the phase priorities right? Should anything move earlier or later?"
- "Are the effort estimates realistic given our team and constraints?"
- "Are there dependencies or risks you've missed?"
- "Do the value-add features align with our business strategy?"
- "Are the open questions the right ones? Can we answer any of them now?"

**Deliverable from Phase 4:** The comprehensive Modernization Plan document — the main deliverable of the entire process.


---

### Phase 5: Supporting Documents

**Purpose:** Produce the documents that stakeholders beyond the technical team need to approve, fund, and support the project.

**Prompt for the AI assistant:**

```
Using the Modernization Plan (Phase 4) and Discovery Summary (Phase 0), produce the following supporting documents:

1. BUSINESS CASE
   - Executive summary (one page, non-technical)
   - Problem statement (what's wrong with the status quo, in business terms)
   - Proposed solution (high-level, non-technical)
   - Cost breakdown (development, infrastructure, ongoing)
   - ROI analysis (cost savings, revenue opportunities, risk reduction, quantified where possible)
   - Timeline summary
   - Risk summary (top 5 risks with mitigations)
   - Recommendation
   
   The business case should be readable by a CFO or CEO who doesn't know what Kubernetes is. Translate technical benefits into business outcomes: "faster feature delivery" → "respond to market changes in days instead of months." "Containerized deployment" → "system updates with zero downtime and instant rollback if something goes wrong."

2. DISASTER RECOVERY PLAN (if applicable based on Phase 0 findings)
   - Current state assessment (what happens today if the building floods / server dies / ransomware hits?)
   - RPO/RTO targets
   - Backup strategy (what, where, how often, how tested)
   - Failover architecture
   - Recovery procedures (step-by-step runbooks)
   - Testing schedule
   - Cost of DR infrastructure
   - Geographic and environmental risk factors

3. STAFFING RECOMMENDATION
   - Honest assessment of current team capacity (can they absorb this alongside maintenance?)
   - ALL staffing models with trade-offs: internal reallocation, new hire, contract, hybrid, managed service
   - Full cost model for new hires — not just salary. Include benefits (25-40%), recruiting ($15K-$50K), onboarding (1-3 months ramp), equipment, management overhead, and bad-hire risk. A "$150K developer" costs $200K-$240K/year fully loaded.
   - Full cost model for contractors — hourly rate × duration, plus procurement overhead, onboarding, knowledge transfer risk
   - Skills gap analysis (what's needed vs. what exists)
   - Key person risk assessment
   - Knowledge transfer plan
   - Post-modernization maintenance staffing
   - Present options with trade-offs, not a predetermined answer — the right model depends on org size, capacity, budget structure, and risk tolerance

4. SECURITY REMEDIATION PLAN (Phase 0 items)
   - Critical vulnerabilities to fix immediately (before modernization begins)
   - Prioritized by severity and exploitability
   - Specific remediation steps for each
   - Estimated effort
   - Verification criteria
```

**Human checkpoint:** Each document gets reviewed by its target audience:
- Business case → management/finance
- DR plan → IT/operations
- Staffing recommendation → management/HR
- Security remediation → IT/security

**Deliverable from Phase 5:** A complete document package ready for executive review and project approval.

---

### Phase 6: Validation & Iteration

**Purpose:** The plan is never done after one pass. This phase is about testing the plan against reality and refining it.

**Activities:**

1. **Answer the open questions.** Go get the information that was missing. Every answered question may change part of the plan.

2. **Prototype the riskiest parts.** If the plan depends on a technology choice that nobody on the team has used, build a small proof of concept before committing. A 2-day prototype that proves (or disproves) a key assumption is worth more than a month of planning.

3. **Validate cost estimates.** Get actual quotes for hardware, licensing, and cloud costs. Check the labor estimates against similar projects the team has done.

4. **Stakeholder review cycle.** Present the plan to everyone who has a stake in it. Collect feedback. Revise. Repeat until there's consensus (or at least informed consent).

5. **Update the plan.** Every correction, every answered question, every prototype result feeds back into the plan. The plan is a living document until the project starts — and even then, it evolves as reality diverges from assumptions.

---

## Regulatory Quick Reference

When the AI identifies the organization's industry and regulatory environment in Phase 0, these frameworks should be carried forward into every subsequent decision:

| Framework | Industry | Key Technical Implications |
|---|---|---|
| HIPAA | Healthcare | Encryption at rest and in transit mandatory. BAA required with all vendors touching PHI. Access logging with 6-year retention. Minimum necessary access principle. No PHI in logs or error messages. |
| PCI-DSS | Payment processing | Network segmentation required. No storage of full card numbers post-authorization. Quarterly vulnerability scans. Annual penetration testing. Specific encryption requirements. |
| SOX | Public companies | Audit trails for financial data changes. Segregation of duties in financial workflows. Change management controls. Data retention requirements. |
| ITAR/EAR | Defense/aerospace | Controlled data may not leave US jurisdiction. Cloud services must be US-only (or on-prem). Foreign persons access restrictions. Self-hosted AI models may be required. Export classification for technical data. |
| FERPA | Education | Student records require consent for disclosure. Parents have access rights. Directory information exceptions. Annual notification requirements. |
| CMMC/DFARS | Defense contractors | NIST 800-171 controls required. CUI handling procedures. Incident reporting within 72 hours. Assessment and certification requirements. |
| GDPR | EU data subjects | Right to erasure. Data portability. Consent management. DPO requirement. 72-hour breach notification. Data processing agreements. Privacy by design. |
| FDA 21 CFR Part 11 | Pharma/medical devices | Electronic signatures with same legal weight as handwritten. Audit trails for all record changes. System validation requirements. Access controls. |
| NRC 10 CFR 73.54 | Nuclear facilities | Cyber security program required. Isolation of safety systems. Specific access control requirements. Incident response procedures. |
| State gaming regulations | Casinos | Varies by state. Generally: detailed audit trails, specific data retention, regulatory reporting, system certification requirements. |
| SOC 2 | Service organizations | Trust service criteria: security, availability, processing integrity, confidentiality, privacy. Annual audit. Continuous monitoring. |

**The AI should not attempt to provide legal compliance advice.** It should identify which frameworks apply, flag decisions that have compliance implications, and recommend that the organization's compliance team or legal counsel validate the technical approach against regulatory requirements.


---

## Scaling: When the Codebase Is Too Large

The engagement that produced this methodology involved three applications totaling roughly 100K-200K lines of code — manageable within an AI IDE's context window with careful navigation. Enterprise codebases can be millions of lines across hundreds of projects. The methodology still applies, but the tooling needs to adapt.

### Option 1: Prioritized Direct Analysis

Don't try to read everything. Focus on:
1. **Entry points** — controllers, pages, API endpoints, main classes. These reveal the application's surface area.
2. **Configuration** — web.config, appsettings.json, docker-compose, deployment scripts. These reveal infrastructure dependencies.
3. **Data access layer** — ORM mappings, database contexts, connection strings, stored procedure references. This reveals the data architecture.
4. **Authentication** — login flows, session management, token handling. This reveals the security posture.
5. **Build/deploy** — CI/CD configs, build scripts, Dockerfiles. This reveals operational maturity.

This gets you 80% of the assessment with 20% of the codebase read.

### Option 2: Vector Database Approach

For very large codebases:

1. **Chunk the codebase** — split every file into logical chunks (functions, classes, or fixed-size segments with overlap).
2. **Embed each chunk** — use a code-aware embedding model (e.g., `code-search-ada-002`, `voyage-code-2`, or an open-source alternative).
3. **Store in a vector database** — Chroma, Qdrant, Weaviate, pgvector, or similar.
4. **Query semantically during analysis** — instead of reading files sequentially, ask questions: "Find all SQL injection vulnerabilities," "Find all authentication checks," "Find all database connection patterns," "Find all error handling patterns."

This approach scales to any codebase size and enables analysis patterns that sequential reading can't match (e.g., "find every place in the codebase where user input reaches a SQL query without parameterization").

**Trade-off:** You lose the holistic understanding that comes from reading code in context. The AI sees fragments, not flows. Combine with Option 1 (read the critical paths directly, use vector search for cross-cutting concerns).

### Option 3: Static Analysis First

Run automated tools before the AI analysis:
- **SonarQube** — code quality, security vulnerabilities, technical debt measurement
- **Snyk / Dependabot** — dependency vulnerability scanning
- **NDepend / Structure101** — architecture visualization, dependency analysis
- **CLOC** — lines of code, language breakdown
- **Database schema tools** — generate ERD diagrams, stored proc inventories

Feed the tool outputs to the AI as structured data. The AI interprets and prioritizes rather than discovering from scratch. This is faster and more reliable for large codebases, but misses the nuanced understanding that comes from reading actual code.

**Recommended approach for large codebases:** Option 3 (automated tools) + Option 1 (prioritized direct analysis of critical paths) + Option 2 (vector search for specific concerns). Use all three.

---

## Anti-Patterns: What Not to Do

These are mistakes we've seen (or almost made) that the methodology should prevent:

**Don't translate line-by-line.** Converting VB.NET to C# or COBOL to Java without rethinking the architecture produces modern-looking code with legacy problems. The 9-database architecture, the SOAP middleware layer, the ViewState-based state management — these are architectural decisions that should be revisited, not preserved in a new language.

**Don't default to microservices.** Microservices add operational complexity (service discovery, distributed tracing, network failure handling, data consistency across services). For a team of 1-5 developers, a modular monolith with clear module boundaries gives you the same code organization benefits without the operational overhead. You can always extract a module into a service later if it needs independent scaling.

**Don't ignore the data layer.** The sexiest part of modernization is the new frontend. The hardest part is the data migration. Stored procedures with embedded business logic, cross-database dependencies, implicit data relationships, and decades of accumulated data quality issues — this is where projects fail. Budget more time for data than you think you need.

**Don't skip the pain point assessment.** Building a pixel-perfect replica of the old system in a new framework is not modernization — it's expensive nostalgia. Talk to users. Find out what's broken. Build something better.

**Don't plan for parallel operation unless you have an API layer.** If the legacy system uses direct database access (LINQ to SQL, raw ADO.NET, JDBC, etc.) with no API layer, there's no clean way to keep old and new systems in sync during a transition. Build-then-cutover is usually the honest answer. Parallel operation sounds safer but the data synchronization complexity can exceed the modernization effort itself.

**Don't underestimate identity migration.** Moving users from a custom auth system to a modern identity provider (Keycloak, Auth0, etc.) is always harder than it looks. Password hashing algorithm differences, session management changes, role model differences, and the "force password reset" user experience all need careful planning.

**Don't forget the ETL.** Legacy systems often have a shadow infrastructure of scheduled jobs, SSIS packages, cron scripts, and manual data transfers that nobody thinks about until they break. Inventory these early.

**Don't build AI features for the sake of AI.** AI should solve specific, measurable problems identified in the pain point assessment. "Add AI" is not a requirement. "Reduce document intake time from 60 minutes to 5 minutes using AI-powered parsing" is a requirement that happens to use AI.

**Don't assume the first draft is the final plan.** The plan that produced this methodology went through 6+ major revisions. Each revision was triggered by the domain expert saying "that's wrong, here's why" or "you missed something important." Build in time for iteration. The plan gets better with every correction.

---

## What This Methodology Produces

At the end of the process, the organization has:

1. **Per-application Architecture Assessments** — detailed technical findings with evidence
2. **Prioritized Pain Point Matrix** — what hurts most, ranked by business impact
3. **Target Architecture Design** — with diagrams, technology choices, buy vs. build decisions, and compliance validation
4. **Comprehensive Modernization Plan** — phased implementation with timelines, costs, risks, and explicit unknowns
5. **Business Case** — executive-readable justification with ROI analysis
6. **Disaster Recovery Plan** — if applicable
7. **Staffing Recommendation** — who does the work and why
8. **Security Remediation Plan** — what to fix immediately

These documents are living artifacts. They evolve as questions get answered, prototypes reveal new information, and stakeholder feedback refines priorities. The methodology is designed to produce a first draft quickly and improve it through iteration — not to produce a perfect plan on the first pass.

---

## Honest Limitations

**The AI will be wrong about some things.** It will misunderstand business logic, make incorrect assumptions about workflows, and propose solutions that don't fit the organization's culture or constraints. This is expected and normal. The methodology accounts for it by building in human review at every phase. The corrections are the most valuable part of the process.

**Domain expertise cannot be replaced.** An AI can read code and identify patterns. It cannot understand why a particular workflow exists, what regulatory requirement drove a design decision, or what the political dynamics are between departments. The domain expert's participation is not optional.

**Effort estimates are estimates.** They're based on patterns from similar projects, adjusted for the specific codebase and team. They will be wrong. Build in contingency (20-30% minimum) and update estimates as the project progresses.

**Compliance validation requires human experts.** The AI can identify which regulatory frameworks apply and flag decisions that have compliance implications. It cannot certify that an architecture is compliant. That requires legal counsel, compliance officers, and in some cases, third-party auditors.

**This methodology works best with AI IDE access to the codebase.** Without it, the codebase analysis phase becomes manual and significantly slower. The methodology can still be followed with manual code review, but the timeline for Phase 1 increases substantially.

---

## Getting Started

1. Open the codebase in an AI IDE (Kiro, Cursor, or equivalent)
2. Start a conversation with the AI assistant
3. Paste or reference the Phase 0 Discovery Interview questions
4. Answer honestly — "I don't know" is always a valid answer
5. Let the process unfold phase by phase, with human review at every checkpoint
6. Expect iteration — the first draft of everything will be improved by your corrections
7. The goal is not a perfect plan — it's a good-enough plan that gets better as you learn more

The best modernization plan is one that's honest about what it knows, what it doesn't know, and what could go wrong. This methodology is designed to produce exactly that.


---

*Developed by [Burly Mingo LLC](https://burlymingo.com). Built with [Kiro](https://kiro.dev) + [Claude Opus 4.6](https://anthropic.com).*

*This methodology is open source under the MIT license. It produces the plan — executing that plan is a separate challenge. Moving to containers, Kubernetes, and a new language/framework is a significant undertaking. Many organizations will (and should) bring in experienced consulting partners to implement. The value of having a detailed blueprint first is that you can scope the engagement precisely, compare vendor proposals against a real spec, and hold implementation partners accountable to defined deliverables. A good plan is leverage.*
