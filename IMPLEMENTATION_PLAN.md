# Radiology Paper Writer Skill - Implementation Plan

> **구현 상태:** ✅ 완료 (2026-01-11)
>
> 모든 파일이 성공적으로 생성되었습니다.

## Overview

영상의학 학술 연구 논문 작성 및 교정을 위한 멀티모델 리뷰 Skill 구현 계획

**핵심 구조:**
- Claude Code: 메인 에디터 (초안 작성, 피드백 통합, 수정)
- GPT 5.4 (Codex CLI): 기술/논리/방법론 리뷰어
- Gemini 3.1 Pro (Gemini CLI): 스타일/가독성/저널 스타일 리뷰어

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
│   ├── codex-prompts.md               # GPT-5.4 리뷰 프롬프트
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
description: AI-assisted radiology research paper writing with multi-model review. Claude drafts, GPT-5.4 reviews methodology, Gemini reviews style. Supports STARD/TRIPOD/CLAIM checklists. Triggers: /radiology-draft, /radiology-review, /radiology-revise
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

**Per-section prompts for GPT-5.4:**
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
   │ codex exec ...  │  │ gemini --approval-mode yolo ...   │
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
   - Codex: codex exec -m gpt-5.4 --sandbox read-only "[prompt]"
   - Gemini: gemini --approval-mode yolo "[prompt]"
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
codex exec -m gpt-5.4 --sandbox read-only "[prompt with draft]"
codex exec resume --last "[follow-up]"
```

**Gemini:**
```bash
gemini --approval-mode yolo "[prompt with draft]"
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
| 5 | references/codex-prompts.md | GPT-5.4 프롬프트 | ✅ |
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

---

## Changelog

### 2026-04-24 — Model upgrade: GPT-5.4 → GPT-5.5, reasoning=xhigh

Upgraded the Codex reviewer/researcher model from `gpt-5.4` to `gpt-5.5` and bumped the `model_reasoning_effort` configuration from `high` to `xhigh` across SKILL.md, README.md, `references/codex-prompts.md`, and `references/section-templates.md`.

**Why:**

- OpenAI Codex CLI 0.124.0 now supports `gpt-5.5` as the current model (see `~/.codex/config.toml` `[notice.model_migrations]` chain: `gpt-5-codex → gpt-5.2-codex → gpt-5.3-codex → gpt-5.4 → gpt-5.5`).
- The `model_reasoning_effort` enum gained an `xhigh` level beyond `high`, suited for clinically high-stakes methodology review and literature gap analysis.

**What changed (active docs):**

- Model name: `gpt-5.4` → `gpt-5.5` (and `GPT-5.4` → `GPT-5.5`) everywhere in SKILL.md, README.md, `references/codex-prompts.md`, `references/section-templates.md`.
- Reasoning effort: `model_reasoning_effort=high` → `model_reasoning_effort=xhigh` in all `codex exec` invocations.
- JSON schema `source` field allowed values transition from `{"claude", "gpt-5.4"}` to `{"claude", "gpt-5.5"}`.

**Scope:**

- Not a SPEC-level change. SPEC-RAD-REFACTOR-001 explicitly listed Codex version upgrade as a Non-Goal; this change is a routine version-track follow-up rather than a refactor.
- Prior `gpt-5.4` references are preserved only in this changelog section for historical traceability.
- Output artifacts already saved under `output/` or `.moai/specs/SPEC-RAD-REFACTOR-001/test-samples/s1_output_methods_*` that list `"source": "gpt-5.5"` remain valid; downstream parsers must accept the new enum.

**Verification:**

- `codex exec -m gpt-5.5 --config model_reasoning_effort=xhigh` accepted by codex-cli 0.124.0 (verified: `reasoning effort: xhigh` appears in the Codex session header; `reasoning_effort` enum `{none, minimal, low, medium, high, xhigh}`).
- Post-change grep: zero `gpt-5.4` / `GPT-5.4` / `reasoning_effort=high` / `reasoning: high` matches in active docs (this changelog section excluded).

### 2026-04-23 — Refactor: 4-model → 3-model architecture

Refactor — Removed Gemini CLI dependency, migrated style review to Claude native, migrated literature research to Codex GPT-5.4 Stage 1 + Claude WebSearch Stage 2.

**4-model → 3-model architecture transition summary:**

- Before (4 roles): Claude Main Editor, Codex GPT-5.4 Technical Reviewer, Gemini 3.1 Pro Style Reviewer, Gemini 3.1 Pro Literature Researcher.
- After (3 roles): Claude Code (Main Editor + Style Reviewer), Codex GPT-5.4 Technical Reviewer, Codex GPT-5.4 Literature Researcher.
- Style review: moved from Gemini CLI to Claude native. The style criteria are now captured in `references/codex-prompts.md` under the header "Style Guidance for Claude Native Review" (medical writing criteria, Korean-to-English guidelines, passive voice handling, terminology consistency).
- Literature research: moved from Gemini 3.1 Pro to Codex GPT-5.4 with `model_reasoning_effort=high`. A new "Introduction Literature Analysis" prompt was added to `references/codex-prompts.md` producing `prior_work`, `citation_hints` (with `suggested_query` and `expected_domains`), and `research_gap_analysis`.
- `/radiology-intro` pipeline: now Stage 1 (Codex literature analysis) → Stage 2 (Claude WebSearch + WebFetch URL verification). On Stage 2 failure, Stage 1 results are returned with `stage2_verification_failed: true` and no error is raised (per OQ-1).
- Output JSON: schema keys preserved (`major_issues`, `clarity_issues`, `terminology_and_consistency`, `candidate_rewrites`, `checklist`). The `source` field value set transitions from `{"gemini", "gpt-5.4"}` to `{"claude", "gpt-5.4"}`. No combined values are emitted (per OQ-2).
- Error handling: the "Gemini timeout / API error / CLI not found" rows were removed. A single "Codex invocation failure" row was added with strategy "Fallback to Claude-only review with explicit notice in output JSON." No Gemini fallback path exists.
- Deleted file: `references/gemini-prompts.md`. Its style-relevant content was migrated into the Style Guidance header in `references/codex-prompts.md`.
- Command parameter signatures (`/radiology-draft`, `/radiology-intro`, `/radiology-review`, `/radiology-revise`) remain unchanged. Checklist types (STARD, TRIPOD, CLAIM, MI-CLEAR-LLM, TRIPOD-LLM, DEAL, STARD-AI, TRIPOD+AI, CLAIM 2024, CHART) remain unchanged.
