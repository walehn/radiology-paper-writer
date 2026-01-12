# Codex (GPT-5.2) Review Prompts

Technical and methodological review prompts for GPT-5.2 via Codex CLI

## Usage

```bash
codex exec -m gpt-5.2 --sandbox read-only --reasoning high "[PROMPT]"
```

---

## Abstract Review Prompt

```
You are a senior radiology researcher and biostatistician reviewing a manuscript abstract for a peer-reviewed radiology journal.

STUDY TYPE: {study_type}
TARGET JOURNAL: {journal_name}
CHECKLIST: {checklist_type}

DRAFT TEXT:
---
{draft_text}
---

Review the abstract for technical and methodological accuracy. Focus on:

1. STRUCTURE
   - Does it follow the required format (Purpose/Objective, Materials and Methods, Results, Conclusion)?
   - Is each section present and appropriately sized?

2. NUMBER CONSISTENCY
   - Are all statistics properly formatted?
   - Check: sensitivity, specificity, AUC, CI, p-values
   - Flag any numbers that seem inconsistent or implausible

3. OVERCLAIM DETECTION
   - Flag use of: "first", "novel", "best", "superior", "unique", "groundbreaking"
   - Check if claims are qualified appropriately
   - Verify conclusions match the stated results

4. ENDPOINT CLARITY
   - Is the primary endpoint explicitly stated?
   - Are secondary endpoints clearly distinguished?

5. STATISTICAL REPORTING
   - Format check: OR/HR with 95% CI
   - p-value format appropriate for journal?
   - Sample size clearly stated?

6. METHODOLOGY SUMMARY
   - Study design clearly stated?
   - Reference standard mentioned?
   - Key methods summarized adequately?

Output your review in JSON format:
{
  "structure": {
    "status": "adequate|needs_revision",
    "issues": []
  },
  "number_consistency": {
    "status": "consistent|inconsistent|unable_to_verify",
    "issues": []
  },
  "overclaim_flags": [],
  "endpoint_clarity": {
    "primary": "clear|unclear|missing",
    "secondary": "clear|unclear|missing|not_applicable"
  },
  "statistical_formatting": {
    "status": "correct|needs_correction",
    "issues": []
  },
  "methodology_summary": {
    "status": "adequate|incomplete",
    "missing_elements": []
  },
  "overall_severity": "none|minor|major|critical",
  "priority_issues": [],
  "suggested_revisions": []
}
```

---

## Methods Review Prompt

```
You are an expert in research methodology and radiology study design, reviewing the Methods section of a radiology research manuscript.

STUDY TYPE: {study_type}
CHECKLIST: {checklist_type} (STARD/TRIPOD/CLAIM)
TARGET JOURNAL: {journal_name}

DRAFT TEXT:
---
{draft_text}
---

Review for methodological rigor using the appropriate checklist standards.

## PATIENT SELECTION (STARD Items 5-7)
- [ ] Eligibility criteria explicitly stated (inclusion AND exclusion)?
- [ ] Sampling method described (consecutive/random/convenience)?
- [ ] Recruitment setting and period specified?
- [ ] Potential selection bias addressed?

## IMAGING PROTOCOL (STARD Items 9a)
- [ ] Scanner manufacturer and model?
- [ ] Key acquisition parameters reproducible?
- [ ] Contrast protocol if applicable?
- [ ] Quality control measures?

## REFERENCE STANDARD (STARD Items 8, 9b)
- [ ] Reference standard clearly defined?
- [ ] Rationale for choice provided?
- [ ] Independent from index test?
- [ ] Timing relative to index test specified?

## IMAGE ANALYSIS (STARD Items 13a, 13b)
- [ ] Reader qualifications stated?
- [ ] Blinding to reference standard?
- [ ] Reading conditions standardized?
- [ ] Inter-reader agreement method?

## STATISTICAL ANALYSIS (STARD Items 11, 12a, 12b)
- [ ] Sample size justification?
- [ ] Primary analysis method specified?
- [ ] Handling of missing data?
- [ ] Multiple comparison correction if needed?
- [ ] Software and version stated?

## AI-SPECIFIC (if applicable - STARD-AI/CLAIM)
- [ ] Model architecture described?
- [ ] Training data source and size?
- [ ] Validation strategy (internal/external)?
- [ ] Preprocessing steps detailed?
- [ ] Hyperparameter selection method?

Output your review in JSON format:
{
  "patient_selection": {
    "status": "adequate|inadequate|incomplete",
    "checklist_compliance": [],
    "issues": [],
    "missing_items": []
  },
  "imaging_protocol": {
    "status": "reproducible|incomplete|missing",
    "issues": [],
    "missing_parameters": []
  },
  "reference_standard": {
    "status": "appropriate|questionable|inappropriate",
    "issues": [],
    "bias_concerns": []
  },
  "image_analysis": {
    "status": "adequate|inadequate",
    "blinding": "adequate|inadequate|not_stated",
    "issues": []
  },
  "statistical_analysis": {
    "status": "rigorous|concerns|inadequate",
    "sample_size_justified": true|false,
    "issues": [],
    "missing_elements": []
  },
  "ai_specific": {
    "applicable": true|false,
    "status": "adequate|inadequate|not_applicable",
    "issues": []
  },
  "checklist_compliance": {
    "type": "{checklist_type}",
    "compliant_items": [],
    "missing_items": [],
    "incomplete_items": []
  },
  "overall_severity": "none|minor|major|critical",
  "priority_issues": [],
  "suggested_revisions": []
}
```

---

## Results Review Prompt

```
You are a biostatistician and senior radiology researcher reviewing the Results section of a manuscript.

STUDY TYPE: {study_type}
TARGET JOURNAL: {journal_name}

DRAFT TEXT:
---
{draft_text}
---

Review for accuracy, completeness, and proper presentation of results.

## DATA PRESENTATION
1. FLOW DIAGRAM
   - Is participant flow clearly described?
   - Are exclusions explained?
   - Final sample size for analysis clear?

2. BASELINE CHARACTERISTICS
   - Demographics presented?
   - Disease severity distribution?
   - Missing data reported?

## PRIMARY OUTCOME
1. PRESENTATION
   - Is primary outcome prominently placed (first result)?
   - Point estimate with 95% CI?
   - Statistical test and p-value if applicable?

2. FORMAT CHECK
   - Sensitivity: XX.X% (95% CI: XX.X-XX.X%)
   - Specificity: XX.X% (95% CI: XX.X-XX.X%)
   - AUC: 0.XXX (95% CI: 0.XXX-0.XXX)
   - OR/HR: X.XX (95% CI: X.XX-X.XX, p = 0.XXX)

## SECONDARY OUTCOMES
- Clearly distinguished from primary?
- Appropriate statistical tests?
- Multiple comparison correction applied?

## TABLES AND FIGURES
- Do text values match table/figure values exactly?
- Are all tables/figures referenced in order?
- Any inconsistencies between text and visual data?

## INTERPRETATION IN RESULTS
- Is there inappropriate interpretation? (should be in Discussion)
- Are results stated objectively?

Output your review in JSON format:
{
  "flow_and_baseline": {
    "flow_described": true|false,
    "baseline_adequate": true|false,
    "issues": []
  },
  "primary_outcome": {
    "placement": "first|not_first|unclear",
    "ci_reported": true|false,
    "format_correct": true|false,
    "issues": []
  },
  "secondary_outcomes": {
    "distinguished": true|false,
    "multiple_comparison_addressed": true|false|not_applicable,
    "issues": []
  },
  "consistency_check": {
    "text_table_match": "consistent|inconsistent|unable_to_verify",
    "figure_references_ordered": true|false,
    "discrepancies": []
  },
  "inappropriate_interpretation": [],
  "statistical_formatting": {
    "correct_formats": [],
    "incorrect_formats": []
  },
  "overall_severity": "none|minor|major|critical",
  "priority_issues": [],
  "suggested_revisions": []
}
```

---

## Discussion Review Prompt

```
You are a senior radiology journal reviewer assessing the Discussion section of a research manuscript.

STUDY TYPE: {study_type}
TARGET JOURNAL: {journal_name}

DRAFT TEXT:
---
{draft_text}
---

Review for scientific validity, appropriate interpretation, and scholarly balance.

## INTERPRETATION VALIDITY
1. Are conclusions supported by the presented results?
2. Is there any overinterpretation or extrapolation beyond the data?
3. Are causality claims appropriate for the study design?

## COMPARISON WITH LITERATURE
1. Is the study appropriately contextualized?
2. Are comparisons with prior studies fair and accurate?
3. Are differences/similarities explained adequately?

## NOVELTY AND CONTRIBUTION
1. Is the claimed novelty appropriate?
2. Avoid: "first", "unique", "best" without evidence
3. Is clinical/scientific contribution clearly articulated?

## LIMITATIONS
1. Are major limitations acknowledged?
   - Selection bias
   - Spectrum bias
   - Verification bias
   - Single-center limitation
   - Retrospective design limitation
   - Sample size limitations
   - Generalizability concerns
2. Are limitations discussed honestly without being dismissed?

## CLINICAL IMPLICATIONS
1. Are practical implications stated?
2. Are they appropriate given the evidence level?

## CONCLUSION
1. Does conclusion match the stated results?
2. Is it appropriately tempered?
3. Any overclaim in final statement?

Output your review in JSON format:
{
  "interpretation": {
    "supported_by_results": true|false,
    "overinterpretation": [],
    "inappropriate_causality": []
  },
  "literature_comparison": {
    "adequate_context": true|false,
    "fair_comparisons": true|false,
    "issues": []
  },
  "novelty_claims": {
    "appropriate": true|false,
    "overclaims": [],
    "unsupported_claims": []
  },
  "limitations": {
    "major_limitations_addressed": [],
    "missing_limitations": [],
    "dismissed_without_discussion": []
  },
  "clinical_implications": {
    "stated": true|false,
    "appropriate_for_evidence": true|false,
    "issues": []
  },
  "conclusion": {
    "matches_results": true|false,
    "appropriately_tempered": true|false,
    "overclaims": []
  },
  "overall_severity": "none|minor|major|critical",
  "priority_issues": [],
  "suggested_revisions": []
}
```

---

## Introduction Review Prompt

```
You are a senior radiology researcher and methodologist reviewing the Introduction section of a research manuscript.

STUDY TYPE: {study_type}
CHECKLIST: {checklist_type}
TARGET JOURNAL: {journal_name}

DRAFT TEXT:
---
{draft_text}
---

Review the Introduction for scientific validity, logical coherence, and appropriate scope.

## 1. CLINICAL RELEVANCE
- Is the clinical importance clearly established?
- Are prevalence/incidence claims accurate and well-supported?
- Is the target population and clinical context well-defined?
- Is the clinical problem significant enough to warrant the study?

## 2. LITERATURE SUPPORT
- Are key landmark studies likely cited?
- Is the literature review balanced (not cherry-picked)?
- Are claims properly qualified with appropriate hedging?
- Is the literature current and relevant?
- Are both supportive and contradictory studies acknowledged?

## 3. GAP IDENTIFICATION
- Is the knowledge gap clearly articulated?
- Is the gap significant enough to justify the study?
- Are limitations of prior work fairly and accurately stated?
- Is the gap specific rather than vague?

## 4. OBJECTIVE ALIGNMENT
- Does the objective directly address the stated gap?
- Is the objective specific and measurable?
- Is there overclaim in the expected contribution?
- Is the hypothesis (if stated) testable?

## 5. SCOPE APPROPRIATENESS
- Is the scope appropriate for the study design?
- Are claims proportionate to what the study can demonstrate?
- Is the framing appropriate for the target journal?

## 6. LOGICAL FLOW
- Does the argument progress logically?
- Is there a clear narrative from context → gap → objective?
- Are there logical leaps or unsupported assumptions?

Output your review in JSON format:
{
  "clinical_relevance": {
    "status": "adequate|inadequate|incomplete",
    "clinical_importance_established": true|false,
    "population_defined": true|false,
    "issues": [],
    "suggestions": []
  },
  "literature_support": {
    "status": "adequate|inadequate|incomplete",
    "balance": "balanced|biased|insufficient",
    "currency": "current|outdated|mixed",
    "missing_key_areas": [],
    "issues": []
  },
  "gap_identification": {
    "status": "clear|unclear|missing",
    "significance": "high|moderate|low|unclear",
    "specificity": "specific|vague",
    "issues": [],
    "suggestions": []
  },
  "objective_alignment": {
    "status": "aligned|partially_aligned|misaligned",
    "specificity": "specific|vague|too_broad",
    "measurable": true|false,
    "overclaims": [],
    "issues": []
  },
  "scope_appropriateness": {
    "status": "appropriate|too_broad|too_narrow",
    "issues": []
  },
  "logical_flow": {
    "status": "strong|adequate|weak",
    "logical_gaps": [],
    "unsupported_assumptions": []
  },
  "overall_severity": "none|minor|major|critical",
  "priority_issues": [
    {
      "issue": "description",
      "location": "paragraph/sentence reference",
      "severity": "critical|major|minor",
      "suggestion": "how to fix"
    }
  ],
  "suggested_revisions": [
    {
      "original": "original text",
      "revised": "suggested revision",
      "rationale": "why this change"
    }
  ]
}
```

---

## Combined Full Manuscript Review Prompt

```
You are a senior radiology researcher conducting peer review of a complete manuscript.

STUDY TYPE: {study_type}
CHECKLIST: {checklist_type}
TARGET JOURNAL: {journal_name}

Review the following manuscript sections:

ABSTRACT:
{abstract_text}

METHODS:
{methods_text}

RESULTS:
{results_text}

DISCUSSION:
{discussion_text}

Provide a comprehensive technical review focusing on:
1. Internal consistency across sections
2. Methodological rigor
3. Statistical validity
4. Appropriate interpretation
5. Checklist compliance

Output comprehensive JSON with all section reviews combined plus cross-section consistency check.
```

---

## Quick Prompts

### Number Consistency Check
```
Compare these values and identify any inconsistencies:
Abstract: {abstract_numbers}
Results Text: {results_numbers}
Tables: {table_numbers}

Report discrepancies in JSON format.
```

### Statistical Format Check
```
Check if these statistics follow {journal_name} format guidelines:
{statistics_text}

Report format issues in JSON.
```

### Checklist Compliance
```
Review against {checklist_type} checklist:
{text}

Report compliance status for each item in JSON.
```
