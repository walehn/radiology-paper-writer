---
name: radiology-paper-writer
description: AI-assisted radiology research paper writing with multi-model review. Claude drafts and reviews style natively, GPT-5.4 (reasoning: high) reviews methodology, GPT-5.4 (reasoning: high) performs literature analysis, and Claude WebSearch/WebFetch verifies citations. Supports STARD/TRIPOD/CLAIM/MI-CLEAR-LLM checklists. Triggers on /radiology-draft, /radiology-intro, /radiology-review, /radiology-revise commands.
---

# Radiology Paper Writer

영상의학 학술 연구 논문 작성 및 교정을 위한 3-모델 리뷰 Skill.

## Model Roles

| Role | When Invoked | Invocation Method |
|------|--------------|-------------------|
| **Claude Code (Main Editor + Style Reviewer)** | 모든 초안 작성, 수정, 버전 관리, /radiology-review Step 2 (스타일 리뷰), /radiology-intro Stage 2 (URL 검증) | Native (no external CLI). Style criteria loaded from `references/codex-prompts.md` "Style Guidance for Claude Native Review" header. URL verification via Claude WebSearch + WebFetch tools. |
| **Codex (GPT-5.4) Technical Reviewer** | /radiology-review Step 1, /radiology-draft optional dual review (technical side), /radiology-intro optional dual review (technical side) | `codex exec -m gpt-5.4 --sandbox read-only --config model_reasoning_effort=high "[prompt]"` with prompts from `references/codex-prompts.md`. |
| **Codex (GPT-5.4) Literature Researcher** | /radiology-intro Stage 1 (Introduction literature analysis) | `codex exec -m gpt-5.4 --sandbox read-only --config model_reasoning_effort=high "[prompt]"` using the "Introduction Literature Analysis" prompt in `references/codex-prompts.md`. |

No fourth reviewer exists. All style review is Claude-native; all literature research is Codex (GPT-5.4) Stage 1 followed by Claude WebSearch + WebFetch Stage 2.

## Supported Study Types

- `diagnostic_accuracy` - 진단 정확도 연구 (STARD checklist)
- `prognostic` - 예후/예측 모델 연구 (TRIPOD checklist)
- `segmentation_ai` - AI 분할 연구 (CLAIM checklist)
- `screening` - 스크리닝 연구 (STARD + screening extensions)
- `interventional` - 중재 영상의학 연구
- `llm_study` - LLM 활용 연구 (MI-CLEAR-LLM + TRIPOD-LLM checklist)

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

### /radiology-draft [section] [language]

초안을 작성합니다. Claude-only 워크플로입니다.

**Workflow:**

1. **Context Gathering** - AskUserQuestion으로 필수 정보 수집
   - Study type (diagnostic_accuracy, prognostic, segmentation_ai, etc.)
   - Target journal
   - Key findings and numbers to include

2. **Draft Generation** - Claude native
   - Load section template from `references/section-templates.md`
   - Load journal profile from `references/journal-profiles.md`
   - Generate draft following template and journal style

3. **Optional Dual Review** - Ask user if they want immediate review
   - If yes, run /radiology-review workflow (Codex technical + Claude native style)

4. **Save Output** - Save draft and review results to `output/{section}_{timestamp}/`
   - Save `review_result.json` and `review_report.md` (if dual review was executed)
   - Save `codex_raw.json` (raw GPT-5.4 response, if available)

**Example:**

```
/radiology-draft Methods en

> Study type: diagnostic_accuracy
> Target journal: Radiology
> Key findings: AUC 0.92, sensitivity 92.5%, specificity 92.2%
```

### /radiology-intro [topic]

문헌 기반 Introduction 섹션을 2단계 파이프라인으로 작성합니다.

**Workflow:**

1. **Context Gathering** - AskUserQuestion으로 필수 정보 수집
   - Research topic (연구 주제)
   - Keywords (핵심 키워드 목록)
   - Study type (diagnostic_accuracy, prognostic, segmentation_ai, etc.)
   - Target journal
   - Language (en/ko)

2. **Stage 1 — Codex Literature Analysis** (GPT-5.4, reasoning: high)

   ```bash
   codex exec -m gpt-5.4 --sandbox read-only --config model_reasoning_effort=high \
     "[Introduction Literature Analysis prompt from references/codex-prompts.md]"
   ```

   Output (strict JSON):
   - `prior_work`: list of landmark studies with `citation_hint`, `study_type`, `key_finding`, `sample_size`, `relevance`
   - `citation_hints`: each with `suggested_query`, `expected_domains`, `rationale`
   - `research_gap_analysis`: `specific_gaps`, `how_study_addresses_gap`, `novelty`, `expected_contribution`, `clinical_translation`
   - `suggested_introduction_structure`: paragraph-level drafts
   - `source: "gpt-5.4"`

3. **Stage 2 — Claude URL Verification** (Claude WebSearch + WebFetch)

   For each `citation_hints[i]`:

   - Execute WebSearch with `suggested_query`.
   - Filter results against `expected_domains` when provided.
   - Execute WebFetch on each candidate URL to confirm the page exists and matches the claimed topic.
   - Attach `verified_url`, `verified_title`, `verified_at` to the corresponding citation hint.

   **Stage 2 failure handling (OQ-1 default):** If WebSearch or WebFetch fails for any reason (network, rate limit, tool unavailable, all candidates rejected), return the unmodified Stage 1 JSON plus the top-level flag `stage2_verification_failed: true`. Do NOT raise an error; the user still gets a usable Introduction draft with citation hints.

4. **Introduction Draft Generation** - Claude synthesizes Stage 1 + Stage 2 output
   - Generate 3-paragraph structure:
     - P1: Clinical Context (임상적 배경)
     - P2: Gap/Limitation (기존 연구 한계)
     - P3: Objective (연구 목적)
   - Integrate verified citations where available; fall back to citation hints (unverified) when Stage 2 failed.

5. **Optional Dual Review** - Ask user if they want immediate review
   - Codex (GPT-5.4): Clinical relevance, gap clarity, objective alignment
   - Claude native: Flow, readability, citation integration

6. **Save Output** - Save results to `output/introduction_{timestamp}/`
   - Save `review_result.json` and `review_report.md` (if dual review was executed)
   - Save `codex_raw.json` when available
   - Save `stage2_verification.json` with per-hint verification status

**Example:**

```
/radiology-intro "Deep learning for liver metastasis detection on CT"

> Keywords: liver metastasis, CT, deep learning, diagnostic accuracy
> Study type: diagnostic_accuracy
> Target journal: Radiology
> Language: en

Stage 1 (Codex GPT-5.4, reasoning: high): Literature analysis, gap identification, citation hints
Stage 2 (Claude WebSearch + WebFetch): URL verification against suggested queries and expected domains
Draft synthesis (Claude): 3-paragraph Introduction with verified citations
```

### /radiology-review [section]

기존 텍스트를 멀티모델로 리뷰합니다. 3-스텝 순차 파이프라인입니다.

**Workflow:**

1. **Parse Input** - draft_text from context or user input.

2. **Determine Checklist** - Based on study_type:
   - diagnostic_accuracy → STARD 2015 + STARD-AI (if AI)
   - prognostic → TRIPOD + TRIPOD+AI (if AI)
   - segmentation_ai → CLAIM 2024
   - llm_study → MI-CLEAR-LLM + TRIPOD-LLM + DEAL

3. **Step 1 — Codex Technical Review** (GPT-5.4, reasoning: high)

   ```bash
   codex exec -m gpt-5.4 --sandbox read-only --config model_reasoning_effort=high \
     "[technical prompt from references/codex-prompts.md]"
   ```

   Emits technical `major_issues` with `source: "gpt-5.4"` and checklist compliance.

4. **Step 2 — Claude Native Style Review**

   Claude loads the "Style Guidance for Claude Native Review" header from `references/codex-prompts.md` and applies it directly to the draft. No external CLI is invoked.

   Emits `clarity_issues`, `terminology_and_consistency`, and `candidate_rewrites` with `source: "claude"` (item-level for issue lists; rewrites omit `source` because they are authorial).

5. **Step 3 — Unified JSON Merge**

   Claude merges Step 1 and Step 2 outputs into the unified JSON schema (see Output Structure below). The `source` field on each issue is preserved per-item; no combined values such as `"claude+gpt-5.4"` are produced (OQ-2 default).

6. **Save Output** - Save all results to `output/{section}_{timestamp}/`
   - Save `review_result.json` (unified structured output)
   - Generate and save `review_report.md` (human-readable Markdown report)
   - Save `codex_raw.json` (raw GPT-5.4 response, if available)
   - Save `claude_style_raw.json` (raw Claude native style review output)

**Example:**

```
/radiology-review Methods

[User provides or Claude reads the Methods section text]

Step 1: Codex GPT-5.4 (reasoning: high) — methodological rigor, statistics, bias, checklist compliance
Step 2: Claude native — clarity, structure, readability, terminology consistency, candidate rewrites
Step 3: Unified JSON merge and review_report.md generation
```

### /radiology-revise

피드백을 반영하여 수정합니다. Claude-only 워크플로입니다.

**Workflow:**

1. Load current draft and accumulated feedback
2. Analyze feedback and prioritize:
   - Critical issues first (methodology, statistics)
   - Then clarity/style issues
   - Then minor polish
3. Generate revised draft
4. Optional: Re-run targeted /radiology-review on changed sections (Codex + Claude native)
5. Track revision history
6. **Save Output** - Save revised draft and any re-review results to `output/{section}_{timestamp}/`

---

## Section-Specific Review Criteria

### Abstract

| GPT-5.4 (Technical) | Claude (Style) |
|---------------------|----------------|
| Structure: 목적-방법-결과-결론 | Sentence brevity (< 25 words) |
| Number consistency with Results | Information density |
| Overclaim detection | Clinical readability |
| Primary endpoint clarity | Word count compliance |
| Statistical formatting | Active voice preference |

### Introduction

| GPT-5.4 (Technical) | Claude (Style) |
|---------------------|----------------|
| Clinical relevance established | General → Specific flow |
| Literature balance (not cherry-picked) | Paragraph unity |
| Gap clarity and significance | Transition sentences |
| Gap-objective alignment | Citation integration |
| Overclaim detection | Readability |
| Scope appropriateness | Objective statement clarity |

### Methods

| GPT-5.4 (Technical) | Claude (Style) |
|---------------------|----------------|
| Patient selection bias | Paragraph organization |
| Spectrum bias | Subsection structure |
| Imaging protocol completeness | Repetition removal |
| Reference standard adequacy | Verb tense consistency |
| Sample size justification | Readability score |
| Multiple comparison correction | |
| Inter-reader agreement | |

### Results

| GPT-5.4 (Technical) | Claude (Style) |
|---------------------|----------------|
| Table/figure vs text consistency | One message per paragraph |
| Primary outcome prominence | Key message first |
| Statistical formatting (OR, CI, p) | Figure reference order |
| Subgroup analysis labeling | Avoid interpretation |

### Discussion

| GPT-5.4 (Technical) | Claude (Style) |
|---------------------|----------------|
| Interpretation validity | Logical flow |
| Novelty vs literature context | Transition sentences |
| Limitation adequacy | Avoid result repetition |
| Overclaim prevention | Future tense for implications |

---

## Output Saving

Review results are automatically saved in dual format (JSON + Markdown) to the `output/` directory.

### Output Directory Structure

```
output/
├── {section}_{timestamp}/
│   ├── review_result.json         # Structured JSON output
│   ├── review_report.md           # Human-readable Markdown report
│   ├── codex_raw.json             # Raw GPT-5.4 response (if available)
│   ├── claude_style_raw.json      # Raw Claude native style review output
│   └── stage2_verification.json   # (introduction only) per-citation-hint URL verification status
```

- `{section}` = section type in lowercase (e.g., methods, abstract, discussion, introduction)
- `{timestamp}` = YYYYMMDD_HHMMSS format (e.g., 20260423_143022)

### review_result.json

Contains the unified structured JSON output (see Output Structure below).

### review_report.md

A human-readable Markdown report with the following sections:

1. **Header** - Section type, study type, target journal, review date
2. **Major Issues** - From `major_issues`, sorted by severity (critical > major > minor), with source attribution (`claude` or `gpt-5.4`)
3. **Clarity Issues** - From `clarity_issues`, with suggestions (source: `claude`)
4. **Terminology & Consistency** - From `terminology_and_consistency` (source: `claude`)
5. **Candidate Rewrites** - Original vs revised text with rationale (diff-style presentation)
6. **Checklist Compliance** - Compliant/missing/incomplete items with compliance rate
7. **Summary Statistics** - Total issues by severity, compliance rate percentage

### Raw Response Files

- `codex_raw.json` - Raw GPT-5.4 response preserved for traceability (saved only when GPT-5.4 review succeeds)
- `claude_style_raw.json` - Raw output of Claude native style review
- `stage2_verification.json` - (introduction only) list of citation hints with verified URLs, titles, and timestamps, or `stage2_verification_failed: true` at top level when verification could not complete

### Output Directory Convention

- All output directories are created under `output/` in the project root
- Each review session creates a new timestamped directory
- Previous results are never overwritten
- Directory naming: `{section}_{YYYYMMDD_HHMMSS}` (e.g., `methods_20260423_143022`)

---

## Output Structure

Review results are presented in this unified format and saved to `review_result.json`. **All JSON keys are preserved verbatim from the previous version** (backward-compatible for downstream consumers). Only the `source` field value set changes: allowed values are `"claude"` and `"gpt-5.4"`.

```json
{
  "major_issues": [
    {
      "type": "methodology|statistics|consistency",
      "location": "paragraph/sentence reference",
      "description": "issue description",
      "severity": "critical|major|minor",
      "source": "gpt-5.4"
    }
  ],
  "clarity_issues": [
    {
      "type": "readability|structure|flow",
      "location": "paragraph/sentence reference",
      "description": "issue description",
      "suggestion": "improvement suggestion",
      "source": "claude"
    }
  ],
  "terminology_and_consistency": [
    {
      "term": "inconsistent term",
      "locations": ["paragraph 1", "paragraph 3"],
      "canonical_form": "recommended single form",
      "source": "claude"
    }
  ],
  "candidate_rewrites": [
    {
      "location": "paragraph/sentence",
      "original": "original text",
      "revised": "suggested revision",
      "rationale": "why this change"
    }
  ],
  "checklist": {
    "type": "STARD|TRIPOD|CLAIM|MI-CLEAR-LLM",
    "compliant": ["item1", "item2"],
    "missing": ["item3"],
    "incomplete": ["item4"]
  }
}
```

Allowed `source` values at item level: `"claude"`, `"gpt-5.4"`. No combined values; each module emits a single source per item (OQ-2).

---

## CLI Command Reference

### GPT-5.4 Execution (via Codex CLI)

```bash
# Technical or methodological review (read-only sandbox, high reasoning)
codex exec -m gpt-5.4 --sandbox read-only --config model_reasoning_effort=high "[prompt]"

# Literature analysis for Introduction (Stage 1 of /radiology-intro)
codex exec -m gpt-5.4 --sandbox read-only --config model_reasoning_effort=high "[literature prompt]"

# Follow-up in same session
codex exec resume --last "[follow-up prompt]"
```

### Claude Native Operations (no external CLI)

- Style review: Claude reads the "Style Guidance for Claude Native Review" header from `references/codex-prompts.md` and applies criteria directly to the draft.
- URL verification (/radiology-intro Stage 2): Claude uses the WebSearch and WebFetch tools sequentially; no shell invocation.
- Draft generation, revision, feedback integration: native Claude reasoning.

---

## Error Handling

| Scenario | Action |
|----------|--------|
| Codex invocation failure (timeout, exit nonzero, empty response, invalid JSON) | Fallback to Claude-only review with explicit notice in output JSON (`"fallback": "claude_only"`, `"codex_error": "<message>"`). Preserve all unified JSON keys; `major_issues` populated by Claude with `source: "claude"`. Retry once before falling back. |
| Claude native style review produces invalid JSON | Retry internal synthesis once; on repeat failure, present text feedback verbatim in `review_report.md` and set `clarity_issues: []` in JSON. |
| Stage 2 URL verification failure (WebSearch or WebFetch) | Return Stage 1 results unchanged plus `stage2_verification_failed: true`. Do NOT error out (OQ-1). |
| Empty input draft | Prompt user for the draft before running any reviewer. |

**No fallback path uses any non-Claude, non-Codex model.** The system operates strictly on 3 models: Claude Code (Main Editor + Style Reviewer), Codex GPT-5.4 Technical Reviewer, Codex GPT-5.4 Literature Researcher.

---

## Language Support

### English (default)

- All prompts in English
- Checklists in English
- Feedback in English

### Korean (language="ko")

- Codex (GPT-5.4) prompts: Keep English (technical consistency)
- Claude native style review: Apply Korean-to-English Guidelines section from `references/codex-prompts.md` "Style Guidance for Claude Native Review"
- Output feedback: Present in Korean when requested
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

- `references/radiology-checklists.md` - STARD, TRIPOD, CLAIM, MI-CLEAR-LLM, TRIPOD-LLM, DEAL, CHART checklists
- `references/section-templates.md` - Section-specific review criteria details
- `references/journal-profiles.md` - Journal requirements and formatting
- `references/codex-prompts.md` - GPT-5.4 review prompts by section, Introduction Literature Analysis prompt, and Style Guidance for Claude Native Review

---

## Migration Notes

This skill transitioned to the current 3-model pipeline (Claude Main Editor + Style Reviewer, Codex GPT-5.4 Technical Reviewer, Codex GPT-5.4 Literature Researcher) on 2026-04-23. Historical details about the prior architecture are recorded only in `IMPLEMENTATION_PLAN.md`. Key guarantees of this transition:

- Style review is now Claude-native. Criteria live in `references/codex-prompts.md` under the "Style Guidance for Claude Native Review" header (medical writing criteria, Korean-to-English guidelines, passive voice handling, terminology consistency).
- Literature research is now performed by Codex GPT-5.4 with `model_reasoning_effort=high` using the "Introduction Literature Analysis" prompt in `references/codex-prompts.md`.
- `/radiology-intro` now has a Stage 2 in which Claude uses WebSearch and WebFetch to verify citation URLs produced by Stage 1. On Stage 2 failure, Stage 1 results are returned with `stage2_verification_failed: true`.
- Output JSON schema keys are unchanged (`major_issues`, `clarity_issues`, `terminology_and_consistency`, `candidate_rewrites`, `checklist`). The `source` field takes `"claude"` or `"gpt-5.4"` only; no other values are emitted.
- Command parameter signatures (`/radiology-draft`, `/radiology-intro`, `/radiology-review`, `/radiology-revise`) are unchanged.
