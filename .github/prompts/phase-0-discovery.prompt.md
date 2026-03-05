# Phase 0: Discovery Interview

**Purpose:** Understand the organization, its industry, its constraints, and its goals before touching a single line of code. The answers fundamentally change architecture, technology choices, compliance requirements, and risk profile.

**How to use:** Present these questions conversationally, not as a checklist. Follow up based on answers. Some answers trigger domain-specific questions (e.g., healthcare → HIPAA questions).

## Opening Prompt

Start the conversation with:

> I'm going to help you build a comprehensive modernization plan for your legacy application(s). Before I look at any code, I need to understand your organization, your constraints, and what you're trying to achieve. I'll ask questions in groups — answer what you can, and say "I don't know" for anything you're unsure about. That's valuable information too.

## Question Groups

### 1. Organization & Industry
- What does your organization do? (core business, one paragraph)
- What industry?
- Approximate employee count? IT/engineering headcount?
- Physical location(s)? (affects DR, latency, regulations)
- Existing cloud accounts (AWS, Azure, GCP)?
- Cloud vs. on-premises preference or mandate?
- Approximate annual IT budget range?

### 2. Regulatory & Compliance
Identify all applicable frameworks: HIPAA, PCI-DSS, SOX, ITAR/EAR, FERPA, GLBA, CMMC/DFARS, FedRAMP, GDPR, CCPA/CPRA, NRC, FDA 21 CFR Part 11, SOC 2, state gaming regulations, other.

Also: dedicated compliance team? Recent audit findings? Data residency requirements? Classified data?

### 3. Current Application Landscape
For each application: name, purpose, users, age, tech stack, database(s), deployment method. How do they communicate? What depends on them? What do they depend on?

### 4. Current Pain Points
Top 3-5 daily frustrations. Manual workarounds. Excel exports. Multi-screen workflows. Recent outages. Deployment process. Rollback capability. Backup status. Security incidents. Requested features the system can't support.

### 5. People & Knowledge
Original developers available? Current maintenance team size/skills? Documentation status? Institutional knowledge in people's heads? Modern tech experience? AI-assisted development stance? Open-source policy?

### 6. Existing Infrastructure
Compute (servers, VMs, hypervisors, utilization, GPUs). Storage (SAN, NAS, capacity vs. usage, backups). Networking (bandwidth, load balancers, firewalls, VPN, segmentation). Licensing (current costs, enterprise agreements, underutilized licenses). Cloud (existing accounts, current usage, monthly spend, credits/commitments).

### 7. Goals & Constraints
Primary modernization driver. Success criteria. Timeline expectations. Budget range. Acceptable downtime. Technology mandates/restrictions. Upcoming deadlines (contract renewals, audits, EOL dates).

## Deliverable

**DISCOVERY_SUMMARY.md** — captures all answers, identifies applicable regulatory frameworks, lists known constraints, flags areas needing more information.

## Human Checkpoint

The domain expert and stakeholders MUST review before proceeding to Phase 1. Correct misunderstandings. Fill gaps.
