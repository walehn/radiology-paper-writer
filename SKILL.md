---
name: radiology-paper-writer
description: AI-assisted radiology research paper writing with multi-model review. Claude drafts, GPT-5.2 (reasoning: high) reviews methodology, Gemini reviews style. Supports STARD/TRIPOD/CLAIM checklists. Triggers on /radiology-draft, /radiology-review, /radiology-revise commands.
---

# Radiology Paper Writer

영상의학 학술 연구 논문 작성 및 교정을 위한 멀티모델 리뷰 Skill

## Model Roles

| Model | Role | Focus Areas |
|-------|------|-------------|
| **Claude Code** | Main Editor | 초안 작성, 피드백 통합, 수정, 버전 관리 |
| **GPT-5.2 (reasoning: high)** | Technical Reviewer | 연구 설계, 통계, 논리 일관성, 방법론 |
| **Gemini 3.0 Pro** | Style Reviewer | 문장 자연스러움, 가독성, 저널 스타일, clarity |

## Supported Study Types

- `diagnostic_accuracy` - 진단 정확도 연구 (STARD checklist)
- `prognostic` - 예후/예측 모델 연구 (TRIPOD checklist)
- `segmentation_ai` - AI 분할 연구 (CLAIM checklist)
- `screening` - 스크리닝 연구 (STARD + screening extensions)
- `interventional` - 중재 영상의학 연구

## Supported Section Types

Title | Abstract | Introduction | Methods | Results | Discussion | Conclusion | CoverLetter

## Input Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| section_type | string | Yes | 작성/리뷰할 섹션 |
| study_type | string | Yes | 연구 유형 (checklist 결정) |
| journal_profile | string | No | 타깃 저널 (Radiology, EurRad, AJR 등) |
| draft_text | string | Review/Revise | 리뷰할 텍스트 |
| reviewer_focus | string | No | balanced \| technical \| clinical_readability \| language_only |
| language | string | No | en (default) \| ko |

---

## Commands

### /radiology-draft [section]

초안을 작성합니다.

**Workflow:**
1. **Context Gathering** - AskUserQuestion으로 필수 정보 수집
   - Study type (diagnostic_accuracy, prognostic, segmentation_ai, etc.)
   - Target journal
   - Key findings and numbers to include

2. **Draft Generation**
   - Load section template from `references/section-templates.md`
   - Load journal profile from `references/journal-profiles.md`
   - Generate draft following template and journal style

3. **Optional Dual Review** - Ask user if they want immediate review
   - If yes, run /radiology-review workflow

**Example:**
```
/radiology-draft Methods

> Study type: diagnostic_accuracy
> Target journal: Radiology
> Key findings: AUC 0.92, sensitivity 92.5%, specificity 92.2%
```

### /radiology-review [section]

기존 텍스트를 멀티모델로 리뷰합니다.

**Workflow:**
1. **Parse Input** - draft_text from context or user input

2. **Determine Checklist** - Based on study_type:
   - diagnostic_accuracy → STARD 2015 + STARD-AI (if AI)
   - prognostic → TRIPOD + TRIPOD+AI (if AI)
   - segmentation_ai → CLAIM 2024

3. **Parallel Review Execution**

   **GPT-5.2 (Technical Review, reasoning: high):**
   ```bash
   codex exec -m gpt-5.2 --sandbox read-only --reasoning high "[prompt from references/codex-prompts.md]"
   ```

   **Gemini (Style Review):**
   ```bash
   gemini -y "[prompt from references/gemini-prompts.md]"
   ```

4. **Feedback Integration**
   - Parse JSON responses from both models
   - Merge into unified output structure
   - Generate checklist compliance status

**Example:**
```
/radiology-review Methods

[User provides or Claude reads the Methods section text]

Reviewing with:
- GPT-5.2 (reasoning: high): Methodological rigor, statistics, bias
- Gemini 3.0 Pro: Clarity, structure, readability
```

### /radiology-revise

피드백을 반영하여 수정합니다.

**Workflow:**
1. Load current draft and accumulated feedback
2. Analyze feedback and prioritize:
   - Critical issues first (methodology, statistics)
   - Then clarity/style issues
   - Then minor polish
3. Generate revised draft
4. Optional: Re-run targeted reviews on changed sections
5. Track revision history

---

## Section-Specific Review Criteria

### Abstract

| GPT-5.2 (Technical) | Gemini (Style) |
|---------------------|----------------|
| Structure: 목적-방법-결과-결론 | Sentence brevity (< 25 words) |
| Number consistency with Results | Information density |
| Overclaim detection | Clinical readability |
| Primary endpoint clarity | Word count compliance |
| Statistical formatting | Active voice preference |

### Methods

| GPT-5.2 (Technical) | Gemini (Style) |
|---------------------|----------------|
| Patient selection bias | Paragraph organization |
| Spectrum bias | Subsection structure |
| Imaging protocol completeness | Repetition removal |
| Reference standard adequacy | Verb tense consistency |
| Sample size justification | Readability score |
| Multiple comparison correction | |
| Inter-reader agreement | |

### Results

| GPT-5.2 (Technical) | Gemini (Style) |
|---------------------|----------------|
| Table/figure vs text consistency | One message per paragraph |
| Primary outcome prominence | Key message first |
| Statistical formatting (OR, CI, p) | Figure reference order |
| Subgroup analysis labeling | Avoid interpretation |

### Discussion

| GPT-5.2 (Technical) | Gemini (Style) |
|---------------------|----------------|
| Interpretation validity | Logical flow |
| Novelty vs literature context | Transition sentences |
| Limitation adequacy | Avoid result repetition |
| Overclaim prevention | Future tense for implications |

---

## Output Structure

Review results are presented in this unified format:

```json
{
  "major_issues": [
    {
      "type": "methodology|statistics|consistency",
      "location": "paragraph/sentence reference",
      "description": "issue description",
      "severity": "critical|major|minor",
      "source": "gpt-5.2"
    }
  ],
  "clarity_issues": [
    {
      "type": "readability|structure|flow",
      "location": "paragraph/sentence reference",
      "description": "issue description",
      "suggestion": "improvement suggestion",
      "source": "gemini"
    }
  ],
  "terminology_and_consistency": [],
  "candidate_rewrites": [
    {
      "location": "paragraph/sentence",
      "original": "original text",
      "revised": "suggested revision",
      "rationale": "why this change"
    }
  ],
  "checklist": {
    "type": "STARD|TRIPOD|CLAIM",
    "compliant": ["item1", "item2"],
    "missing": ["item3"],
    "incomplete": ["item4"]
  }
}
```

---

## CLI Command Reference

### GPT-5.2 Execution (via Codex CLI)

```bash
# Single review (read-only sandbox, high reasoning)
codex exec -m gpt-5.2 --sandbox read-only --reasoning high "[prompt]"

# Follow-up in same session
codex exec resume --last "[follow-up prompt]"
```

### Gemini Execution

```bash
# Auto-approve mode (non-interactive)
gemini -y "[prompt]"

# With JSON output preference
gemini -y "[prompt requesting JSON output]"
```

---

## Error Handling

| Scenario | Action |
|----------|--------|
| GPT-5.2 timeout/failure | Retry once, then Claude-only review with notice |
| Gemini error | Continue with GPT-5.2 review only, notify user |
| Both CLI failures | Claude provides basic review with apology |
| Invalid JSON response | Extract text feedback, present as unstructured |
| Empty response | Retry with simplified prompt |

---

## Language Support

### English (default)
- All prompts in English
- Checklists in English
- Feedback in English

### Korean (language="ko")
- GPT-5.2 prompts: Keep English (technical consistency)
- Gemini prompts: Add Korean style guidelines
- Output feedback: Present in Korean
- Checklists: Item names in English, descriptions bilingual

Korean-specific style rules:
- 존댓말 일관성
- 영어 의학용어 vs 한글 용어 선택 기준
- 문장 길이: 40자 이내 권장

---

## Best Practices

1. **Always validate numbers** - Abstract numbers must match Results/Tables
2. **Check for overclaims** - Avoid "first", "novel", "superior" without evidence
3. **Statistical formatting** - Follow journal style (OR [95% CI], p-values)
4. **Reference standard** - Ensure adequacy for diagnostic studies
5. **Patient selection** - Explicit, reproducible criteria
6. **Blinding** - Document reader blinding appropriately
7. **Limitations** - Address all major limitations honestly

---

## Reference Files

- `references/radiology-checklists.md` - STARD, TRIPOD, CLAIM checklists
- `references/section-templates.md` - Section-specific review criteria details
- `references/journal-profiles.md` - Journal requirements and formatting
- `references/codex-prompts.md` - GPT-5.2 review prompts by section
- `references/gemini-prompts.md` - Gemini review prompts by section
