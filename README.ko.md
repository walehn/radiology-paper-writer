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
  <a href="https://github.com/openai/codex"><img src="https://img.shields.io/badge/Codex_CLI-GPT--5.4-412991?style=flat&logo=openai&logoColor=white" alt="Codex CLI"></a>
  <a href="https://github.com/google/gemini-cli"><img src="https://img.shields.io/badge/Gemini_CLI-3.1_Pro-4285F4?style=flat&logo=google&logoColor=white" alt="Gemini CLI"></a>
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

**멀티모델 시스템**: 각 모델이 전문 분야에 집중

```
┌─────────────────────────────────────────────────────────────┐
│                    Claude Code (메인 에디터)                   │
│         초안 작성 · 피드백 통합 · 수정 · 버전 관리            │
└─────────────────────────────────────────────────────────────┘
                              │
       ┌──────────────────────┼──────────────────────┐
       ▼                      ▼                      ▼
┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐
│ GPT-5.4          │  │ Gemini 3.1 Pro   │  │ Gemini 3.1 Pro   │
│ (reasoning: high)│  │ 스타일 리뷰어    │  │ 문헌 리서처      │
├──────────────────┤  ├──────────────────┤  ├──────────────────┤
│ • 연구 설계 검증 │  │ • 문장 자연스러움│  │ • 문헌 검토      │
│ • 통계 분석      │  │ • 가독성 향상    │  │ • Gap 분석       │
│ • 논리적 일관성  │  │ • 저널 스타일    │  │ • 인용 제안      │
│ • 방법론 체크    │  │ • Clarity 개선   │  │ • 배경 연구      │
└──────────────────┘  └──────────────────┘  └──────────────────┘
```

---

## 설치

### 사전 요구사항

- **Node.js** (v18+) 및 **npm** — Codex CLI, Gemini CLI 설치에 필요

세 가지 CLI 도구를 모두 설치하고 인증을 완료해야 합니다:

| CLI | 설치 | 인증 방법 |
|-----|------|----------|
| [Claude Code](https://claude.ai/claude-code) | `curl -fsSL https://claude.ai/install.sh \| bash` (macOS/Linux/WSL) | 구독 (Pro/Max) 또는 API 키 (`ANTHROPIC_API_KEY`) |
| [Codex CLI](https://github.com/openai/codex) | `npm install -g @openai/codex` | OAuth 로그인 (`codex auth`) 또는 API 키 (`OPENAI_API_KEY`) |
| [Gemini CLI](https://github.com/google/gemini-cli) | `npm install -g @google/gemini-cli` | Google OAuth (`gemini auth`) 또는 API 키 (`GEMINI_API_KEY`) |

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
- **Introduction** — 배경 및 목적
- **Methods** — 방법론 (체크리스트 기반 검증)
- **Results** — 결과 제시
- **Discussion** — 고찰 및 제한점
- **Conclusion** — 결론
- **Cover Letter** — 투고 커버레터

### 주요 기능

- **문헌 리서치**: Gemini 3.1 Pro를 활용한 자동 문헌 검토 및 gap 분석
- **체크리스트 검증**: STARD, TRIPOD, CLAIM 자동 검증
- **수치 일관성**: Abstract-Results-Tables 수치 일치 확인
- **과장 표현 감지**: "first", "novel", "superior" 등 과장 표현 탐지
- **통계 포맷**: 저널별 통계 표기 형식 검증
- **이중 언어 지원**: 영어/한국어 지원

---

## 명령어

### `/radiology-draft [section]`

논문 섹션 초안을 작성합니다.

```
/radiology-draft Methods

> Study type: diagnostic_accuracy
> Target journal: Radiology
> Key findings: AUC 0.92, sensitivity 92.5%, specificity 92.2%
```

**워크플로우:**
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

**문헌 리서치 워크플로우:**
```
Research Topic + Keywords
          │
          ▼
┌─────────────────────────┐
│ Gemini 3.1 Pro          │
│ 문헌 분석               │
├─────────────────────────┤
│ • 임상적 배경           │
│ • 주요 문헌             │
│ • Knowledge gaps        │
│ • 인용 제안             │
└─────────────────────────┘
          │
          ▼
┌─────────────────────────┐
│ Claude                  │
│ Introduction 초안       │
├─────────────────────────┤
│ P1: 임상적 배경         │
│ P2: 기존 연구 한계      │
│ P3: 연구 목적           │
└─────────────────────────┘
          │
          ▼
┌─────────────────────────┐
│ 이중 리뷰 (선택)       │
├─────────────────────────┤
│ GPT-5.4: Gap 정합성     │
│ Gemini: 흐름, 명확성    │
└─────────────────────────┘
```

### `/radiology-review [section]`

기존 텍스트를 멀티모델로 리뷰합니다.

```
/radiology-review Methods

[Methods 섹션 텍스트 제공]
```

**병렬 리뷰 프로세스:**
```
GPT-5.4 (reasoning: high)          Gemini 3.1 Pro
        │                                │
        ▼                                ▼
  기술적 이슈                       스타일 이슈
  - 환자 선택 편향                  - 문단 구성
  - Reference standard 적절성       - 시제 일관성
  - 통계적 엄밀성                   - 가독성 점수
        │                                │
        └────────────┬───────────────────┘
                     ▼
            통합 피드백 JSON
```

### `/radiology-revise`

리뷰 피드백을 반영하여 수정합니다.

**우선순위:**
1. Critical 방법론 이슈
2. 통계/일관성 이슈
3. 명확성/스타일 개선
4. 사소한 수정

---

## 출력 형식

리뷰 결과는 구조화된 JSON으로 제공되며, `output/{section}_{timestamp}/` 디렉토리에 JSON과 Markdown 두 가지 형식으로 자동 저장됩니다 (자세한 내용은 [출력 파일](#출력-파일) 참조):

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

## 출력 파일

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

1. **Header** — 섹션 유형, 연구 유형, 타깃 저널, 리뷰 일시
2. **Major Issues** — 심각도순 정렬 (critical > major > minor), 모델 출처 표시
3. **Clarity Issues** — 가독성/구조/흐름 이슈 및 개선 제안
4. **Terminology & Consistency** — 용어 및 일관성 이슈
5. **Candidate Rewrites** — 원문 vs 수정안 비교 (근거 포함)
6. **Checklist Compliance** — 준수/누락/불완전 항목 및 준수율
7. **Summary Statistics** — 심각도별 이슈 수, 체크리스트 준수율 (%)

### 디렉토리 규칙

- 모든 출력은 프로젝트 루트의 `output/` 디렉토리 아래에 생성됩니다
- 각 리뷰 세션마다 새로운 타임스탬프 디렉토리가 생성됩니다
- 이전 결과는 덮어쓰지 않습니다
- 디렉토리 이름: `{섹션}_{YYYYMMDD_HHMMSS}` (예: `methods_20260308_143022`)

---

## 섹션별 리뷰 기준

### Methods 섹션 예시

| GPT-5.4 (기술적 리뷰) | Gemini (스타일 리뷰) |
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

### 모델 설정

| 모델 | CLI | 파라미터 | 용도 |
|------|-----|---------|------|
| GPT-5.4 | Codex | `--sandbox read-only --config model_reasoning_effort=high` | 기술적 리뷰 |
| Gemini 3.1 Pro | Gemini | `--approval-mode yolo` (자동 승인) | 스타일 리뷰 |
| Gemini 3.1 Pro | Gemini | `-m gemini-3.1-pro-preview --approval-mode yolo` | 문헌 리서치 |

### 실행 명령어

```bash
# GPT-5.4 기술적 리뷰
codex exec -m gpt-5.4 --sandbox read-only --config model_reasoning_effort=high "[prompt]"

# Gemini 스타일 리뷰
gemini --approval-mode yolo "[prompt]"

# Gemini 3.1 Pro 문헌 리서치
gemini -m gemini-3.1-pro-preview --approval-mode yolo "[research prompt]"
```

### 에러 처리

| 상황 | 대응 |
|------|------|
| GPT-5.4 타임아웃/실패 | 1회 재시도 후 Claude 단독 리뷰 |
| Gemini 에러 | GPT-5.4 리뷰만으로 계속 진행 |
| 두 CLI 모두 실패 | Claude가 기본 리뷰 제공 |
| 잘못된 JSON 응답 | 텍스트 피드백 추출, 비구조화 형태로 제시 |

---

## 언어 지원

### 영어 (기본)
- 모든 프롬프트 및 피드백이 영어로 제공
- 체크리스트 영어

### 한국어 (`language="ko"`)
- GPT-5.4 프롬프트: 영어 (기술적 일관성)
- Gemini 프롬프트: 한국어 스타일 가이드라인 추가
- 출력 피드백: 한국어
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
├── SKILL.md                          # 메인 스킬 정의
├── README.md                         # 영어 문서
├── README.ko.md                      # 한국어 문서
├── IMPLEMENTATION_PLAN.md            # 개발 로드맵
├── references/
│   ├── radiology-checklists.md       # STARD, TRIPOD, CLAIM 체크리스트
│   ├── section-templates.md          # 섹션별 템플릿
│   ├── journal-profiles.md           # 저널 요구사항
│   ├── codex-prompts.md              # GPT-5.4 리뷰 프롬프트
│   └── gemini-prompts.md             # Gemini 리뷰 프롬프트
└── output/                           # 자동 생성 리뷰 출력
    ├── methods_20260308_143022/
    │   ├── review_result.json
    │   ├── review_report.md
    │   ├── codex_raw.json
    │   └── gemini_raw.json
    └── abstract_20260309_091500/
        └── ...
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
