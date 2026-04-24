<h1 align="center">Radiology Paper Writer</h1>

<p align="center">
  <strong>AI-assisted radiology research paper writing with multi-model review system</strong>
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

## Overview

### The Problem

Common challenges when writing radiology research papers:
- Complying with complex checklists (STARD, TRIPOD, CLAIM, etc.)
- Maintaining number consistency across Abstract, Results, and Tables
- Balancing methodological rigor with readability
- Meeting different formatting requirements per journal

### The Solution

**3-Model System**: Each role focuses on its area of expertise. Claude Code acts as both the Main Editor and the Style Reviewer (native, no external CLI). GPT-5.5 (via Codex CLI, reasoning: xhigh) handles technical review in one lane and literature analysis in another.

```
┌─────────────────────────────────────────────────────────────┐
│              Claude Code (Main Editor + Style Reviewer)      │
│   Drafting · Native Style Review · WebSearch/WebFetch        │
│            Feedback Integration · Revision                   │
└─────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┴───────────────┐
              ▼                               ▼
    ┌──────────────────────┐       ┌──────────────────────┐
    │ GPT-5.5 via Codex    │       │ GPT-5.5 via Codex    │
    │ Technical Reviewer   │       │ Literature Researcher│
    │ reasoning: xhigh     │       │ reasoning: xhigh     │
    ├──────────────────────┤       ├──────────────────────┤
    │ • Study design       │       │ • Prior work         │
    │ • Statistics         │       │ • Gap analysis       │
    │ • Methodology        │       │ • Citation hints     │
    │ • Checklist compl.   │       │ • Paragraph drafts   │
    └──────────────────────┘       └──────────────────────┘
```

No fourth reviewer exists. All style review is Claude-native; all literature research is Codex (GPT-5.5) Stage 1 followed by Claude WebSearch + WebFetch Stage 2.

---

## Installation

### Prerequisites

- **Node.js** (v18+) and **npm** — required for Codex CLI installation

Two CLI tools must be installed and authenticated:

| CLI | Install | Authentication |
|-----|---------|----------------|
| [Claude Code](https://claude.ai/claude-code) | `curl -fsSL https://claude.ai/install.sh \| bash` (macOS/Linux/WSL) | Subscription (Pro/Max) or API key (`ANTHROPIC_API_KEY`) |
| [Codex CLI](https://github.com/openai/codex) | `npm install -g @openai/codex` | OAuth login (`codex login`) or API key (`OPENAI_API_KEY`) |

> **Note:** Claude Code on Windows: PowerShell `irm https://claude.ai/install.ps1 | iex` / CMD `curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd`. Windows requires [Git for Windows](https://gitforwindows.org/).

### Setup

1. Clone the repository:
```bash
git clone https://github.com/walehn/radiology-paper-writer.git
cd radiology-paper-writer
```

2. Create skill directory and copy files:
```bash
mkdir -p ~/.claude/skills/radiology-paper-writer/references
cp SKILL.md IMPLEMENTATION_PLAN.md ~/.claude/skills/radiology-paper-writer/
cp references/*.md ~/.claude/skills/radiology-paper-writer/references/
```

3. Verify in Claude Code:
```bash
claude
# /radiology-draft, /radiology-intro, /radiology-review, /radiology-revise commands available
```

---

## Features

### Supported Study Types

| Study Type | Description | Checklist |
|------------|-------------|-----------|
| `diagnostic_accuracy` | Diagnostic accuracy study | STARD 2015 + STARD-AI |
| `prognostic` | Prognostic/prediction model study | TRIPOD + TRIPOD+AI |
| `segmentation_ai` | AI segmentation study | CLAIM 2024 |
| `screening` | Screening study | STARD + screening extensions |
| `interventional` | Interventional radiology study | Custom criteria |
| `llm_study` | LLM-based study | MI-CLEAR-LLM + TRIPOD-LLM + DEAL |

### Supported Sections

- **Title** — Title optimization
- **Abstract** — Structured abstract (Purpose/Methods/Results/Conclusion)
- **Introduction** — Background and objectives (2-stage literature pipeline)
- **Methods** — Methodology (checklist-based validation)
- **Results** — Result presentation
- **Discussion** — Discussion and limitations
- **Conclusion** — Conclusion
- **Cover Letter** — Submission cover letter

### Input Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `section_type` | string | Yes | Section to draft or review |
| `study_type` | string | Yes | Study type (drives checklist selection) |
| `journal_profile` | string | No | Target journal (Radiology, EurRad, AJR, etc.) |
| `draft_text` | string | Review/Revise | Text to review |
| `reviewer_focus` | string | No | balanced \| technical \| clinical_readability \| language_only |
| `language` | string | No | en (default) \| ko |

### Key Capabilities

- **Literature Research**: Automated 2-stage pipeline — Codex (GPT-5.5) literature analysis followed by Claude WebSearch + WebFetch URL verification
- **Checklist Compliance**: Automatic STARD, STARD-AI, TRIPOD, TRIPOD+AI, CLAIM 2024, MI-CLEAR-LLM, TRIPOD-LLM, DEAL, CHART verification where applicable
- **Number Consistency**: Abstract–Results–Tables number matching
- **Overclaim Detection**: Flags "first", "novel", "superior" without evidence
- **Statistical Formatting**: Journal-specific statistical notation validation
- **Bilingual Support**: English and Korean

---

## Commands

### `/radiology-draft [section] [language]`

Generates a draft for a paper section. Claude-only workflow.

```
/radiology-draft Methods en

> Study type: diagnostic_accuracy
> Target journal: Radiology
> Key findings: AUC 0.92, sensitivity 92.5%, specificity 92.2%
```

**Workflow:**
1. Collect required info (study type, journal, key findings)
2. Load section template and journal profile
3. Generate draft (Claude native)
4. (Optional) Run immediate review via `/radiology-review` (Codex technical + Claude native style)

### `/radiology-intro [topic]`

Generates a literature-based Introduction section via a 2-stage pipeline.

```
/radiology-intro "Deep learning for liver metastasis detection on CT"

> Keywords: liver metastasis, CT, deep learning, diagnostic accuracy
> Study type: diagnostic_accuracy
> Target journal: Radiology
> Language: en
```

**Literature Research Workflow:**
```
Research Topic + Keywords
           │
           ▼
┌───────────────────────────────┐
│ Stage 1 — Codex (GPT-5.5)     │
│ Literature Analysis           │
│ reasoning: xhigh              │
├───────────────────────────────┤
│ • Prior work (landmark)       │
│ • Citation hints + queries    │
│ • Research gap analysis       │
│ • Paragraph draft suggestions │
└───────────────────────────────┘
           │
           ▼
┌───────────────────────────────┐
│ Stage 2 — Claude              │
│ WebSearch + WebFetch          │
│ URL Verification              │
├───────────────────────────────┤
│ • Execute suggested_query     │
│ • Filter by expected_domains  │
│ • Verify each candidate URL   │
│ • Attach verified_url/title   │
└───────────────────────────────┘
           │
           ▼
┌───────────────────────────────┐
│ Claude — Draft Synthesis      │
├───────────────────────────────┤
│ P1: Clinical Context          │
│ P2: Gap / Limitation          │
│ P3: Objective                 │
└───────────────────────────────┘
```

On Stage 2 failure (network, rate limit, tool unavailable, all candidates rejected), the command returns Stage 1 results unchanged plus the top-level flag `stage2_verification_failed: true`. The user still gets a usable Introduction draft with citation hints.

### `/radiology-review [section]`

Reviews existing text with a 3-step sequential pipeline.

```
/radiology-review Methods

[Provide Methods section text]
```

**3-Step Review Process:**
```
┌───────────────────────────────┐
│ Step 1 — Codex (GPT-5.5)      │
│ Technical Review              │
│ reasoning: xhigh              │
├───────────────────────────────┤
│ • Patient selection bias      │
│ • Reference standard adequacy │
│ • Statistical rigor           │
│ • Checklist compliance        │
│   (source: "gpt-5.5")         │
└───────────────────────────────┘
           │
           ▼
┌───────────────────────────────┐
│ Step 2 — Claude Native        │
│ Style Review                  │
├───────────────────────────────┤
│ • Sentence flow / brevity     │
│ • Paragraph organization      │
│ • Verb tense consistency      │
│ • Terminology consistency     │
│ • Candidate rewrites          │
│   (source: "claude")          │
└───────────────────────────────┘
           │
           ▼
┌───────────────────────────────┐
│ Step 3 — Unified JSON Merge   │
│ Per-item source preserved     │
└───────────────────────────────┘
```

Claude loads the **Style Guidance for Claude Native Review** header from `references/codex-prompts.md` and applies it directly to the draft. No external CLI is invoked for Step 2.

### `/radiology-revise`

Revises draft based on review feedback. Claude-only workflow.

**Priority Order:**
1. Critical methodology issues
2. Statistical/consistency issues
3. Clarity/style improvements
4. Minor polish

Optional: Re-run targeted `/radiology-review` on changed sections.

---

## Output Format

Review results are provided as structured JSON and automatically saved to `output/{section}_{timestamp}/` in both JSON and Markdown formats (see [Output Files](#output-files)). All JSON keys are preserved verbatim for backward compatibility. Allowed `source` values at item level are `"claude"` and `"gpt-5.5"`.

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

## Output Files

Review results are automatically saved to the `output/` directory in both JSON and Markdown formats.

### Directory Structure

```
output/
├── methods_20260423_143022/
│   ├── review_result.json         # Unified structured JSON review result
│   ├── review_report.md           # Human-readable Markdown report
│   ├── codex_raw.json             # Raw GPT-5.5 response (on success)
│   └── claude_style_raw.json      # Raw Claude native style review output
├── introduction_20260423_150145/
│   ├── review_result.json
│   ├── review_report.md
│   ├── codex_raw.json
│   ├── claude_style_raw.json
│   └── stage2_verification.json   # (introduction only) per-hint URL verification status
└── abstract_20260424_091500/
    ├── review_result.json
    ├── review_report.md
    └── ...
```

### File Descriptions

| File | Format | Description |
|------|--------|-------------|
| `review_result.json` | JSON | Unified review result (`major_issues`, `clarity_issues`, `terminology_and_consistency`, `candidate_rewrites`, `checklist`) |
| `review_report.md` | Markdown | Report with header, issue summary, rewrite suggestions, checklist status |
| `codex_raw.json` | JSON | Raw GPT-5.5 response (for traceability and debugging) |
| `claude_style_raw.json` | JSON | Raw Claude native style review output |
| `stage2_verification.json` | JSON | Introduction only: per-citation-hint URL verification status, or `stage2_verification_failed: true` at top level when verification could not complete |

### review_report.md Structure

1. **Header** — Section type, study type, target journal, review date
2. **Major Issues** — Sorted by severity (critical > major > minor), source attribution (`claude` or `gpt-5.5`)
3. **Clarity Issues** — Readability/structure/flow issues with suggestions (source: `claude`)
4. **Terminology & Consistency** — Terminology and consistency issues (source: `claude`)
5. **Candidate Rewrites** — Original vs revised text comparison with rationale
6. **Checklist Compliance** — Compliant/missing/incomplete items with compliance rate
7. **Summary Statistics** — Issue count by severity, checklist compliance rate (%)

### Directory Conventions

- All outputs are created under `output/` in the project root
- Each review session creates a new timestamped directory
- Previous results are never overwritten
- Directory naming: `{section}_{YYYYMMDD_HHMMSS}` (e.g., `methods_20260423_143022`)

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

## Technical Details

### Model Configuration

| Role | CLI / Invocation | Parameters | Purpose |
|------|------------------|------------|---------|
| Claude Code (Main Editor + Style Reviewer) | Native (no external CLI) | Loads "Style Guidance for Claude Native Review" from `references/codex-prompts.md`; uses WebSearch + WebFetch for Stage 2 URL verification | Drafting, revision, style review, citation verification |
| GPT-5.5 (Technical Reviewer) | Codex CLI | `-m gpt-5.5 --sandbox read-only --config model_reasoning_effort=xhigh` | Technical/methodological review |
| GPT-5.5 (Literature Researcher) | Codex CLI | `-m gpt-5.5 --sandbox read-only --config model_reasoning_effort=xhigh` | `/radiology-intro` Stage 1 literature analysis |

### Execution Commands

```bash
# Technical or methodological review (read-only sandbox, xhigh reasoning)
codex exec -m gpt-5.5 --sandbox read-only --config model_reasoning_effort=xhigh "[prompt]"

# Literature analysis for Introduction (Stage 1 of /radiology-intro)
codex exec -m gpt-5.5 --sandbox read-only --config model_reasoning_effort=xhigh "[literature prompt]"

# Follow-up in the same Codex session
codex exec resume --last "[follow-up prompt]"
```

Claude native operations — style review, URL verification (WebSearch + WebFetch), draft generation, revision, feedback integration — invoke no external CLI.

### Error Handling

| Scenario | Action |
|----------|--------|
| Codex invocation failure (timeout, exit nonzero, empty response, invalid JSON) | Fallback to Claude-only review with explicit notice in the JSON (`"fallback": "claude_only"`, `"codex_error": "<message>"`). All unified JSON keys are preserved; `major_issues` is populated by Claude with `source: "claude"`. Retry once before falling back. |
| Claude native style review produces invalid JSON | Retry internal synthesis once; on repeat failure, present text feedback verbatim in `review_report.md` and set `clarity_issues: []` in JSON. |
| Stage 2 URL verification failure (WebSearch or WebFetch) | Return Stage 1 results unchanged plus `stage2_verification_failed: true`. Do not error out. |
| Empty input draft | Prompt user for the draft before running any reviewer. |

No fallback path uses any non-Claude, non-Codex model. The system operates strictly on 3 model roles: Claude Code (Main Editor + Style Reviewer), Codex GPT-5.5 Technical Reviewer, Codex GPT-5.5 Literature Researcher.

---

## Language Support

### English (default)
- All prompts and feedback in English
- Checklists in English

### Korean (`language="ko"`)
- Codex (GPT-5.5) prompts: kept in English for technical consistency
- Claude native style review: applies the Korean-to-English Guidelines section from `references/codex-prompts.md` (Style Guidance for Claude Native Review)
- Output feedback: presented in Korean when requested
- Checklists: item names in English, descriptions bilingual
- Korean-specific rules:
  - Consistent honorific style (존댓말 일관성)
  - English medical terms vs Korean terms selection
  - Sentence length: under 40 characters recommended

---

## Best Practices

1. **Always validate numbers** — Abstract numbers must match Results/Tables
2. **Check for overclaims** — Avoid "first", "novel", "superior" without evidence
3. **Statistical formatting** — Follow journal style (OR [95% CI], p-values)
4. **Reference standard** — Ensure adequacy for diagnostic studies
5. **Patient selection** — Explicit, reproducible criteria
6. **Blinding** — Document reader blinding appropriately
7. **Limitations** — Address all major limitations honestly

---

## File Structure

```
~/.claude/skills/radiology-paper-writer/
├── SKILL.md                          # Main skill definition (3-model pipeline)
├── README.md                         # English documentation
├── README.ko.md                      # Korean documentation
├── IMPLEMENTATION_PLAN.md            # Development roadmap and migration history
├── references/
│   ├── radiology-checklists.md       # STARD, TRIPOD, CLAIM, MI-CLEAR-LLM,
│   │                                 # TRIPOD-LLM, DEAL, CHART checklists
│   ├── section-templates.md          # Section-specific templates
│   ├── journal-profiles.md           # Journal requirements and formatting
│   └── codex-prompts.md              # GPT-5.5 review prompts +
│                                     # Style Guidance for Claude Native Review +
│                                     # Introduction Literature Analysis prompt
└── output/                           # Auto-generated review outputs
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

## Roadmap

- [ ] Additional journal profiles (Korean journals)
- [ ] Citation formatting integration
- [ ] Figure/Table caption generation
- [ ] Reviewer response letter generation
- [ ] Integration with reference managers

---

## Contributing

Feedback and improvement suggestions for this skill are always welcome.

---

## License

MIT License

---

## Acknowledgments

- STARD Initiative (Standards for Reporting Diagnostic Accuracy)
- TRIPOD Group (Transparent Reporting of a multivariable prediction model)
- CLAIM Checklist (Checklist for Artificial Intelligence in Medical Imaging)
- MI-CLEAR-LLM (Medical Imaging Checklist for LLM Evaluation and Reporting)
- TRIPOD-LLM (Transparent Reporting of LLM Prediction Models)
- DEAL (Developing and Evaluating LLMs)
- CHART (Chatbot Assessment Reporting Tool)
