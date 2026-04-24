<h1 align="center">Radiology Paper Writer</h1>

<p align="center">
  <strong>멀티모델 리뷰 시스템 기반 영상의학 연구 논문 작성 AI 도구</strong>
</p>

<p align="center">
  <a href="./README.md">English</a> ·
  <a href="./README.ko.md">한국어</a>
</p>

<p align="center">
  <a href="https://claude.ai/claude-code"><img src="https://img.shields.io/badge/Claude_Code-Skill-blueviolet?style=flat&logo=anthropic" alt="Claude Code"></a>
  <a href="https://github.com/openai/codex"><img src="https://img.shields.io/badge/Codex_CLI-GPT--5.5-412991?style=flat&logo=openai&logoColor=white" alt="Codex CLI"></a>
  <a href="https://github.com/openai/codex"><img src="https://img.shields.io/badge/Reasoning-xhigh-ff6f00?style=flat" alt="Reasoning xhigh"></a>
  <a href="./LICENSE"><img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License: MIT"></a>
</p>

---

## 개요

### 문제점

영상의학 연구 논문 작성 시 흔히 겪는 어려움:
- STARD, TRIPOD, CLAIM 등 복잡한 체크리스트 준수
- 통계 수치의 일관성 유지 (Abstract ↔ Results ↔ Tables)
- 방법론적 엄밀성과 가독성의 균형
- 저널별 다른 포맷 요구사항

### 해결책

**3-모델 시스템**: 각 역할이 전문 분야에 집중합니다. Claude Code가 메인 에디터이자 스타일 리뷰어(네이티브, 외부 CLI 없음)를 겸하고, GPT-5.5(Codex CLI 경유, reasoning: xhigh)가 기술 리뷰와 문헌 리서치 두 레인을 담당합니다.

```
┌─────────────────────────────────────────────────────────────┐
│           Claude Code (메인 에디터 + 스타일 리뷰어)          │
│  초안 작성 · 네이티브 스타일 리뷰 · WebSearch/WebFetch      │
│           피드백 통합 · 수정 · 버전 관리                     │
└─────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┴───────────────┐
              ▼                               ▼
    ┌──────────────────────┐       ┌──────────────────────┐
    │ GPT-5.5 via Codex    │       │ GPT-5.5 via Codex    │
    │ 기술 리뷰어          │       │ 문헌 리서처          │
    │ reasoning: xhigh     │       │ reasoning: xhigh     │
    ├──────────────────────┤       ├──────────────────────┤
    │ • 연구 설계 검증     │       │ • 선행 연구 정리     │
    │ • 통계 분석          │       │ • Gap 분석           │
    │ • 방법론 체크        │       │ • Citation hints     │
    │ • 체크리스트 준수    │       │ • 단락 초안 제안     │
    └──────────────────────┘       └──────────────────────┘
```

네 번째 리뷰어는 존재하지 않습니다. 모든 스타일 리뷰는 Claude 네이티브에서 수행되고, 문헌 리서치는 Codex(GPT-5.5) Stage 1 후 Claude WebSearch + WebFetch Stage 2로 이어집니다.

---

## 설치

### 사전 요구사항

- **Node.js** (v18+) 및 **npm** — Codex CLI 설치에 필요

두 가지 CLI 도구를 설치하고 인증을 완료해야 합니다:

| CLI | 설치 | 인증 방법 |
|-----|------|----------|
| [Claude Code](https://claude.ai/claude-code) | `curl -fsSL https://claude.ai/install.sh \| bash` (macOS/Linux/WSL) | 구독 (Pro/Max) 또는 API 키 (`ANTHROPIC_API_KEY`) |
| [Codex CLI](https://github.com/openai/codex) | `npm install -g @openai/codex` | OAuth 로그인 (`codex login`) 또는 API 키 (`OPENAI_API_KEY`) |

> **Note:** Claude Code Windows 설치: PowerShell `irm https://claude.ai/install.ps1 | iex` / CMD `curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd`. Windows의 경우 [Git for Windows](https://gitforwindows.org/) 설치가 필요합니다.

### 설정

1. 저장소 클론:
```bash
git clone https://github.com/walehn/radiology-paper-writer.git
cd radiology-paper-writer
```

2. 스킬 디렉토리 생성 및 파일 복사:
```bash
mkdir -p ~/.claude/skills/radiology-paper-writer/references
cp SKILL.md IMPLEMENTATION_PLAN.md ~/.claude/skills/radiology-paper-writer/
cp references/*.md ~/.claude/skills/radiology-paper-writer/references/
```

3. Claude Code에서 스킬 확인:
```bash
claude
# /radiology-draft, /radiology-intro, /radiology-review, /radiology-revise 명령어 사용 가능
```

---

## 기능

### 지원 연구 유형

| 연구 유형 | 설명 | 체크리스트 |
|-----------|------|-----------|
| `diagnostic_accuracy` | 진단 정확도 연구 | STARD 2015 + STARD-AI |
| `prognostic` | 예후/예측 모델 연구 | TRIPOD + TRIPOD+AI |
| `segmentation_ai` | AI 분할 연구 | CLAIM 2024 |
| `screening` | 스크리닝 연구 | STARD + screening extensions |
| `interventional` | 중재 영상의학 연구 | Custom criteria |
| `llm_study` | LLM 활용 연구 | MI-CLEAR-LLM + TRIPOD-LLM + DEAL |

### 지원 섹션

- **Title** — 제목 최적화
- **Abstract** — 구조화된 초록 (Purpose/Methods/Results/Conclusion)
- **Introduction** — 배경 및 목적 (2단계 문헌 파이프라인)
- **Methods** — 방법론 (체크리스트 기반 검증)
- **Results** — 결과 제시
- **Discussion** — 고찰 및 제한점
- **Conclusion** — 결론
- **Cover Letter** — 투고 커버레터

### 입력 파라미터

| 파라미터 | 타입 | 필수 여부 | 설명 |
|----------|------|-----------|------|
| `section_type` | string | 필수 | 작성 또는 리뷰할 섹션 |
| `study_type` | string | 필수 | 연구 유형 (체크리스트 결정) |
| `journal_profile` | string | 선택 | 타깃 저널 (Radiology, EurRad, AJR 등) |
| `draft_text` | string | Review/Revise 시 | 리뷰할 텍스트 |
| `reviewer_focus` | string | 선택 | balanced \| technical \| clinical_readability \| language_only |
| `language` | string | 선택 | en (기본) \| ko |

### 주요 기능

- **문헌 리서치**: 2단계 자동 파이프라인 — Codex(GPT-5.5) 문헌 분석 후 Claude WebSearch + WebFetch URL 검증
- **체크리스트 검증**: STARD, STARD-AI, TRIPOD, TRIPOD+AI, CLAIM 2024, MI-CLEAR-LLM, TRIPOD-LLM, DEAL, CHART 자동 확인(해당 연구 유형에 한해)
- **수치 일관성**: Abstract-Results-Tables 수치 일치 확인
- **과장 표현 감지**: "first", "novel", "superior" 등 과장 표현 탐지
- **통계 포맷**: 저널별 통계 표기 형식 검증
- **이중 언어 지원**: 영어/한국어 지원

---

## 명령어

### `/radiology-draft [section] [language]`

논문 섹션 초안을 작성합니다. Claude-only 워크플로입니다.

```
/radiology-draft Methods en

> Study type: diagnostic_accuracy
> Target journal: Radiology
> Key findings: AUC 0.92, sensitivity 92.5%, specificity 92.2%
```

**워크플로우:**
1. 필수 정보 수집 (study type, journal, key findings)
2. 섹션 템플릿 및 저널 프로파일 로드
3. 초안 생성 (Claude 네이티브)
4. (선택) `/radiology-review` 즉시 실행 — Codex 기술 리뷰 + Claude 네이티브 스타일 리뷰

### `/radiology-intro [topic]`

문헌 기반 Introduction 섹션을 2단계 파이프라인으로 작성합니다.

```
/radiology-intro "Deep learning for liver metastasis detection on CT"

> Keywords: liver metastasis, CT, deep learning, diagnostic accuracy
> Study type: diagnostic_accuracy
> Target journal: Radiology
> Language: en
```

**문헌 리서치 워크플로우:**
```
Research Topic + Keywords
           │
           ▼
┌───────────────────────────────┐
│ Stage 1 — Codex (GPT-5.5)     │
│ 문헌 분석                     │
│ reasoning: xhigh              │
├───────────────────────────────┤
│ • 선행 연구 (landmark)        │
│ • Citation hints + queries    │
│ • 연구 공백 분석              │
│ • 단락 초안 제안              │
└───────────────────────────────┘
           │
           ▼
┌───────────────────────────────┐
│ Stage 2 — Claude              │
│ WebSearch + WebFetch          │
│ URL 검증                      │
├───────────────────────────────┤
│ • suggested_query 실행        │
│ • expected_domains 필터링     │
│ • 각 후보 URL 검증            │
│ • verified_url / title 첨부   │
└───────────────────────────────┘
           │
           ▼
┌───────────────────────────────┐
│ Claude — 초안 합성            │
├───────────────────────────────┤
│ P1: 임상적 배경               │
│ P2: 기존 연구 한계            │
│ P3: 연구 목적                 │
└───────────────────────────────┘
```

Stage 2 실패(네트워크, 속도 제한, 도구 사용 불가, 모든 후보 거부) 시 Stage 1 결과를 그대로 반환하고 최상위에 `stage2_verification_failed: true` 플래그를 추가합니다. 에러로 중단하지 않으며, 사용자는 citation hints가 포함된 사용 가능한 Introduction 초안을 받습니다.

### `/radiology-review [section]`

기존 텍스트를 3-스텝 순차 파이프라인으로 리뷰합니다.

```
/radiology-review Methods

[Methods 섹션 텍스트 제공]
```

**3-스텝 리뷰 프로세스:**
```
┌───────────────────────────────┐
│ Step 1 — Codex (GPT-5.5)      │
│ 기술 리뷰                     │
│ reasoning: xhigh              │
├───────────────────────────────┤
│ • 환자 선택 편향              │
│ • Reference standard 적절성   │
│ • 통계적 엄밀성               │
│ • 체크리스트 준수             │
│   (source: "gpt-5.5")         │
└───────────────────────────────┘
           │
           ▼
┌───────────────────────────────┐
│ Step 2 — Claude Native        │
│ 스타일 리뷰                   │
├───────────────────────────────┤
│ • 문장 흐름 / 간결성          │
│ • 문단 구성                   │
│ • 시제 일관성                 │
│ • 용어 일관성                 │
│ • Candidate rewrites          │
│   (source: "claude")          │
└───────────────────────────────┘
           │
           ▼
┌───────────────────────────────┐
│ Step 3 — 통합 JSON 병합       │
│ 항목별 source 보존            │
└───────────────────────────────┘
```

Claude는 `references/codex-prompts.md`의 **Style Guidance for Claude Native Review** 헤더를 로드하여 초안에 직접 적용합니다. Step 2에서는 외부 CLI를 호출하지 않습니다.

### `/radiology-revise`

리뷰 피드백을 반영하여 수정합니다. Claude-only 워크플로입니다.

**우선순위:**
1. Critical 방법론 이슈
2. 통계/일관성 이슈
3. 명확성/스타일 개선
4. 사소한 수정

선택 사항: 변경된 섹션에 대해 `/radiology-review`를 타깃 재실행.

---

## 출력 형식

리뷰 결과는 구조화된 JSON으로 제공되며, `output/{section}_{timestamp}/` 디렉토리에 JSON과 Markdown 두 가지 형식으로 자동 저장됩니다 (자세한 내용은 [출력 파일](#출력-파일) 참조). 모든 JSON 키는 하위 호환성을 위해 그대로 유지됩니다. 항목 레벨의 `source` 허용 값은 `"claude"`와 `"gpt-5.5"` 두 가지입니다.

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
      "description": "Long compound sentence reduces clarity",
      "suggestion": "Break into two sentences for clarity",
      "source": "claude"
    }
  ],
  "terminology_and_consistency": [
    {
      "term": "deep learning vs DL",
      "locations": ["Introduction paragraph 1", "Methods paragraph 4"],
      "canonical_form": "deep learning (DL) on first use; DL thereafter",
      "source": "claude"
    }
  ],
  "candidate_rewrites": [
    {
      "location": "Methods, paragraph 2",
      "original": "The patients were selected...",
      "revised": "Consecutive patients who met eligibility criteria...",
      "rationale": "Specifies sampling method and makes subject explicit"
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

## 출력 파일

리뷰 결과는 자동으로 `output/` 디렉토리에 JSON과 Markdown 두 가지 형식으로 저장됩니다.

### 출력 디렉토리 구조

```
output/
├── methods_20260423_143022/
│   ├── review_result.json         # 통합 구조화 JSON 리뷰 결과
│   ├── review_report.md           # 사람이 읽기 쉬운 Markdown 리포트
│   ├── codex_raw.json             # GPT-5.5 원본 응답 (성공 시)
│   └── claude_style_raw.json      # Claude 네이티브 스타일 리뷰 원본 출력
├── introduction_20260423_150145/
│   ├── review_result.json
│   ├── review_report.md
│   ├── codex_raw.json
│   ├── claude_style_raw.json
│   └── stage2_verification.json   # (introduction 전용) citation hint별 URL 검증 상태
└── abstract_20260424_091500/
    ├── review_result.json
    ├── review_report.md
    └── ...
```

### 파일 설명

| 파일 | 형식 | 설명 |
|------|------|------|
| `review_result.json` | JSON | 통합 리뷰 결과 (`major_issues`, `clarity_issues`, `terminology_and_consistency`, `candidate_rewrites`, `checklist`) |
| `review_report.md` | Markdown | 헤더, 이슈 요약, 수정 제안, 체크리스트 현황 등 포함된 리포트 |
| `codex_raw.json` | JSON | GPT-5.5의 원본 응답 (추적 및 디버깅용) |
| `claude_style_raw.json` | JSON | Claude 네이티브 스타일 리뷰의 원본 출력 |
| `stage2_verification.json` | JSON | Introduction 전용: citation hint별 URL 검증 상태. 검증 실패 시 최상위에 `stage2_verification_failed: true`만 기록됨 |

### review_report.md 구성

1. **Header** — 섹션 유형, 연구 유형, 타깃 저널, 리뷰 일시
2. **Major Issues** — 심각도순 정렬 (critical > major > minor), source 표시(`claude` 또는 `gpt-5.5`)
3. **Clarity Issues** — 가독성/구조/흐름 이슈와 개선 제안 (source: `claude`)
4. **Terminology & Consistency** — 용어 및 일관성 이슈 (source: `claude`)
5. **Candidate Rewrites** — 원문 vs 수정안 비교 (근거 포함)
6. **Checklist Compliance** — 준수/누락/불완전 항목 및 준수율
7. **Summary Statistics** — 심각도별 이슈 수, 체크리스트 준수율 (%)

### 디렉토리 규칙

- 모든 출력은 프로젝트 루트의 `output/` 디렉토리 아래에 생성됩니다
- 각 리뷰 세션마다 새로운 타임스탬프 디렉토리가 생성됩니다
- 이전 결과는 덮어쓰지 않습니다
- 디렉토리 이름: `{섹션}_{YYYYMMDD_HHMMSS}` (예: `methods_20260423_143022`)

---

## 섹션별 리뷰 기준

### Methods 섹션 예시

| GPT-5.5 (기술적 리뷰) | Claude (스타일 리뷰) |
|-----------------------|---------------------|
| 환자 선택 편향 | 문단 구성 |
| Spectrum bias | 소제목 구조 |
| Imaging protocol 완전성 | 반복 제거 |
| Reference standard 적절성 | 시제 일관성 |
| 표본 크기 정당성 | 가독성 점수 |
| 다중 비교 보정 | |
| 판독자 간 일치도 | |

---

## 기술 상세

### 모델 구성

| 역할 | CLI / 호출 | 파라미터 | 용도 |
|------|-----------|---------|------|
| Claude Code (메인 에디터 + 스타일 리뷰어) | 네이티브 (외부 CLI 없음) | `references/codex-prompts.md`의 "Style Guidance for Claude Native Review" 로드; Stage 2 URL 검증에 WebSearch + WebFetch 사용 | 초안 작성, 수정, 스타일 리뷰, 인용 URL 검증 |
| GPT-5.5 (기술 리뷰어) | Codex CLI | `-m gpt-5.5 --sandbox read-only --config model_reasoning_effort=xhigh` | 기술/방법론 리뷰 |
| GPT-5.5 (문헌 리서처) | Codex CLI | `-m gpt-5.5 --sandbox read-only --config model_reasoning_effort=xhigh` | `/radiology-intro` Stage 1 문헌 분석 |

### 실행 명령어

```bash
# 기술/방법론 리뷰 (read-only 샌드박스, reasoning xhigh)
codex exec -m gpt-5.5 --sandbox read-only --config model_reasoning_effort=xhigh "[prompt]"

# Introduction 문헌 분석 (/radiology-intro Stage 1)
codex exec -m gpt-5.5 --sandbox read-only --config model_reasoning_effort=xhigh "[literature prompt]"

# 동일 Codex 세션 내 후속 호출
codex exec resume --last "[follow-up prompt]"
```

Claude 네이티브 작업 — 스타일 리뷰, URL 검증(WebSearch + WebFetch), 초안 생성, 수정, 피드백 통합 — 은 외부 CLI를 호출하지 않습니다.

### 에러 처리

| 상황 | 대응 |
|------|------|
| Codex 호출 실패 (타임아웃, 비정상 종료, 빈 응답, JSON 파싱 실패) | Claude 단독 리뷰로 폴백하며 JSON에 명시적 표기(`"fallback": "claude_only"`, `"codex_error": "<message>"`). 통합 JSON 키는 모두 보존되고 `major_issues`는 Claude가 `source: "claude"`로 채움. 폴백 전 1회 재시도. |
| Claude 네이티브 스타일 리뷰의 JSON이 유효하지 않음 | 내부 합성 1회 재시도; 반복 실패 시 `review_report.md`에 텍스트 피드백을 그대로 제시하고 JSON에는 `clarity_issues: []` 설정. |
| Stage 2 URL 검증 실패 (WebSearch 또는 WebFetch) | Stage 1 결과를 그대로 반환하고 `stage2_verification_failed: true` 추가. 에러로 중단하지 않음. |
| 빈 입력 초안 | 리뷰어를 실행하기 전에 사용자에게 초안 입력을 요구. |

폴백 경로에는 Claude와 Codex 이외의 모델이 포함되지 않습니다. 시스템은 엄격히 3개의 모델 역할로만 구성됩니다: Claude Code(메인 에디터 + 스타일 리뷰어), Codex GPT-5.5 기술 리뷰어, Codex GPT-5.5 문헌 리서처.

---

## 언어 지원

### 영어 (기본)
- 모든 프롬프트 및 피드백이 영어로 제공
- 체크리스트 영어

### 한국어 (`language="ko"`)
- Codex(GPT-5.5) 프롬프트: 기술적 일관성을 위해 영어 유지
- Claude 네이티브 스타일 리뷰: `references/codex-prompts.md`의 "Style Guidance for Claude Native Review" 중 Korean-to-English Guidelines 섹션을 적용
- 출력 피드백: 요청 시 한국어로 제시
- 체크리스트: 항목 이름은 영어, 설명은 이중 언어
- 한국어 전용 규칙:
  - 존댓말 일관성
  - 영어 의학용어 vs 한글 용어 선택 기준
  - 문장 길이: 40자 이내 권장

---

## 모범 사례

1. **수치 검증 필수** — Abstract 수치가 Results/Tables와 반드시 일치해야 함
2. **과장 표현 주의** — 근거 없이 "first", "novel", "superior" 사용 금지
3. **통계 포맷** — 저널 스타일 준수 (OR [95% CI], p-values)
4. **Reference standard** — 진단 연구에서 적절한 gold standard 확보
5. **환자 선택** — 명확하고 재현 가능한 선택 기준
6. **판독자 블라인딩** — 판독자 블라인딩 적절히 기술
7. **제한점** — 모든 주요 제한점을 솔직하게 기술

---

## 파일 구조

```
~/.claude/skills/radiology-paper-writer/
├── SKILL.md                          # 메인 스킬 정의 (3-모델 파이프라인)
├── README.md                         # 영어 문서
├── README.ko.md                      # 한국어 문서
├── IMPLEMENTATION_PLAN.md            # 개발 로드맵 및 마이그레이션 이력
├── references/
│   ├── radiology-checklists.md       # STARD, TRIPOD, CLAIM, MI-CLEAR-LLM,
│   │                                 # TRIPOD-LLM, DEAL, CHART 체크리스트
│   ├── section-templates.md          # 섹션별 템플릿
│   ├── journal-profiles.md           # 저널 요구사항 및 포맷
│   └── codex-prompts.md              # GPT-5.5 리뷰 프롬프트 +
│                                     # Style Guidance for Claude Native Review +
│                                     # Introduction Literature Analysis 프롬프트
└── output/                           # 자동 생성 리뷰 출력
    ├── methods_20260423_143022/
    │   ├── review_result.json
    │   ├── review_report.md
    │   ├── codex_raw.json
    │   └── claude_style_raw.json
    └── introduction_20260423_150145/
        ├── review_result.json
        ├── review_report.md
        ├── codex_raw.json
        ├── claude_style_raw.json
        └── stage2_verification.json
```

---

## 로드맵

- [ ] 추가 저널 프로파일 (한국 저널)
- [ ] 참고문헌 형식 변환 통합
- [ ] Figure/Table 캡션 생성
- [ ] 리뷰어 응답 레터 생성
- [ ] 참고문헌 관리자 연동

---

## 기여

이 스킬에 대한 피드백이나 개선 제안은 언제든 환영합니다.

---

## 라이선스

MIT License

---

## 감사의 글

- STARD Initiative (Standards for Reporting Diagnostic Accuracy)
- TRIPOD Group (Transparent Reporting of a multivariable prediction model)
- CLAIM Checklist (Checklist for Artificial Intelligence in Medical Imaging)
- MI-CLEAR-LLM (Medical Imaging Checklist for LLM Evaluation and Reporting)
- TRIPOD-LLM (Transparent Reporting of LLM Prediction Models)
- DEAL (Developing and Evaluating LLMs)
- CHART (Chatbot Assessment Reporting Tool)
