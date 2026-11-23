# Security Audit Project — Botium Toys

**Analyst:** Isaac Osei Mensah
**Programme:** Google Cybersecurity Certificate coursework (August 2023)
**What this is:** A complete internal IT security audit of Botium Toys, a fictional toy company, carried out as certificate coursework. All company details, assets, and findings are from the course scenario — this is training work, not employment, and no real organisation's systems were involved. The audit is documented in five working papers in my Google Drive: audit scope and goals, risk assessment, controls assessment, compliance checklist, and a stakeholder memorandum signed and dated 24 August 2023.

---

## Audit scope and goals

The scope was the company's **entire security program**: current user permissions, implemented controls, and procedures and protocols across the accounting system, endpoint detection, firewalls, the intrusion detection system (IDS), and the security information and event management (SIEM) tool — plus confirming that all technology, hardware, and system access was accounted for and aligned with compliance requirements.

The goals set for the audit were to:

- Adhere to the NIST Cybersecurity Framework (NIST CSF)
- Establish a better process for ensuring systems are compliant
- Fortify system controls
- Implement least privilege in user credential management
- Establish policies and procedures, including playbooks
- Ensure compliance requirements are met

## Risk assessment

The asset inventory covered on-premises equipment, employee end-user devices and remote workstations, the systems and services the IT department manages (accounting, telecommunications, database, security, e-commerce, inventory management), internet access, the internal network, vendor access management, data centre hosting, data retention and storage, badge readers, and legacy end-of-life systems needing human monitoring.

The assessment found **inadequate management of assets** and that the company **did not have the proper controls in place and may not be compliant with U.S. and international regulations and standards**. Working from the NIST CSF **Identify** function, it concluded the company needs to dedicate resources to managing assets and determine the impact of losing existing assets on business continuity.

**Risk score: 8 out of 10 (high)** — driven by the lack of controls and non-adherence to compliance regulations. The potential impact of losing an asset was rated medium (the IT department did not know which assets would be lost), while the likelihood of a lost asset or regulatory fines was rated high.

## Controls assessment

Each control was assessed as implemented or needing implementation, with a priority. The firewall was the one technical control already in place (marked N/A).

**Administrative controls — all needed, all HIGH priority:** least privilege; disaster recovery plans; password policies; access control policies; account management policies; separation of duties.

**Technical controls needed:** intrusion detection system (HIGH); encryption (HIGH); backups (HIGH); antivirus software (HIGH); manual monitoring, maintenance, and intervention for legacy systems (HIGH); password management system (MEDIUM).

**Physical controls needed:** CCTV surveillance (HIGH); locks (HIGH); locking cabinets for network gear (MEDIUM); fire detection and prevention (MEDIUM); time-controlled safe, adequate lighting, and alarm-provider signage (LOW).

## Compliance checklist

The checklist worked through the frameworks and regulations relevant to the scenario, with a plain-language explanation of each:

- **GDPR** — protects EU citizens' data and privacy; a breach compromising an EU citizen's data must be reported within 72 hours. Relevant because the company sells to EU customers.
- **PCI DSS** — the standard for storing, accepting, processing, and transmitting credit card information securely; relevant to the company's card payments.
- **FERC-NERC, HIPAA, and SOC Type 1 / Type 2** were also reviewed and explained, including why they did or did not bear on this company (for example, FERC-NERC applies to organisations working with the power grid).

## Stakeholder memorandum — findings and recommendation

The closing memorandum to the IT Manager and stakeholders (from Isaac Osei Mensah, 24 August 2023) split the findings into two groups:

- **Critical findings (address immediately):** least privilege, disaster recovery plans, password policies, access control policies, account management policies, separation of duties, IDS, encryption, backups, password management system, antivirus software, manual monitoring/maintenance/intervention for legacy systems, CCTV surveillance, and locks.
- **Findings (address, no immediate need):** time-controlled safe, adequate lighting, locking cabinets for network gear, alarm-service signage, and fire detection and prevention.

Its summary recommendation: *"the company needs to act quickly to put all measures in place and adhere to all standards and practices for the smooth operation of the company going forward."*

## Skills this demonstrates

Scoping an audit against the NIST CSF; building an asset inventory; rating risk with stated likelihood/impact reasoning; assessing administrative, technical, and physical controls and prioritising the gaps; mapping a business to the compliance regimes that actually apply to it (GDPR, PCI DSS); and reporting findings to non-technical stakeholders in a signed memorandum with critical-vs-routine prioritisation.
