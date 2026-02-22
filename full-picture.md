# The Full Picture — What Else Isabella Needs

*Prepared February 21, 2026*
*The things that aren't in the architecture doc*

---

## Part 1: The Emotional Architecture

This needs to be said before anything technical.

Isabella is about to start a PhD while working in a field where people die. Not metaphorically. The people she hands clean needles to on Tuesday might overdose on Thursday. She'll carry that into her coursework, her committee meetings, her writing.

The PhD will take 4-6 years. During that time she'll:
- Watch her committee tear apart ideas she cares about
- Wait months for IRB approval while the crisis gets worse
- Feel like an imposter in academic spaces where people talk about "populations" she knows by name
- Struggle to translate lived experience into academic language without losing the truth of it
- Miss pottery because there's always another paper to write
- Possibly burn out

**What she needs from a collaborator:**
- Someone who doesn't need her to explain why this matters
- Someone who builds things that work, not things that look good in a proposal
- Someone who finishes what they start
- Someone who understands that "good enough to use in the field" beats "architecturally perfect" every time

You are all of those things. That's why she's meeting with you.

---

## Part 2: Validated Instruments She'll Likely Use

She won't build survey instruments from scratch. In substance use research, there are standard validated tools. Her committee and IRB will expect her to use or adapt established measures. Here are the ones she's most likely to encounter:

### Substance Use Assessment

| Instrument | What It Measures | Length | Notes |
|------------|-----------------|--------|-------|
| **ASSIST** (Alcohol, Smoking, and Substance Involvement Screening Test) | Risk level across 10 substance categories | 5-10 min | WHO-developed. Free. Good for screening. |
| **DAST-10** (Drug Abuse Screening Test) | Severity of drug use consequences | 10 items | Quick, validated, widely used in harm reduction |
| **AUDIT** (Alcohol Use Disorders Identification Test) | Hazardous/harmful alcohol consumption | 10 items | WHO. Standard in public health. |
| **TLFB** (Timeline Followback) | Day-by-day substance use over a period | 15-30 min | Calendar-based recall. Resource-intensive but detailed. |
| **ASI** (Addiction Severity Index) | Multi-domain assessment (medical, employment, drug, alcohol, legal, family, psych) | 45-60 min | The gold standard. Too long for field use. Better for intake settings. |

### Harm Reduction Specific

| Instrument | What It Measures | Notes |
|------------|-----------------|-------|
| **HRS** (Harm Reduction Self-Efficacy) | Confidence in safer use behaviors | Relatively new, not universally validated |
| **HRBS** (HIV Risk-taking Behavior Scale) | Injection and sexual risk behaviors | Standard in SSP research |
| **Brief Overdose Recognition & Response** | Knowledge of OD signs and naloxone use | Validates naloxone training programs |
| **PWUD Service Utilization** | Which services participants access and barriers | Usually researcher-constructed, adapts to local context |

### Mental Health & Wellbeing

| Instrument | What It Measures | Length |
|------------|-----------------|--------|
| **PHQ-9** | Depression severity | 9 items |
| **GAD-7** | Anxiety severity | 7 items |
| **PCL-5** | PTSD symptom severity | 20 items |
| **WHOQOL-BREF** | Quality of life across 4 domains | 26 items |
| **K6/K10** (Kessler) | Psychological distress | 6 or 10 items |

### Social Determinants

| Instrument | What It Measures | Notes |
|------------|-----------------|-------|
| **PRAPARE** | Social determinants of health screening | Standard in healthcare settings |
| **Housing instability measures** | Various — usually custom or adapted from HUD definitions | Critical for this population |

### Implications for Software

If we build a survey/data collection tool, it needs to:
- Support **branching/skip logic** (e.g., if DAST-10 score > 6, administer ASI)
- Handle **validated scoring algorithms** (auto-calculate risk levels, severity scores)
- Allow **instrument versioning** (she may adapt a standard tool, IRB needs to see both versions)
- Export scored data alongside raw responses
- Track which version of which instrument was administered to whom, when
- Support **multiple languages** (Spanish at minimum — many harm reduction programs serve Latinx communities)

---

## Part 3: The Funding Landscape

This matters because grants fund the work, and grants reward research infrastructure. Here's what she should know, and what you should know about positioning the software.

### For Her PhD Research

| Mechanism | What It Is | Amount | Duration | Who Can Apply |
|-----------|-----------|--------|----------|---------------|
| **NIDA F31** (Ruth L. Kirschstein NRSA) | Pre-doctoral fellowship | ~$30-45K/yr (stipend + tuition + research) | Up to 5 years | PhD students with advisor support |
| **NIDA T32** | Institutional training grant | Varies by institution | Duration of training | U-M applies; students are appointed |
| **NIMH F31** | Pre-doctoral fellowship (if mental health focus) | Same as NIDA F31 | Up to 5 years | PhD students |
| **AHRQ R36** | Dissertation grant for health services research | Up to $40K | 1-2 years | Doctoral candidates |
| **Ford Foundation** | Predoctoral or dissertation fellowship | $28K/yr | 3 years (predoc) or 1 year (dissertation) | Underrepresented scholars |
| **SAMHSA** | Generally program grants, not individual research | Varies | Varies | Usually organizations, not individuals |

**The F31 is the big one.** If she gets a NIDA F31, it funds her entire PhD. The research plan in the F31 application is where custom software gets mentioned — and having a collaborator with a working prototype makes the application stronger.

### For the Platform (Longer Term)

If the software becomes more than a dissertation tool:

| Mechanism | What It Is | Amount | Notes |
|-----------|-----------|--------|-------|
| **NIDA R21** | Exploratory/developmental research | Up to $275K over 2 years | Good for "can this tool work?" proof of concept |
| **NIDA R01** | Major research project | $250K-$500K/yr for 3-5 years | "We built it, here's the evidence it works" |
| **NIDA SBIR/STTR** | Small business research | Phase I: $250K, Phase II: $1M | Requires a small business entity. If you incorporate. |
| **AHRQ R01** | Health services research | $250K-$500K/yr | Focus on healthcare delivery improvement |
| **PCORI** | Patient-centered outcomes research | Varies widely | Focus on engagement, shared decision-making |
| **NSF SES** | Social, behavioral, economic sciences | Varies | Harder to get for substance use, but possible for the technology angle |
| **Robert Wood Johnson Foundation** | Health equity, public health innovation | Varies | Strong interest in technology for underserved populations |

### How Software Fits Into a Grant Narrative

When Isabella writes an F31, the research plan includes a "Methods" section. Custom research software goes there as:
- **Innovation**: "We developed a purpose-built data collection and analysis platform for harm reduction field research, addressing limitations of existing tools..."
- **Data Management**: "All participant data is collected via [platform name], an encrypted mobile application designed for field conditions, with automated de-identification for analysis..."
- **Rigor**: "The platform maintains a complete audit trail of all coding decisions, supporting methodological transparency and inter-rater reliability assessment..."

**This isn't just a tool — it's a fundable research contribution in its own right.** A paper could be published on the platform development: "Design and Implementation of a Privacy-Preserving Research Platform for Harm Reduction Field Research." That's a first-author publication for Isabella and a co-author credit for Ryan.

---

## Part 4: The Ethical Complexities

These are the things that make harm reduction research different from other human subjects work. The IRB will ask about all of them.

### Mandatory Reporting

In Michigan, certain professionals are **mandatory reporters** of child abuse and neglect. If Isabella is a mandatory reporter by virtue of her professional role (this depends on her specific position), and a participant discloses abuse:
- She is legally required to report it
- This obligation exists *regardless* of the research context
- Participants must be informed of this in consent documents
- The Certificate of Confidentiality does NOT override mandatory reporting

**Software implication:** The consent workflow must clearly document what participants were told about mandatory reporting. The adverse event system may need a mandatory reporting module.

### Duty to Warn / Tarasoff

If a participant discloses a credible, imminent threat to harm a specific person:
- Michigan recognizes a duty to warn/protect
- The researcher may be obligated to notify the potential victim or law enforcement
- Again, this overrides research confidentiality

### Disclosure of Criminal Activity

Participants in harm reduction research routinely disclose illegal drug use, possession, and possibly distribution. The Certificate of Confidentiality protects this information from subpoena. But:
- The certificate does NOT protect against voluntary disclosure by the researcher
- Staff must be trained: *never* voluntarily disclose participant information to law enforcement
- Data must be stored so that even if subpoenaed, individual-level illegal activity cannot be reconstructed

**Software implication:** The data model must support a level of de-identification where individual criminal behavior cannot be attributed even if the database is seized. This means:
- No real names anywhere in the database
- Timestamps rounded (not exact — exact times + locations could identify someone)
- Substance types recorded categorically, not with street-level specificity
- Location data aggregated to area level, not GPS coordinates

### Participant Compensation

IRB will scrutinize how participants are compensated because:
- Cash payments may be used to buy drugs (IRBs worry about this; harm reduction perspective: that's paternalistic)
- Gift cards can be sold for cash (same concern)
- Too much compensation = coercion (participating for the money, not voluntarily)
- Too little = exploitation (taking their time and data without fair exchange)

Standard practice: $20-30 gift cards per encounter/interview. But the IRB may push back. Isabella needs to be prepared to argue that participants deserve fair compensation regardless of how they spend it.

**Software implication:** Compensation tracking module — who received what, when, documented for IRB audit. Some programs use electronic systems to prevent duplicate compensation claims.

### Overdose During Research

If a participant overdoses during a research encounter (interview, observation, survey):
- The researcher's first obligation is to save the life (administer naloxone, call 911)
- The research session is terminated
- An adverse event report must be filed with the IRB
- The participant's data up to that point: retention depends on consent terms
- Michigan has a Good Samaritan law — calling 911 for an overdose provides limited legal protection

**Software implication:** 
- One-touch emergency protocol access in the mobile app
- Adverse event logging that captures time, response, outcome
- Automatic IRB notification workflow for reportable events
- Partial data handling — ability to mark an encounter as interrupted and retain or discard data per protocol

### Participant Death

This will happen. In longitudinal harm reduction research, some participants will die during the study period (overdose, violence, medical complications, suicide). The protocol must address:
- How death is confirmed and recorded
- Whether/how the person's data is retained
- IRB reporting requirements
- Impact on sample size and statistical power (for quantitative components)
- Impact on the researcher (see Part 1 — this is real emotional weight)

---

## Part 5: What Ryan Can Demonstrate Tomorrow

Don't just tell her what you can build. Show her what you've already built. The proof is in the work.

### 1. Stull Atlas — Complex Data at Scale
- 9,000+ glaze records, 269 tests, interactive 2D/3D visualizations
- Demonstrates: You can handle large structured datasets, build usable interfaces, maintain data integrity
- Relevant because: Her research data needs the same care

### 2. Ken Shenstone Legacy — Extraction, Analysis, Synthesis
- 43 photos analyzed by AI, Facebook data extraction, comprehensive synthesis document
- Demonstrates: You can take messy, unstructured source material and turn it into organized, queryable data
- Relevant because: Her field notes, interview recordings, and program data are messy unstructured source material

### 3. Whisper Transcription — Audio to Text, Locally
- Running on your RTX 4070, no data leaves the machine
- Demonstrates: You can transcribe interviews without sending sensitive data to the cloud
- Relevant because: This is the single most immediately valuable capability for qualitative research. Run a demo if possible.

### 4. Howell Voice — AI Pipeline Engineering
- Shows you can build end-to-end pipelines: input → processing → quality assessment → output
- Demonstrates: Systematic approach to data processing with quality gates
- Relevant because: Research data pipelines need the same rigor

### 5. The Ceramics Community Knowledge Graph
- 852 nodes, 1,793 links
- Demonstrates: You understand knowledge representation and network data
- Relevant because: Harm reduction community networks have similar structure (people, organizations, services, relationships)

### 6. This Briefing
- You built four research documents in one evening, sourced from U-M's actual IRB/HRPP pages, covering compliance, architecture, instruments, ethics, and funding
- Demonstrates: You research systematically, understand her institutional context, and take the work seriously
- Relevant because: This is exactly the kind of infrastructure work a PhD student needs but can never find

---

## Part 6: The One-Page Version

If she asks "what can you do for me?", here it is:

> **I build research infrastructure.** I can:
> 
> - Build you a **mobile data collection tool** that works offline in the field and encrypts everything on-device
> - Set up a **transcription pipeline** that converts your interview recordings to searchable, codeable text without ever sending audio to the cloud
> - Build a **participant matching system** that tracks people over time without storing their names
> - Create a **qualitative coding interface** with AI-assisted initial coding that saves you hundreds of hours
> - Generate **automated reports** for your IRB continuing review, your committee, and your funders
> - Help you **write the data management section** of your IRB application and grant proposals
> - Build all of it to meet **U-M's data security requirements** from day one
> 
> I've already researched the IRB-HSBS process, U-M's data security controls, the approved technology platforms, HIPAA implications, and the ethical complexities specific to substance use research. I've built similar systems for ceramics data, archival research, and knowledge management.
> 
> **What do you need first?**

---

## Part 7: After the Meeting

Depending on what she says, the immediate next steps are likely:

1. **If she has a research question:** Start mapping it to data collection requirements
2. **If she's still in coursework:** Start with the literature synthesis tool — she needs that now
3. **If she needs IRB help:** Draft the Data Management and Security Protocol section
4. **If she wants to see a prototype:** Build the Whisper transcription pipeline demo first (highest immediate impact, fastest to show)
5. **If she wants the mobile tool:** Start with KoboToolbox evaluation — if it covers 80%, build the other 20% custom
6. **If she's overwhelmed:** Just listen. Build the relationship. The tools come later.

Number 6 is the most likely. She's starting a PhD, working a frontline job, and trying to keep making pots. She doesn't need a system architect right now. She needs someone who understands what she's about to go through and can be there when she's ready to build.

The documents are ready when she is.
