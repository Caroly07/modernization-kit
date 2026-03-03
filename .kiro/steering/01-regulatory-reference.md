---
inclusion: auto
---

# Regulatory & Compliance Quick Reference

When the organization's industry and regulatory environment are identified during Phase 0 Discovery, carry the applicable frameworks forward into EVERY subsequent decision. Every technology choice, data flow, and deployment decision must be validated against them.

## Framework Reference

| Framework | Industry | Key Technical Implications |
|---|---|---|
| HIPAA | Healthcare | Encryption at rest + in transit mandatory. BAA with all vendors touching PHI. Access logging with 6-year retention. Minimum necessary access. No PHI in logs/errors. |
| PCI-DSS | Payment processing | Network segmentation. No full card storage post-auth. Quarterly vuln scans. Annual pen testing. Specific encryption requirements. |
| SOX | Public companies | Audit trails for financial data. Segregation of duties. Change management controls. Data retention. |
| ITAR/EAR | Defense/aerospace | Data may not leave US jurisdiction. Cloud must be US-only or on-prem. Foreign person restrictions. Self-hosted AI may be required. |
| FERPA | Education | Student records require consent for disclosure. Parent access rights. Directory info exceptions. |
| CMMC/DFARS | Defense contractors | NIST 800-171 controls. CUI handling. 72-hour incident reporting. Assessment/certification. |
| GDPR | EU data subjects | Right to erasure. Data portability. Consent management. DPO. 72-hour breach notification. Privacy by design. |
| FDA 21 CFR Part 11 | Pharma/medical devices | Electronic signatures = handwritten. Audit trails for all changes. System validation. Access controls. |
| NRC 10 CFR 73.54 | Nuclear | Cyber security program. Safety system isolation. Specific access controls. Incident response. |
| State gaming regs | Casinos | Varies by state. Detailed audit trails, data retention, regulatory reporting, system certification. |
| SOC 2 | Service organizations | Trust criteria: security, availability, integrity, confidentiality, privacy. Annual audit. Continuous monitoring. |

## Important

The AI should NOT provide legal compliance advice. It should:
- Identify which frameworks apply
- Flag decisions that have compliance implications
- Recommend the organization's compliance team or legal counsel validate the technical approach
- Note compliance constraints in every deliverable where they affect a decision
