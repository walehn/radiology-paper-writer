# Radiology Paper Writer Skill - Implementation Plan

> **구현 상태:** ✅ 완료 (2026-01-11)
>
> 모든 파일이 성공적으로 생성되었습니다.

## Overview

영상의학 학술 연구 논문 작성 및 교정을 위한 멀티모델 리뷰 Skill 구현 계획

**핵심 구조:**
- Claude Code: 메인 에디터 (초안 작성, 피드백 통합, 수정)
- GPT 5.2 (Codex CLI): 기술/논리/방법론 리뷰어
- Gemini 3.0 Pro (Gemini CLI): 스타일/가독성/저널 스타일 리뷰어

---

## Directory Structure

```
~/.claude/skills/radiology-paper-writer/
├── SKILL.md                           # 메인 스킬 정의
├── IMPLEMENTATION_PLAN.md             # 이 파일
├── references/
│   ├── radiology-checklists.md        # STARD, TRIPOD, CLAIM-AI
│   ├── section-templates.md           # 섹션별 리뷰 기준
│   ├── journal-profiles.md            # 저널별 요구사항
│   ├── codex-prompts.md               # GPT-5.2 리뷰 프롬프트
│   └── gemini-prompts.md              # Gemini 리뷰 프롬프트
```

---

## Implementation Steps

### Step 1: Create SKILL.md ✅

**File:** `~/.claude/skills/radiology-paper-writer/SKILL.md`

**Frontmatter:**
```yaml
---
name: radiology-paper-writer
description: AI-assisted radiology research paper writing with multi-model review. Claude drafts, GPT-5.2 reviews methodology, Gemini reviews style. Supports STARD/TRIPOD/CLAIM checklists. Triggers: /radiology-draft, /radiology-review, /radiology-revise
---
```

**Body sections:**
1. Overview & Model Roles
2. Supported Study Types (diagnostic_accuracy, prognostic, segmentation_ai, screening, interventional)
3. Supported Section Types (Title, Abstract, Introduction, Methods, Results, Discussion, Conclusion, CoverLetter)
4. Input Parameters (section_type, study_type, journal_profile, draft_text, reviewer_focus, language)
5. Workflow Phases (Context → Draft → Parallel Review → Integration)
6. CLI Commands (codex exec, gemini)
7. Output Structure (major_issues, clarity_issues, candidate_rewrites, checklist)

### Step 2: Create radiology-checklists.md ✅

**File:** `references/radiology-checklists.md`

**Contents:**
- STARD 2015 (30 items) - diagnostic accuracy studies
- STARD-AI Extension (12 items) - AI diagnostic studies
- TRIPOD (22 items) - prediction model studies
- TRIPOD+AI (5 items) - AI prediction studies
- CLAIM 2024 (48 items, 7 categories) - AI in medical imaging

### Step 3: Create section-templates.md ✅

**File:** `references/section-templates.md`

**Per-section review criteria:**

| Section | Codex (Technical) | Gemini (Style) |
|---------|-------------------|----------------|
| Abstract | 수치 일관성, 과장 탐지, endpoint 명확성, 통계 포맷 | 문장 간결성, 정보 밀도, 임상 가독성, 단어 수 |
| Methods | 환자 선택, imaging protocol, reference standard, 통계 | 문단 구조, 소제목, 반복 제거, 가독성 |
| Results | 표/그림-텍스트 일치, primary outcome, 통계 포맷 | 문단당 메시지, figure 참조 순서 |
| Discussion | 해석 타당성, 문헌 비교, limitation, overclaim | 논리 흐름, 전환 문장, 연결어 |

### Step 4: Create journal-profiles.md ✅

**File:** `references/journal-profiles.md`

**Journals:**
- Radiology (RSNA): 250 words abstract, 4500 words main, P < .05 format
- European Radiology: 250 words, Key Points required
- AJR: 250 words, active voice preferred
- Radiology: AI: 300 words, CLAIM mandatory
- JMRI: 300 words, MRI-specific parameters
- Academic Radiology: 200 words
- Investigative Radiology: 250 words
- Korean Journal of Radiology: 250 words, Key Points required

### Step 5: Create codex-prompts.md ✅

**File:** `references/codex-prompts.md`

**Per-section prompts for GPT-5.2:**
- JSON 출력 형식 지정
- 기술적 평가 항목 명시
- checklist compliance 확인
- Abstract, Methods, Results, Discussion 각각의 리뷰 프롬프트

### Step 6: Create gemini-prompts.md ✅

**File:** `references/gemini-prompts.md`

**Per-section prompts for Gemini:**
- JSON 출력 형식 지정
- 스타일/가독성 평가 항목 명시
- 저널 스타일 준수 확인
- Korean language style prompt 포함

---

## Workflow Design

### /radiology-draft [section]

```
1. Context Gathering (Sequential)
   - Study type 확인
   - Target journal 확인
   - Key findings/data 수집

2. Draft Generation (Claude)
   - section-templates.md 로드
   - journal-profiles.md 로드
   - 초안 생성

3. Parallel Review (Optional)
   ┌─────────────────┐  ┌─────────────────┐
   │ Codex Technical │  │ Gemini Style    │
   │ codex exec ...  │  │ gemini -y ...   │
   └─────────────────┘  └─────────────────┘

4. Integration (Claude)
   - 피드백 병합
   - checklist compliance 생성
   - 초안 + 주석 제시
```

### /radiology-review [section]

```
1. Parse draft_text
2. Determine checklist (STARD/TRIPOD/CLAIM)
3. Parallel execution:
   - Codex: codex exec -m gpt-5.2 --sandbox read-only "[prompt]"
   - Gemini: gemini -y "[prompt]"
4. Parse & merge responses
5. Present unified feedback
```

### /radiology-revise

```
1. Load current draft + accumulated feedback
2. Claude analyzes & prioritizes changes
3. Generate revised draft
4. Optional: re-run targeted reviews
5. Update revision history
```

---

## CLI Command Patterns

**Codex:**
```bash
codex exec -m gpt-5.2 --sandbox read-only "[prompt with draft]"
codex exec resume --last "[follow-up]"
```

**Gemini:**
```bash
gemini -y "[prompt with draft]"
```

---

## Output Structure

```json
{
  "major_issues": [],           // Codex 중심
  "clarity_issues": [],         // Gemini 중심
  "terminology_and_consistency": [],
  "candidate_rewrites": [],     // 섹션/문단별 수정안
  "checklist": {
    "type": "STARD|TRIPOD|CLAIM",
    "compliant": [],
    "missing": [],
    "incomplete": []
  }
}
```

---

## Verification

### Test Plan

1. **SKILL.md 로드 테스트:**
   - `/radiology-draft Methods` 명령 실행
   - AskUserQuestion으로 study_type, journal 질문 확인

2. **Codex 연동 테스트:**
   - 샘플 Methods 텍스트로 리뷰 실행
   - JSON 응답 파싱 확인

3. **Gemini 연동 테스트:**
   - 동일 텍스트로 리뷰 실행
   - JSON 응답 파싱 확인

4. **병렬 리뷰 테스트:**
   - `/radiology-review Methods` 실행
   - 두 리뷰어 피드백 병합 확인

5. **Checklist 테스트:**
   - diagnostic_accuracy → STARD 체크리스트 적용 확인
   - segmentation_ai → CLAIM 체크리스트 적용 확인

---

## Files Created

| Priority | File | Purpose | Status |
|----------|------|---------|--------|
| 1 | SKILL.md | 메인 스킬 정의, 워크플로우, 명령어 | ✅ |
| 2 | references/radiology-checklists.md | STARD/TRIPOD/CLAIM 체크리스트 | ✅ |
| 3 | references/section-templates.md | 섹션별 리뷰 기준 | ✅ |
| 4 | references/journal-profiles.md | 저널별 요구사항 | ✅ |
| 5 | references/codex-prompts.md | GPT-5.2 프롬프트 | ✅ |
| 6 | references/gemini-prompts.md | Gemini 프롬프트 | ✅ |

---

## Notes

- 한국어 지원: language="ko" 시 피드백은 한국어로 제시
- 에러 처리: CLI 실패 시 Claude-only 리뷰로 fallback
- 성능: Codex/Gemini 병렬 실행으로 시간 단축
- 버전 관리: 개정 이력 추적 가능

---

## Future Enhancements

- [ ] 자동 참고문헌 형식 변환
- [ ] 그래픽 초록 생성 지원
- [ ] 투고 전 최종 체크리스트 자동 생성
- [ ] 리비전 대응 지원 (reviewer comment → response draft)
