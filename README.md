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
  <a href="https://github.com/openai/codex"><img src="https://img.shields.io/badge/Codex_CLI-GPT--5.4-412991?style=flat&logo=openai&logoColor=white" alt="Codex CLI"></a>
  <a href="https://github.com/google/gemini-cli"><img src="https://img.shields.io/badge/Gemini_CLI-3.1_Pro-4285F4?style=flat&logo=google&logoColor=white" alt="Gemini CLI"></a>
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

**Multi-Model System**: Each model focuses on its area of expertise

```
┌─────────────────────────────────────────────────────────────┐
│                    Claude Code (Main Editor)                 │
│            Drafting · Feedback Integration · Revision        │
└─────────────────────────────────────────────────────────────┘
                              │
       ┌──────────────────────┼──────────────────────┐
       ▼                      ▼                      ▼
┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐
│ GPT-5.4          │  │ Gemini 3.1 Pro   │  │ Gemini 3.1 Pro   │
│ (reasoning: high)│  │ Style Reviewer   │  │ Lit. Researcher  │
├──────────────────┤  ├──────────────────┤  ├──────────────────┤
│ • Study design   │  │ • Sentence flow  │  │ • Lit. review    │
│ • Statistics     │  │ • Readability    │  │ • Gap analysis   │
│ • Logical rigor  │  │ • Journal style  │  │ • Citations      │
│ • Methodology    │  │ • Clarity        │  │ • Background     │
└──────────────────┘  └──────────────────┘  └──────────────────┘
```

---

## Installation

### Prerequisites

- **Node.js** (v18+) and **npm** — required for Codex CLI and Gemini CLI installation

All three CLI tools must be installed and authenticated:

| CLI | Install | Authentication |
|-----|---------|----------------|
| [Claude Code](https://claude.ai/claude-code) | `curl -fsSL https://claude.ai/install.sh \| bash` (macOS/Linux/WSL) | Subscription (Pro/Max) or API key (`ANTHROPIC_API_KEY`) |
| [Codex CLI](https://github.com/openai/codex) | `npm install -g @openai/codex` | OAuth login (`codex auth`) or API key (`OPENAI_API_KEY`) |
| [Gemini CLI](https://github.com/google/gemini-cli) | `npm install -g @google/gemini-cli` | Google OAuth (`gemini auth`) or API key (`GEMINI_API_KEY`) |

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
- **Introduction** — Background and objectives
- **Methods** — Methodology (checklist-based validation)
- **Results** — Result presentation
- **Discussion** — Discussion and limitations
- **Conclusion** — Conclusion
- **Cover Letter** — Submission cover letter

### Key Capabilities

- **Literature Research**: Automated literature review and gap analysis via Gemini 3.1 Pro
- **Checklist Compliance**: Automatic STARD, TRIPOD, CLAIM verification
- **Number Consistency**: Abstract–Results–Tables number matching
- **Overclaim Detection**: Flags "first", "novel", "superior" without evidence
- **Statistical Formatting**: Journal-specific statistical notation validation
- **Bilingual Support**: English and Korean

---

## Commands

### `/radiology-draft [section]`

Generates a draft for a paper section.

```
/radiology-draft Methods

> Study type: diagnostic_accuracy
> Target journal: Radiology
> Key findings: AUC 0.92, sensitivity 92.5%, specificity 92.2%
```

**Workflow:**
1. Collect required info (study type, journal, key findings)
2. Load section template and journal profile
3. Generate draft
4. (Optional) Run immediate review

### `/radiology-intro [topic]`

Generates a literature-based Introduction section.

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

Reviews existing text with multi-model feedback.

```
/radiology-review Methods

[Provide Methods section text]
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

Revises draft based on review feedback.

**Priority Order:**
1. Critical methodology issues
2. Statistical/consistency issues
3. Clarity/style improvements
4. Minor polish

---

## Output Format

Review results are provided as structured JSON and automatically saved to `output/{section}_{timestamp}/` in both JSON and Markdown formats (see [Output Files](#output-files)):

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

Review results are automatically saved to the `output/` directory in both JSON and Markdown formats.

### Directory Structure

```
output/
├── methods_20260308_143022/
│   ├── review_result.json       # Structured JSON review result
│   ├── review_report.md         # Human-readable Markdown report
│   ├── codex_raw.json           # Raw GPT-5.4 response (on success)
│   └── gemini_raw.json          # Raw Gemini response (on success)
├── abstract_20260309_091500/
│   ├── review_result.json
│   ├── review_report.md
│   └── ...
```

### File Descriptions

| File | Format | Description |
|------|--------|-------------|
| `review_result.json` | JSON | Unified review result (major_issues, clarity_issues, checklist, etc.) |
| `review_report.md` | Markdown | Report with header, issue summary, rewrite suggestions, checklist status |
| `codex_raw.json` | JSON | Raw GPT-5.4 response (for traceability and debugging) |
| `gemini_raw.json` | JSON | Raw Gemini response (for traceability and debugging) |

### review_report.md Structure

1. **Header** — Section type, study type, target journal, review date
2. **Major Issues** — Sorted by severity (critical > major > minor), source attribution
3. **Clarity Issues** — Readability/structure/flow issues with suggestions
4. **Terminology & Consistency** — Terminology and consistency issues
5. **Candidate Rewrites** — Original vs revised text comparison (with rationale)
6. **Checklist Compliance** — Compliant/missing/incomplete items with compliance rate
7. **Summary Statistics** — Issue count by severity, checklist compliance rate (%)

### Directory Conventions

- All outputs are created under `output/` in the project root
- Each review session creates a new timestamped directory
- Previous results are never overwritten
- Directory naming: `{section}_{YYYYMMDD_HHMMSS}` (e.g., `methods_20260308_143022`)

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
  - Consistent honorific style
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
├── SKILL.md                          # Main skill definition
├── README.md                         # English documentation
├── README.ko.md                      # Korean documentation
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
