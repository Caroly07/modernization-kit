# Phase 0: Discovery Interview — Requirements

## Overview
Before touching any code, understand the organization, its industry, constraints, and goals. The answers to these questions fundamentally change architecture, technology choices, compliance requirements, and risk profile.

## Requirements

### REQ-1: Organization Profile
- [ ] Document what the organization does (core business)
- [ ] Identify the industry
- [ ] Capture organization size (employees, IT/engineering headcount)
- [ ] Document physical location(s) — affects DR planning, latency, regulations
- [ ] Identify existing cloud accounts and preferences
- [ ] Capture approximate annual IT budget range

### REQ-2: Regulatory & Compliance Framework
- [ ] Identify all applicable regulatory frameworks (HIPAA, PCI-DSS, ITAR, SOX, GDPR, etc.)
- [ ] Document whether a dedicated compliance officer/team exists
- [ ] Note recent audit findings if any
- [ ] Identify data residency requirements
- [ ] Determine data classification levels

### REQ-3: Application Landscape
- [ ] Inventory all applications to be modernized (name, purpose, users, age, tech stack, databases)
- [ ] Map inter-application communication (SOAP, REST, shared DB, file drops, etc.)
- [ ] Identify dependent systems (reporting tools, ETL, third-party integrations)
- [ ] Identify upstream dependencies (AD, email, file shares, external APIs)

### REQ-4: Current Pain Points (Initial)
- [ ] Document top 3-5 daily user frustrations per application
- [ ] Identify manual workarounds and Excel exports
- [ ] Document deployment process and rollback capability
- [ ] Assess backup and disaster recovery status
- [ ] Note any security incidents or breaches

### REQ-5: People & Knowledge
- [ ] Identify original developers and their availability
- [ ] Assess current maintenance team size and skills
- [ ] Evaluate documentation status (honest assessment)
- [ ] Identify institutional knowledge trapped in people's heads
- [ ] Assess team's modern technology experience
- [ ] Determine organization's stance on AI-assisted development
- [ ] Determine organization's open-source software policy

### REQ-6: Existing Infrastructure Inventory
- [ ] Inventory compute resources (servers, VMs, hypervisors, utilization levels)
- [ ] Inventory storage (SAN, NAS, backup appliances, capacity vs. usage)
- [ ] Inventory networking (bandwidth, load balancers, firewalls, VPN, segmentation)
- [ ] Inventory GPU hardware (servers, workstations — any NVIDIA cards anywhere?)
- [ ] Catalog current software licenses and costs (databases, OS, tools, BI platforms)
- [ ] Identify underutilized or decommissioned hardware

### REQ-7: Goals & Constraints
- [ ] Identify the primary driver for modernization
- [ ] Define success criteria
- [ ] Capture timeline expectations
- [ ] Capture budget range
- [ ] Determine acceptable downtime during cutover
- [ ] Note technology mandates or restrictions
- [ ] Identify upcoming deadlines (contract renewals, audits, EOL dates)

## Deliverable
A **DISCOVERY_SUMMARY.md** document capturing all answers, identifying applicable regulatory frameworks, listing known constraints, and flagging areas where more information is needed.

## Human Checkpoint
The domain expert and stakeholders MUST review the Discovery Summary before proceeding to Phase 1. Correct any misunderstandings. Fill in gaps. This is where domain knowledge is most critical.
