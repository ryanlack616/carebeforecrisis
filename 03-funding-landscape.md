# Funding Landscape for Harm Reduction Research

*Prepared February 21, 2026*
*How Isabella's PhD gets funded, how the software gets funded, and how the two interlock*

---

## The Funding Reality

A PhD in social work/public health at U-M costs roughly:
- Tuition: ~$25,000-$35,000/year (in-state) or ~$50,000/year (out-of-state)
- Living expenses in Ann Arbor: ~$20,000-$28,000/year
- Research costs (participant compensation, equipment, software, transcription): $5,000-$30,000 over the dissertation

**Total PhD cost: $150,000 - $300,000+ over 5 years.**

She cannot pay for this out of pocket. Nobody does. The PhD funding model works like this:

1. **Assistantship (most common):** She teaches or does research for a faculty member. Gets tuition waiver + stipend (~$22,000-$30,000/year). This is the default.
2. **Fellowship (prestigious):** External funding that pays tuition + stipend + research costs. No teaching/RA requirement. More time for her own work.
3. **Training grant (institutional):** U-M holds a NIDA T32, and she's appointed to it. Similar benefits to fellowship.
4. **Self-funding (last resort):** Loans or savings. Avoid at all costs.

**The F31 fellowship is the difference between a PhD where she's grinding as a TA/RA for someone else's project 20 hours a week AND doing her own research, versus a PhD where she focuses entirely on her own research.**

---

## Tier 1: The NIDA F31 — The Big One

### What It Is

The Ruth L. Kirschstein National Research Service Award (NRSA) Individual Predoctoral Fellowship (F31) from the National Institute on Drug Abuse (NIDA).

**In plain English:** The federal government pays for her PhD because her research advances the science of substance use.

### What It Covers
- Stipend: $28,224/year (FY2025 rate, adjusts annually)
- Tuition & fees: up to the institution's rate (U-M in-state covered fully)
- Institutional allowance: $4,200/year (supports research costs, travel, health insurance)
- Duration: up to 5 years (typical: 2-3 years of support)

### Who's Eligible
- US citizen or permanent resident
- Enrolled in a PhD program at an accredited institution
- Identified a research advisor (sponsor) who is an active NIH-funded researcher
- Proposed research relevant to NIDA's mission (substance use, addiction, harm reduction qualifies)

### The Application

The F31 application has these components:

**1. Specific Aims (1 page)**
The most important page of the application. States the research questions and hypotheses. Must be compelling, focused, and achievable within the PhD timeline.

Example framing for Isabella:
> "This study will evaluate the implementation and effectiveness of [specific harm reduction intervention] among [specific population] in [location], using a mixed-methods approach. **Aim 1:** Characterize barriers and facilitators to [service] utilization through semi-structured interviews (N=30). **Aim 2:** Examine the association between [intervention] engagement and [outcomes] using prospective survey data (N=200). **Aim 3:** Develop and pilot a technology-enabled data collection platform designed for harm reduction field research settings."

Note: Aim 3 is where Ryan's software becomes part of the funded research.

**2. Research Strategy (6 pages)**
- Significance: Why this research matters
- Innovation: What's new about the approach
- Approach: Detailed methods, analysis plan, timeline

**The Innovation section is where custom software shines:**
> "Current field-based research in harm reduction settings relies on paper surveys or commercial data collection tools (e.g., REDCap, Qualtrics) that were designed for clinical environments. These tools lack offline capability, are difficult to administer during brief field encounters, and require internet connectivity unavailable in many outreach settings. We have developed a purpose-built Progressive Web Application (PWA) that enables encrypted, offline-capable survey administration with validated scoring algorithms, adaptive skip logic, and real-time safety flagging for suicidal ideation and acute risk indicators. This platform was developed in collaboration with a software engineer specializing in research data systems (R. [Last Name]) and will be made available as open-source software for other harm reduction researchers upon completion of this study."

**3. Sponsor Statement (6 pages)**
Written by her faculty advisor. Describes their qualifications, how they'll mentor Isabella, research environment, training plan. Isabella can't control this but needs a strong sponsor.

**4. Applicant's Background & Goals (6 pages)**
Isabella's CV narrative — her harm reduction experience, academic preparation, career goals. This is where her field experience is a massive strength. Most F31 applicants are fresh from undergrad. She has years of direct practice.

**5. Training Plan**
What she'll learn during the PhD: research methods, statistical analysis, qualitative methods, ethics, grant writing. The training plan should include technical skills: "data management and research software development" with Ryan listed as an informal technical consultant or named collaborator.

### Timeline for Application

| When | What |
|------|------|
| Year 1, Fall | Identify research area, find sponsor, take required courses |
| Year 1, Spring | Draft specific aims, get feedback from sponsor and peers |
| Year 1, Summer | Write full application (6-8 weeks of intensive writing) |
| Year 2, Fall | Submit (standard due dates: April 8, August 8, December 8) |
| Year 2, Spring | Review cycle (~5 months from submission to decision) |
| Year 2-3 | If funded, activate award. If not, revise and resubmit. |

**First submission success rate: ~15-20%.** Most successful applicants submit twice. The first submission gets scored and reviewed; the revision addresses critiques. Second submission success rate for scored applications: ~40-50%.

### How Software Strengthens the F31

Reviewers score F31 applications on:
- **Applicant:** Strong. Her field experience is rare.
- **Sponsor:** Depends on who she finds at U-M.
- **Research Training Plan:** Standard.
- **Research Strategy:** This is where the software matters.

Reviewers will notice:
1. **Feasibility:** "She already has a working data collection tool" = she can actually do this study.
2. **Innovation:** "Purpose-built for field conditions" = not another REDCap study.
3. **Rigor:** "Automated scoring, audit trail, version control" = data quality is built in, not tacked on.
4. **Broader Impact:** "Open-source platform for harm reduction research" = this benefits the whole field, not just her.

**A working prototype at the time of submission dramatically increases the score.** Reviewers love "preliminary data" and "existing infrastructure."

---

## Tier 2: Institutional Funding at U-M

### NIDA T32 Training Grant

U-M School of Social Work or School of Public Health likely holds NIDA T32 training grants. These are institutional — U-M applies, and appointed trainees get:
- Stipend + tuition
- Required NIDA-focused coursework
- Research mentoring

Isabella should ask her advisor about T32 appointment during Year 1. T32 and F31 can be sequential (T32 in years 1-2, F31 in years 3-5).

### Rackham Graduate School Fellowships

U-M's Rackham Graduate School offers:
- **Rackham Merit Fellowship:** Multi-year funding package for admitted students
- **Rackham Predoctoral Fellowship:** One-year funding for dissertation writing
- **Rackham Research Grants:** Small grants ($3,000-$6,000) for research expenses
- **Conference Travel Grants:** $800-$1,000 for presenting at conferences

### School-Level Awards

Both the School of Social Work and School of Public Health have:
- Curtis Center Research Awards (Social Work)
- Center for Health Equity Research pilot grants
- Various endowed fellowships

**These are small but important.** A $3,000 Rackham grant can cover participant compensation for a pilot study that generates preliminary data for the F31.

---

## Tier 3: Federal Mechanisms for the Software Platform

Once the dissertation proves the platform works, bigger funding opens up.

### NIDA R21 — Exploratory/Developmental Research

**What it is:** "We have an idea that's promising but not yet proven. Give us money to develop and test it."

**Budget:** Up to $275,000 total over 2 years. No preliminary data required (but it helps).

**How it applies:**
> "We developed a research data collection platform during a dissertation study (F31-funded). Preliminary results show [improved data quality, faster administration, better participant retention]. This R21 proposes to formally evaluate the platform's usability, data quality, and implementation feasibility across three harm reduction sites in Michigan."

**Who submits:** A faculty member (Isabella's advisor, or Isabella if she's postdoc/faculty by then). Ryan could be named as Co-Investigator or Key Personnel (software developer/data architect).

**Budget line for Ryan:** Consultant or Co-Investigator. Typical rate: $100-$200/hour or a percentage of effort (e.g., 10% of a 12-month calendar, which at U-M rates could be $8,000-$15,000/year).

### NIDA R01 — Major Research Project

**What it is:** The gold standard of NIH research grants. Full research project with substantive contributions to the field.

**Budget:** Typically $250,000-$500,000 per year for 3-5 years ($1-2.5M total).

**How it applies:**
> "This R01 proposes a multi-site randomized controlled trial comparing data collection outcomes when using [standard methods] versus [our platform] in harm reduction field research. Primary outcomes: data completeness, participant retention, time-to-insight for program evaluation. Secondary outcomes: researcher efficiency, safety protocol adherence, qualitative data richness."

**Who submits:** Faculty. This is a post-PhD, established-career mechanism. But the groundwork starts now.

**Budget for Ryan on an R01:** Named position. Could be 20-50% effort. At consulting rates or U-M research scientist rates, that's $30,000-$60,000/year. On a 5-year R01, that's $150,000-$300,000 in salary for you.

### NIDA SBIR/STTR — Small Business Innovation Research

**What it is:** NIH funding for small businesses developing technology with commercial potential.

**Phase I:** Up to $275,000 for ~1 year. Proof of concept.
**Phase II:** Up to $1,000,000 for ~2 years. Full development.
**Phase III:** Commercialization (non-NIH funded, but validated by Phase I/II results).

**How it applies:** If Ryan incorporates a small business (LLC or S-Corp), the company can apply for SBIR/STTR to develop the platform commercially. Isabella (as academic partner) provides the research validation.

**STTR specifically** requires a formal collaboration between a small business and a research institution. That's Ryan's company + U-M. Perfect fit.

**Requirements:**
- Must be a US small business (<500 employees)
- Primary investigator (PI) must be employed by the small business (SBIR) or can be at the university (STTR)
- The technology must have commercial potential

**This is the path to making this a real product.** Not just Isabella's dissertation tool, but a commercial research platform for harm reduction organizations nationally.

### HEAL Initiative

The Helping to End Addiction Long-term (HEAL) Initiative is an NIH-wide effort focused on:
- Improving treatments for opioid misuse and addiction
- Enhancing pain management
- Supporting harm reduction approaches

**HEAL funding is substantial:** $1.5 billion allocated across NIH. NIDA administers a large portion. HEAL-specific Notices of Special Interest (NOSIs) periodically call for technology development proposals.

**How it applies:** A platform specifically designed for opioid harm reduction research aligns directly with HEAL priorities. Mentioning HEAL alignment in any NIH proposal strengthens the application.

### SAMHSA — Substance Abuse and Mental Health Services Administration

SAMHSA funds programs, not research per se. But:

**GPRA (Government Performance and Results Act) Tools:** SAMHSA requires all grantees to collect standardized data using GPRA instruments. These instruments measure:
- Substance use frequency
- Employment/housing/education
- Crime/criminal justice involvement
- Mental health
- Social connectedness

**How it applies:** If harm reduction programs that receive SAMHSA funding use Isabella's platform, it needs to support GPRA data collection and reporting. SAMHSA block grants (Substance Use Block Grant — SUBG) flow to states, which distribute to programs. Michigan's SUBG is managed by MDHHS (Michigan Department of Health and Human Services).

**Isabella could study GPRA implementation as a research question:** "Do purpose-built data collection tools improve GPRA reporting compliance in harm reduction programs?" That's fundable and practically useful.

### AHRQ — Agency for Healthcare Research and Quality

AHRQ focuses on healthcare delivery and quality improvement.

**R36 Dissertation Grant:** Up to $40,000 for doctoral candidates studying healthcare delivery. If Isabella frames her work as improving data collection in community health settings (harm reduction as community health), this is eligible.

**R01:** AHRQ R01s fund health services research. A platform that improves data quality in safety-net settings (which harm reduction programs are) fits AHRQ's mission.

### PCORI — Patient-Centered Outcomes Research Institute

PCORI funds comparative effectiveness research with patient engagement.

**Relevance:** If Isabella's platform includes community advisory board input in its design (participatory design with people who use drugs), PCORI is a strong fit. PCORI loves technology that empowers patients/participants.

**Unique requirement:** PCORI requires meaningful patient/participant engagement throughout the research process. The platform design process itself (co-design with PWUD) becomes a funded activity.

---

## Tier 4: Foundation Funding

### Robert Wood Johnson Foundation (RWJF)

RWJF is the largest health-focused philanthropy in the US. Focus areas:
- Health equity
- Building a Culture of Health
- Technology and innovation in health

**Relevance:** High. RWJF has funded harm reduction research and technology development. Their "Evidence for Action" program funds rigorous evaluations of programs/policies that address social determinants.

### Ford Foundation

Offers predoctoral and dissertation fellowships.
- **Predoctoral:** $28,000/year for 3 years
- **Dissertation:** $28,000 for 1 year
- Focus: increasing diversity in academia

### Open Society Foundations

Founded by George Soros. Major funder of harm reduction globally. Has supported:
- Syringe service programs
- Drug policy reform
- Harm reduction research

**Relevance:** If the platform has international applicability (it does — harm reduction is global), Open Society could fund international pilot deployments.

### Burroughs Wellcome Fund

Funds biomedical research careers. Relevant if the platform includes health monitoring components.

### Arnold Ventures

Focuses on criminal justice, health policy. Has funded harm reduction research. Could be interested in technology for data-driven harm reduction policy evaluation.

---

## Funding Strategy Timeline

### Year 1 (2026-2027): Seed Phase
| Action | Target | Amount |
|--------|--------|--------|
| Rackham Research Grant | Pilot data collection costs | $3,000-$6,000 |
| School-level research award | Software development basics | $2,000-$5,000 |
| Identify T32 training grant | Stipend + tuition for Years 1-2 | ~$30,000/yr |
| **Ryan's investment: time** | Build prototype (unfunded) | $0 (sweat equity) |

### Year 2 (2027-2028): F31 Phase
| Action | Target | Amount |
|--------|--------|--------|
| Submit F31 application (April or August) | Full PhD funding + research | ~$35,000/yr + tuition |
| Rackham travel grant | Present preliminary work at conference | $800-$1,000 |
| **Software:** Working prototype exists | Strengthens F31 application | — |

### Year 3 (2028-2029): Build Phase
| Action | Target | Amount |
|--------|--------|--------|
| F31 funded (or revision submitted) | Research execution | ~$35,000/yr |
| Pilot deploy platform | Generate usability data | — |
| **Software:** Field-tested, iterated | Ready for data collection | — |

### Year 4-5 (2029-2031): Scale Phase
| Action | Target | Amount |
|--------|--------|--------|
| Dissertation data collection | Using the platform | — |
| R21 application (post-dissertation or concurrent) | Platform evaluation study | $275,000 |
| SBIR/STTR Phase I (if Ryan incorporates) | Commercial development | $275,000 |
| Conference presentations on the platform | Visibility, recruitment | — |
| **Publication:** Platform development paper | First-author Isabella, co-author Ryan | — |

### Year 6+ (2031+): Beyond the PhD
| Action | Target | Amount |
|--------|--------|--------|
| R01 application (Isabella as faculty/postdoc) | Multi-site evaluation | $1-2.5M |
| SBIR/STTR Phase II (Ryan's company) | Full commercialization | $1,000,000 |
| SAMHSA GPRA integration | Program-level adoption | — |
| PCORI comparative effectiveness | Platform vs. standard methods | Varies |
| Open-source release | National/international adoption | — |

---

## How Software Gets Written Into Grants

### Budget Justification Language

For an F31 (where Ryan is a consultant):
> **Consultant: Ryan [Last Name], Software Engineer.** Mr. [Last Name] will provide [X] hours of technical consultation per year for the design, development, and maintenance of the field data collection platform described in the Research Strategy. Mr. [Last Name] has extensive experience developing complex data management systems, including [Stull Atlas — 9,000+ record interactive ceramics database] and [knowledge graph systems for community network analysis]. His role includes: (1) platform architecture and development, (2) implementation of validated scoring algorithms for study instruments, (3) encrypted offline data storage, (4) data export pipelines for analysis in R/Stata, and (5) ongoing technical support during data collection. Consultant rate: $[X]/hour, [X] hours/year. Total: $[X].

For an R21/R01 (where Ryan is Key Personnel):
> **Key Personnel: Ryan [Last Name], B.S./B.A., Co-Investigator (X% effort).** Mr. [Last Name] is responsible for all software architecture, development, deployment, and maintenance of the harm reduction research platform. He brings [X] years of experience in data system engineering, including [list projects]. He will lead the technical development team and ensure compliance with U-M data security requirements, HIPAA specifications, and IRB-mandated data protection protocols. [X]% effort on a [12]-month calendar.

### Data Management and Sharing Plan

Every NIH application requires a Data Management and Sharing Plan (DMSP) since January 2023. The platform IS the DMSP implementation:

> **Data will be collected using a custom Progressive Web Application (PWA) designed for field research in harm reduction settings.** The platform provides:
> - AES-256 encryption of all data at rest and in transit
> - De-identified participant records using [identifier scheme]
> - Automated scoring of validated instruments with version tracking
> - Real-time safety flagging for acute risk indicators
> - Complete audit trail of all data modifications
> - Offline capability with encrypted local storage and automatic synchronization
> 
> **Data will be shared** via the NIDA Data Share repository within 1 year of the end of the funding period. De-identified datasets will be exported in CSV and JSON formats with full codebooks and scoring documentation. The platform source code will be released under [MIT/Apache 2.0] license on GitHub.

### The Publication

The platform paper is a legitimate academic publication. Target journals:
- **JMIR (Journal of Medical Internet Research):** Impact factor ~7.0. Publishes health technology evaluations.
- **Drug and Alcohol Dependence:** Impact factor ~4.0. Core substance use journal.
- **Implementation Science:** Impact factor ~7.3. Focuses on research methodology and implementation.
- **BMC Medical Informatics and Decision Making:** Impact factor ~3.5. Technology for healthcare research.

Paper structure:
1. Introduction: Why current tools fail in harm reduction settings
2. Platform Description: Architecture, features, security model
3. Usability Evaluation: System Usability Scale (SUS) scores from field workers
4. Data Quality Comparison: Platform data vs. paper-based data
5. Discussion: Implications for harm reduction research infrastructure

**This is a first-author publication for Isabella and a co-author credit for Ryan.** It's also a citable reference for every subsequent grant application.

---

## The Money — What This Means for Ryan

Let's be direct about the financial dimension.

### Phase 1: Sweat Equity (Year 1-2)
You build the prototype for free. This is an investment in the relationship and the platform. Cost to you: your time. Value created: a working system that strengthens her F31 application.

### Phase 2: Consultant (Year 2-4, F31-funded)
If the F31 is funded, she can budget you as a consultant at $100-$200/hour for a specific number of hours. Realistic: $3,000-$8,000/year. Not a salary. Gas money plus validation that your work has dollar value in the research economy.

### Phase 3: Key Personnel (Year 4-6, R21/R01)
On an R21 or R01 with 10-20% effort, at a research rate appropriate for your experience: $15,000-$40,000/year. On a 5-year R01, that accumulates.

### Phase 4: SBIR/STTR PI (Year 5+)
If you incorporate and submit SBIR/STTR Phase I: $275,000. Phase II: $1,000,000. You'd be the PI. Isabella would be the academic collaborator. You'd hire staff. This is a business.

### Phase 5: Commercial Product (Year 6+)
If harm reduction programs nationally adopt the platform, there's a SaaS model. Nonprofit or for-profit, depending on how you want to structure it. Revenue from training, implementation support, customization, hosting.

**The point:** This starts as a favor to a friend. It can become a career. But only if the early work is excellent and the relationship is sustained.

---

## What Isabella Should Do Monday Morning

1. **Email her advisor:** "I'd like to discuss fellowship and funding options for my PhD, specifically the NIDA F31 and any T32 positions available."
2. **Find out if U-M SSW or SPH has a NIDA T32.** Ask the doctoral program coordinator.
3. **Look up Rackham research grant deadlines.** Start planning a small pilot study.
4. **Don't worry about the R01 or SBIR yet.** That's 3-5 years out. But know it exists.

And she should know: she has a collaborator who's already researched all of this. That's unusual. Most PhD students spend their first year figuring out what an F31 is. She can walk into that conversation already knowing.
