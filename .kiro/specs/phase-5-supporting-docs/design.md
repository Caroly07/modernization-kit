# Phase 5: Supporting Documents — Design

## Approach

Each document targets a different audience. Adjust language, detail level, and focus accordingly.

## Business Case

**Audience:** CFO, CEO, board, non-technical stakeholders

**Key principle:** Translate technical benefits into business outcomes.
- "Containerized deployment" → "System updates with zero downtime and instant rollback"
- "Faster feature delivery" → "Respond to market changes in days instead of months"
- "Database consolidation" → "Single source of truth, no more conflicting data between systems"
- "AI-assisted workflows" → "Tasks that take 60 minutes today take 5 minutes"

**Structure:** Problem → Solution → Cost → ROI → Timeline → Risks → Recommendation

## Disaster Recovery Plan

**Audience:** IT operations, management, auditors

**Key principle:** Specific, actionable runbooks that someone can follow at 2am during a crisis.
- Not "restore from backup" but "SSH to backup-server, run `pg_restore -d production /backups/latest.dump`, verify with `SELECT count(*) FROM identity.organizations`"
- Include contact information, escalation procedures, and decision trees

## Staffing Recommendation

**Audience:** Management, HR, procurement

**Key principle:** Present options, not a predetermined answer. The right staffing model depends on the organization's size, current team capacity, budget structure, and risk tolerance. A 500-person company with a 20-person IT team has different options than a 30-person company with one developer.

**Critical: get the cost model right.** Comparing a contractor's hourly rate to an employee's salary is apples to oranges. The true cost of a new hire includes:
- Base salary
- Benefits (health, dental, vision, 401k match, PTO — typically 25-40% on top of salary)
- Payroll taxes (employer-side FICA, unemployment — ~7.65% + state)
- Recruiting costs (agency fees, job postings, interview time, background checks — $15K-$50K)
- Onboarding costs (equipment, workspace, training, reduced productivity for 1-3 months)
- Management overhead (someone has to manage them)
- Risk premium (bad hire costs 1.5-2x annual salary to replace)

A "$150K developer" actually costs $200K-$240K/year fully loaded, plus $30K-$60K in first-year acquisition costs. Compare THAT number to the contractor rate, not the salary alone.

**Don't assume external is always the answer.** Large organizations often have:
- Existing modernization teams or centers of excellence
- Innovation budgets specifically for this kind of work
- Internal developers who WANT to work on modern tech (retention benefit)
- Procurement processes that make external hiring slower than internal reallocation

**Don't assume internal is always the answer either.** Small-to-mid organizations often have:
- Every developer fully committed to maintenance and keeping the lights on
- No slack capacity to absorb a multi-month modernization project
- Skills gaps in modern frameworks, containers, cloud, or AI
- No recruiting pipeline for specialized skills

Present all options with honest trade-offs and let the organization decide based on their specific situation.

## Security Remediation

**Audience:** IT, security team, compliance

**Key principle:** Fix the dangerous stuff NOW, before modernization begins.
- Plain text passwords don't wait for the new system
- Disabled error handling doesn't wait for the new system
- Known vulnerabilities in EOL frameworks don't wait for the new system
