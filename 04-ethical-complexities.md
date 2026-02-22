# Ethical Complexities in Harm Reduction Research

*Prepared February 21, 2026*
*The legal, ethical, and human dimensions that make this research different from everything else*

---

## Why Ethics in Harm Reduction Research Are Different

In most research, the ethical framework is straightforward: don't hurt the participants, get their consent, protect their data. The Belmont Report principles — respect for persons, beneficence, justice — are clear enough when you're studying college students filling out surveys.

Harm reduction research breaks everything.

The participants are involved in illegal activity. They may be impaired during the research encounter. They may be in active crisis. They may die during the study. The researcher may witness crimes, overdoses, and violence. The data, if disclosed, could lead to arrest, child removal, loss of housing, or other catastrophic consequences.

And yet: this research is essential. Without it, we can't improve the services that keep people alive. The ethical obligation to conduct the research is as strong as the ethical obligation to protect the participants.

Isabella will live in this tension for her entire PhD.

---

## The Certificate of Confidentiality

### What It Is

A Certificate of Confidentiality (CoC) is a federal protection that prevents identifiable research data from being used in any legal proceeding — including subpoenas, court orders, and law enforcement requests.

Since 2017, all NIH-funded research automatically receives a CoC. If Isabella gets F31 funding, she has one. If she doesn't get NIH funding, she can apply for one separately.

### What It Protects

- **No forced disclosure.** A prosecutor cannot subpoena her data. A judge cannot compel her to testify about what a participant told her. Law enforcement cannot seize her records.
- **No voluntary disclosure.** Isabella and her team cannot voluntarily disclose identifying information in response to a legal demand, even if the participant gives permission.
- **Permanent.** The protection survives the end of the study. Data collected under a CoC is protected forever.

### What It Does NOT Protect

This is the critical part. The CoC has exceptions:

1. **Voluntary disclosure by the participant.** If a participant tells police what they told Isabella, that's the participant's choice. The CoC doesn't prevent participants from disclosing their own information.

2. **Information for auditing or evaluation by the funding agency.** NIH can review the data for compliance purposes (but not share it with law enforcement).

3. **Required reporting under federal, state, or local law.** This is the big one:
   - **Child abuse/neglect:** Michigan law requires certain professionals to report. If Isabella is a mandatory reporter and a participant discloses abuse of a child, she must report it regardless of the CoC.
   - **Communicable disease reporting:** Certain diagnoses (HIV, hepatitis, TB) have mandatory reporting requirements to public health authorities.
   - **Imminent harm:** If a participant discloses a credible plan to harm themselves or a specific other person, duty to warn/protect may apply.

4. **Information needed for treatment/medical care.** If a participant overdoses and medical personnel need information for treatment, the CoC doesn't prevent sharing medically necessary information.

### Software Implications

The CoC shapes the entire data architecture:

```
CERTIFICATE_OF_CONFIDENTIALITY_REQUIREMENTS:

  data_storage:
    - Identifying information MUST be stored separately from research data
    - The "link" between identifiers and research data must be:
      * Encrypted (AES-256 minimum)
      * Access-controlled (PI and designated key personnel only)
      * Destroyable (can be deleted without destroying research data)
    - Upon study completion: destroy the link. Keep de-identified data.

  consent_documentation:
    - Consent form must explain CoC protections AND limitations
    - Must explicitly list exceptions (mandatory reporting, imminent harm)
    - Participant must sign (or verbally consent, documented) acknowledging exceptions
    - Software must log: consent version, date, method, exceptions acknowledged

  disclosure_logging:
    - If ANY disclosure occurs (mandatory report, safety intervention), log:
      * Date/time of disclosure
      * Reason (which exception triggered it)
      * What information was disclosed and to whom
      * Whether participant was notified
      * IRB notification status
    - This log is itself protected by the CoC

  data_seizure_defense:
    - Even if hardware is seized (stolen laptop, search warrant on a partner org):
      * Encrypted data cannot be compelled to be decrypted
      * De-identified data has no identifying information to disclose
      * Separated identifiers mean that even if one system is compromised, 
        identities cannot be linked to research responses
```

---

## Mandatory Reporting in Michigan

### Who Is a Mandatory Reporter?

Under Michigan's Child Protection Law (MCL 722.623), mandatory reporters include:
- Licensed social workers
- Teachers and school personnel
- Physicians, nurses, and other healthcare professionals
- Law enforcement
- Clergy (with exceptions)
- "Any other person who is required to report"

**The question for Isabella:** Is she a mandatory reporter?

If she is a **licensed social worker** (LMSW, LBSW, LLMSW), yes. If she's currently working in a harm reduction program in a clinical capacity, almost certainly yes.

If her role is **researcher only** (no clinical duties), the answer is less clear. Michigan's mandatory reporting law targets professionals who work with children or vulnerable adults in a professional capacity. A researcher who encounters evidence of child abuse during a research interview is in a gray area that most IRBs resolve by treating the researcher as a mandatory reporter anyway.

**The practical resolution:** Isabella's IRB application will include a statement like:

> "If a participant discloses information indicating that a child is being abused or neglected, the research team member will follow Michigan mandatory reporting requirements. The participant will be informed of this obligation during the consent process. The consent form will include the following language: 'If you tell us something that makes us believe a child is being hurt or in danger, we are required by law to report this to the authorities. We will tell you if we need to make a report.'"

### What Gets Reported

Mandatory reporting is triggered by "reasonable cause to suspect" child abuse or neglect. This includes:
- Physical abuse (injuries, bruises, burns)
- Sexual abuse
- Neglect (failure to provide food, shelter, medical care, supervision)
- Emotional abuse
- Exposure to domestic violence (Michigan considers this neglect in some circumstances)
- Exposure to drug manufacturing (meth labs, etc.)
- Prenatal substance exposure (Michigan considers this grounds for investigation, though not always formally CPS involvement)

**The harm reduction complication:** Many participants are parents. Some are using drugs while parenting. Discussing substance use patterns may indirectly disclose potential child neglect. This creates a chilling effect: if participants know that talking about their drug use could trigger a CPS investigation, they won't talk. The research dies.

**How to handle this ethically:**
1. Consent form clearly states what triggers a report (and what doesn't)
2. The research questions are framed to avoid directly eliciting child welfare information
3. Staff are trained on the specific threshold: "reasonable cause to suspect" — not "any mention of parenting + drug use"
4. If a report is needed, the participant is told first (when safe to do so)
5. The report is documented and reported to the IRB as a protocol event

### Software Implementation

```
MANDATORY_REPORTING_MODULE:

  consent_workflow:
    mandatory_reporting_disclosure:
      text: "We are required by Michigan law to report suspected 
             child abuse or neglect. If you tell us something that 
             gives us reason to believe a child is being hurt or 
             neglected, we will need to make a report. We will tell 
             you if this happens."
      acknowledged: boolean
      method: "verbal" | "written" | "audio-recorded"
      timestamp: datetime
      witness: string  # name of second person present

  incident_protocol:
    trigger: "researcher judgment"  # NOT automated — this is never an algorithm decision
    steps:
      1. "Note the disclosure (do not probe for details)"
      2. "Consult with PI within 24 hours (or immediately if imminent danger)"
      3. "PI determines whether 'reasonable cause to suspect' threshold is met"
      4. "If yes: file report with Michigan DHHS (855-444-3911)"
      5. "Inform participant that a report was filed (when safe)"
      6. "Document everything in incident log"
      7. "File adverse event report with IRB within 5 business days"
      8. "Debrief with team — secondary trauma support"

  incident_log:
    date: datetime
    participant_id: de-identified
    disclosure_summary: string  # minimal detail, no names
    pi_consulted: boolean
    pi_consultation_date: datetime
    report_filed: boolean
    report_date: datetime
    report_agency: "michigan_dhhs"
    participant_notified: boolean
    irb_notified: boolean
    irb_notification_date: datetime
    researcher_debrief: boolean
    notes: string
```

---

## Duty to Warn / Tarasoff Obligations

### The Legal Framework

The *Tarasoff v. Regents of the University of California* (1976) case established that mental health professionals have a duty to protect identifiable potential victims when a patient/client makes a credible threat of violence.

Michigan follows a modified Tarasoff doctrine:
- Mental health professionals have a duty to warn when a patient threatens a specific, identifiable victim
- The duty may extend to other professionals in a therapeutic or clinical relationship
- Researchers are in an ambiguous position — generally not considered to have a therapeutic relationship, but IRBs err on the side of inclusion

### How This Applies to Harm Reduction Research

Participants in harm reduction research may disclose:
- Plans to harm a specific person (domestic violence, retaliation)
- Plans for self-harm or suicide (see PHQ-9 Q9 discussion in instruments document)
- Knowledge of imminent violence against others
- Active involvement in serious criminal activity (not drug use — more like trafficking, assault, weapons)

**The key distinction:** Duty to warn applies when there is a **credible, imminent threat to an identifiable person.** It does not apply to:
- Past violence ("I beat someone up last year")
- General anger ("I hate my landlord")
- Drug use (even illegal — protected by CoC)
- Property crime

### The Informed Consent Obligation

Participants must know, before the interview begins, that there are limits to confidentiality. The consent form must include language like:

> "Everything you tell us is confidential and protected by a Certificate of Confidentiality. However, there are situations where we may need to break confidentiality:
> 1. If you tell us you are planning to hurt yourself or someone else
> 2. If you tell us about a child being abused or neglected
> 3. If you need emergency medical care during our conversation
> 
> In these situations, we will tell you what we need to do and try to involve you in the decision."

### Software Implementation

```
DUTY_TO_WARN_PROTOCOL:

  # This is NEVER automated. No algorithm. Ever.
  # A machine cannot assess credibility, imminence, or identifiability.
  # The software provides protocol guidance and documentation — 
  # the HUMAN makes the judgment.

  protocol_access:
    location: "always accessible from any screen — single tap/click"
    display: "Emergency Protocols" button, visible but not intrusive
    
  when_activated:
    display_steps:
      1. "Remain calm. Do not show alarm."
      2. "Do not probe for details beyond what was voluntarily disclosed."
      3. "Assess: Is there a specific, identifiable person at risk?"
      4. "Assess: Is the threat credible? (Not hypothetical or past)"
      5. "Assess: Is the threat imminent? (Near-term timeframe)"
      6. "If YES to all three: contact PI immediately."
      7. "If uncertain: contact PI for consultation."
      8. "Document the disclosure and your assessment."
    
  pi_decision_tree:
    if credible_imminent_identifiable_threat:
      → "Contact local law enforcement or notify potential victim"
      → "Notify participant that disclosure will be made"
      → "File IRB adverse event report"
      → "Document everything"
    if uncertain:
      → "Consult with IRB analyst"
      → "Consult with U-M legal counsel if needed"
      → "Document the consultation and decision"
    if not_reportable:
      → "Document why the threshold was not met"
      → "Continue research encounter if appropriate"
      → "Offer resources/referrals to participant"
```

---

## Suicidal Ideation During Research

### The Problem

Substance use and suicidality are deeply intertwined. People who use drugs have significantly elevated suicide risk. In harm reduction research, suicidal ideation will surface — sometimes through instruments (PHQ-9 Q9), sometimes spontaneously during interviews.

### The Ethical Framework

The researcher is not a therapist. She does not provide clinical intervention. But she has an ethical obligation to:
1. Assess the immediate level of risk
2. Provide appropriate resources
3. Ensure the person is not in immediate danger before ending the encounter
4. Document the event
5. Report to the IRB

### The Safety Protocol

Most IRBs in substance use research require a **Safety Protocol** that specifies exactly what happens when suicidal ideation is disclosed:

```
SUICIDE_SAFETY_PROTOCOL:

  trigger_1: "PHQ-9 Q9 > 0 (any endorsement of self-harm thoughts)"
  trigger_2: "Spontaneous verbal disclosure of suicidal thoughts/plans"
  trigger_3: "Researcher observation of acute distress/self-harm"

  immediate_steps:
    1. "Pause the research. This is no longer a research encounter."
    2. "Express concern without clinical judgment."
       example: "I want to make sure you're okay. You mentioned 
                 [thoughts about hurting yourself]. Can we talk 
                 about that for a minute?"
    3. "Administer the Columbia Suicide Severity Rating Scale (C-SSRS) 
        screening questions (6 items, <2 minutes)"

  C_SSRS_SCREENING:
    Q1: "Have you wished you were dead or wished you could go to sleep 
         and not wake up?" (passive ideation)
    Q2: "Have you actually had any thoughts of killing yourself?" 
         (active ideation)
    
    IF Q2 == Yes:
      Q3: "Have you been thinking about how you might do this?" (method)
      Q4: "Have you had these thoughts and had some intention of 
           acting on them?" (intent)
      Q5: "Have you started to work out or worked out the details of 
           how to kill yourself? Do you intend to carry out this plan?" 
           (plan)
      Q6: "Have you ever done anything, started to do anything, or 
           prepared to do anything to end your life?" (behavior)

  risk_determination:
    LOW_RISK: "Q1 only — passive ideation, no active thoughts"
      → Provide crisis resources (988 Lifeline, local crisis number)
      → Ask if they'd like to continue the research encounter or stop
      → Document in incident log
      → Notify PI within 24 hours
      
    MODERATE_RISK: "Q2 or Q3 — active ideation, with or without method"
      → Provide crisis resources
      → Strongly encourage calling 988 or going to ER
      → Do NOT leave the person alone until they have a plan for safety
      → End the research encounter
      → Notify PI immediately
      → File IRB adverse event report
      
    HIGH_RISK: "Q4, Q5, or Q6 — intent, plan, or past behavior"
      → Call 988 or 911 WITH the participant (or for them)
      → Do NOT leave the person alone
      → Stay until emergency services arrive
      → End the research encounter
      → Notify PI immediately
      → File IRB adverse event report as SERIOUS adverse event
      → Team debrief within 72 hours (researcher secondary trauma)

  crisis_resources_card:
    # Pre-loaded on the device. Accessible offline. Updated quarterly.
    national:
      - "988 Suicide and Crisis Lifeline: call or text 988"
      - "Crisis Text Line: text HOME to 741741"
    local_ann_arbor:
      - "U-M Psychiatric Emergency Services: (734) 996-4747"
      - "Washtenaw County CMH Crisis Line: (734) 544-3050"  
      - "Dawn Farm (24-hour): (734) 485-8725"
    harm_reduction_specific:
      - "SAMHSA National Helpline: 1-800-662-4357"
      - "Local SSP contact (updated with program-specific number)"
```

### Software Implementation

```
SAFETY_PROTOCOL_IN_APP:

  PHQ9_Q9_handler:
    on_response(value):
      if value > 0:
        pause_instrument()
        display_safety_protocol()
        log_safety_event({
          trigger: "PHQ-9 Q9",
          value: value,
          timestamp: now(),
          participant_id: current_participant,
          researcher_id: current_user
        })
        # Researcher follows protocol steps on screen
        # After completing protocol:
        prompt_researcher({
          options: [
            "Participant is safe — continue research encounter",
            "Participant referred to crisis services — end encounter",
            "Emergency services contacted — end encounter",
            "Participant declined all services — document and end"
          ]
        })
        log_safety_resolution(selected_option)
        
  spontaneous_disclosure_button:
    # Always visible, always accessible
    label: "Safety Protocol"
    on_click:
      display_safety_protocol()
      log_safety_event({ trigger: "spontaneous_disclosure" })

  crisis_resources:
    # Cached on device. No internet required.
    # Updated when device syncs with server.
    accessible_from: "every screen in the application"
```

---

## Overdose During a Research Encounter

### This Will Happen

If Isabella is conducting research in harm reduction settings — at syringe service programs, during outreach, near places where people use — she will encounter overdose. Not "might." Will.

She needs to be prepared for:
1. A participant who arrives intoxicated and deteriorates during the encounter
2. A participant who uses immediately before or after the research encounter
3. An overdose occurring nearby during research activities (not a participant, but someone in the area)
4. Finding an unresponsive person while traveling to/from a field site

### The Protocol

```
OVERDOSE_RESPONSE_PROTOCOL:

  step_1_recognize:
    signs: [
      "Unresponsive to voice or touch",
      "Slow, shallow, or absent breathing",
      "Blue or gray lips/fingertips",
      "Pinpoint pupils",
      "Snoring or gurgling sounds",
      "Limp body"
    ]

  step_2_respond:
    1. "Call 911 immediately"
       # Michigan Good Samaritan Law (MCL 333.7453) provides limited 
       # immunity for calling 911 for an overdose
    2. "Administer naloxone (Narcan)"
       # Isabella should ALWAYS carry naloxone in the field
       # Two doses minimum
       # Intranasal (4mg spray) — no training required
    3. "Begin rescue breathing if trained"
    4. "Place person in recovery position (on their side)"
    5. "Stay until EMS arrives"
    6. "Provide EMS with any relevant information 
        (DO NOT share research data — only immediate medical needs)"

  step_3_document:
    AFTER the emergency is resolved:
    
    adverse_event_report:
      date: datetime
      location: string (general area, not GPS)
      person: "participant" | "non-participant" | "unknown"
      participant_id: de-identified (if applicable)
      substances_suspected: string (if known)
      naloxone_administered: boolean
      naloxone_doses: integer
      ems_called: boolean
      ems_arrival_time: datetime
      outcome: "recovered_scene" | "transported" | "deceased" | "unknown"
      research_data_status: "collected_prior" | "partial" | "none"
      data_retained: boolean  # per protocol and consent terms
      researcher_debriefed: boolean
      
    irb_reporting:
      report_type: "serious_adverse_event" if participant
      report_deadline: "within 5 business days of becoming aware"
      report_to: "IRB-HSBS via eRRM"
      
  step_4_aftermath:
    researcher_support:
      - "Debrief with PI within 24 hours"
      - "Access to university counseling services"
      - "No expectation to return to field that day"
      - "Incident review with team (not blame — learning)"
    
    data_decisions:
      if participant_data_collected_before_overdose:
        - "PI reviews consent terms for data retention"
        - "If consent allows, data is retained and flagged"
        - "If consent is ambiguous, consult IRB analyst"
        - "If participant deceased, data retention follows 
           death protocol (see below)"
```

### Naloxone Supply Chain

The app could include a naloxone inventory tracker:
```
NALOXONE_TRACKER:
  researcher_inventory:
    current_doses: integer
    expiration_date: date
    resupply_source: "SSP" | "pharmacy" | "study supply"
    
  alerts:
    if current_doses < 2: "RESUPPLY NEEDED"
    if expiration_date < today + 30: "EXPIRING SOON"
    
  usage_log:
    # When naloxone is administered in any context
    date: datetime
    reason: "participant_overdose" | "bystander_overdose" | "training_demo"
    doses_used: integer
    outcome: string
```

---

## Participant Death

### The Reality

In longitudinal harm reduction research with people who use drugs, mortality rates are 2-5x higher than the general population. In a study following 200 participants over 2-3 years, it is statistically expected that 2-10 participants will die. Causes include:
- Overdose (most common)
- Violence (homicide)
- Suicide
- Medical complications (endocarditis, sepsis, hepatitis, HIV-related)
- Accidents
- Exposure/hypothermia (for unsheltered participants)

### What the Protocol Must Address

**1. How death is identified and confirmed**
The researcher may learn of a death through:
- Program staff at the SSP/harm reduction site
- Other participants ("Did you hear about Marcus?")
- Failed follow-up attempts (participant unreachable for months)
- Media reports
- Obituary/death records

**Important:** Participant death must be confirmed through reliable means before being recorded. Word-of-mouth from other participants is not confirmation — people may be incarcerated, hospitalized, or simply unreachable.

In studies with vulnerable populations, researchers sometimes use the National Death Index (NDI) or state vital records to confirm deaths. This requires advance IRB approval and specific data use agreements.

**2. Data retention**
When a participant dies:
- Their existing data is generally retained (consent typically covers this)
- Consent forms should include language about posthumous data use
- De-identification becomes even more critical — the person can no longer advocate for their own privacy
- Data is flagged as belonging to a deceased participant
- Analysis notes the death as a form of "attrition" — but codes it honestly

**3. Impact on the study**
- Sample size decreases (affects statistical power)
- Loss to follow-up analysis must distinguish between death and other reasons for dropout
- If deaths cluster in a subgroup, this itself may be a finding
- Survival analysis becomes part of the methodology

**4. IRB reporting**
- Participant death is a Serious Adverse Event (SAE)
- Must be reported to IRB within 5 business days
- IRB will evaluate whether the death is related to the research (almost certainly not — these are deaths due to substance use or its consequences, not due to research participation)
- Continuing review must include counts of participant deaths
- If death rate exceeds expected levels, IRB may require protocol modification

**5. Impact on Isabella**
A participant she has interviewed, whose stories she has transcribed, whose voice she recognizes — dies. And she has to:
- Write it up as an adverse event report
- Code their reason for study dropout as "deceased"
- Continue analyzing their data, hearing their voice on recordings
- Present findings at her committee meeting as though this is just a data point

It's not just a data point. And she needs a collaborator who understands that.

### Software Implementation

```
PARTICIPANT_DEATH_PROTOCOL:

  recording:
    participant_status: "active" | "withdrawn" | "lost_to_followup" | "deceased"
    
    when status_changed_to "deceased":
      death_record:
        date_of_death: date | "unknown"
        date_research_team_notified: date
        source_of_information: "program_staff" | "other_participant" | 
                                "death_records" | "media" | "family" | "other"
        confirmed: boolean
        confirmation_source: string
        cause_of_death: "overdose" | "violence" | "suicide" | "medical" | 
                        "accident" | "unknown" | "not_available"
        related_to_research: "definitely_not" | "unlikely" | "possibly" | "likely"
        
  data_handling:
    existing_data: "retained per consent terms"
    data_flag: "deceased — handle with additional care"
    de_identification_review: "PI reviews all data for re-identification risk"
    posthumous_quotes:
      policy: "May be used in publications if de-identified"
      extra_care: "Remove any details that could identify the person 
                   to family or community members who may read the publication"
    audio_recordings:
      policy: "Retained for analysis per consent"
      transcription: "Continue as planned"
      voice_identification_risk: "Flag for PI review"
      
  irb_reporting:
    report_type: "serious_adverse_event"
    deadline: "5 business days"
    content: [
      "participant_id (de-identified)",
      "date of death (if known)",
      "cause (if known)",
      "relationship to research participation (assessment)",
      "total participant deaths to date",
      "comparison to expected mortality rates"
    ]
    
  team_support:
    immediate:
      - "PI acknowledges the death with the team"
      - "No expectation to 'move on quickly'"
      - "Team meeting is not about data — it's about the person"
    ongoing:
      - "Access to counseling services"
      - "Permission to skip analysis of that participant's data 
         for a period (someone else does it)"
      - "Annual team reflection on cumulative loss"
```

---

## Participant Compensation Ethics

### The Tension

People who participate in research deserve compensation. Their time has value. Their knowledge has value. The data they provide advances science and eventually (hopefully) improves services for people like them.

But IRBs worry about "undue inducement" — the idea that compensation could be so attractive that it clouds someone's ability to make a truly voluntary decision about participating.

In substance use research, this worry gets amplified by a secondary concern: that cash or near-cash compensation will be used to buy drugs.

### The Harm Reduction Response

The harm reduction perspective says:
1. People who use drugs deserve the same fair compensation as any other research participant
2. What participants do with their compensation is their business — just like college student participants who spend their $20 gift card on beer
3. Withholding compensation or providing only non-cash alternatives (bus tokens, food vouchers) is paternalistic and potentially discriminatory
4. The difference between a harm reduction participant and a psychology-department participant is stigma, not ethics

### What the IRB Will Ask

Isabella's IRB application will need to justify:
- **Amount:** Why $[X] per encounter? (Usually: comparable to minimum wage for time spent, comparable to similar published studies)
- **Form:** Cash, gift card, check? (Usually gift cards; some IRBs allow cash for populations without bank accounts or addresses)
- **Timing:** Paid immediately after each encounter, or accumulated? (Immediate is better — delayed payment is a retention tool, not ethical compensation)
- **Total exposure:** Over the whole study, what's the maximum a participant could receive? (Usually capped to avoid "professional participant" concern)
- **Partial completion:** If a participant completes half the survey, do they get full payment? (Yes — payment is for time and willingness, not for data completeness)

### Standard Practices

| Study Component | Typical Compensation | Notes |
|----------------|---------------------|-------|
| Screening survey (10-15 min) | $10-15 | Sometimes no compensation for screening |
| Baseline assessment (30-60 min) | $25-40 | |
| Follow-up survey (15-30 min) | $20-30 | |
| In-depth interview (60-90 min) | $40-50 | |
| Focus group (90-120 min) | $35-50 | Plus food/drink |
| Biological sample (urine, blood) | Additional $10-15 | On top of encounter compensation |

### Software Implementation

```
COMPENSATION_TRACKING:

  participant_compensation:
    participant_id: de-identified
    encounters: [
      {
        date: datetime,
        type: "screening" | "baseline" | "followup" | "interview" | "focus_group",
        duration_minutes: integer,
        compensation_amount: decimal,
        compensation_form: "visa_gift_card" | "cash" | "check" | "other",
        gift_card_number: string (last 4 digits only — for audit, not identification),
        receipt_signed: boolean,
        partial_completion: boolean,
        notes: string
      }
    ]
    total_compensation: computed decimal
    maximum_allowed: decimal (per protocol)
    
  ALERTS:
    if total_compensation > maximum_allowed * 0.8:
      "Participant approaching maximum compensation limit"
    if total_compensation >= maximum_allowed:
      "Maximum compensation reached — no further payments"
      
  AUDIT_TRAIL:
    # IRB may request compensation records during continuing review
    export_format: "date, participant_id, amount, form, encounter_type"
    summary: "total participants compensated, total amount disbursed, 
              average per participant"
    
  DUPLICATE_PREVENTION:
    # For programs serving many people, prevent the same person 
    # from enrolling multiple times for compensation
    method: "match on study-specific identifier — 
             NOT on name or personal information"
    # This is related to the unique identifier problem 
    # discussed in the architecture document
```

---

## The Consent Problem

### Why Standard Consent Doesn't Work

Standard informed consent assumes:
- The participant is sober and cognitively clear
- They can read and comprehend a multi-page document
- They have time to review the document and ask questions
- The setting is private and comfortable
- They can sign their name without fear

In harm reduction field research:
- The participant may be intoxicated, in withdrawal, or both
- Reading level may be low; document may be in wrong language
- Time may be limited (they're waiting for services, staff, or someone)
- The setting is a parking lot, van, or drop-in center with no privacy
- Signing a document = leaving a paper trail of illegal activity proximity

### Models for Consent in the Field

**1. Simplified Written Consent**
- Maximum 2 pages
- Written at 5th-grade reading level
- Large font, bullet points, no legal jargon
- Covers: what the study is about, what they'll do, risks, benefits, confidentiality protections and exceptions, voluntary participation, who to contact
- Signature line

**2. Verbal Consent with Documentation**
- Researcher reads consent information aloud
- Participant asks questions
- Participant gives verbal agreement
- Researcher signs a form documenting that consent was obtained verbally
- A witness (second staff member) also signs
- IRB must approve waiver of written consent (documented in section 5 of the protocol)

**3. Audio-Recorded Consent**
- Same as verbal, but recorded
- Creates an auditable record without requiring signature
- Recording stored with same encryption as research data
- Particularly useful for participants who are illiterate or who refuse to sign documents

**4. Tiered Consent**
- Consent for different components separately:
  * "Do you consent to the survey?" (Y/N)
  * "Do you consent to audio recording?" (Y/N)
  * "Do you consent to follow-up contact?" (Y/N)
  * "Do you consent to your de-identified data being shared with other researchers?" (Y/N)
- Allows participation at the level they're comfortable with

**5. Ongoing Consent / Process Consent**
- Not a one-time event
- At each encounter: "We talked before about this study. You're still participating because you want to, right? Remember, you can stop at any time."
- Particularly important for longitudinal studies where circumstances change
- Participant who consented while stable may be in crisis at follow-up

### Competency to Consent

The hardest ethical question in harm reduction research: **can a person who is currently intoxicated give informed consent?**

The answer is nuanced:
- **Active intoxication does not automatically invalidate consent.** Many people who use substances maintain decision-making capacity while intoxicated.
- **Severe intoxication (incoherent, cannot track conversation, unable to repeat back what the study involves) does invalidate consent.** 
- **Withdrawal may impair consent as well** — pain, desperation, and distress affect judgment.
- The researcher must assess capacity at the time of consent, not assume it based on substance use status.

```
CONSENT_CAPACITY_ASSESSMENT:

  minimum_criteria:
    - "Participant can state what the study is about (in their own words)"
    - "Participant can identify at least one risk of participation"
    - "Participant can state that they can stop at any time"
    - "Participant is oriented to person, place, and time"
    - "Participant's speech is coherent and responsive to questions"
    
  if_criteria_not_met:
    - "Do NOT proceed with consent or research encounter"
    - "Offer to reschedule when they're feeling better"
    - "Provide services (naloxone, supplies) regardless — these are 
       not contingent on research participation"
    - "Document the encounter as 'approached but not consented — 
       capacity concerns'"
    - "Do NOT record the reason as 'intoxicated' — 
       record as 'unable to demonstrate understanding of study'"

  re_consent:
    # At each follow-up, briefly confirm ongoing consent
    check: "You're here for the [study name] today. 
            You know you can stop at any time, right? 
            Anything you want to ask before we start?"
    log: { participant_id, date, re_consent_confirmed: boolean, notes }
```

### Software Implementation for Consent

```
CONSENT_MODULE:

  consent_workflow:
    step_1_capacity_check:
      researcher_confirms: [
        "Can state study purpose",
        "Can identify a risk", 
        "Knows they can stop",
        "Oriented and coherent"
      ]
      all_confirmed: boolean → proceed or reschedule
      
    step_2_disclosure:
      method: "read_aloud" | "participant_reads" | "audio_playback"
      language: "en" | "es" | "ar"
      version: consent_form_version_id
      
    step_3_questions:
      questions_asked: boolean
      questions_summary: string (free text)
      
    step_4_agreement:
      method: "written_signature" | "verbal" | "audio_recorded"
      tiered_consent: {
        survey: boolean,
        audio_recording: boolean,
        follow_up_contact: boolean,
        data_sharing: boolean
      }
      
    step_5_witness:
      witness_name: string
      witness_role: string
      
    step_6_documentation:
      consent_id: UUID
      participant_id: de-identified
      date: datetime
      form_version: string
      method: string
      tiered_selections: object
      capacity_confirmed: boolean
      researcher_id: string
      witness_id: string
      
    storage:
      # Consent documentation is IDENTIFYING (signature or voice)
      # Store SEPARATELY from research data
      # Encrypted
      # Access restricted to PI
```

---

## Research Staff Safety

### The Risks Are Real

Harm reduction fieldwork is not dangerous in the way Hollywood portrays. Most people who use drugs are not violent. But the environments are unpredictable:
- Informal settlements (encampments) may have territorial dynamics
- Drug markets have their own security concerns
- Mental health crises can escalate quickly
- Needlestick injuries from improperly disposed syringes
- Exposure to fentanyl (skin contact risk is minimal but anxiety is real)
- Traffic (much fieldwork happens in parking lots and under overpasses)
- Weather exposure (winter outreach in Michigan)

### Safety Protocol

```
RESEARCHER_SAFETY_SYSTEM:

  before_fieldwork:
    check_in:
      who: "research coordinator or PI"
      when: "before leaving for field site"
      information: [
        "Location (general area, not exact address)",
        "Expected duration",
        "Number of team members going",
        "Check-in time (e.g., 'will check in by 3pm')"
      ]
      
  during_fieldwork:
    buddy_system: "minimum 2 people at all times"
    communication: "charged phone, PI number on speed dial"
    supplies: [
      "Naloxone (2+ doses)",
      "First aid kit",
      "PPE (gloves, sharps container)",
      "Water, snacks (for staff AND participants)",
      "Charged phone",
      "Study materials (device, consent forms, gift cards)"
    ]
    
  check_in_system:
    scheduled_check_in: datetime
    check_in_method: "text or call to coordinator"
    
    if_missed_check_in:
      grace_period: "15 minutes"
      first_response: "coordinator calls researcher"
      second_response: "coordinator calls second team member"
      if_no_response_within_30_min:
        → "coordinator calls PI"
        → "PI decides whether to contact emergency services"
        
    dead_mans_switch:
      # In the app: "Tap to check in"
      # Auto-prompts at scheduled intervals
      # If not tapped within grace period, sends alert to coordinator
      
  after_fieldwork:
    debrief: "brief check-in with team — what happened, how are you"
    incident_log: "any safety concerns documented"
    sharps_disposal: "proper disposal of any collected sharps"
    
  incident_types:
    verbal_aggression: 
      response: "disengage, leave area, document"
    physical_threat:
      response: "leave immediately, call 911 if needed, document"
    needlestick:
      response: "immediate first aid, report to occupational health, 
                 PEP evaluation within 2 hours"
    witnessed_crime:
      response: "do NOT intervene, leave area if unsafe, 
                 do NOT report to police unless immediate danger 
                 (participant safety and trust > crime reporting)"
    vehicle_issues:
      response: "AAA/roadside assistance, coordinator notified"

SOFTWARE_IMPLEMENTATION:
  
  check_in_feature:
    on_fieldwork_start:
      prompt: "Starting fieldwork at [location]. Check-in due by [time]."
      send_to: coordinator_phone
    
    at_check_in_time:
      notification: "Tap to check in"
      if_tapped: send_confirmation_to_coordinator
      if_not_tapped_within_15_min: send_alert_to_coordinator
      
    panic_button:
      always_accessible: true
      on_press: 
        → send_gps_to_coordinator
        → auto_dial_911
        → start_audio_recording (evidence)
      
    incident_log:
      type: select from incident_types
      date: datetime
      location: general_area
      description: free_text
      follow_up_needed: boolean
      reported_to_pi: boolean
```

---

## Secondary Traumatic Stress

### What It Is

Secondary traumatic stress (STS) — also called vicarious trauma or compassion fatigue — occurs when a person is repeatedly exposed to others' traumatic experiences. It manifests as:
- Intrusive thoughts about participants' stories
- Avoidance of trauma-related material
- Emotional numbing
- Hypervigilance
- Sleep disruption
- Changes in worldview (loss of faith in safety, justice, human goodness)

### Why It's Relevant Here

Isabella is already at elevated risk from her harm reduction work. Adding a research layer — where she systematically reviews, codes, and analyzes traumatic narratives — increases exposure. She's not just hearing the stories once. She's transcribing them. Reading them. Coding them. Quoting them in her dissertation. Presenting them at conferences.

### What the IRB Should (But May Not) Require

Some IRBs require researcher safety plans. Most don't. But Isabella should have one:

```
RESEARCHER_WELLBEING_PLAN:

  proactive:
    - "Regular supervision with advisor (not just about research — about how she's doing)"
    - "Peer support group (other doctoral students in similar research)"
    - "Access to university counseling (U-M CAPS: (734) 764-8312)"
    - "Physical activity as stress management (pottery counts)"
    - "Clear boundaries: field hours, analysis hours, off hours"
    - "Vacation. Actual vacation."
    
  monitoring:
    - "ProQOL (Professional Quality of Life Scale) self-assessment quarterly"
    - "STS threshold: if ProQOL compassion fatigue subscale > 42, 
       seek supervision"
    - "Track cumulative exposure: participant deaths, overdoses witnessed, 
       distressing disclosures"
       
  intervention:
    if "feeling unable to face field work":
      → "tell PI — no penalty"
      → "take a break from data collection (not from the PhD — from the field)"
      → "someone else covers field shifts"
    if "intrusive thoughts about participant stories":
      → "clinical counseling, not just peer support"
      → "reduce transcript review temporarily"
      → "consider whether AI-assisted coding can reduce direct exposure"
    if "considering leaving the PhD":
      → "this is common and valid"
      → "talk to advisor, trusted colleague, counselor"
      → "usually this passes — but if it doesn't, leaving is okay"
```

### Software Connection

The AI-assisted qualitative coding in the architecture document isn't just about efficiency. It's about reducing the number of times Isabella has to read, word by word, a participant's account of being assaulted, overdosing, losing their child. If the local LLM can do a first pass on coding — identifying themes and applying initial codes — Isabella reviews and corrects the coding rather than doing it from scratch. She still engages with the data. But she's reviewing codes, not re-reading raw trauma narratives 17 times.

This is an ethically meaningful design decision, not just a productivity feature.

---

## The Ethics of AI in Vulnerable Population Research

### The New Question

Using AI (even local, private AI) in research with vulnerable populations raises questions that have no established answers yet:

**1. Can AI code qualitative data from people who didn't consent to AI processing?**
- Most consent forms written today don't mention AI
- If Isabella uses Ollama/local LLM to assist with coding, this should be in the consent form
- Language: "Your de-identified interview transcript may be processed by artificial intelligence software running on a secure, private computer (not sent to any cloud service) to assist with initial thematic coding. A human researcher will review and verify all AI-generated codes."

**2. Can Whisper transcription alter meaning?**
- Whisper isn't perfect. It can mishear drug names, street slang, dialectical speech patterns
- Transcription errors in sensitive material can change meaning (mishearing "I thought about it" vs. "I fought about it")
- Mitigation: human review of all transcripts against audio. AI-assisted, not AI-generated.

**3. Do AI models trained on internet data carry bias into coding?**
- Yes. LLMs may over-categorize Black speech patterns as "informal" or "uneducated." They may misinterpret harm reduction terminology. They may apply stigmatizing frames.
- Mitigation: custom coding guidelines, explicit anti-stigma instructions in the prompt, human review of all codes, inter-rater reliability assessment between AI codes and human codes.

**4. Is local processing actually private enough?**
- If the LLM is running locally (Ollama on Ryan's RTX 4070 or on U-M's Armis2 cluster), no data leaves the controlled environment
- But: the model was trained on internet data. Can it "memorize" participant quotes? Current LLMs used for inference only don't retain information between sessions. But this should be documented.
- IRB language: "All AI processing occurs locally. No participant data is transmitted to external services. The AI model is used for inference only and does not learn from or retain participant data."

---

## Summary: The Ethical Architecture

Everything above comes down to seven principles:

1. **Protect identity.** No names, no GPS, no exact timestamps, no linkable data.
2. **Expect the worst.** People will overdose, die, disclose violence, attempt suicide. The system must handle all of these gracefully.
3. **Consent is ongoing.** Not a signature on a page. A continuous, informed, voluntary choice.
4. **Know the exceptions.** The Certificate of Confidentiality is powerful but not absolute. Mandatory reporting, duty to warn, and medical emergencies override it.
5. **Compensate fairly.** People's time and knowledge have value. Pay them. Don't police how they spend it.
6. **Protect the researcher.** Isabella is a human absorbing trauma. Build systems that reduce unnecessary exposure, monitor wellbeing, and maintain safety.
7. **Be transparent about AI.** If code touches data, participants should know. If AI assists analysis, document how and why. Stay ahead of the ethics review.

The software embodies these principles or it doesn't deserve to exist.
