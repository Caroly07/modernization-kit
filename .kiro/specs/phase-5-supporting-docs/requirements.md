# Phase 5: Supporting Documents — Requirements

## Overview
Produce the documents that stakeholders beyond the technical team need to approve, fund, and support the project.

## Requirements

### REQ-1: Business Case
- [ ] Executive summary (one page, non-technical, readable by CFO/CEO)
- [ ] Problem statement in business terms
- [ ] Proposed solution (high-level, non-technical)
- [ ] Cost breakdown
- [ ] ROI analysis (savings, revenue opportunities, risk reduction — quantified)
- [ ] Timeline summary
- [ ] Top 5 risks with mitigations
- [ ] Clear recommendation

### REQ-2: Disaster Recovery Plan (if applicable)
- [ ] Current state assessment
- [ ] RPO/RTO targets with justification
- [ ] Backup strategy (what, where, how often, how tested)
- [ ] Failover architecture
- [ ] Recovery runbooks (step-by-step)
- [ ] Testing schedule
- [ ] DR infrastructure cost
- [ ] Geographic/environmental risk factors

### REQ-3: Staffing Recommendation
- [ ] Assess current team capacity honestly:
  - Can existing staff absorb modernization work alongside maintenance?
  - What percentage of current team time is spent keeping the lights on vs. new development?
  - If >60% is maintenance, the team likely cannot self-modernize without backfill or external help
  - Large organizations with dedicated modernization teams or innovation budgets may have capacity already — don't assume they need to hire
- [ ] Present ALL staffing models with honest trade-offs:
  - **Internal team (existing staff):** Pros: domain knowledge, long-term ownership, no ramp-up. Cons: pulls from maintenance/feature work, may lack modern stack skills, opportunity cost of delayed roadmap items.
  - **Internal hire (new FTE):** Full cost analysis — not just salary. Include: benefits (typically 25-40% of salary), recruiting costs ($15K-$30K+ agency fees or 3-6 months of internal recruiting time), onboarding (1-3 months before productive), equipment/workspace, training, management overhead, risk of bad hire (cost of termination + re-hiring), and the 3-6 month ramp to domain knowledge. A $150K/year developer actually costs $195K-$230K/year fully loaded, plus $30K-$60K in first-year recruiting and onboarding costs.
  - **Contract/consulting (external):** Full cost analysis — hourly/daily rate × duration, plus: procurement overhead, onboarding time, knowledge transfer risk when engagement ends, potential IP/security concerns, management overhead. Higher hourly cost but zero benefits, zero recruiting cost, and you can end the engagement when the work is done.
  - **Hybrid:** Internal staff for domain knowledge and long-term ownership + external for specialized skills or surge capacity. Often the most practical for mid-size organizations.
  - **Managed service / consulting engagement:** For organizations that want to hand off the entire project. Highest cost but lowest internal burden. Appropriate when internal team has zero capacity or the skills gap is too large.
- [ ] Skills gap analysis: what skills does the project need vs. what the current team has
- [ ] Produce a skills matrix: rows = required skills (by phase), columns = team members, cells = proficiency level. Identify gaps visually.
- [ ] Key person risk assessment (what if the lead developer leaves mid-project?)
- [ ] Knowledge transfer plan (how does project knowledge get into the organization's permanent team?)
- [ ] Post-modernization maintenance staffing (who maintains the new system long-term?)
- [ ] Recommendation with justification — but present it as "here are your options with trade-offs" not "you must hire X"

### REQ-4: Security Remediation Plan
- [ ] Critical vulnerabilities to fix immediately (before modernization)
- [ ] Prioritized by severity and exploitability
- [ ] Specific remediation steps
- [ ] Effort estimates
- [ ] Verification criteria

## Deliverables
- **BUSINESS_CASE.md**
- **DISASTER_RECOVERY_PLAN.md** (if applicable)
- **STAFFING_RECOMMENDATION.md**
- **SECURITY_REMEDIATION.md**

## Human Checkpoint
Each document reviewed by its target audience: business case → management/finance, DR → IT/ops, staffing → management/HR, security → IT/security.
