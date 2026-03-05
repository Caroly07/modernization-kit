# Phase 0: Discovery Interview

Understand the organization, its industry, its constraints, and its goals before touching any code. Present these questions conversationally, not as a checklist. Follow up based on answers.

## Opening

> I'm going to help you build a comprehensive modernization plan for your legacy application(s). Before I look at any code, I need to understand your organization, your constraints, and what you're trying to achieve. I'll ask questions in groups — answer what you can, and say "I don't know" for anything you're unsure about.

## Question Groups

**1. Organization & Industry:** Core business, industry, employee count, IT headcount, locations, cloud accounts, cloud vs. on-prem preference, IT budget range.

**2. Regulatory & Compliance:** Identify all applicable frameworks (HIPAA, PCI-DSS, SOX, ITAR, FERPA, GLBA, CMMC, FedRAMP, GDPR, CCPA, NRC, FDA 21 CFR Part 11, SOC 2, gaming regulations). Compliance team? Recent audits? Data residency? Classified data?

**3. Current Applications:** For each app — name, purpose, users, age, tech stack, database(s), deployment. Inter-app communication? Dependent systems? Upstream dependencies?

**4. Pain Points:** Top 3-5 daily frustrations. Manual workarounds. Excel exports. Deployment process. Rollback capability. Backup status. Security incidents. Requested features.

**5. People & Knowledge:** Original developers available? Maintenance team size/skills? Documentation? Institutional knowledge? Modern tech experience? AI-assisted dev stance? Open-source policy?

**6. Existing Infrastructure:** Compute (servers, VMs, hypervisors, utilization, GPUs). Storage (SAN, NAS, capacity, backups). Networking (bandwidth, load balancers, firewalls, VPN). Licensing (costs, enterprise agreements, underutilized). Cloud (accounts, usage, spend, credits).

**7. Goals & Constraints:** Modernization driver. Success criteria. Timeline. Budget. Acceptable downtime. Technology mandates. Upcoming deadlines.

## Deliverable

**DISCOVERY_SUMMARY.md** — all answers, applicable regulatory frameworks, known constraints, areas needing more information.

## Human Checkpoint

Domain expert and stakeholders MUST review before proceeding to Phase 1.
