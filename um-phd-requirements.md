# U of M PhD Research — Technical & Compliance Requirements

*Prepared February 21, 2026 — reference for Isabella project*
*Sources: U-M HRPP, IRB-HSBS, ITS Research Services (all accessed Feb 21, 2026)*

---

## 1. The IRB — What It Is and Why It Matters

Isabella's PhD research will involve human subjects (people who use drugs, outreach workers, recovery community members, etc.). **No research activity can begin until the IRB approves or exempts the study.**

U-M has two IRBs. Hers will almost certainly be:

- **IRB-HSBS** (Health Sciences and Behavioral Sciences)
  - 2800 Plymouth Road, Bldg 520, Rm 2144, Ann Arbor
  - Phone: (734) 936-0933
  - Email: irbhsbs@umich.edu
  - Two boards (Blue and Maize), meet monthly
  - Expedited review available on a rolling basis

The other IRB (IRBMED) handles Medical School / Health System / FDA-regulated research. If her research involves Michigan Medicine patients or clinical procedures, it could go there instead.

### IRB Application Process

1. All applications submitted through **eResearch Regulatory Management (eRRM)**: [eresearch.umich.edu](http://www.eresearch.umich.edu/)
2. Only the PI can submit (includes attestation of responsibility)
3. Student PIs must designate a **faculty advisor** who shares responsibility
4. Application sections: General Study Info → Sponsor Info → Performance Sites → Research Design → Benefits & Risks → Special Considerations → Subject Information → Informed Consent → Confidentiality/Security/Privacy

### Application Types (one of these will apply)

| Type | When |
|------|------|
| **Human Subjects Research — Interaction/Intervention** | Collecting info or biospecimens directly from people |
| **Secondary Research Uses** | Analyzing previously collected data/biospecimens only |
| **Not Regulated** | Projects involving people but not meeting "human subjects research" definition |

For harm reduction field research, it's almost certainly **Interaction/Intervention**.

### Review Levels

- **Exempt**: Certain low-risk categories (e.g., surveys, interviews with non-vulnerable populations, educational tests). Staff make the determination.
- **Expedited**: Minimal risk research in specific federal categories. Single reviewer, rolling basis.
- **Full Board**: Greater than minimal risk, or involving vulnerable populations. Monthly meetings. **Substance-using populations are typically considered vulnerable** — expect full board or at minimum expedited.

---

## 2. Required Training

### PEERRS (Program for Education and Evaluation in Responsible Research and Scholarship)

**Everyone on the study team must complete this before working with subjects or identifiable data.**

- Course: "Human Subjects Research Protections" (HSP)
- Time: 2–4 hours online
- Passing score: 85%
- Renewal: Every 3 years
- Register via [My LINC](https://maislinc.umich.edu)
- Alternative: CITI HSP training accepted with [waiver application](https://umich.qualtrics.com/jfe/form/SV_eJvyLPsya63nU0K)

**If Ryan is listed on the study team (e.g., as Research Staff or a technical contributor working with identifiable data), he must also complete PEERRS.** If he only builds tools and never touches participant data, he may not need to be on the IRB application — but this needs to be discussed with Isabella's faculty advisor.

### PEERRS Course Content

1. Ethics & Regulatory Overview (Belmont Principles, Common Rule)
2. Study Team Considerations — Risks & Safeguards
3. Informed Consent Process
4. IRB Oversight
5. Privacy, Confidentiality, HIPAA

---

## 3. Data Security Requirements (U-M Specific)

This is where software design gets directly constrained. U-M has **11 core data security controls** that apply to all human subjects research:

### Core Controls

1. **Sensitive Data Guide** — U-M maintains a classification system. What tools you can use depends on data classification. Access requires U-M login: [safecomputing.umich.edu/dataguide](https://www.safecomputing.umich.edu/dataguide/)
2. **All collection/storage devices must be password-protected** with strong passwords
3. **Portable devices: all sensitive data must be encrypted**
4. **Access limited to study team members only**
5. **Identifiers, data, and keys stored in separate encrypted files in different locations**
6. **Portable device data transferred to approved system ASAP** — locked up when not in use. Consult departmental **Security Unit Liaison (SUL)** for device configuration.
7. **U-M Google Mail/Calendar CANNOT be used** for sensitive human subjects data or PHI
8. **Cloud services must follow U-M safecomputing guidelines** and [IT policies](https://it.umich.edu/information-technology-policies/general-policies)
9. **Data on portable devices deleted after transfer to approved service**
10. **Outside consultants/vendors must sign confidentiality agreements** + comply with [Third Party Vendor requirements](https://safecomputing.umich.edu/protect-the-u/protect-your-unit/third-party-vendor-security-compliance)
11. **Delete/destroy identifiable information ASAP** when research design allows

### Data Classification Definitions

| Term | Meaning | Implication |
|------|---------|-------------|
| **Anonymous** | No one, not even the researcher, can connect data to individual | No identifiers collected at all. Lowest risk. |
| **Confidential** | Link exists between data and individual; team must protect it | Code keys, separate storage, access controls |
| **De-identified** | All identifiers and codes stripped and destroyed | Can't re-link. May enable broader sharing. |

### Data Management and Security Protocol

The IRB may require a formal **Data Management and Security Protocol** document. This outlines:
- What data is collected and its sensitivity level
- Where data is stored (must be approved systems)
- Who has access and how access is controlled
- How data is transmitted
- Retention and destruction timeline
- Breach notification procedures

---

## 4. U-M Approved Research Technology

These are the institutional tools available. Any custom software must interoperate with or meet the same standards as these:

### Computing & Storage

| Service | Purpose | Data Level |
|---------|---------|------------|
| **Great Lakes HPC** | High-performance computing cluster | General research data |
| **Armis2 HPC** | Secure computing for HIPAA-regulated data | PHI / HIPAA |
| **U-M Research Computing Package** | Bundled HPC + storage access | General |

### Data Collection & Surveys

| Service | Purpose | Notes |
|---------|---------|-------|
| **Qualtrics** | Online surveys | U-M licensed, available to all faculty/staff/students. Check Sensitive Data Guide for data level restrictions. |
| **REDCap** | Research data capture | HIPAA-compliant. Standard for clinical / public health research. Hosted by U-M. |

### Research Administration

| Service | Purpose |
|---------|---------|
| **eResearch Regulatory Management (eRRM)** | IRB applications, amendments, continuing review |
| **eResearch Proposal Management** | Grant submissions, award management |
| **M-Inform** | Conflict of interest management |
| **InfoReady** | Internal funding applications |

### Communication & Security

| Service | Purpose |
|---------|---------|
| **Virtru for Gmail** | End-to-end encrypted email for sensitive data |
| **MiVideo** | Secure multimedia storage/sharing (interview recordings) |
| **U-M API Directory** | Institutional data access for research applications |

---

## 5. HIPAA Considerations

If Isabella's research involves:
- Health records from Michigan Medicine
- Clinical data from treatment programs
- PHI (Protected Health Information) from partner organizations

Then **HIPAA applies** on top of IRB requirements. This adds:

- Business Associate Agreements (BAAs) with any vendor touching PHI
- HIPAA Security Rule compliance (encryption at rest + in transit, access logging, audit trails)
- Use of HIPAA-approved systems only (e.g., Armis2 for computing, specific storage services)
- Minimum Necessary Standard — only access/collect the minimum PHI needed

**Harm reduction research may or may not trigger HIPAA** — it depends on whether she's accessing clinical records or only collecting her own observational/survey/interview data. Key question for the meeting.

---

## 6. Certificate of Confidentiality

For research involving sensitive topics (substance use absolutely qualifies), the NIH issues **Certificates of Confidentiality** that:

- Protect researchers from being compelled to disclose identifying information in legal proceedings
- Automatically apply to all NIH-funded research since 2017
- Can be requested for non-NIH-funded research
- U-M has a specific process: [hrpp.umich.edu/certificates-of-confidentiality](https://hrpp.umich.edu/certificates-of-confidentiality/)

This is important for harm reduction research because participants may disclose illegal activity. The certificate protects both them and the researcher.

---

## 7. What This Means for Software We Build

### Must-Haves (Non-Negotiable)

- [ ] **Encryption at rest** for all participant data (AES-256 minimum)
- [ ] **Encryption in transit** (TLS 1.2+)
- [ ] **Authentication** — strong passwords, ideally U-M SSO/Shibboleth integration
- [ ] **Access controls** — role-based, study-team-only access
- [ ] **Identifier separation** — participant IDs stored separately from data, separate from linking keys
- [ ] **Audit logging** — who accessed what, when
- [ ] **Data export** — must produce datasets compatible with academic analysis tools (R, SPSS, Stata, Python)
- [ ] **Retention controls** — ability to delete/destroy data per protocol timeline
- [ ] **No U-M Google services** for sensitive data
- [ ] **Third-party vendor compliance** if hosted externally

### Should-Haves (Practical)

- [ ] **Offline capability** — harm reduction fieldwork happens in places with bad connectivity
- [ ] **Mobile-first** — data collection on phones, not laptops
- [ ] **Local-first architecture** — data on device until sync to approved system (reduces cloud dependency)
- [ ] **De-identification pipeline** — automated stripping of identifiers for analysis datasets
- [ ] **Consent workflow** — digital informed consent capture that meets IRB requirements
- [ ] **Backup/recovery** — encrypted, access-controlled backups

### Architecture Recommendation (Preliminary)

Given U-M's constraints, the strongest approach is likely:

1. **Mobile PWA or native app** for field data collection
2. **Local encryption** on device before any transmission
3. **Sync to U-M-approved storage** (Armis2, or an approved on-premises server)
4. **Separate identifier database** with its own encryption and access controls
5. **Analysis pipeline** that works with de-identified exports only
6. **No commercial cloud** (no AWS, no GCP, no Azure) unless U-M has a BAA in place

### Ryan's Role — Key Decision

If Ryan is an **outside consultant/vendor**:
- Must sign confidentiality agreement
- Must comply with U-M Third Party Vendor Security requirements
- Cannot have unsupervised access to identifiable data
- Software must be reviewed/approved

If Ryan is added to the **study team** (e.g., as Research Staff):
- Must complete PEERRS training
- Must be listed on IRB application
- Gets access under the same rules as other team members
- More flexibility but more compliance obligations

**Recommendation: Discuss with Isabella's faculty advisor which arrangement makes more sense.**

---

## 8. Questions for Isabella (Updated)

1. What's your research question / area?
2. Which school/department is the PhD in? (Public Health, Social Work, Psychology?)
3. Who is your faculty advisor? (They'll shape IRB requirements)
4. Will you be collecting identifiable data from participants?
5. Will you need to access clinical/health records (HIPAA trigger)?
6. What's your current data workflow — paper, spreadsheets, REDCap, other?
7. Do you have IRB approval yet, or are you still in coursework/pre-proposal?
8. What's your timeline? (First year coursework → qualifying exams → proposal → data collection → dissertation)
9. Does your research touch the clay/art connection, or are those separate?
10. Do you need a tool built, or help with analysis, or both?
11. What funding do you have / are pursuing? (NIDA, SAMHSA, institutional grants?)

---

## Key Links

- IRB-HSBS: https://hrpp.umich.edu/irb-health-sciences-and-behavioral-sciences-hsbs/
- eResearch: http://www.eresearch.umich.edu/
- Data Security Guidelines: https://hrpp.umich.edu/irb-health-sciences-and-behavioral-sciences-hsbs/irb-application-process/data-security-guidelines/
- PEERRS Training: https://hrpp.umich.edu/peerrs-human-subjects-research-protections-course-details/
- Sensitive Data Guide: https://www.safecomputing.umich.edu/dataguide/
- Certificate of Confidentiality: https://hrpp.umich.edu/certificates-of-confidentiality/
- Third Party Vendor Compliance: https://safecomputing.umich.edu/protect-the-u/protect-your-unit/third-party-vendor-security-compliance
- HRPP Operations Manual (PDF): https://hrpp.umich.edu/wp-content/uploads/sites/4/2024/10/1-HRPP-Operations-Manual.pdf.pdf
- Armis2 (HIPAA HPC): https://arc.umich.edu/armis2/
- U-M IT Policies: https://it.umich.edu/information-technology-policies/general-policies
