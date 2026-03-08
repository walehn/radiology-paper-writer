# Radiology Paper Writer

> AI-assisted radiology research paper writing with multi-model review system

영상의학 연구 논문 작성 및 교정을 위한 Claude Code 스킬입니다. 네 가지 AI 모델이 협업하여 학술 논문의 문헌 리서치, 초안 작성, 방법론 검토, 문체 교정까지 체계적으로 지원합니다.

---

## Overview

### The Problem

영상의학 연구 논문 작성 시 흔히 겪는 어려움:
- STARD, TRIPOD, CLAIM 등 복잡한 체크리스트 준수
- 통계 수치의 일관성 유지 (Abstract ↔ Results ↔ Tables)
- 방법론적 엄밀성과 가독성의 균형
- 저널별 다른 포맷 요구사항

### The Solution

**Multi-Model System**: 각 모델이 전문 분야에 집중

```
┌─────────────────────────────────────────────────────────────┐
│                    Claude Code (Main Editor)                 │
│         초안 작성 · 피드백 통합 · 수정 · 버전 관리            │
└─────────────────────────────────────────────────────────────┘
                              │
       ┌──────────────────────┼──────────────────────┐
       ▼                      ▼                      ▼
┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐
│ GPT-5.4          │  │ Gemini 3.1 Pro   │  │ Gemini 3.1 Pro   │
│ (reasoning: high)│  │ Style Reviewer   │  │ Lit. Researcher  │
├──────────────────┤  ├──────────────────┤  ├──────────────────┤
│ • 연구 설계 검증 │  │ • 문장 자연스러움│  │ • 문헌 검토      │
│ • 통계 분석      │  │ • 가독성 향상    │  │ • Gap 분석       │
│ • 논리적 일관성  │  │ • 저널 스타일    │  │ • 인용 제안      │
│ • 방법론 체크    │  │ • Clarity 개선   │  │ • 배경 연구      │
└──────────────────┘  └──────────────────┘  └──────────────────┘
```

---

## Installation

### Prerequisites

- **Node.js** (v18+) 및 **npm** — Codex CLI, Gemini CLI 설치에 필요

세 가지 CLI 도구를 모두 설치하고 인증을 완료해야 합니다:

| CLI | 설치 | 인증 방법 |
|-----|------|----------|
| [Claude Code](https://claude.ai/claude-code) | `curl -fsSL https://claude.ai/install.sh \| bash` (macOS/Linux/WSL) | 구독 (Pro/Max) 또는 API 키 (`ANTHROPIC_API_KEY`) |
| [Codex CLI](https://github.com/openai/codex) | `npm install -g @openai/codex` | OAuth 로그인 (`codex auth`) 또는 API 키 (`OPENAI_API_KEY`) |
| [Gemini CLI](https://github.com/google/gemini-cli) | `npm install -g @google/gemini-cli` | Google OAuth (`gemini auth`) 또는 API 키 (`GEMINI_API_KEY`) |

> **Note:** Claude Code Windows 설치: PowerShell `irm https://claude.ai/install.ps1 | iex` / CMD `curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd`. Windows의 경우 [Git for Windows](https://gitforwindows.org/) 설치가 필요합니다.

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
- **Introduction** - 배경 및 목적
- **Methods** - 방법론 (체크리스트 기반 검증)
- **Results** - 결과 제시
- **Discussion** - 고찰 및 제한점
- **Conclusion** - 결론
- **Cover Letter** - 투고 커버레터

### Key Capabilities

- **Literature Research**: Gemini 3.1 Pro를 활용한 자동 문헌 검토 및 gap 분석
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

### `/radiology-intro [topic]`

문헌 기반 Introduction 섹션을 작성합니다.

```
/radiology-intro "Deep learning for liver metastasis detection on CT"

> Keywords: liver metastasis, CT, deep learning, diagnostic accuracy
> Study type: diagnostic_accuracy
> Target journal: Radiology
```

**Literature Research Workflow:**
```
Research Topic + Keywords
          │
          ▼
┌─────────────────────────┐
│ Gemini 3.1 Pro          │
│ Literature Analysis     │
├─────────────────────────┤
│ • Clinical context      │
│ • Key literature        │
│ • Knowledge gaps        │
│ • Citation suggestions  │
└─────────────────────────┘
          │
          ▼
┌─────────────────────────┐
│ Claude                  │
│ Introduction Draft      │
├─────────────────────────┤
│ P1: Clinical Context    │
│ P2: Gap/Limitation      │
│ P3: Objective           │
└─────────────────────────┘
          │
          ▼
┌─────────────────────────┐
│ Dual Review (Optional)  │
├─────────────────────────┤
│ GPT-5.4: Gap alignment  │
│ Gemini: Flow, clarity   │
└─────────────────────────┘
```

### `/radiology-review [section]`

기존 텍스트를 멀티모델로 리뷰합니다.

```
/radiology-review Methods

[Methods 섹션 텍스트 제공]
```

**Parallel Review Process:**
```
GPT-5.4 (reasoning: high)          Gemini 3.1 Pro
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

리뷰 결과는 구조화된 JSON으로 제공되며, `output/{section}_{timestamp}/` 디렉토리에 JSON과 Markdown 두 가지 형식으로 자동 저장됩니다 (자세한 내용은 [Output Files](#output-files) 참조):

```json
{
  "major_issues": [
    {
      "type": "methodology",
      "location": "Patient Selection, paragraph 2",
      "description": "Eligibility criteria not explicitly stated",
      "severity": "critical",
      "source": "gpt-5.4"
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

## Output Files

리뷰 결과는 자동으로 `output/` 디렉토리에 JSON과 Markdown 두 가지 형식으로 저장됩니다.

### 출력 디렉토리 구조

```
output/
├── methods_20260308_143022/
│   ├── review_result.json       # 구조화된 JSON 리뷰 결과
│   ├── review_report.md         # 사람이 읽기 쉬운 Markdown 리포트
│   ├── codex_raw.json           # GPT-5.4 원본 응답 (성공 시)
│   └── gemini_raw.json          # Gemini 원본 응답 (성공 시)
├── abstract_20260309_091500/
│   ├── review_result.json
│   ├── review_report.md
│   └── ...
```

### 파일 설명

| 파일 | 형식 | 설명 |
|------|------|------|
| `review_result.json` | JSON | 통합된 리뷰 결과 (major_issues, clarity_issues, checklist 등) |
| `review_report.md` | Markdown | 헤더, 이슈 요약, 수정 제안, 체크리스트 현황 등 포함된 리포트 |
| `codex_raw.json` | JSON | GPT-5.4의 원본 응답 (추적 및 디버깅용) |
| `gemini_raw.json` | JSON | Gemini의 원본 응답 (추적 및 디버깅용) |

### review_report.md 구성

생성되는 Markdown 리포트는 다음 섹션으로 구성됩니다:

1. **Header** - 섹션 유형, 연구 유형, 타깃 저널, 리뷰 일시
2. **Major Issues** - 심각도순 정렬 (critical > major > minor), 모델 출처 표시
3. **Clarity Issues** - 가독성/구조/흐름 이슈 및 개선 제안
4. **Terminology & Consistency** - 용어 및 일관성 이슈
5. **Candidate Rewrites** - 원문 vs 수정안 비교 (근거 포함)
6. **Checklist Compliance** - 준수/누락/불완전 항목 및 준수율
7. **Summary Statistics** - 심각도별 이슈 수, 체크리스트 준수율 (%)

### 디렉토리 규칙

- 모든 출력은 프로젝트 루트의 `output/` 디렉토리 아래에 생성됩니다
- 각 리뷰 세션마다 새로운 타임스탬프 디렉토리가 생성됩니다
- 이전 결과는 덮어쓰지 않습니다
- 디렉토리 이름: `{섹션}_{YYYYMMDD_HHMMSS}` (예: `methods_20260308_143022`)

---

## Section-Specific Review Criteria

### Methods Section Example

| GPT-5.4 (Technical) | Gemini (Style) |
|---------------------|----------------|
| Patient selection bias | Paragraph organization |
| Spectrum bias | Subsection structure |
| Imaging protocol completeness | Repetition removal |
| Reference standard adequacy | Verb tense consistency |
| Sample size justification | Readability score |
| Multiple comparison correction | |
| Inter-reader agreement | |

---

## Technical Details

### Model Configuration

| Model | CLI | Parameters | Purpose |
|-------|-----|------------|---------|
| GPT-5.4 | Codex | `--sandbox read-only --config model_reasoning_effort=high` | Technical Review |
| Gemini 3.1 Pro | Gemini | `--approval-mode yolo` (auto-approve) | Style Review |
| Gemini 3.1 Pro | Gemini | `-m gemini-3.1-pro-preview --approval-mode yolo` | Literature Research |

### Execution Commands

```bash
# GPT-5.4 Technical Review
codex exec -m gpt-5.4 --sandbox read-only --config model_reasoning_effort=high "[prompt]"

# Gemini Style Review
gemini --approval-mode yolo "[prompt]"

# Gemini 3.1 Pro Literature Research
gemini -m gemini-3.1-pro-preview --approval-mode yolo "[research prompt]"
```

### Error Handling

| Scenario | Action |
|----------|--------|
| GPT-5.4 timeout/failure | Retry once, then Claude-only review |
| Gemini error | Continue with GPT-5.4 review only |
| Both CLI failures | Claude provides basic review |
| Invalid JSON response | Extract text feedback, present as unstructured |

---

## Language Support

### English (default)
- All prompts and feedback in English
- Checklists in English

### Korean (`language="ko"`)
- GPT-5.4 prompts: English (technical consistency)
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
├── references/
│   ├── radiology-checklists.md       # STARD, TRIPOD, CLAIM checklists
│   ├── section-templates.md          # Section-specific templates
│   ├── journal-profiles.md           # Journal requirements
│   ├── codex-prompts.md              # GPT-5.4 review prompts
│   └── gemini-prompts.md             # Gemini review prompts
└── output/                           # Auto-generated review outputs
    ├── methods_20260308_143022/
    │   ├── review_result.json
    │   ├── review_report.md
    │   ├── codex_raw.json
    │   └── gemini_raw.json
    └── abstract_20260309_091500/
        └── ...
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
