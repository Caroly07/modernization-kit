# Phase 1: Codebase Analysis — Design

## Approach

Analyze each application systematically. Read actual code — don't guess from file names. Use the AI IDE's workspace access to navigate the codebase, search for patterns, and trace execution flows.

## Analysis Order (per application)

1. **Configuration first** — web.config, appsettings, package.json, pom.xml, etc. Reveals framework versions, dependencies, database connections, and deployment settings.
2. **Entry points** — controllers, pages, API endpoints, main classes. Reveals the application's surface area and routing.
3. **Data access layer** — ORM mappings, DB contexts, connection strings, stored proc references. Reveals data architecture and complexity.
4. **Authentication** — login flows, session management, password handling. Reveals security posture.
5. **Business logic** — service classes, domain models, stored procedures. Reveals where the real complexity lives.
6. **Build/deploy** — CI/CD configs, build scripts, Dockerfiles, deployment docs. Reveals operational maturity.

## Severity Ratings

- **Critical** — Active security vulnerability, data loss risk, or compliance violation
- **High** — Significant technical debt, performance bottleneck, or maintainability blocker
- **Moderate** — Suboptimal pattern, missing best practice, or minor security concern
- **Low** — Code quality issue, cosmetic concern, or minor inefficiency
- **Info** — Observation that provides context but isn't a problem

## Key Principle

This phase is DIAGNOSTIC ONLY. Document what IS, not what SHOULD BE. Recommendations come in Phase 3. Mixing diagnosis with prescription at this stage leads to premature solution design before the full picture is understood.

The one exception is the 7 R's classification — this is a diagnostic classification (what approach fits this application?) not a design recommendation. It answers "what kind of modernization does this need?" based on the evidence, which frames the scope for Phase 3.
