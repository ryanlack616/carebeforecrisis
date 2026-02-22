# Building Research Infrastructure for Harm Reduction — Deep Architecture

*Prepared February 21, 2026*
*For: Isabella project — PhD research support system*

---

## The Problem Nobody Talks About

Most research software is built for labs. Clean subjects, controlled environments, scheduled appointments, reliable contact information. Harm reduction research is the opposite of all of that.

Your participants:
- May not have a phone number that works next week
- May be in withdrawal, high, or in crisis when you encounter them
- May give you a different name each time
- Will not sit for a 45-minute survey
- Have every reason to distrust anyone with a clipboard
- May disclose information that could get them arrested
- Might die between your data collection points

The software has to be built for *that* reality, not the lab version. Everything below flows from this.

---

## Part 1: The Research Lifecycle (What She'll Need, When)

### Phase 1: Coursework & Literature Review (Year 1–2)

**What's happening:** She's reading 200+ papers, identifying gaps, building a conceptual framework, developing her research question.

**What she needs:**
- **Literature management** — Zotero (free, U-M supported) or Mendeley. But the real problem isn't storing papers, it's *synthesizing* them.
- **Synthesis tooling** — A structured way to extract findings across studies. Most PhD students do this in spreadsheets. We could build something better:
  - Study extraction database: citation, population, methods, sample size, key findings, limitations, effect sizes
  - Auto-tagging by theme, population, intervention type
  - Gap identification: "No study has examined X in population Y using method Z"
  - Narrative synthesis support: grouping studies by theme for lit review chapters

**What we could build:** A literature extraction and synthesis tool. Input: PDF or DOI. Output: structured extraction record. Aggregation: searchable database with theme coding. This alone would save her 100+ hours over the PhD.

### Phase 2: Qualifying Exams & Proposal (Year 2)

**What's happening:** She's defining her study — research questions, theoretical framework, methodology, expected contribution. Her committee will tear it apart and she'll rebuild it.

**What she needs:**
- **Conceptual framework visualization** — How variables relate. Most people draw these in PowerPoint. A structured tool that maintains the relationships and updates when the model changes would be useful.
- **Power analysis / sample size justification** — For quantitative components. G*Power is standard but clunky.
- **Preliminary data** — If she can show pilot data, her proposal is stronger. This might mean helping her analyze existing program data before she collects her own.

### Phase 3: IRB Application & Instrument Design (Year 2–3)

**What's happening:** She's building her data collection instruments, writing consent documents, designing her protocol, and submitting to IRB-HSBS.

**What she needs:**

#### Instruments (the actual data collection tools)

For harm reduction research, the instruments typically include some combination of:

| Instrument | What It Captures | Design Challenge |
|------------|------------------|------------------|
| **Structured survey** | Demographics, substance use patterns, service utilization, health status | Must be SHORT. 5-10 minutes max. Every additional question loses participants. |
| **Semi-structured interview guide** | Lived experience, barriers to care, intervention perceptions | Open-ended but consistent enough for thematic analysis. 30-60 min recorded. |
| **Observation protocol** | Field notes from outreach, program operations, interactions | Structured enough to code, flexible enough for emergent phenomena. |
| **Service encounter form** | What happened during each interaction (supplies distributed, referrals made, etc.) | Must be fillable in <60 seconds while standing in a parking lot. |
| **Follow-up tracker** | Longitudinal data — same person across multiple time points | The hardest problem: re-identifying people who don't want to be identified. |
| **Adverse event log** | Overdoses, hospitalizations, deaths, safety incidents | Required by IRB. Must be immediate. |

#### The Consent Problem

Standard informed consent is a 3-page document in 12pt font that a participant reads and signs. In harm reduction:

- Participants may be illiterate
- They may not be in a cognitive state to read
- Written signatures create a record of illegal activity  
- The consent process itself can feel coercive ("sign this or you don't get clean needles")

**Solutions used in the field:**
- Verbal consent with witness (IRB must approve)
- Simplified consent documents (6th grade reading level, 1 page)
- Waiver of documentation of consent (no signature required, IRB must approve)
- Ongoing consent (re-confirmed at each encounter, not just once)
- Community consent models (for CBPR)

**What we could build:** A consent workflow module that:
- Presents simplified consent text (configurable reading level)
- Supports verbal consent with audio recording of the consent conversation
- Timestamps and logs consent without requiring participant signature
- Handles re-consent at subsequent encounters
- Generates IRB-ready consent audit reports

#### The Unique Identifier Problem

This is the hardest technical problem in the entire project.

You need to track the same person over time to measure outcomes. But:
- You can't use real names (legal risk)
- You can't use SSNs or government IDs (many don't have them, all distrust giving them)
- You can't use phone numbers (they change constantly)
- You can't use addresses (many are unhoused)
- You can't use biometrics (invasive, trust-destroying)

**Solutions used in the field:**
- **RHBM (Radford Hill Biometric Method)** — initials + birth month/day + partial features. Not biometric. Creates a pseudo-unique code. Problem: error-prone, collisions.
- **Self-generated unique codes** — Participant creates their own code from answers to questions only they know (mother's first name initial + birth city + childhood pet name, etc.). Problem: participants forget their own codes.
- **Encounter-based matching** — No unique ID. Instead, match on combinations of non-identifying features (age range, gender, approximate height, distinguishing but non-identifying characteristics). Works for small programs, fails at scale.
- **Token systems** — Physical tokens (cards, wristbands) that carry an encrypted ID. Problem: participants lose them, sell them, share them.

**What we could build:** A participant enrollment/matching system that:
- Uses a multi-factor self-generated code (configurable questions)
- Fuzzy matches on partial code + demographics when codes are forgotten
- Never stores the real name — only the hash
- Flags potential duplicates for human review
- Handles the "this might be the same person from 6 months ago" problem
- Generates de-identified longitudinal datasets for analysis

### Phase 4: Data Collection (Year 3–4)

**What's happening:** She's in the field. Outreach vans, syringe service programs, shelters, encampments, treatment centers. Possibly also analyzing existing program data.

**What she needs:**

#### Mobile Data Collection

- **Works offline** — cell service is unreliable in encampments, rural areas, some shelters
- **Fast entry** — 60 seconds per encounter for service data, 5-10 minutes for surveys
- **Encrypted on-device** — data protected before any network transmission
- **Syncs when connected** — to U-M approved storage
- **Minimizes typing** — touch targets, dropdowns, quick-select, voice notes
- **Handles interruption** — auto-saves partial entries (encounters get interrupted by crises)
- **Works in bad conditions** — cold fingers, rain, poor lighting, one hand occupied

#### Interview Recording & Management

- **Encrypted audio recording** — on phone or dedicated recorder
- **Automatic transcription** — Whisper (local, no cloud = no data leak)
- **Transcript review/correction UI** — transcripts need human review
- **Speaker diarization** — who said what (important for multi-participant focus groups)
- **Timestamp linking** — click a transcript line, hear the audio
- **Redaction tools** — before sharing transcripts with committee or in publications

#### Real-Time Data Quality Monitoring

If she's collecting data over 6-12 months, she needs to know *during collection* if:
- Response rates are sufficient
- Certain populations are underrepresented
- Skip patterns are working correctly
- Data entry errors are accumulating
- She needs to adjust recruitment strategy

**What we could build:** A data quality dashboard that:
- Shows enrollment progress against targets
- Flags missing data patterns
- Monitors demographic balance
- Alerts to unusual response patterns
- Tracks consent status and follow-up schedules

#### Safety System

Harm reduction fieldwork is physically dangerous. She may be:
- Alone in encampments
- Present during overdoses
- Exposed to needlestick risk
- In situations involving violence or law enforcement

**What a safety module includes:**
- Check-in/check-out system (she logs when she's going into the field, expected return)
- Dead-man's-switch alert (if she doesn't check out by expected time, designated contacts get notified)
- Emergency protocol quick-access (overdose response, needlestick protocol, de-escalation)
- Incident logging (required by IRB for adverse events)

### Phase 5: Analysis & Writing (Year 4–5)

**What's happening:** Turning raw data into findings, chapters, publications.

**What she needs:**

#### Qualitative Analysis Pipeline

This is where your AI tooling becomes most valuable.

The standard workflow:
1. Transcribe interviews (done by Whisper)
2. Read all transcripts and develop initial codes (open coding)
3. Apply codes to text segments (line-by-line coding)
4. Group codes into categories and themes
5. Test themes against data (constant comparison)
6. Write up findings with supporting quotes

The standard tool is NVivo ($1,200/year academic license, clunky, Windows-only). Atlas.ti is the alternative. Both are painful.

**What we could build:**
- **AI-assisted initial coding** — Run transcripts through a local LLM. It suggests codes based on content. Human reviews and accepts/rejects/modifies. Not replacing the researcher's judgment — accelerating the mechanical part.
- **Code management** — Hierarchical codebook with definitions, examples, decision rules
- **Inter-rater reliability** — If she has a second coder (common for rigor), the tool tracks agreement rates (Cohen's kappa)
- **Theme development workspace** — Visual grouping of codes into themes, with linked data excerpts
- **Audit trail** — Every coding decision logged with timestamp and rationale (IRB and methodological rigor requirement)
- **Export to publication** — Generate quotes-with-context for manuscripts, codebook appendices

#### Quantitative Analysis

She'll almost certainly use R or SPSS (possibly Stata). We don't need to replace these — we need to feed them clean data.

**What we build:**
- **Export pipeline** — De-identified datasets in .csv, .sav (SPSS), .dta (Stata), .rds (R)
- **Data dictionary generator** — Variable names, labels, value codes, skip logic documentation
- **Descriptive statistics dashboard** — Sample characteristics, response distributions, missingness patterns
- **Codebook** — The full methodological record that accompanies any published dataset

#### Mixed Methods Integration

If she's doing mixed methods (very likely in this field), she needs a way to:
- Link quantitative findings to qualitative themes
- Identify cases that illustrate statistical patterns
- Build joint displays (the standard mixed methods visualization)
- Document the integration process for methodological transparency

This is an underserved area — no tool does it well. It's usually done in Word documents and PowerPoint.

---

## Part 2: The Data Model

Here's what a research data system for harm reduction actually needs to represent:

```
PARTICIPANTS
├── enrollment_id (self-generated unique code)
├── demographics (age_range, gender, race_ethnicity, housing_status)
├── substance_use_profile (substances, route, frequency, duration)
├── service_history (prior treatment, harm reduction access, healthcare)
└── consent_records[]

ENCOUNTERS
├── encounter_id
├── participant_id (linked or anonymous)
├── date, time, location_type (outreach, SSP, shelter, clinic)
├── services_provided[] (supplies, naloxone, wound_care, referral, etc.)
├── brief_assessment (optional structured survey responses)
└── field_notes (free text, encrypted)

INTERVIEWS
├── interview_id  
├── participant_id
├── date, duration, location
├── audio_file (encrypted)
├── transcript (encrypted)
├── codes[] (linked to codebook)
└── memos[] (researcher reflections)

OBSERVATIONS
├── observation_id
├── date, time, location, context
├── structured_fields (configurable per protocol)
├── field_notes
└── photos[] (if IRB-approved, faces never captured)

ADVERSE_EVENTS
├── event_id
├── date, time
├── type (overdose, injury, hospitalization, death, safety_incident)
├── participant_id (if known)
├── response_taken
├── reported_to_irb (boolean, date)
└── outcome

CODEBOOK
├── code_id
├── code_name
├── definition
├── decision_rules (when to apply, when not to)
├── examples[]
├── parent_code (hierarchical)
└── theme_assignment

ANALYSIS
├── themes[]
├── joint_displays[] (mixed methods integration)
├── memos[] (analytic decisions, researcher positionality)
└── audit_trail[]
```

---

## Part 3: The Reporting Layer

She'll need to produce reports for multiple audiences:

| Audience | What They Want | Format |
|----------|---------------|--------|
| **Dissertation committee** | Methodological rigor, findings, theoretical contribution | Chapters (Word/LaTeX) |
| **IRB (continuing review)** | Enrollment numbers, adverse events, protocol deviations | Annual report via eRRM |
| **Funders (NIDA, SAMHSA, etc.)** | Progress toward aims, budget justification, publications | Grant progress reports |
| **Community advisory board** | Plain-language findings, how the research benefits the community | Presentations, briefs |
| **Journal reviewers** | Methods transparency, reproducibility, CONSORT/COREQ compliance | Manuscripts |
| **Harm reduction program** | Operational data — what's working, what's not | Dashboards, summaries |

**What we could build:** A reporting module that generates:
- IRB continuing review data automatically from the encounter database
- Enrollment/demographics tables for manuscripts (Table 1)
- COREQ checklist (Consolidated Criteria for Reporting Qualitative Research) pre-filled from audit trail
- Community-facing summary briefs (plain language, infographics)
- Funder progress templates auto-populated with current numbers

---

## Part 4: Technology Stack (Recommendation)

Given U-M constraints, the fieldwork reality, and what we'd be building:

### Frontend (Data Collection)
- **Progressive Web App (PWA)** — works offline, installable on any phone, no app store review
- **Framework:** React or Svelte (Ryan's preference — you know both)
- **Offline storage:** IndexedDB with encrypted records
- **Sync:** Background sync when connectivity returns, to approved endpoint

### Backend (Data Storage & Processing)
- **Option A: U-M hosted** — Server on Armis2 (HIPAA-compliant HPC) or departmental server
- **Option B: Self-hosted on U-M network** — Docker container on a VM within the U-M network perimeter
- **Database:** PostgreSQL with row-level encryption for identifiable fields
- **API:** REST or tRPC, TLS-only, token authentication (U-M SSO/Shibboleth)

### Analysis Pipeline
- **Transcription:** Whisper (local, no cloud) — can run on Ryan's RTX 4070 or Armis2 GPU nodes
- **Qualitative coding:** Custom web UI backed by the PostgreSQL codebook tables
- **AI-assisted coding:** Local LLM (Ollama) for initial code suggestions — never sends data to external API
- **Quantitative export:** R/SPSS/Stata format generation
- **Visualization:** D3.js or Plotly for dashboards (Ryan already uses Plotly in Stull Atlas)

### Security Layer
- **Encryption at rest:** AES-256 on all participant data
- **Encryption in transit:** TLS 1.2+
- **Key management:** Identifier keys stored separately from data, different access controls
- **Audit logging:** Every data access event logged with user, timestamp, action
- **Automated de-identification:** Pipeline that strips identifiers for analysis exports
- **Backup:** Encrypted, access-controlled, stored on U-M infrastructure

---

## Part 5: What Exists Already (Buy vs. Build Decision)

Before building anything custom, it's worth knowing what's out there:

| Tool | What It Does | Why It Might Not Work |
|------|-------------|----------------------|
| **REDCap** | Survey/data capture, U-M hosted, HIPAA | Poor offline support. Not designed for field encounters. No qualitative features. |
| **Qualtrics** | Surveys | Online-only. No encounter tracking. No longitudtic |
| **NVivo** | Qualitative coding | $1,200/yr. Clunky. No AI assistance. No field integration. |
| **Dedoose** | Mixed methods analysis | Cloud-based (data security concern). No field tools. |
| **CommCare** | Mobile data collection for public health | Open source, offline-capable. Closest existing tool. Worth evaluating. |
| **KoboToolbox** | Mobile data collection for humanitarian work | Open source, offline. Designed for field conditions. Worth evaluating. |
| **ODK (Open Data Kit)** | Mobile forms, offline, open source | Strong for structured data. No qualitative. No participant matching. |
| **Otter.ai / Rev** | Transcription | Cloud-based = data leaves the machine. HIPAA concern. |

**The gap:** No single tool handles the full pipeline — field collection + participant matching + interview transcription + qualitative coding + mixed methods integration + automated reporting. That's the system we'd build. But some components (especially mobile data collection) might be better served by extending an existing tool (CommCare, KoboToolbox) rather than building from zero.

**Recommendation:** Evaluate CommCare and KoboToolbox for the mobile data collection layer. Build custom for: participant matching, transcription pipeline, qualitative coding, reporting automation, and the integration layer that connects all of it.

---

## Part 6: Phased Build Plan

Don't try to build everything at once. Match the build to her PhD timeline:

### Phase 0: Assessment (Now → Month 1)
- Understand her exact research question and methods
- Identify which existing tools she's already using or her department expects
- Meet her faculty advisor (they may have strong opinions)
- Determine IRB application timeline

### Phase 1: Literature & Planning Tools (Months 1–3)
- Literature extraction database
- Study design documentation
- IRB application support (data management and security protocol draft)

### Phase 2: Instrument & Consent Development (Months 3–6)
- Survey instrument builder with skip logic
- Consent workflow module
- Participant enrollment system with self-generated unique codes

### Phase 3: Field Data Collection (Months 6–9)
- Mobile PWA for encounter data
- Offline capability + encrypted sync
- Interview recording + Whisper transcription pipeline

### Phase 4: Analysis Tools (Months 9–12)
- Qualitative coding interface
- Codebook management
- Data export pipeline
- Quality monitoring dashboard

### Phase 5: Reporting & Integration (Months 12–15)
- Automated reporting templates
- Mixed methods integration workspace
- Community-facing summaries

### Ongoing
- Security audits
- IRB amendment support as protocol evolves
- Bug fixes and field-driven refinements

---

## Part 7: What She Should Bring to the Meeting

If we're going to do this right, we need from her:

1. **Her research area** (even if the question isn't finalized) — what population, what intervention, what outcomes
2. **Her methodology** — quantitative, qualitative, mixed methods? Cross-sectional or longitudinal?
3. **Her current tools** — what does she use now for work data? What does her department expect?
4. **Her timeline** — when does she need to start collecting data?
5. **Her faculty advisor's name** — we'll want to understand their research program and methodological expectations
6. **Her funding status** — does she have a fellowship, RA position, or external funding? This affects what's possible.
7. **Her biggest pain point** — what is currently the hardest part of her work that technology could fix?

---

## The Bigger Picture

This isn't just a dissertation tool. If we build this right, it's a **platform for harm reduction research** that other researchers could use. The intersection of:

- Field-ready mobile data collection
- Privacy-preserving participant tracking
- AI-assisted qualitative analysis
- Mixed methods integration
- Automated compliance reporting

...doesn't exist as an integrated system. Every harm reduction researcher in the country cobbles together 5-6 tools and loses data in the gaps between them.

Isabella's dissertation could be the first deployment. But the platform could serve every School of Public Health program doing community-based substance use research. That's grant-fundable (NIDA R21/R01 for research infrastructure, AHRQ for health services research tools, NSF for sociotechnical systems).

That's the pitch, if she wants to think big.
