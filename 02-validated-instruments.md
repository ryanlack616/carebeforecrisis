# Validated Instruments for Harm Reduction Research

*Prepared February 21, 2026*
*Standard measures Isabella will use, adapt, or encounter — and what each means for the software*

---

## Why This Matters

Isabella won't invent her own survey. She shouldn't. In substance use research, validated instruments are instruments that have been tested for reliability (consistent results across time/raters) and validity (actually measuring what they claim to measure). Her committee and IRB will expect established tools.

But "validated" doesn't mean "perfect for the field." Most of these instruments were developed in clinical settings — hospitals, treatment centers, offices with chairs and clipboards. Harm reduction happens in parking lots, under bridges, in needle exchange vans. The gap between where these instruments were validated and where Isabella will use them is enormous.

That's where the software comes in. Not to change the instruments — that would invalidate them — but to make them administrable in the conditions where they're actually needed.

---

## Tier 1: Substance Use Assessment

These are the core instruments she'll almost certainly use or encounter.

### ASSIST — Alcohol, Smoking, and Substance Involvement Screening Test

**What it is:** WHO-developed screening tool. Assesses risk level for 10 substance categories: tobacco, alcohol, cannabis, cocaine, amphetamines, inhalants, sedatives, hallucinogens, opioids, and "other."

**Structure:**
- 8 questions, administered per substance
- 3 frequency-of-use items (lifetime, past 3 months, strong desire/urge)
- 4 consequence items (health, social, role, failed control)
- 1 injection item (specific to IV use)
- Skip logic: If "no" to lifetime use, skip remaining questions for that substance

**Scoring:**
- Each substance scored 0-31+
- Risk levels: Low (0-3), Moderate (4-26), High (27+)
- Specific thresholds differ for alcohol vs. other substances
- Low risk: no intervention. Moderate: brief intervention. High: specialist referral.

**Licensing:** Free. Public domain. WHO encourages adaptation. Available in 20+ languages.

**Time:** 5-15 minutes depending on number of substances endorsed.

**Software implications:**
```
SCORING_ALGORITHM:
  for each substance in [tobacco, alcohol, cannabis, cocaine, 
                         amphetamine, inhalant, sedative, 
                         hallucinogen, opioid, other]:
    if Q1[substance] == "No":
      score[substance] = 0
      SKIP to next substance
    else:
      score[substance] = sum(Q2..Q7 for substance)
      if substance == "any" and Q8 == "Yes, in past 3 months":
        flag_injection = true
        score[substance] += Q8_value

  risk_level[substance] = 
    "low" if score < 4 (or < 11 for alcohol)
    "moderate" if score 4-26 (or 11-26 for alcohol)
    "high" if score >= 27

  SPECIAL: injection use (Q8) triggers immediate high-risk 
           classification regardless of total score
```

**Field challenges:**
- 10 substance categories × 8 questions = potentially 80 items. That's too long for a parking lot encounter.
- Participants may not know street names for all substance categories.
- "Past 3 months" recall is unreliable for people with chaotic living situations.
- Need: substance-specific skip logic that's fast to navigate on a phone.

---

### DAST-10 — Drug Abuse Screening Test (10-item version)

**What it is:** Quick screening for drug-related problems. Excludes alcohol (that's what AUDIT is for).

**Structure:**
- 10 yes/no questions
- Items cover: loss of control, guilt, withdrawal, medical problems, social problems, family problems
- Simple binary scoring

**Scoring:**
```
SCORING_ALGORITHM:
  # All items scored 1 for "Yes" EXCEPT:
  # Q3 ("Are you always able to stop using drugs when you want to?")
  # Q3 is REVERSE SCORED: "No" = 1, "Yes" = 0
  
  total = sum(Q1, Q2, reverse(Q3), Q4..Q10)
  
  severity = 
    "no problems" if total == 0
    "low level" if total 1-2
    "moderate level" if total 3-5
    "substantial level" if total 6-8
    "severe level" if total 9-10
```

**Licensing:** Public domain. Free to use.

**Time:** 1-2 minutes.

**Field challenges:**
- The reverse-scored item (Q3) is a classic data quality trap. If the software auto-scores, it must handle the reversal. If a paper version is transcribed, data entry staff will miss this.
- "Drugs" is vague — some participants won't count marijuana or prescribed medications. The instrument doesn't clarify.
- Very quick, which is good for field use. Often paired with AUDIT for a complete substance use screen.

**Software implications:**
- Auto-reverse-score Q3
- Present yes/no as large touch targets (field use on phone)
- Calculate and display severity immediately after completion
- Flag: if DAST-10 ≥ 6, suggest administering full ASI

---

### AUDIT — Alcohol Use Disorders Identification Test

**What it is:** WHO-developed. The gold standard for alcohol screening. Used worldwide.

**Structure:**
- 10 questions in 3 domains:
  - Hazardous consumption (Q1-3): frequency, quantity, binge
  - Dependence symptoms (Q4-6): impaired control, morning drinking, guilt
  - Harmful consequences (Q7-10): blackouts, injury, others concerned

**Scoring:**
```
SCORING_ALGORITHM:
  # Q1-8 scored 0-4
  # Q9-10 scored 0, 2, or 4 (no middle options)
  
  total = sum(Q1..Q10)  # range: 0-40
  
  risk_zone =
    "Zone I: Low risk" if total 0-7
    "Zone II: Hazardous" if total 8-15
    "Zone III: Harmful" if total 16-19
    "Zone IV: Possible dependence" if total 20-40
  
  # AUDIT-C (short version): Q1-3 only
  audit_c_score = sum(Q1..Q3)  # range: 0-12
  audit_c_positive = 
    score >= 4 (men) or score >= 3 (women)
```

**Licensing:** Public domain (WHO). Free.

**Time:** 2-3 minutes.

**Software implications:**
- Q9-10 have non-standard response options (0/2/4 instead of 0-4). UI must handle this.
- AUDIT-C (3-item version) is often sufficient for screening. Option to administer short or full version.
- Gender-specific cutoffs for AUDIT-C — software needs to apply correct threshold based on participant demographics.
- Frequency questions use different timeframes than ASSIST — "how often" vs "in the past 3 months." Translating between instruments requires clear labeling.

---

### TLFB — Timeline Followback

**What it is:** Calendar-based recall method. The participant and researcher work through a calendar together, day by day, recording substance use.

**Structure:**
- Not a questionnaire. A procedure.
- Uses a physical or digital calendar
- Researcher helps participant anchor recall with key dates (holidays, paydays, birthdays, events)
- For each day: which substances, how much, route of administration
- Typical recall period: 30, 60, or 90 days

**Scoring:**
```
DATA_STRUCTURE:
  for each day in recall_period:
    date: YYYY-MM-DD
    substances: [
      { type: "heroin", amount: "2 bags", route: "IV" },
      { type: "alcohol", amount: "6 beers", route: "oral" },
      { type: "fentanyl", amount: "unknown", route: "smoked" }
    ]
    abstinent: boolean
    
  DERIVED MEASURES:
    total_use_days = count(days where abstinent == false)
    percent_days_abstinent = (abstinent_days / total_days) * 100
    heavy_use_days = count(days with amount > threshold)
    longest_abstinent_streak = max consecutive abstinent days
    substance_specific_frequency = count per substance type
```

**Licensing:** Requires training in administration. The method itself is published, but quality depends on trained administration.

**Time:** 15-45 minutes depending on recall period and use complexity.

**Field challenges:** This is the hardest instrument to administer in the field.
- Requires focused, uninterrupted time with the participant
- Calendar recall is cognitively demanding for people in crisis
- "How much" is imprecise — street drug quantities aren't standardized
- Polydrug use days are complex to record
- Participants may not reliably recall a specific Tuesday 6 weeks ago

**Software implications — this is where Ryan can add enormous value:**
- Interactive calendar widget optimized for touch
- Auto-populate anchor dates (holidays, first of month/payday)
- Allow participant to mark "I have no idea" for specific days (missing data, not zero)
- Drag-to-fill for patterns ("I used every day that week")
- Substance shortcuts — most participants have 2-3 primary substances, don't make them select from scratch each day
- Visual pattern display — show them their own calendar with color coding as they complete it
- Auto-calculate all derived measures
- This instrument alone could justify a custom tool. REDCap cannot do this well.

---

### ASI — Addiction Severity Index

**What it is:** The comprehensive assessment. Gold standard for multi-domain evaluation of substance use severity.

**Structure:**
- 7 domains: Medical, Employment/Support, Drug Use, Alcohol Use, Legal, Family/Social, Psychiatric
- ~200 items total (5th edition)
- Mix of lifetime and past-30-day items
- Interviewer-administered (not self-report — requires trained interviewer)
- Includes severity ratings by the interviewer (0-9 scale, subjective)

**Scoring:**
```
SCORING_ALGORITHM:
  # Composite scores for each domain (0.00 to 1.00)
  # Calculated from specific item subsets — NOT simple averages
  # Each domain uses a different calculation formula
  # Formulas are published but complex
  
  # Example: Drug Use composite
  drug_composite = weighted_function(
    past30_days_drug_use,
    past30_days_drug_problems,
    money_spent_drugs,
    severity_rating
  )
  
  # CRITICAL: Composite score formulas are copyrighted
  # Must use published formulas exactly
  # McLellan et al., 1992 provides the original
  # ASI-6 (6th edition) has updated scoring
```

**Licensing:** The ASI-5 is in the public domain. The ASI-6 may have licensing restrictions depending on use context. Check with the publisher (Treatment Research Institute).

**Time:** 45-75 minutes with a trained interviewer.

**Field challenges:**
- Way too long for field use. Period. This is an intake tool, not a street tool.
- Requires a trained interviewer — can't be self-administered
- Some items are invasive (income, legal history, psychiatric symptoms)
- The interviewer severity ratings are subjective and require calibration across raters

**Software implications:**
- If she uses ASI, it's in a controlled setting (office, appointment)
- A tablet-based guided interview could help interviewers stay consistent
- Scoring is complex enough that manual calculation introduces errors. Auto-scoring is essential.
- The interviewer severity ratings need inter-rater reliability tracking — two interviewers should rate the same participant similarly. Software can flag discrepancies.
- She probably won't use the full ASI. More likely she'll use specific domains (Drug Use + Psychiatric) combined with shorter screeners for other areas.

---

## Tier 2: Mental Health & Wellbeing

These are almost always administered alongside substance use measures because comorbidity is the norm, not the exception.

### PHQ-9 — Patient Health Questionnaire (Depression)

**Structure:** 9 items, each rated 0-3 (not at all / several days / more than half the days / nearly every day) over the past 2 weeks.

**Scoring:**
```
total = sum(Q1..Q9)  # range: 0-27

severity =
  "minimal" if total 0-4
  "mild" if total 5-9
  "moderate" if total 10-14
  "moderately severe" if total 15-19
  "severe" if total 20-27

# CRITICAL SAFETY FLAG:
if Q9 > 0:  # Q9 asks about thoughts of self-harm or suicide
  TRIGGER suicide_risk_protocol
  # This is not optional. IRB will require a safety protocol.
  # Software must flag this immediately and guide the researcher
  # through the appropriate response.
```

**Licensing:** Free. Public domain. Developed by Pfizer but released for unrestricted use.

**Software critical requirement:** Q9 (suicidal ideation) MUST trigger an immediate safety protocol if the response is anything other than 0. This means:
- Visual alert to the researcher
- Display of safety protocol steps
- Option to pause the survey and administer safety screening
- Logging that the protocol was activated and what action was taken
- This is non-negotiable from an IRB and ethical standpoint

---

### GAD-7 — Generalized Anxiety Disorder Scale

**Structure:** 7 items, same 0-3 scale as PHQ-9, past 2 weeks.

**Scoring:**
```
total = sum(Q1..Q7)  # range: 0-21

severity =
  "minimal" if total 0-4
  "mild" if total 5-9
  "moderate" if total 10-14
  "severe" if total 15-21
```

**Licensing:** Free. Public domain (same developer as PHQ-9).

**Software implications:** 
- Often administered as a pair with PHQ-9 (PHQ-9 + GAD-7 = 16 items, ~5 minutes)
- No safety flag items like PHQ-9 Q9, but severity above moderate should be noted in participant record
- Same response scale as PHQ-9 — UI can be identical

---

### PCL-5 — PTSD Checklist for DSM-5

**Structure:** 20 items, rated 0-4 (not at all / a little bit / moderately / quite a bit / extremely), past month.

**Scoring:**
```
total = sum(Q1..Q20)  # range: 0-80

provisional_diagnosis = total >= 33  # cutoff for probable PTSD

# Cluster scoring (maps to DSM-5 PTSD criteria):
cluster_B = sum(Q1..Q5)    # Intrusion (re-experiencing)
cluster_C = sum(Q6..Q7)    # Avoidance
cluster_D = sum(Q8..Q14)   # Negative cognitions/mood
cluster_E = sum(Q15..Q20)  # Arousal/reactivity

# DSM-5 diagnostic rule:
# At least 1 B item ≥ 2
# At least 1 C item ≥ 2
# At least 2 D items ≥ 2
# At least 2 E items ≥ 2
# AND total ≥ 33
```

**Licensing:** Free. Developed by the National Center for PTSD (US Department of Veterans Affairs). Public domain.

**Why PCL-5 matters here:** PTSD prevalence in substance-using populations is 30-60%. In people who use opioids specifically, it's higher. Many participants in harm reduction research have experienced violence, sexual assault, overdose (witnessing or personal), childhood trauma. The PCL-5 surfaces this.

**Software implications:**
- Cluster-level scoring enables more nuanced analysis than just the total
- Items 1-5 ask about a specific traumatic event — the researcher needs to establish an index event first
- No specific safety-trigger items, but high total scores should trigger a referral note
- 20 items at 5 response options = more cognitive load than PHQ-9/GAD-7. Consider larger fonts, simpler layout.

---

### K6 / K10 — Kessler Psychological Distress Scale

**Structure:** 6 items (K6) or 10 items (K10). Each rated 1-5 (none/a little/some/most/all of the time), past 30 days.

**Scoring:**
```
K6:
  total = sum(Q1..Q6)  # range: 6-30
  serious_mental_illness = total >= 13

K10:
  total = sum(Q1..Q10)  # range: 10-50
  risk_level =
    "likely well" if total 10-19
    "likely mild disorder" if total 20-24
    "likely moderate disorder" if total 25-29
    "likely severe disorder" if total 30-50
```

**Licensing:** Public domain. 

**Why use K6 instead of PHQ-9 + GAD-7?** It's shorter (6 items vs. 16) and measures general distress rather than specific diagnoses. Better for field screening where you want a quick mental health check without asking 20 diagnostic questions in a parking lot.

---

## Tier 3: Harm Reduction Specific

These are less standardized than the Tier 1/2 instruments. Some are well-validated, others are adapted locally.

### HRBS — HIV Risk-taking Behavior Scale

**What it is:** Assesses injection risk (sharing needles, equipment) and sexual risk (unprotected sex, multiple partners).

**Structure:** 11 items in 2 domains:
- Injection domain (6 items): sharing needles, cleaning equipment, using new needles
- Sexual domain (5 items): number of partners, condom use, sex work

**Why it matters:** Standard outcome measure for syringe service program (SSP) evaluation. If Isabella studies SSP effectiveness, she'll use this or something similar.

**Software implications:**
- Sensitive items require careful UI. "Have you exchanged sex for drugs or money?" needs to be presented without judgment.
- Response options are frequency-based ("every time" to "never") — visual slider might work better than radio buttons.
- Injection domain only applies to people who inject — branching logic needed.

### Overdose Experience Measures

No single standard instrument. Commonly measured:
```
OVERDOSE_DATA_POINTS:
  personal_overdose:
    lifetime_count: integer
    past_year_count: integer
    most_recent_date: date
    substances_involved: [list]
    naloxone_administered: boolean
    emergency_services_called: boolean
    outcome: "recovered at scene" | "hospitalized" | "other"
  
  witnessed_overdose:
    lifetime_count: integer
    past_year_count: integer
    naloxone_administered: boolean
    training_received: boolean
    carries_naloxone: boolean
    naloxone_source: "SSP" | "pharmacy" | "friend" | "other"
```

**Software implications:**
- Overdose is a sensitive topic. UI should allow "prefer not to answer" for every item.
- Witnessed overdose items are less sensitive than personal overdose — separate them.
- "Substances involved" is critical data but hard to capture accurately (participants may not know exact substances, especially with fentanyl contamination).

---

## Tier 4: Social Determinants

### PRAPARE — Protocol for Responding to and Assessing Patients' Assets, Risks, and Experiences

**Structure:** 16 core items + 5 optional items covering:
- Personal characteristics (race, ethnicity, education, employment)
- Family/home (housing status, food insecurity, transportation)
- Money/resources (insurance, income, material hardship)
- Social/emotional health (social support, stress, safety)
- Optional: incarceration, refugee status, safety

**Licensing:** Free. Developed by the National Association of Community Health Centers.

**Why it matters:** Social determinants are often stronger predictors of health outcomes than substance use severity. Housing instability, food insecurity, and lack of transportation directly impact whether someone can access harm reduction services, maintain treatment, or stabilize their life.

**Software implications:**
- Housing status is fluid — "couch surfing" today, "shelter" tomorrow, "street" next week. Need to capture as point-in-time and track longitudinally.
- Some items are sensitive for demographic reasons, not clinical ones. "Have you been in prison?" — trust must be established first.
- Standard in community health centers, so data comparability is a strength.

---

## Cross-Cutting Software Architecture for All Instruments

### 1. Instrument Engine Design

```
INSTRUMENT_ENGINE:
  
  instrument_definition:
    id: UUID
    name: string
    version: string  # e.g., "DAST-10 v2.1"
    irb_approved_version: string  # link to IRB-approved version
    language: string  # "en", "es", etc.
    items: [
      {
        id: item_UUID
        text: string
        response_type: "likert" | "binary" | "numeric" | "text" | "date" | "multi-select"
        response_options: [{ value: int, label: string }]
        reverse_scored: boolean
        skip_logic: {
          condition: "Q1 == 0"
          action: "skip_to_Q9" | "end_instrument"
        }
        safety_flag: {
          trigger: "value > 0"
          protocol: "suicide_risk" | "mandatory_report" | "referral"
        }
        required: boolean
        allow_decline: boolean  # "prefer not to answer"
      }
    ]
    
  scoring_rules:
    subscales: [
      {
        name: "total" | "cluster_B" | "drug_composite"
        items: [item_ids]
        calculation: "sum" | "weighted" | "custom_function"
        reverse_items: [item_ids]
        cutoffs: [
          { label: "minimal", min: 0, max: 4 },
          { label: "mild", min: 5, max: 9 }
        ]
      }
    ]
    
  administration:
    mode: "self-report" | "interviewer-administered" | "either"
    estimated_time_minutes: integer
    requires_training: boolean
    minimum_reading_level: "4th grade" | "6th grade" | "8th grade"
```

### 2. Skip Logic Engine

Skip logic is the most important software feature for field use. Every second saved is a second the participant isn't getting restless, isn't looking over their shoulder, isn't deciding to walk away.

```
SKIP_LOGIC_TYPES:
  
  simple_skip:
    # If this answer, skip to that item
    "If Q1 == No → skip to next instrument"
  
  conditional_branch:
    # Different paths based on response
    "If DAST-10 score >= 6 → administer ASI Drug Use domain"
    "If DAST-10 score < 6 → skip ASI, proceed to PHQ-9"
  
  adaptive_battery:
    # The instrument set adapts to participant profile
    "If participant injects → add HRBS injection domain"
    "If participant is homeless → add full PRAPARE"
    "If participant has witnessed OD → add OD experience module"
  
  safety_interrupt:
    # Override all skip logic for immediate action
    "If PHQ-9 Q9 > 0 → PAUSE all instruments, activate safety protocol"
    "If any QOL item indicates intimate partner violence → activate safety protocol"
```

### 3. Multilingual Support

Not a nice-to-have. Harm reduction programs in Michigan serve Latinx, Arabic-speaking, and other non-English-speaking communities.

```
LANGUAGE_REQUIREMENTS:
  
  minimum_languages: ["en", "es"]
  ideal_languages: ["en", "es", "ar"]  # Arabic for Dearborn/SE Michigan
  
  implementation:
    - Instrument text stored in language-keyed JSON
    - Validated translations only (not Google Translate)
    - ASSIST available in 20+ validated translations (via WHO)
    - PHQ-9 available in 60+ validated translations
    - PRAPARE available in English and Spanish
    - Custom instruments: professional translation + back-translation required
    
  UI_CONSIDERATIONS:
    - RTL (right-to-left) support for Arabic
    - Font size adjustable (literacy varies)
    - Audio playback of items (for low-literacy participants)
    - Visual response scales (smiley faces for Likert scales)
```

### 4. Instrument Version Control

IRB approves a specific version of each instrument. If Isabella modifies an item (even punctuation), it's a new version requiring IRB amendment.

```
VERSION_CONTROL:
  
  instrument_version:
    version_id: UUID
    instrument_id: UUID
    version_number: "1.0" | "1.1" | "2.0"
    changes_from_previous: string
    irb_amendment_number: string | null
    irb_approval_date: date | null
    status: "draft" | "irb-submitted" | "approved" | "retired"
    
  data_linkage:
    # Every response is linked to the exact version administered
    response.instrument_version_id → instrument_version.version_id
    
  RULE: Once an instrument version is "approved" and has been 
        administered to any participant, it CANNOT be modified.
        Any change creates a new version.
```

### 5. Scoring Dashboard

After a field session, Isabella needs to see results immediately — not for analysis, but for safety and referral.

```
IMMEDIATE_DISPLAY:
  
  participant_screen_summary:
    DAST-10: 7 (substantial) ⚠️
    AUDIT: 12 (hazardous)
    PHQ-9: 18 (moderately severe) 🚨
      → Q9 flagged: safety protocol activated, documented
    GAD-7: 11 (moderate)
    K6: 15 (probable SMI)
    
  ALERTS:
    🚨 PHQ-9 Q9 = 2 — safety protocol required
    ⚠️ DAST-10 ≥ 6 — consider full ASI at next visit
    ℹ️ AUDIT Zone II — brief intervention recommended
    
  ACTIONS_TAKEN:
    [ ] Safety screening completed
    [ ] Referral provided
    [ ] Participant declined referral
    [ ] Documented in adverse event log
```

---

## What She Should Know About Instrument Selection

### The Battery Problem

Every instrument she adds is time she's asking from a participant's life. A "comprehensive" battery might include:

| Instrument | Time |
|-----------|------|
| ASSIST | 10 min |
| AUDIT | 3 min |
| PHQ-9 | 3 min |
| GAD-7 | 2 min |
| PCL-5 | 8 min |
| PRAPARE | 5 min |
| HRBS | 5 min |
| TLFB (30-day) | 20 min |
| **Total** | **~56 min** |

Nobody standing in a parking lot during needle exchange is going to give you 56 minutes. They'll give you 10. Maybe 15 if they trust you.

**The art of the battery is cutting.** She needs to figure out:
- What's absolutely required for her research questions?
- What can be shortened (AUDIT-C instead of full AUDIT)?
- What can be administered at different timepoints (TLFB at office visit, screeners in the field)?
- What can she drop entirely without weakening the study?

Her committee will push for more measures. The IRB will ask why she's burdening participants with 56 minutes of surveys. She'll be caught between them. The software can help by making whatever she keeps as fast and painless as possible to administer.

### The Literacy Problem

Average reading level for people accessing harm reduction services is estimated at 5th-8th grade. Many instruments are written at a 10th-12th grade level.

This doesn't mean her participants are uneducated. It means they may have disrupted schooling, learning disabilities, brain injury from overdose/hypoxia, or simply not be native English speakers.

**Software solution:** Audio playback of every item. Large fonts. Simple layouts. No scrolling through dense text. One item per screen. Visual response options alongside text.

### The "Prefer Not to Answer" Problem

Standard instruments don't include this option. But harm reduction ethics demand it — you can't be truly voluntary if there's no option to decline specific items without abandoning the whole survey.

**Software solution:** Every item includes a "prefer not to answer" option. Missing data is coded distinctly from "zero" or "no." The scoring algorithm handles partial completion (many do; some require minimum item counts for valid scoring).

---

## Practical Instrument Administration Flows

### Flow 1: Quick Field Screen (10 minutes)
```
1. DAST-10 (2 min)
2. AUDIT-C (1 min)
3. K6 (1 min)
4. PHQ-9 Q9 only — single-item suicide screen (30 sec)
5. Overdose experience — 3 key items (1 min)
6. PRAPARE housing items only (1 min)
7. Service utilization — 3 items (1 min)

→ Auto-score all
→ Flag any safety concerns
→ Generate referral list based on scores
→ Total: ~8-10 minutes
```

### Flow 2: Enrolled Participant Baseline (30 minutes)
```
1. ASSIST (10 min)
2. PHQ-9 + GAD-7 (5 min)
3. PCL-5 (8 min) 
4. PRAPARE full (5 min)
5. HRBS if injection involved (3 min)

→ Auto-score all
→ Generate baseline profile
→ Schedule follow-up measures
→ Total: ~30 minutes
```

### Flow 3: Follow-Up Visit (20 minutes)
```
1. TLFB since last visit (10 min)
2. PHQ-9 + GAD-7 (5 min)
3. Service utilization since last visit (3 min)
4. Any changed risk items (2 min)

→ Auto-score all
→ Compare to baseline
→ Calculate change scores
→ Flag clinically significant changes (e.g., PHQ-9 increase ≥ 5 points)
→ Total: ~20 minutes
```

---

## What Ryan Builds

The instrument engine is the core of the data collection tool. It needs to:

1. **Define instruments once, administer everywhere.** The JSON instrument definition drives the UI, the scoring, and the data storage. Change the definition, everything updates.

2. **Score in real-time.** No batch processing. Scores display immediately after last item. Safety flags fire immediately on the relevant item, not after submission.

3. **Work offline.** IndexedDB stores completed instruments. Sync when connected. No data loss.

4. **Maintain audit trail.** Every answer timestamped. Every score calculated with the exact algorithm version. If she re-scores data in 2 years, the result must be identical.

5. **Export clean.** CSV with raw responses, calculated scores, instrument version, administration date, participant ID (de-identified), and completion status. Ready for SPSS, R, or Stata.

This is buildable. It's a well-defined problem with clear specifications. The TLFB calendar is the hardest part. Everything else is structured data entry with scoring algorithms — exactly the kind of work that produces clean, reliable output when done right.
