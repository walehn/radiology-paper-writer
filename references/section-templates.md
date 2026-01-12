# Section-Specific Review Templates

섹션별 리뷰 기준 및 작성 가이드

---

## Abstract

### Structure Requirements

**Standard Structure (IMRAD):**
1. **Purpose/Objective** (1-2 sentences)
   - State the study aim clearly
   - Include index test and target condition
2. **Materials and Methods** (2-3 sentences)
   - Study design (retrospective/prospective)
   - Participants (n, inclusion criteria brief)
   - Imaging protocol (modality, key parameters)
   - Reference standard
3. **Results** (2-3 sentences)
   - Primary outcomes with numbers
   - 95% CI when possible
   - p-values if applicable
4. **Conclusion** (1-2 sentences)
   - Directly address the purpose
   - Clinical implication

### Technical Review Criteria (Codex)

| Category | Check | Severity |
|----------|-------|----------|
| **Structure** | Does it follow Purpose-Methods-Results-Conclusion? | Major |
| **Number Consistency** | Do abstract numbers match Results section? | Critical |
| **Overclaim Detection** | Flag "first", "novel", "superior", "best" without qualification | Major |
| **Endpoint Clarity** | Is primary endpoint explicitly stated? | Major |
| **Statistical Format** | OR/HR with 95% CI, p-value format | Minor |
| **Sample Size** | Is n clearly stated? | Minor |

### Style Review Criteria (Gemini)

| Category | Check | Severity |
|----------|-------|----------|
| **Sentence Length** | Flag sentences > 25 words | Minor |
| **Information Density** | One key message per sentence | Minor |
| **Jargon** | Replace unnecessary technical terms | Minor |
| **Active Voice** | Prefer active over passive | Minor |
| **Word Count** | Within journal limit | Major |

### Writing Tips
- Start Purpose with action verb: "To evaluate...", "To develop...", "To compare..."
- Results: Lead with primary outcome, then secondary
- Avoid interpretation in Abstract (save for Discussion)
- No references in Abstract

---

## Introduction

### Structure Requirements

**3-Paragraph Structure (Funnel Model):**

```
┌─────────────────────────────────────────────────────┐
│ Paragraph 1: CLINICAL CONTEXT (Broad)               │
│ • Why this clinical problem matters                 │
│ • Prevalence/incidence, patient impact              │
│ • Current diagnostic/treatment landscape            │
│ • Citations: 3-5 key epidemiological/clinical refs  │
└─────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────┐
│ Paragraph 2: GAP/LIMITATION (Narrowing)             │
│ • What has been done (existing research)            │
│ • What's missing or problematic                     │
│ • Why current approaches are insufficient           │
│ • Citations: 4-6 methodological/research refs       │
└─────────────────────────────────────────────────────┘
                         ↓
┌─────────────────────────────────────────────────────┐
│ Paragraph 3: OBJECTIVE (Specific)                   │
│ • "The purpose of this study was to..."             │
│ • Specific, measurable study aim                    │
│ • Brief mention of approach/novelty                 │
│ • Optional: hypothesis statement                    │
└─────────────────────────────────────────────────────┘
```

### Literature Research Workflow (/radiology-intro)

**Step 1: Input Collection**
- Research topic (e.g., "Deep learning for liver metastasis detection on CT")
- Keywords (e.g., ["liver metastasis", "CT", "deep learning", "diagnostic accuracy"])
- Study type (diagnostic_accuracy, prognostic, segmentation_ai)
- Target journal

**Step 2: Gemini 3 Pro Literature Analysis**
```bash
gemini -m gemini-3-pro -y "[Research Prompt]"
```
Output includes:
- Clinical context summary
- Key literature with citation hints
- State-of-the-art methods
- Knowledge gaps identified
- Research opportunity analysis
- Suggested paragraph drafts

**Step 3: Draft Generation (Claude)**
- Synthesize Gemini research output
- Generate 3-paragraph Introduction
- Integrate suggested citations

**Step 4: Dual Review (Optional)**
- GPT-5.2: Clinical relevance, gap clarity, objective alignment
- Gemini: Flow, readability, citation integration

### Paragraph-Specific Guidelines

#### Paragraph 1: Clinical Context

**Content Checklist:**
- [ ] Clinical problem importance established
- [ ] Prevalence/incidence data (if applicable)
- [ ] Current standard of care mentioned
- [ ] Patient impact or healthcare burden
- [ ] Broad clinical relevance to radiology

**Sentence Templates:**
```
- "[Condition] is a [major/common/significant] clinical problem, affecting [X%] of [population] (1)."
- "Current [diagnostic/prognostic] approaches rely on [method], which [limitation] (2,3)."
- "Early [detection/diagnosis] of [condition] is crucial for [clinical outcome] (4)."
```

**Common Pitfalls:**
- Starting too specific (save specifics for P2)
- Missing epidemiological data
- Overgeneralizing without citations

#### Paragraph 2: Gap/Limitation

**Content Checklist:**
- [ ] Existing research summarized fairly
- [ ] Specific limitations identified
- [ ] Gap is significant (not trivial)
- [ ] Gap directly leads to study rationale
- [ ] Balanced view (not dismissive of prior work)

**Sentence Templates:**
```
- "Previous studies have demonstrated [finding] (5-8); however, [limitation remains]."
- "While [method] has shown promising results (9), [specific gap] has not been adequately addressed."
- "The [generalizability/reproducibility/clinical applicability] of these findings remains [uncertain/limited] because [reason] (10)."
```

**Gap Types to Consider:**
| Gap Type | Example |
|----------|---------|
| **Sample Size** | "Prior studies were limited by small sample sizes (n < 100)" |
| **Generalizability** | "External validation in diverse populations is lacking" |
| **Methodology** | "Previous work used [outdated method]; [new method] may offer advantages" |
| **Clinical Utility** | "The clinical impact of [finding] on patient outcomes is unknown" |
| **Technology** | "Earlier studies predated [new technology/technique]" |

#### Paragraph 3: Objective

**Content Checklist:**
- [ ] Directly addresses the stated gap
- [ ] Specific and measurable
- [ ] Appropriately scoped
- [ ] Includes primary aim
- [ ] Optional: secondary aims, hypothesis

**Sentence Templates:**
```
- "The purpose of this study was to [verb: evaluate/develop/compare/validate] [specific target] using [method] in [population]."
- "We hypothesized that [intervention/method] would [expected outcome]."
- "Secondary objectives included [list if applicable]."
```

**Objective Verbs by Study Type:**
| Study Type | Appropriate Verbs |
|------------|-------------------|
| Diagnostic Accuracy | evaluate, assess, determine, compare |
| AI Development | develop, train, validate, externally validate |
| Prognostic | develop, validate, evaluate predictive value of |
| Comparative | compare, assess superiority/noninferiority of |

### Technical Review Criteria (GPT-5.2)

| Category | Check | Severity |
|----------|-------|----------|
| **Clinical Relevance** | Is the clinical problem clearly established with evidence? | Major |
| **Literature Balance** | Are both supportive and contradictory studies acknowledged? | Major |
| **Gap Clarity** | Is the knowledge gap specific and significant? | Critical |
| **Gap-Objective Alignment** | Does the objective directly address the stated gap? | Critical |
| **Objective Specificity** | Is the study aim measurable and testable? | Major |
| **Scope Appropriateness** | Are claims proportionate to study design? | Major |
| **Overclaim Detection** | Avoid "first", "novel", "unique" without evidence | Major |

### Style Review Criteria (Gemini)

| Category | Check | Severity |
|----------|-------|----------|
| **Funnel Structure** | General → Specific progression | Major |
| **Paragraph Unity** | One main idea per paragraph | Major |
| **Transitions** | Clear connections between paragraphs | Major |
| **Citation Integration** | Citations flow naturally, not disruptive | Minor |
| **Sentence Length** | Average < 25 words, no run-ons | Minor |
| **Length** | Typically 400-600 words (3 paragraphs) | Minor |
| **Objective Placement** | Last paragraph, clearly stated | Major |

### Writing Tips

1. **Start broad, end specific** - Funnel from clinical context to specific study aim
2. **Every claim needs support** - Cite sources for factual claims
3. **Gap must be significant** - Trivial gaps don't justify studies
4. **Objective must address gap** - Direct logical connection
5. **Avoid overclaiming** - "First study to..." requires evidence
6. **Be fair to prior work** - Acknowledge strengths, not just limitations
7. **Match journal scope** - Frame appropriately for target audience

### Example Introduction Structure

```
[PARAGRAPH 1 - Clinical Context]
Hepatocellular carcinoma (HCC) is the sixth most common malignancy
worldwide, with increasing incidence in Western countries (1,2). Early
detection is crucial for curative treatment eligibility, as prognosis
is strongly associated with tumor stage at diagnosis (3). Current
surveillance relies on ultrasound every 6 months in at-risk patients;
however, sensitivity ranges from 47% to 84% depending on patient
factors (4,5).

[PARAGRAPH 2 - Gap/Limitation]
Recent advances in deep learning have shown promise for liver lesion
detection on cross-sectional imaging (6-9). However, most studies have
focused on CT or MRI in patients already referred for diagnostic
imaging rather than screening populations. Furthermore, the
generalizability of these algorithms to diverse clinical settings and
scanner vendors remains uncertain (10,11). External validation in
real-world surveillance cohorts is lacking.

[PARAGRAPH 3 - Objective]
The purpose of this study was to develop and externally validate a
deep learning algorithm for HCC detection on surveillance MRI in
high-risk patients. We hypothesized that the algorithm would achieve
diagnostic performance comparable to expert radiologists while
reducing interpretation time.
```

---

## Methods

### Structure Requirements for Diagnostic Accuracy Studies

**Recommended Subsections:**
1. **Study Design and Setting**
   - Retrospective/prospective
   - Single/multicenter
   - IRB approval statement
   - Informed consent statement
2. **Participants**
   - Inclusion criteria
   - Exclusion criteria
   - Recruitment period
   - Sample size justification
3. **Imaging Protocol**
   - Modality and scanner
   - Acquisition parameters
   - Contrast protocol if applicable
4. **Reference Standard**
   - Definition
   - How obtained
   - Why appropriate
5. **Image Analysis**
   - Reader qualifications
   - Blinding
   - Reading conditions
   - Measurement methods
6. **Statistical Analysis**
   - Primary analysis method
   - Secondary analyses
   - Software used
   - Significance level

### Technical Review Criteria (Codex)

| Category | Check | Severity |
|----------|-------|----------|
| **Patient Selection Bias** | Consecutive/random sampling stated? | Critical |
| **Spectrum Bias** | Disease severity distribution described? | Major |
| **Verification Bias** | All patients receive reference standard? | Critical |
| **Imaging Protocol** | Reproducible parameters? | Major |
| **Reference Standard** | Appropriate gold standard? | Critical |
| **Blinding** | Index test blinding to reference standard? | Major |
| **Sample Size** | Justification provided? | Major |
| **Statistics - Primary** | Primary endpoint analysis clear? | Critical |
| **Statistics - Multiple Comparisons** | Correction for multiple testing? | Major |
| **Inter-reader Agreement** | ICC/Kappa reported? | Major |
| **ROC Analysis** | AUC with CI? Threshold selection method? | Major |

### Style Review Criteria (Gemini)

| Category | Check | Severity |
|----------|-------|----------|
| **Organization** | Clear subsection structure | Major |
| **Verb Tense** | Past tense throughout | Minor |
| **Passive Voice** | Acceptable in Methods | - |
| **Repetition** | No redundant information | Minor |
| **Readability** | Flesch-Kincaid grade < 14 | Minor |
| **Paragraph Length** | Max 10 sentences per paragraph | Minor |

---

## Results

### Structure Requirements

**Standard Organization:**
1. **Study Population**
   - Flow diagram reference
   - Baseline characteristics
   - Final sample size
2. **Primary Outcome**
   - Main results with CI
   - Table/figure reference
3. **Secondary Outcomes**
   - Additional analyses
   - Subgroup results
4. **Ancillary Findings**
   - Other relevant observations

### Technical Review Criteria (Codex)

| Category | Check | Severity |
|----------|-------|----------|
| **Table/Figure Consistency** | Numbers match text exactly? | Critical |
| **Primary Outcome First** | Is primary outcome prominently placed? | Major |
| **Statistical Formatting** | OR (95% CI: X.XX-X.XX), p = 0.XXX | Minor |
| **CI Reporting** | 95% CI for all key estimates? | Major |
| **Subgroup Analysis** | Pre-specified vs exploratory labeled? | Major |
| **Missing Data** | Handling explained? | Minor |
| **Negative Results** | Reported without spin? | Minor |

### Style Review Criteria (Gemini)

| Category | Check | Severity |
|----------|-------|----------|
| **One Message Per Paragraph** | Each paragraph has single focus | Major |
| **Key Message First** | Lead with result, then detail | Major |
| **Figure/Table Reference Order** | Sequential references | Minor |
| **No Interpretation** | Save interpretation for Discussion | Major |
| **Verb Tense** | Past tense | Minor |
| **Number Presentation** | Consistent decimal places | Minor |

### Statistical Formatting Guide

```
Sensitivity: 92.5% (95% CI: 86.7-96.4%)
Specificity: 92.2% (95% CI: 88.8-94.8%)
AUC: 0.936 (95% CI: 0.912-0.960)
OR: 3.45 (95% CI: 2.12-5.61, p < .001)
ICC: 0.89 (95% CI: 0.82-0.94)
```

---

## Discussion

### Structure Requirements

**7-Paragraph Structure:**
1. **Summary of Main Findings** (1 paragraph)
   - Restate key results without numbers
   - Answer the study question
2. **Interpretation** (1-2 paragraphs)
   - What do these results mean?
   - Why did we observe these findings?
3. **Comparison with Literature** (2 paragraphs)
   - How do results compare to prior studies?
   - Explain differences/similarities
4. **Clinical Implications** (1 paragraph)
   - How should practice change?
   - Who benefits most?
5. **Limitations** (1 paragraph)
   - Study weaknesses honestly
   - Potential biases
   - Generalizability concerns
6. **Future Directions** (optional, 1 paragraph)
   - Next steps in research
7. **Conclusion** (1 paragraph)
   - Take-home message
   - Clinical bottom line

### Technical Review Criteria (Codex)

| Category | Check | Severity |
|----------|-------|----------|
| **Interpretation Validity** | Claims supported by results? | Critical |
| **Overclaim** | "first", "best", "superior" qualified? | Critical |
| **Novelty Context** | Appropriately positioned vs literature? | Major |
| **Limitation Adequacy** | All major limitations addressed? | Major |
| **Bias Discussion** | Selection, spectrum, verification bias? | Major |
| **Generalizability** | External validity discussed? | Major |
| **Conclusion Scope** | Not exceeding what data show? | Critical |

### Style Review Criteria (Gemini)

| Category | Check | Severity |
|----------|-------|----------|
| **Logical Flow** | Summary → Interpretation → Comparison → Significance → Limitations | Major |
| **Transition Sentences** | Clear connections between paragraphs | Major |
| **Avoid Result Repetition** | Don't restate exact numbers | Minor |
| **Future Tense** | For implications and future directions | Minor |
| **Length Balance** | Limitations not disproportionately long | Minor |
| **Verb Tense** | Present for established facts, past for study findings | Minor |

---

## Conclusion

### Structure Requirements

**2-3 Sentence Format:**
1. Main finding (answer to study question)
2. Clinical implication
3. (Optional) Future direction

### Technical Review Criteria (Codex)

| Category | Check | Severity |
|----------|-------|----------|
| **Supported by Data** | All claims backed by results? | Critical |
| **No New Information** | Nothing not discussed earlier? | Major |
| **No Overclaim** | Tempered language? | Critical |

### Style Review Criteria (Gemini)

| Category | Check | Severity |
|----------|-------|----------|
| **Brevity** | 2-3 sentences max | Major |
| **Actionable** | Clear take-home message | Major |
| **Standalone** | Understandable without context | Minor |

---

## Cover Letter

### Structure Requirements

**4-Paragraph Format:**
1. **Introduction**
   - Manuscript title
   - Submission request
2. **Study Summary**
   - Key findings (2-3 sentences)
   - Why important
3. **Journal Fit**
   - Why this journal
   - Reader relevance
4. **Closing**
   - Authorship statement
   - Conflict of interest
   - Contact information

### Review Criteria

- Professional tone
- Concise (< 300 words)
- Specific to journal
- No overselling
