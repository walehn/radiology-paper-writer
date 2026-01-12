# Radiology Paper Writer

> AI-assisted radiology research paper writing with multi-model review system

영상의학 연구 논문 작성 및 교정을 위한 Claude Code 스킬입니다. 세 가지 AI 모델이 협업하여 학술 논문의 초안 작성부터 방법론 검토, 문체 교정까지 체계적으로 지원합니다.

---

## Overview

### The Problem

영상의학 연구 논문 작성 시 흔히 겪는 어려움:
- STARD, TRIPOD, CLAIM 등 복잡한 체크리스트 준수
- 통계 수치의 일관성 유지 (Abstract ↔ Results ↔ Tables)
- 방법론적 엄밀성과 가독성의 균형
- 저널별 다른 포맷 요구사항

### The Solution

**Multi-Model Review System**: 각 모델이 전문 분야에 집중

```
┌─────────────────────────────────────────────────────────────┐
│                    Claude Code (Main Editor)                 │
│         초안 작성 · 피드백 통합 · 수정 · 버전 관리            │
└─────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┴───────────────┐
              ▼                               ▼
┌─────────────────────────┐     ┌─────────────────────────┐
│  GPT-5.2 (reasoning: high)  │     │    Gemini 3.0 Pro       │
│    Technical Reviewer    │     │     Style Reviewer      │
├─────────────────────────┤     ├─────────────────────────┤
│ • 연구 설계 검증         │     │ • 문장 자연스러움        │
│ • 통계 분석 적절성       │     │ • 가독성 향상           │
│ • 논리적 일관성          │     │ • 저널 스타일 준수       │
│ • 방법론 체크리스트      │     │ • Clarity 개선          │
└─────────────────────────┘     └─────────────────────────┘
```

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

### Supported Sections

- **Title** - 제목 최적화
- **Abstract** - 구조화된 초록 (Purpose/Methods/Results/Conclusion)
- **Introduction** - 배경 및 목적
- **Methods** - 방법론 (체크리스트 기반 검증)
- **Results** - 결과 제시
- **Discussion** - 고찰 및 제한점
- **Conclusion** - 결론
- **Cover Letter** - 투고 커버레터

### Key Capabilities

- **Checklist Compliance**: STARD, TRIPOD, CLAIM 자동 검증
- **Number Consistency**: Abstract-Results-Tables 수치 일치 확인
- **Overclaim Detection**: "first", "novel", "superior" 등 과장 표현 감지
- **Statistical Formatting**: 저널별 통계 표기 형식 검증
- **Bilingual Support**: 영어/한국어 지원

---

## Commands

### `/radiology-draft [section]`

논문 섹션 초안을 작성합니다.

```
/radiology-draft Methods

> Study type: diagnostic_accuracy
> Target journal: Radiology
> Key findings: AUC 0.92, sensitivity 92.5%, specificity 92.2%
```

**Workflow:**
1. 필수 정보 수집 (study type, journal, key findings)
2. 섹션 템플릿 및 저널 프로파일 로드
3. 초안 생성
4. (선택) 즉시 리뷰 실행

### `/radiology-review [section]`

기존 텍스트를 멀티모델로 리뷰합니다.

```
/radiology-review Methods

[Methods 섹션 텍스트 제공]
```

**Parallel Review Process:**
```
GPT-5.2 (reasoning: high)          Gemini 3.0 Pro
        │                                │
        ▼                                ▼
  Technical Issues                 Style Issues
  - Patient selection bias         - Paragraph organization
  - Reference standard adequacy    - Verb tense consistency
  - Statistical rigor              - Readability score
        │                                │
        └────────────┬───────────────────┘
                     ▼
            Unified Feedback JSON
```

### `/radiology-revise`

리뷰 피드백을 반영하여 수정합니다.

**Priority Order:**
1. Critical methodology issues
2. Statistical/consistency issues
3. Clarity/style improvements
4. Minor polish

---

## Output Format

리뷰 결과는 구조화된 JSON으로 제공됩니다:

```json
{
  "major_issues": [
    {
      "type": "methodology",
      "location": "Patient Selection, paragraph 2",
      "description": "Eligibility criteria not explicitly stated",
      "severity": "critical",
      "source": "gpt-5.2"
    }
  ],
  "clarity_issues": [
    {
      "type": "readability",
      "location": "Statistical Analysis, sentence 3",
      "suggestion": "Break into two sentences for clarity",
      "source": "gemini"
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

| GPT-5.2 (Technical) | Gemini (Style) |
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
- [Codex CLI](https://github.com/openai/codex) 설치 및 OpenAI API 키 설정
- [Gemini CLI](https://github.com/google/gemini-cli) 설치 및 Google API 키 설정

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
# /radiology-draft, /radiology-review, /radiology-revise 명령어 사용 가능
```

---

## Technical Details

### Model Configuration

| Model | CLI | Parameters |
|-------|-----|------------|
| GPT-5.2 | Codex | `--sandbox read-only --reasoning high` |
| Gemini 3.0 Pro | Gemini | `-y` (auto-approve) |

### Execution Commands

```bash
# GPT-5.2 Technical Review
codex exec -m gpt-5.2 --sandbox read-only --reasoning high "[prompt]"

# Gemini Style Review
gemini -y "[prompt]"
```

### Error Handling

| Scenario | Action |
|----------|--------|
| GPT-5.2 timeout/failure | Retry once, then Claude-only review |
| Gemini error | Continue with GPT-5.2 review only |
| Both CLI failures | Claude provides basic review |
| Invalid JSON response | Extract text feedback, present as unstructured |

---

## Language Support

### English (default)
- All prompts and feedback in English
- Checklists in English

### Korean (`language="ko"`)
- GPT-5.2 prompts: English (technical consistency)
- Gemini prompts: Korean style guidelines added
- Output feedback: Korean
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
├── IMPLEMENTATION_PLAN.md            # Development roadmap
└── references/
    ├── radiology-checklists.md       # STARD, TRIPOD, CLAIM checklists
    ├── section-templates.md          # Section-specific templates
    ├── journal-profiles.md           # Journal requirements
    ├── codex-prompts.md              # GPT-5.2 review prompts
    └── gemini-prompts.md             # Gemini review prompts
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
