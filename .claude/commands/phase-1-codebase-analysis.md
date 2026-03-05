# Phase 1: Codebase Analysis

Read the actual source code and produce a factual technical assessment of each application. No recommendations yet — purely diagnostic.

## For Each Application, Assess:

**Architecture & Structure:** Pattern, project structure, entry points, request flow, inter-app communication, third-party dependencies with version/EOL status.

**Code Quality & Patterns:** Language/framework versions, design patterns, separation of concerns, error handling, logging, configuration management.

**Data Layer:** Database tech/version/count, relationships, cross-DB dependencies, ORM/data access patterns, schema complexity, credential handling.

**Security Posture:** Password storage, auth/session management, SQL injection, XSS, CSRF, encryption, secrets management, compliance-relevant findings.

**Business Logic Location:** App code vs. stored procs vs. triggers vs. config tables. Quantify the ratio. Identify black-box components.

**Deployment & Operations:** Build process, deployment mechanism, environments, monitoring, backup/recovery.

**7 R's Classification:** For each app/component — Retain, Retire, Rehost, Replatform, Refactor, Rearchitect, or Rebuild. Tag each as [Tactical] or [Strategic] with justification.

## Multi-App: Consolidated Assessment

After per-app assessments, produce **CONSOLIDATED_ASSESSMENT.md**: shared data sources, data ownership conflicts, overlapping business logic, hidden coupling, data consistency risks, blast radius of schema changes, consolidation opportunities, inter-app dependency map.

## Deliverables

- **ASSESSMENT.md** per application (findings with evidence — file paths, code snippets, config values)
- **CONSOLIDATED_ASSESSMENT.md** for multi-app engagements

## Scaling (Large Codebases)

1. Prioritize: entry points, config, data access, auth, build/deploy (80/20 rule)
2. Run static analysis tools first (SonarQube, Snyk), feed results for interpretation
3. Consider vector DB approach for semantic code search

## Human Checkpoint

Domain expert reviews each assessment. Corrections feed into Phase 2.
