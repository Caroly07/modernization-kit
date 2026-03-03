# Phase 1: Codebase Analysis — Tasks

## Tasks

- [ ] 1. Analyze configuration and dependencies for each application
  Read config files, identify framework versions, map dependencies, check for EOL/deprecated libraries.

- [ ] 2. Map entry points and request flow for each application
  Identify all controllers/pages/endpoints. Trace how requests flow through the application.

- [ ] 3. Assess data layer for each application
  Identify databases, ORM patterns, stored procedures, cross-database dependencies, credential handling.

- [ ] 4. Assess security posture for each application
  Check password storage, auth mechanisms, injection exposure, encryption, secrets management. Flag compliance-relevant findings.

- [ ] 5. Map business logic locations
  Determine where business logic lives — application code vs. stored procedures vs. triggers vs. config tables vs. black-box components.

- [ ] 6. Assess deployment and operations
  Document build process, deployment mechanism, environment management, monitoring, backup procedures.

- [ ] 7. Classify each application using the 7 R's framework
  Based on findings, classify each app/component as Retain, Retire, Rehost, Replatform, Refactor, Rearchitect, or Rebuild. Tag as Tactical or Strategic. Flag uncertain classifications.

- [ ] 8. Produce ASSESSMENT.md for each application
  Compile findings with severity ratings, evidence, and 7 R's classification. Organize by category.

- [ ] 9. Produce CONSOLIDATED_ASSESSMENT.md (multi-app engagements)
  After all per-app assessments are complete, analyze the landscape as a whole. Map shared data sources, identify data ownership conflicts, document overlapping business logic, assess hidden coupling, identify consolidation opportunities, and produce a dependency map. This step often reveals migration constraints and opportunities invisible in per-app assessments.

- [ ] 10. Human review checkpoint
  Present all assessments (per-app and consolidated) to domain expert. Collect corrections, missing context, and additional findings. Validate 7 R's classifications. The consolidated assessment is especially important to review — the domain expert often knows about inter-app dependencies that aren't visible in the code. Update assessments.
