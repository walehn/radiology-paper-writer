# Radiology Paper Writer

> AI-assisted radiology research paper writing with a 3-model review system

영상의학 연구 논문 작성 및 교정을 위한 Claude Code 스킬입니다. 세 가지 AI 역할이 협업하여 학술 논문의 문헌 리서치, 초안 작성, 방법론 검토, 문체 교정까지 체계적으로 지원합니다.

---

## Overview

### The Problem

영상의학 연구 논문 작성 시 흔히 겪는 어려움:
- STARD, TRIPOD, CLAIM, MI-CLEAR-LLM 등 복잡한 체크리스트 준수
- 통계 수치의 일관성 유지 (Abstract ↔ Results ↔ Tables)
- 방법론적 엄밀성과 가독성의 균형
- 저널별 다른 포맷 요구사항

### The Solution

**3-Model System**: 각 역할이 전문 분야에 집중.

```
┌───────────────────────────────────────────────────────────────────┐
│           Claude Code  (Main Editor + Style Reviewer)             │
│  Draft · Style review (native) · URL verify (WebSearch/Fetch)     │
│             Feedback integration · Revision · History             │
└───────────────────────────────────────────────────────────────────┘
                                 │
                 ┌───────────────┴────────────────┐
                 ▼                                ▼
     ┌────────────────────────┐      ┌────────────────────────┐
     │ Codex GPT-5.5          │      │ Codex GPT-5.5          │
     │ (reasoning: xhigh)      │      │ (reasoning: xhigh)      │
     │ Technical Reviewer     │      │ Literature Researcher  │
     ├────────────────────────┤      ├────────────────────────┤
     │ • Study design         │      │ • Prior work synthesis │
     │ • Statistical rigor    │      │ • Citation hints       │
     │ • Methodological bias  │      │ • Expected domains     │
     │ • Checklist compliance │      │ • Research gap analysis│
     └────────────────────────┘      └────────────────────────┘
```

Only three roles exist. Style review runs inside Claude; literature research runs via Codex and is verified by Claude WebSearch and WebFetch.

---

## Features

### Supported Study Types

| Study Type | Description | Checklist |
|------------|-------------|-----------|
| `diagnostic_accuracy` | 진단 정확도 연구 | STARD 2015 + STARD-AI |
| `prognostic` | 예후/예측 모델 연구 | TRIPOD + TRIPOD+AI |
| `segmentation_ai` | AI 분할 연구 | CLAIM 2024 |
| `screening` | 스크리닝 연구 | STARD + screening extensions |
| `interventional` | 중재 영상의학 연구 | Custom criteria |
| `llm_study` | LLM 활용 연구 | MI-CLEAR-LLM + TRIPOD-LLM + DEAL |

### Supported Sections

- **Title** - 제목 최적화
- **Abstract** - 구조화된 초록 (Purpose/Methods/Results/Conclusion)
- **Introduction** - 배경 및 목적 (2-stage literature pipeline)
- **Methods** - 방법론 (체크리스트 기반 검증)
- **Results** - 결과 제시
- **Discussion** - 고찰 및 제한점
- **Conclusion** - 결론
- **Cover Letter** - 투고 커버레터

### Key Capabilities

- **Literature Research**: Codex GPT-5.5 (reasoning: xhigh) 기반 구조화된 문헌 분석 + Claude WebSearch/WebFetch URL 검증
- **Checklist Compliance**: STARD, TRIPOD, CLAIM, MI-CLEAR-LLM 자동 검증
- **Number Consistency**: Abstract-Results-Tables 수치 일치 확인
- **Overclaim Detection**: "first", "novel", "superior" 등 과장 표현 감지
- **Statistical Formatting**: 저널별 통계 표기 형식 검증
- **Bilingual Support**: 영어/한국어 지원 (한국어 → 영어 가이드라인 내장)

---

## Commands

### `/radiology-draft [section]`

논문 섹션 초안을 작성합니다 (Claude-only).

```
/radiology-draft Methods

> Study type: diagnostic_accuracy
> Target journal: Radiology
> Key findings: AUC 0.92, sensitivity 92.5%, specificity 92.2%
```

**Workflow:**
1. 필수 정보 수집 (study type, journal, key findings)
2. 섹션 템플릿 및 저널 프로파일 로드
3. 초안 생성 (Claude native)
4. (선택) 즉시 리뷰 실행 → /radiology-review 호출

### `/radiology-intro [topic]`

문헌 기반 Introduction 섹션을 2단계 파이프라인으로 작성합니다.

```
/radiology-intro "Deep learning for liver metastasis detection on CT"

> Keywords: liver metastasis, CT, deep learning, diagnostic accuracy
> Study type: diagnostic_accuracy
> Target journal: Radiology
```

**Literature Pipeline:**

```
Research Topic + Keywords
          │
          ▼
┌─────────────────────────────┐
│ Stage 1 — Codex GPT-5.5     │
│ Literature Analysis         │
│ (reasoning: xhigh)           │
├─────────────────────────────┤
│ • prior_work                │
│ • citation_hints            │
│   (suggested_query,         │
│    expected_domains)        │
│ • research_gap_analysis     │
│ • suggested structure       │
└─────────────────────────────┘
          │
          ▼
┌─────────────────────────────┐
│ Stage 2 — Claude            │
│ WebSearch + WebFetch        │
├─────────────────────────────┤
│ For each citation_hint:     │
│  • WebSearch(query)         │
│  • Filter by domain         │
│  • WebFetch verification    │
│  • Attach verified_url      │
│ On failure:                 │
│  stage2_verification_failed │
│  = true  (return Stage 1)   │
└─────────────────────────────┘
          │
          ▼
┌─────────────────────────────┐
│ Claude Introduction Draft   │
├─────────────────────────────┤
│ P1: Clinical Context        │
│ P2: Gap/Limitation          │
│ P3: Objective               │
└─────────────────────────────┘
          │
          ▼
┌─────────────────────────────┐
│ Dual Review (Optional)      │
├─────────────────────────────┤
│ Codex GPT-5.5: Gap align.   │
│ Claude native: Flow, clarity│
└─────────────────────────────┘
```

### `/radiology-review [section]`

기존 텍스트를 3-step 파이프라인으로 리뷰합니다.

```
/radiology-review Methods

[Methods 섹션 텍스트 제공]
```

**Sequential Review Process:**

```
Step 1: Codex GPT-5.5 Technical
         (reasoning: xhigh)
          │
          ▼
  • Patient selection bias
  • Reference standard adequacy
  • Statistical rigor
  • Checklist compliance
          │
          ▼
Step 2: Claude Native Style
          │
          ▼
  • Paragraph organization
  • Verb tense consistency
  • Readability
  • Terminology consistency
          │
          ▼
Step 3: Unified JSON Merge
          │
          ▼
  review_result.json + review_report.md
```

### `/radiology-revise`

리뷰 피드백을 반영하여 수정합니다 (Claude-only).

**Priority Order:**
1. Critical methodology issues
2. Statistical/consistency issues
3. Clarity/style improvements
4. Minor polish

---

## Output Format

리뷰 결과는 구조화된 JSON으로 제공됩니다. JSON 키는 이전 버전과 완전 호환되며 `source` 필드 값만 `"claude"` 또는 `"gpt-5.5"`로 정정되었습니다.

```json
{
  "major_issues": [
    {
      "type": "methodology",
      "location": "Patient Selection, paragraph 2",
      "description": "Eligibility criteria not explicitly stated",
      "severity": "critical",
      "source": "gpt-5.5"
    }
  ],
  "clarity_issues": [
    {
      "type": "readability",
      "location": "Statistical Analysis, sentence 3",
      "suggestion": "Break into two sentences for clarity",
      "source": "claude"
    }
  ],
  "terminology_and_consistency": [
    {
      "term": "deep learning",
      "locations": ["Methods para 1", "Methods para 4"],
      "canonical_form": "Define DL on first use; use DL thereafter.",
      "source": "claude"
    }
  ],
  "candidate_rewrites": [
    {
      "location": "Methods, Statistical Analysis, sentence 3",
      "original": "...",
      "revised": "...",
      "rationale": "Split for readability"
    }
  ],
  "checklist": {
    "type": "STARD",
    "compliant": ["item1", "item2"],
    "missing": ["item5"],
    "incomplete": ["item7"]
  }
}
```

---

## Section-Specific Review Criteria

### Methods Section Example

| GPT-5.5 (Technical) | Claude (Style) |
|---------------------|----------------|
| Patient selection bias | Paragraph organization |
| Spectrum bias | Subsection structure |
| Imaging protocol completeness | Repetition removal |
| Reference standard adequacy | Verb tense consistency |
| Sample size justification | Readability score |
| Multiple comparison correction | |
| Inter-reader agreement | |

---

## Installation

### Prerequisites

- [Claude Code](https://claude.ai/claude-code) CLI 설치
- [Codex CLI](https://github.com/openai/codex) 설치 및 OpenAI API 키 설정 (GPT-5.5 접근 필요)

No other external CLI is required. Claude's native tools (WebSearch, WebFetch) handle URL verification.

### Setup

1. 스킬 디렉토리 생성:
```bash
mkdir -p ~/.claude/skills/radiology-paper-writer/references
```

2. 스킬 파일 복사:
```bash
# SKILL.md와 references/ 디렉토리를 복사
cp -r radiology-paper-writer/* ~/.claude/skills/radiology-paper-writer/
```

3. Claude Code에서 스킬 확인:
```bash
claude-code
# /radiology-draft, /radiology-intro, /radiology-review, /radiology-revise 명령어 사용 가능
```

---

## Technical Details

### Model Configuration

| Role | CLI | Parameters | Purpose |
|------|-----|------------|---------|
| Claude Code | (native, no CLI) | — | Drafting, style review, URL verification, revision |
| Codex GPT-5.5 Technical | Codex | `--sandbox read-only --config model_reasoning_effort=xhigh` | Methodological review |
| Codex GPT-5.5 Literature | Codex | `--sandbox read-only --config model_reasoning_effort=xhigh` | Introduction literature analysis (Stage 1) |

### Execution Commands

```bash
# Technical review (GPT-5.5, high reasoning)
codex exec -m gpt-5.5 --sandbox read-only --config model_reasoning_effort=xhigh "[prompt]"

# Literature analysis (GPT-5.5, high reasoning, Stage 1 of /radiology-intro)
codex exec -m gpt-5.5 --sandbox read-only --config model_reasoning_effort=xhigh "[literature prompt]"
```

Claude native operations (style review, URL verification, draft generation, revision) do not shell out.

### Error Handling

| Scenario | Action |
|----------|--------|
| Codex invocation failure | Retry once, then fallback to Claude-only review with `"fallback": "claude_only"` notice in output JSON |
| Stage 2 URL verification failure | Return Stage 1 results unchanged + `stage2_verification_failed: true` |
| Invalid JSON response | Extract text feedback, present as unstructured in `review_report.md` |
| Empty input draft | Prompt user before running any reviewer |

---

## Language Support

### English (default)
- All prompts and feedback in English
- Checklists in English

### Korean (`language="ko"`)
- Codex prompts: English (technical consistency)
- Claude native style review: applies Korean-to-English guidelines from the style guidance header
- Output feedback: Korean when requested
- Korean-specific rules:
  - 존댓말 일관성
  - 영어 의학용어 vs 한글 용어 선택
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

## File Structure

```
~/.claude/skills/radiology-paper-writer/
├── SKILL.md                          # Main skill definition
├── README.md                         # This documentation
├── IMPLEMENTATION_PLAN.md            # Development roadmap and changelog
└── references/
    ├── radiology-checklists.md       # STARD, TRIPOD, CLAIM, MI-CLEAR-LLM checklists
    ├── section-templates.md          # Section-specific templates
    ├── journal-profiles.md           # Journal requirements
    └── codex-prompts.md              # GPT-5.5 review prompts + Introduction Literature Analysis + Style Guidance for Claude Native Review
```

---

## Roadmap

- [ ] Additional journal profiles (Korean journals)
- [ ] Citation formatting integration
- [ ] Figure/Table caption generation
- [ ] Reviewer response letter generation
- [ ] Integration with reference managers

---

## Contributing

이 스킬에 대한 피드백이나 개선 제안은 언제든 환영합니다.

---

## License

MIT License

---

## Acknowledgments

- STARD Initiative (Standards for Reporting Diagnostic Accuracy)
- TRIPOD Group (Transparent Reporting of a multivariable prediction model)
- CLAIM Checklist (Checklist for Artificial Intelligence in Medical Imaging)
- MI-CLEAR-LLM (Medical Imaging — Clear Reporting of LLM studies)
