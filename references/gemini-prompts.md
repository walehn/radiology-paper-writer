# Gemini Prompts

Style, readability, journal compliance, and literature research prompts for Gemini via CLI

## Usage

```bash
# Style review (Gemini 3.0 Pro)
gemini -y "[STYLE PROMPT]"

# Literature research (Gemini 3 Pro with extended reasoning)
gemini -m gemini-3-pro -y "[RESEARCH PROMPT]"
```

---

## Abstract Review Prompt

```
You are a medical writing specialist and journal editor reviewing a radiology manuscript abstract.

TARGET JOURNAL: {journal_name}
WORD LIMIT: {word_limit}
LANGUAGE: {language}

DRAFT TEXT:
---
{draft_text}
---

Review for readability, clarity, and journal style compliance. DO NOT change any numerical values or statistical conclusions.

## 1. SENTENCE BREVITY
- Flag sentences longer than 25 words
- Identify complex sentences that could be split
- Check for run-on sentences

## 2. INFORMATION DENSITY
- One key message per sentence?
- Any redundant phrases?
- Unnecessary filler words?

## 3. CLINICAL READABILITY
- Would a general radiologist understand easily?
- Technical jargon that could be simplified?
- Acronyms defined on first use?

## 4. VOICE AND TONE
- Active voice preferred
- Appropriate scientific tone
- Consistent tense usage

## 5. WORD COUNT
- Current word count
- Within limit?
- Suggest cuts if over

## 6. JOURNAL STYLE
- Matches {journal_name} abstract format?
- Appropriate section headers?

Output your review in JSON format:
{
  "word_count": {
    "current": {number},
    "limit": {word_limit},
    "status": "within_limit|over_limit|significantly_over",
    "suggested_cuts": []
  },
  "long_sentences": [
    {
      "text": "original sentence",
      "word_count": {number},
      "suggestion": "revised version or split suggestion"
    }
  ],
  "redundant_phrases": [
    {
      "phrase": "redundant phrase",
      "suggestion": "shorter alternative or 'delete'"
    }
  ],
  "jargon_replacements": [
    {
      "term": "technical term",
      "context": "sentence context",
      "suggestion": "clearer alternative"
    }
  ],
  "voice_issues": [
    {
      "sentence": "passive voice sentence",
      "suggestion": "active voice version"
    }
  ],
  "readability": {
    "flesch_kincaid_estimate": "grade level",
    "overall_clarity": "excellent|good|fair|poor",
    "issues": []
  },
  "journal_compliance": {
    "format_matches": true|false,
    "issues": []
  },
  "suggested_rewrites": [
    {
      "original": "original text",
      "revised": "improved version",
      "rationale": "why this change improves clarity"
    }
  ],
  "overall_assessment": "ready|minor_revisions|major_revisions"
}
```

---

## Methods Review Prompt

```
You are a medical writing specialist focusing on scientific methods clarity.

TARGET JOURNAL: {journal_name}
LANGUAGE: {language}

DRAFT TEXT:
---
{draft_text}
---

Review for clarity, organization, and readability. DO NOT evaluate scientific validity - focus only on writing quality.

## 1. PARAGRAPH STRUCTURE
- One topic per paragraph?
- Logical paragraph order?
- Appropriate paragraph length (3-10 sentences)?

## 2. SUBSECTION ORGANIZATION
- Clear subsection headers?
- Logical flow between subsections?
- Standard organization for study type?
  (Study Design → Participants → Protocol → Analysis)

## 3. REPETITION
- Repeated information across paragraphs?
- Redundant phrases within sentences?
- Information that could be consolidated?

## 4. READABILITY
- Average sentence length
- Active vs passive voice ratio
- Technical term consistency
- Verb tense consistency (past for Methods)

## 5. TRANSITIONS
- Clear connections between paragraphs?
- Transition phrases where needed?

## 6. CLARITY ISSUES
- Ambiguous pronouns?
- Unclear antecedents?
- Confusing sentence structures?

Output your review in JSON format:
{
  "paragraph_analysis": {
    "total_paragraphs": {number},
    "issues": [
      {
        "paragraph": {number},
        "issue": "description",
        "suggestion": "improvement"
      }
    ]
  },
  "subsection_structure": {
    "headers_present": ["list of headers"],
    "missing_headers": ["suggested headers"],
    "flow_issues": []
  },
  "repetitions": [
    {
      "repeated_content": "what's repeated",
      "locations": ["paragraph 1", "paragraph 3"],
      "suggestion": "consolidation suggestion"
    }
  ],
  "readability_metrics": {
    "avg_sentence_length": {number},
    "active_voice_percent": {number},
    "flesch_kincaid_grade": "estimate",
    "technical_term_consistency": "consistent|inconsistent"
  },
  "tense_issues": [
    {
      "sentence": "sentence with tense issue",
      "current_tense": "present|future",
      "should_be": "past",
      "correction": "corrected version"
    }
  ],
  "transition_needs": [
    {
      "between": "paragraph X and Y",
      "suggestion": "transition phrase or sentence"
    }
  ],
  "clarity_issues": [
    {
      "sentence": "unclear sentence",
      "issue": "ambiguous pronoun|unclear structure|etc",
      "suggestion": "clearer version"
    }
  ],
  "suggested_rewrites": [],
  "overall_assessment": "ready|minor_revisions|major_revisions"
}
```

---

## Results Review Prompt

```
You are a medical writing specialist reviewing Results section presentation.

TARGET JOURNAL: {journal_name}
LANGUAGE: {language}

DRAFT TEXT:
---
{draft_text}
---

Review for clarity and presentation quality. DO NOT verify statistical accuracy - focus on writing quality.

## 1. PARAGRAPH FOCUS
- One main finding per paragraph?
- Key message stated first in each paragraph?
- Supporting details follow logically?

## 2. FIGURE/TABLE REFERENCES
- All figures/tables referenced in text?
- References in sequential order?
- Reference format correct: "Figure 1", "Table 2"?

## 3. NUMBER PRESENTATION
- Consistent decimal places?
- Consistent formatting (e.g., always "X%" not sometimes "X percent")?
- Numbers in parentheses formatted consistently?

## 4. OBJECTIVITY
- Any interpretive statements? (should be in Discussion)
- Value judgments ("importantly", "notably")?
- Results stated objectively?

## 5. FLOW
- Logical order of results?
- Primary outcome first?
- Transitions between findings?

## 6. CONCISENESS
- Unnecessarily verbose descriptions?
- Redundant with tables/figures?

Output your review in JSON format:
{
  "paragraph_focus": {
    "paragraphs_with_multiple_findings": [],
    "paragraphs_with_buried_key_message": [],
    "suggestions": []
  },
  "figure_table_references": {
    "all_referenced": true|false,
    "sequential_order": true|false,
    "format_consistent": true|false,
    "issues": []
  },
  "number_formatting": {
    "decimal_consistency": true|false,
    "format_consistency": true|false,
    "issues": []
  },
  "objectivity_issues": [
    {
      "sentence": "interpretive sentence",
      "issue": "interpretation|value_judgment|opinion",
      "suggestion": "objective version or 'move to Discussion'"
    }
  ],
  "flow_assessment": {
    "logical_order": true|false,
    "primary_outcome_first": true|false,
    "transition_issues": []
  },
  "conciseness_issues": [
    {
      "text": "verbose text",
      "suggestion": "concise version"
    }
  ],
  "suggested_rewrites": [],
  "overall_assessment": "ready|minor_revisions|major_revisions"
}
```

---

## Discussion Review Prompt

```
You are a medical writing specialist reviewing Discussion section structure and flow.

TARGET JOURNAL: {journal_name}
LANGUAGE: {language}

DRAFT TEXT:
---
{draft_text}
---

Review for logical flow, structure, and readability. DO NOT evaluate scientific validity of interpretations.

## 1. STRUCTURE
Standard Discussion structure:
1. Summary of main findings (1 para)
2. Interpretation in context (1-2 para)
3. Comparison with literature (2-3 para)
4. Clinical implications (1 para)
5. Limitations (1 para)
6. Future directions (optional)
7. Conclusion (1 para)

Is this structure followed?

## 2. LOGICAL FLOW
- Does each paragraph follow logically from the previous?
- Are transition sentences present?
- Is there a clear narrative arc?

## 3. RESULT REPETITION
- Are results repeated verbatim? (should paraphrase)
- Too many numbers in Discussion?

## 4. PARAGRAPH UNITY
- Does each paragraph have one central idea?
- Are there paragraph breaks in logical places?

## 5. BALANCE
- Limitations appropriately weighted?
- Not overly long on any section?
- Clinical implications proportionate to evidence?

## 6. TONE
- Appropriately hedged language?
- Confident but not overconfident?
- Scholarly but accessible?

Output your review in JSON format:
{
  "structure_analysis": {
    "summary_present": true|false,
    "interpretation_present": true|false,
    "literature_comparison_present": true|false,
    "implications_present": true|false,
    "limitations_present": true|false,
    "conclusion_present": true|false,
    "structure_issues": []
  },
  "flow_analysis": {
    "logical_progression": true|false,
    "transition_sentences_adequate": true|false,
    "narrative_coherence": "strong|adequate|weak",
    "issues": []
  },
  "result_repetition": {
    "verbatim_repetitions": [],
    "excessive_numbers": true|false,
    "suggestions": []
  },
  "paragraph_unity": {
    "multi_idea_paragraphs": [],
    "suggested_breaks": []
  },
  "balance_assessment": {
    "limitations_weight": "appropriate|too_brief|too_long",
    "section_proportions": "balanced|unbalanced",
    "issues": []
  },
  "tone_issues": [
    {
      "sentence": "problematic sentence",
      "issue": "overconfident|dismissive|unclear",
      "suggestion": "improved version"
    }
  ],
  "suggested_rewrites": [],
  "overall_assessment": "ready|minor_revisions|major_revisions"
}
```

---

## Korean Language Style Prompt

```
You are a Korean medical writing specialist reviewing a radiology manuscript in English for Korean authors.

LANGUAGE: ko
TARGET: International radiology journal

DRAFT TEXT:
---
{draft_text}
---

Review for issues common in Korean-to-English medical writing:

## 1. SENTENCE STRUCTURE
- Long, complex sentences (Korean sentence patterns)?
- Subject missing (common in Korean)?
- Incorrect word order?

## 2. ARTICLE USAGE
- Missing articles (a, an, the)?
- Incorrect article choice?

## 3. VERB ISSUES
- Subject-verb agreement?
- Tense consistency?
- Passive voice overuse?

## 4. PREPOSITION USAGE
- Incorrect prepositions?
- Missing prepositions?

## 5. WORD CHOICE
- Direct translations that sound unnatural?
- Korean medical terms needing English equivalents?

## 6. FORMALITY
- Appropriate academic register?
- Overly formal or informal expressions?

Output your review in JSON format:
{
  "sentence_structure_issues": [],
  "article_issues": [],
  "verb_issues": [],
  "preposition_issues": [],
  "word_choice_issues": [],
  "formality_issues": [],
  "korean_specific_suggestions": [],
  "overall_assessment": "ready|minor_revisions|major_revisions"
}
```

---

## Quick Style Checks

### Sentence Length Check
```
Analyze sentence lengths in this text and flag any over 25 words:
{text}

Return JSON with flagged sentences and suggested splits.
```

### Passive Voice Audit
```
Identify passive voice constructions in:
{text}

Suggest active voice alternatives where appropriate. Return JSON.
```

### Readability Score
```
Estimate readability metrics for:
{text}

Return Flesch-Kincaid grade level and suggestions for improvement.
```

### Journal Style Match
```
Check if this text matches {journal_name} style guidelines:
{text}

Focus on: formatting, terminology, structure. Return compliance issues.
```

---

## Introduction Research Prompt

**Model:** Gemini 3 Pro (`gemini -m gemini-3-pro`)
**Purpose:** Literature review and knowledge gap analysis for Introduction writing

```
You are a radiology research expert conducting a comprehensive literature review for a research manuscript.

Think step-by-step through the literature landscape.
First, identify the most impactful studies in this area.
Then, analyze what questions remain unanswered.
Finally, synthesize how the proposed research fills the gap.

RESEARCH TOPIC: {topic}
KEYWORDS: {keywords}
STUDY TYPE: {study_type}
TARGET JOURNAL: {journal_name}

Your task is to provide a structured literature analysis for writing an Introduction section.

### Required Analysis:

## 1. CLINICAL CONTEXT
- Current clinical practice and importance of the topic
- Prevalence/incidence of the condition or clinical problem
- Standard diagnostic/treatment approaches currently used
- Why this topic matters clinically (patient outcomes, healthcare burden)

## 2. EXISTING RESEARCH LANDSCAPE
- Key landmark studies in this area (cite author names and approximate years when possible)
- Current state-of-the-art methods and their reported performance
- Performance metrics from literature (sensitivity, specificity, AUC ranges)
- Consensus points and controversies in the field
- Recent trends and emerging approaches

## 3. KNOWLEDGE GAP ANALYSIS
- Specific limitations of existing studies (sample size, methodology, generalizability)
- Unaddressed clinical questions
- Methodological weaknesses in prior research
- What remains to be explored or validated

## 4. RESEARCH OPPORTUNITY
- How the proposed study addresses the identified gap
- Potential contribution to the field
- Expected clinical impact and relevance

### Output Format (JSON):

{
  "clinical_context": {
    "clinical_importance": "Why this topic matters clinically",
    "prevalence_data": "Known prevalence/incidence information",
    "current_practice": "Standard approaches currently used",
    "unmet_clinical_need": "What clinical problem needs solving"
  },
  "key_literature": [
    {
      "citation_hint": "Author et al., Year (approximate)",
      "study_type": "diagnostic accuracy|cohort|meta-analysis|etc",
      "key_finding": "Main result or conclusion",
      "sample_size": "approximate if known",
      "relevance": "Why this study is important for context"
    }
  ],
  "state_of_the_art": {
    "best_methods": "Current leading approaches",
    "performance_range": "Reported sensitivity/specificity/AUC ranges",
    "consensus_points": ["Points generally agreed upon"],
    "controversies": ["Debated issues in the field"]
  },
  "knowledge_gaps": [
    {
      "gap_description": "Specific gap in current knowledge",
      "why_important": "Why this gap matters",
      "severity": "critical|major|minor"
    }
  ],
  "research_opportunity": {
    "how_addresses_gap": "How proposed study fills the gap",
    "novelty": "What makes this approach new or different",
    "expected_contribution": "Anticipated impact on the field",
    "clinical_translation": "Potential for clinical application"
  },
  "suggested_introduction_structure": {
    "paragraph_1_clinical_context": "Draft sentences for clinical background...",
    "paragraph_2_literature_and_gap": "Draft sentences for existing research and limitations...",
    "paragraph_3_objective": "Draft objective statement..."
  },
  "recommended_citations": [
    {
      "topic": "What aspect this citation supports",
      "search_terms": "Suggested PubMed search terms to find this reference"
    }
  ]
}
```

---

## Introduction Style Review Prompt

**Model:** Gemini 3.0 Pro (`gemini -y`)
**Purpose:** Style and flow review for Introduction section

```
You are a medical writing specialist reviewing the Introduction section of a radiology manuscript.

TARGET JOURNAL: {journal_name}
LANGUAGE: {language}

DRAFT TEXT:
---
{draft_text}
---

Review for clarity, flow, and scholarly writing quality. DO NOT evaluate scientific validity - focus on writing quality.

## 1. STRUCTURE ANALYSIS
Standard Introduction structure (3-paragraph model):
- Paragraph 1: Clinical context and importance
- Paragraph 2: Existing research and knowledge gap
- Paragraph 3: Study objective and approach

Is this structure present and clear?

## 2. FLOW ANALYSIS
- General → Specific progression (funnel structure)?
- Smooth transitions between paragraphs?
- Logical connection between gap and objective?

## 3. PARAGRAPH UNITY
- Each paragraph focused on one main idea?
- Topic sentences clear?
- Supporting sentences relevant?

## 4. CITATION INTEGRATION
- Citations flow naturally within sentences?
- Not too citation-heavy (reading disrupted)?
- Not too sparse (claims unsupported)?
- Parenthetical vs. narrative citations appropriate?

## 5. READABILITY
- Sentence length appropriate (average < 25 words)?
- Technical terms defined when first used?
- Jargon minimized for general radiology audience?

## 6. OBJECTIVE STATEMENT
- Clear and specific?
- Directly addresses stated gap?
- Appropriately scoped (not too broad/narrow)?

Output your review in JSON format:
{
  "structure_assessment": {
    "clinical_context_present": true|false,
    "literature_review_present": true|false,
    "gap_articulated": true|false,
    "objective_stated": true|false,
    "structure_issues": []
  },
  "flow_analysis": {
    "general_to_specific": true|false,
    "paragraph_transitions": "smooth|adequate|weak",
    "gap_to_objective_link": "strong|adequate|weak",
    "issues": []
  },
  "paragraph_unity": {
    "paragraphs_with_multiple_ideas": [],
    "weak_topic_sentences": [],
    "suggestions": []
  },
  "citation_integration": {
    "density": "appropriate|too_heavy|too_sparse",
    "flow_disruption": true|false,
    "issues": []
  },
  "readability": {
    "avg_sentence_length": {number},
    "long_sentences": [],
    "undefined_terms": [],
    "jargon_issues": []
  },
  "objective_statement": {
    "clarity": "clear|somewhat_clear|unclear",
    "specificity": "specific|vague|too_broad",
    "gap_alignment": "aligned|partially_aligned|misaligned",
    "suggested_revision": "improved version if needed"
  },
  "suggested_rewrites": [
    {
      "original": "original text",
      "revised": "improved version",
      "rationale": "why this improves the writing"
    }
  ],
  "overall_assessment": "ready|minor_revisions|major_revisions"
}
```
