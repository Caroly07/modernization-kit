---
inclusion: auto
---

# Modernization Anti-Patterns

Avoid these mistakes. Each one has derailed real projects.

**Don't translate line-by-line.** Converting VB.NET to C# or COBOL to Java without rethinking the architecture produces modern-looking code with legacy problems. Revisit architectural decisions, don't just preserve them in a new language.

**Don't default to microservices.** For teams of 1-5 developers, a modular monolith gives the same code organization benefits without the operational complexity of distributed systems. Extract to services later if specific modules need independent scaling.

**Don't ignore the data layer.** The hardest part of modernization is data migration — stored procedures with embedded business logic, cross-database dependencies, decades of data quality issues. Budget more time for data than you think you need.

**Don't skip the pain point assessment.** A pixel-perfect replica of the old system in a new framework is expensive nostalgia. Talk to users. Find out what's broken. Build something better.

**Don't plan parallel operation without an API layer.** If the legacy system uses direct database access with no API layer, there's no clean way to sync old and new during transition. Build-then-cutover is usually the honest answer.

**Don't underestimate identity migration.** Moving users from custom auth to a modern identity provider is always harder than it looks. Password hashing differences, session changes, role model differences, forced password resets.

**Don't forget the ETL shadow infrastructure.** Scheduled jobs, SSIS packages, cron scripts, manual data transfers — nobody thinks about these until they break. Inventory them early.

**Don't build AI features for the sake of AI.** AI should solve specific, measurable problems from the pain point assessment. "Add AI" is not a requirement. "Reduce intake time from 60 minutes to 5 minutes" is.

**Don't assume the first draft is final.** Plans get better with every correction from the domain expert. Build in time for iteration. Expect 3-6 major revisions.

**Don't ignore existing infrastructure.** Underutilized servers, storage, networking gear, and licenses can dramatically change the cost and architecture. Always inventory what exists before specifying what to buy.
