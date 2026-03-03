# Phase 1: Codebase Analysis — Requirements

## Overview
Read the actual source code and produce a factual technical assessment of each application. No recommendations yet — this phase is purely diagnostic.

## Requirements

### REQ-1: Architecture & Structure Assessment
- [ ] Identify architecture pattern (monolith, n-tier, microservices, etc.)
- [ ] Map project structure and organization
- [ ] Trace entry points and request flow
- [ ] Document inter-application communication patterns
- [ ] Inventory third-party dependencies with version/EOL status

### REQ-2: Code Quality & Patterns Assessment
- [ ] Identify language and framework versions
- [ ] Document design patterns in use (or absence thereof)
- [ ] Assess code organization and separation of concerns
- [ ] Evaluate error handling patterns
- [ ] Assess logging and observability
- [ ] Review configuration management (hardcoded values, config files, env vars)

### REQ-3: Data Layer Assessment
- [ ] Identify database technology, version, and instance count
- [ ] Map all databases and their relationships
- [ ] Document ORM/data access patterns
- [ ] Assess schema complexity (table count, relationships, stored proc count)
- [ ] Identify cross-database dependencies
- [ ] Review connection string management and credential handling

### REQ-4: Security Posture Assessment
- [ ] Check password storage (hashed? salted? plain text?)
- [ ] Assess authentication and session management
- [ ] Check for SQL injection exposure
- [ ] Check for XSS and CSRF protection
- [ ] Review encryption (at rest, in transit)
- [ ] Assess secrets management
- [ ] Flag compliance-relevant findings per Phase 0 regulatory frameworks

### REQ-5: Business Logic Location Map
- [ ] Determine where business logic lives (app code, stored procs, triggers, config tables, external?)
- [ ] Quantify stored procedure vs. application code business logic ratio
- [ ] Identify black-box components (compiled DLLs, undocumented services)

### REQ-6: Deployment & Operations Assessment
- [ ] Document build process
- [ ] Document deployment mechanism
- [ ] Assess environment configuration (dev, staging, prod)
- [ ] Evaluate monitoring and alerting
- [ ] Review backup and recovery procedures

### REQ-7: 7 R's Classification (Per Application)
- [ ] For each application (or major component), classify using the 7 R's framework:
  - Retain — keep as-is for now (acceptable risk, low pain, other priorities)
  - Retire — decommission (no longer needed, replaced by another system)
  - Rehost — move to new infrastructure without code changes (lift and shift)
  - Replatform — minor changes to run on modern infrastructure (e.g., containerize without rewriting)
  - Refactor — restructure code without changing external behavior (improve maintainability)
  - Rearchitect — redesign the application architecture (new patterns, new data model)
  - Rebuild — rewrite from scratch using modern stack
- [ ] Justify the classification based on codebase findings, pain points, and business value
- [ ] Tag each classification as **[Tactical]** (fix the bleeding, reduce immediate risk) or **[Strategic]** (invest in the future, enable new capabilities)
- [ ] Identify applications where the classification is uncertain — flag as open questions for stakeholder input

## Deliverable
An **ASSESSMENT.md** per application with findings organized by category, severity ratings, and specific evidence (file paths, code snippets, configuration values).

For multi-application engagements, also produce a **CONSOLIDATED_ASSESSMENT.md** that analyzes the application landscape as a whole:

### REQ-8: Consolidated Cross-Application Assessment (Multi-App Only)
- [ ] Map shared data sources — which applications read/write to the same databases, schemas, or tables?
- [ ] Identify data ownership conflicts — when multiple apps write to the same tables, who owns the schema? Who owns the data?
- [ ] Document overlapping business logic — are the same calculations, validations, or workflows implemented differently in different apps?
- [ ] Map shared infrastructure — common auth systems, shared file storage, shared message queues, shared caches
- [ ] Identify hidden coupling — App A's stored proc is called by App B, App C reads App A's audit tables, App B writes to a queue that App A processes
- [ ] Assess data consistency risks — are the same entities (customers, orders, products) represented differently across apps? Different field names, different formats, different update frequencies?
- [ ] Document the "blast radius" of schema changes — if you alter a table, which apps break?
- [ ] Identify consolidation opportunities — shared databases that should become a single service, duplicated logic that should be unified, separate auth systems that should merge
- [ ] Produce a dependency map showing all inter-application data flows, shared resources, and coupling points

This consolidated view often reveals insights invisible in per-app assessments: migration sequencing constraints (you can't touch the shared database until all apps are ready), consolidation opportunities (three apps maintaining separate customer records should share one), and risk amplification (a schema change that looks safe for App A breaks Apps B and C).

## Scaling Note
If the codebase is too large for direct analysis:
1. Prioritize: entry points, config, data access, auth, build/deploy (80% of insight from 20% of code)
2. Use static analysis tools (SonarQube, Snyk) for automated scanning, feed results to AI for interpretation
3. Consider vector DB approach for semantic code search across very large codebases

## Human Checkpoint
Domain expert reviews each assessment. Key questions: "Is this accurate?" "Is there context I'm missing?" "You didn't mention X — that's actually important." Corrections feed into Phase 2.
