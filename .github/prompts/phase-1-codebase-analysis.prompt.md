# Phase 1: Codebase Analysis

**Purpose:** Read the actual source code and produce a factual technical assessment of each application. No recommendations yet — this phase is purely diagnostic.

## What to Analyze

For each application in the workspace, assess:

### Architecture & Structure
- Architecture pattern (monolith, n-tier, microservices, etc.)
- Project structure and organization
- Entry points and request flow
- Inter-application communication patterns
- Third-party dependencies with version/EOL status

### Code Quality & Patterns
- Language and framework versions
- Design patterns in use (or absent)
- Code organization and separation of concerns
- Error handling, logging, observability
- Configuration management (hardcoded values, config files, env vars)

### Data Layer
- Database technology, version, instance count
- Database relationships and cross-database dependencies
- ORM/data access patterns
- Schema complexity (tables, relationships, stored proc count)
- Connection string management and credential handling

### Security Posture
- Password storage (hashed? salted? plain text?)
- Authentication and session management
- SQL injection, XSS, CSRF exposure
- Encryption (at rest, in transit)
- Secrets management
- Compliance-relevant findings per Phase 0 frameworks

### Business Logic Location
- Where does business logic live? (app code, stored procs, triggers, config tables, external?)
- Stored procedure vs. application code ratio
- Black-box components (compiled DLLs, undocumented services)

### Deployment & Operations
- Build process, deployment mechanism
- Environment configuration (dev, staging, prod)
- Monitoring, alerting, backup/recovery

### 7 R's Classification
For each application or major component, classify:
- **Retain** — keep as-is (acceptable risk, low pain)
- **Retire** — decommission (no longer needed)
- **Rehost** — lift and shift to new infrastructure
- **Replatform** — minor changes for modern infrastructure
- **Refactor** — restructure without changing behavior
- **Rearchitect** — redesign architecture
- **Rebuild** — rewrite from scratch

Tag each as **[Tactical]** or **[Strategic]** with justification.

## Multi-Application: Consolidated Assessment

After per-app assessments, produce a **CONSOLIDATED_ASSESSMENT.md**:
- Map shared data sources (which apps read/write same databases/tables)
- Identify data ownership conflicts
- Document overlapping business logic implemented differently across apps
- Map hidden coupling (App A's stored proc called by App B, etc.)
- Assess data consistency risks (same entities represented differently)
- Document blast radius of schema changes
- Identify consolidation opportunities
- Produce inter-application dependency map

## Deliverables

- **ASSESSMENT.md** per application (findings with evidence — file paths, code snippets, config values)
- **CONSOLIDATED_ASSESSMENT.md** for multi-app engagements

## Scaling (Large Codebases)

If too large for direct analysis:
1. Prioritize: entry points, config, data access, auth, build/deploy (80/20 rule)
2. Run static analysis tools (SonarQube, Snyk) first, feed results for interpretation
3. Consider vector DB approach for semantic code search

## Human Checkpoint

Domain expert reviews each assessment. "Is this accurate?" "Is there context I'm missing?" "You didn't mention X — that's important." Corrections feed into Phase 2.
