# Radiology Research Checklists

영상의학 연구 논문 작성을 위한 표준 체크리스트 모음

## Table of Contents
- [STARD 2015](#stard-2015) - Diagnostic Accuracy Studies
- [STARD-AI Extension](#stard-ai-extension) - AI Diagnostic Studies
- [TRIPOD](#tripod) - Prediction Model Studies
- [TRIPOD+AI](#tripod-ai) - AI Prediction Studies
- [CLAIM 2024](#claim-2024) - AI in Medical Imaging
- [MI-CLEAR-LLM 2025](#mi-clear-llm-2025) - LLM Accuracy Studies
- [TRIPOD-LLM](#tripod-llm) - LLM Prediction/Application Studies
- [DEAL](#deal) - LLM Development and Evaluation
- [CHART](#chart) - LLM Chatbot Clinical Studies

---

## STARD 2015

### Standards for Reporting of Diagnostic Accuracy Studies
**Use for:** diagnostic_accuracy study type

### Title and Abstract (Items 1-2)

- [ ] **1. Title** - Identify the article as a study of diagnostic accuracy (recommend MeSH heading "sensitivity and specificity")
- [ ] **2. Abstract** - Structured abstract with: study design, methods, results (sensitivity, specificity, predictive values), conclusions

### Introduction (Item 3)

- [ ] **3a. Intended use** - Describe intended use of index test and clinical role (diagnosis, staging, screening, monitoring, prognosis)
- [ ] **3b. Hypotheses** - State specific study hypotheses or objectives

### Methods (Items 4-18)

**Study Design:**
- [ ] **4. Design** - Whether prospective or retrospective data collection; specify if this was a planned study or secondary analysis of existing data
- [ ] **5. Participants** - Eligibility criteria including:
  - Inclusion criteria
  - Exclusion criteria
  - Settings and locations where data were collected
- [ ] **6. Sampling** - On what basis potentially eligible participants were identified (consecutive, random, convenience)
- [ ] **7. Data collection** - Whether data collection was planned before index test and reference standard were performed (prospective) or after (retrospective)

**Test Methods:**
- [ ] **8. Reference standard** - Definition of reference standard and rationale for choosing it (if alternatives exist)
- [ ] **9a. Index test specification** - Technical specifications of index test and how it was performed
- [ ] **9b. Reference standard specification** - Technical specifications of reference standard and how it was performed
- [ ] **10a. Positivity threshold** - Definition of what was considered positive/negative for index test
- [ ] **10b. Threshold timing** - Whether threshold was pre-specified
- [ ] **11. Sample size** - Rationale for sample size (if formally calculated)

**Analysis:**
- [ ] **12a. Intended analysis** - Methods for calculating or comparing measures of diagnostic accuracy, and statistical methods for quantifying uncertainty (e.g., 95% CI)
- [ ] **12b. Missing data** - How indeterminate results, missing responses, and outliers were handled

**Blinding:**
- [ ] **13a. Index test blinding** - Whether readers of index test were blinded to reference standard results; any other clinical information available
- [ ] **13b. Reference standard blinding** - Whether readers of reference standard were blinded to index test results; any other clinical information available

### Results (Items 19-25)

**Participants:**
- [ ] **19. Flow diagram** - Show flow of participants through the study:
  - Number potentially eligible
  - Number enrolled
  - Number undergoing index test
  - Number undergoing reference standard
  - Number in final analysis
  - Reasons for exclusion at each stage
- [ ] **20. Baseline characteristics** - Demographic and clinical characteristics of participants
- [ ] **21a. Disease severity** - Distribution of severity of disease in those with target condition
- [ ] **21b. Alternative diagnoses** - Distribution of alternative diagnoses in those without target condition
- [ ] **22. Time interval** - Time interval and any clinical interventions between index test and reference standard

**Test Results:**
- [ ] **23. Cross tabulation** - Cross tabulation of index test results by reference standard results (including indeterminate and missing)
- [ ] **24. Diagnostic accuracy** - Estimates of diagnostic accuracy and their precision (95% CI):
  - Sensitivity
  - Specificity
  - Positive predictive value
  - Negative predictive value
  - Likelihood ratios
  - AUC (if applicable)
- [ ] **25. Adverse events** - Any adverse events from performing index test or reference standard

### Discussion (Items 26-27)

- [ ] **26. Limitations** - Study limitations including:
  - Sources of potential bias
  - Statistical uncertainty
  - Applicability concerns
- [ ] **27. Implications** - Implications for practice, including intended use and clinical role of index test

---

## STARD-AI Extension

### Additional Items for AI Diagnostic Studies
**Use for:** diagnostic_accuracy with AI component

### Methods - AI Specific

- [ ] **AI-1. Model architecture** - Describe the AI model architecture (CNN, transformer, etc.)
- [ ] **AI-2. Training data** - Source, size, and characteristics of training data
  - Number of images/cases
  - Class distribution
  - Preprocessing applied
- [ ] **AI-3. Validation strategy** - Internal validation method (cross-validation, train/validation/test split)
- [ ] **AI-4. External validation** - Describe external validation dataset:
  - Source institution(s)
  - Time period
  - Demographics
- [ ] **AI-5. Preprocessing** - Image preprocessing steps:
  - Normalization
  - Windowing (CT)
  - Augmentation during training
- [ ] **AI-6. Training parameters** - Report:
  - Number of epochs
  - Batch size
  - Learning rate and scheduler
  - Loss function
  - Optimizer
  - Early stopping criteria
- [ ] **AI-7. Hardware/Software** - Report:
  - GPU specifications
  - Deep learning framework and version
  - Code availability
- [ ] **AI-8. Human-AI comparison** - Methodology for comparing AI to human readers

### Results - AI Specific

- [ ] **AI-9. Performance metrics** - Report performance with confidence intervals:
  - AUC
  - Sensitivity at specific specificity
  - Specificity at specific sensitivity
  - Operating point threshold
- [ ] **AI-10. Subgroup performance** - Performance across relevant subgroups:
  - Demographics
  - Disease severity
  - Image quality
  - Scanner manufacturers
- [ ] **AI-11. Failure analysis** - Analysis of cases where AI failed:
  - Characteristics of false positives
  - Characteristics of false negatives
  - Systematic error patterns
- [ ] **AI-12. Human comparison results** - Results of human reader comparison:
  - Reader experience levels
  - Reading conditions
  - Statistical comparison method

---

## TRIPOD

### Transparent Reporting of a Multivariable Prediction Model for Individual Prognosis or Diagnosis
**Use for:** prognostic study type

### Title and Abstract (Items 1-2)

- [ ] **1. Title** - Identify the study as developing/validating a multivariable prediction model, the target population, and the outcome
- [ ] **2. Abstract** - Provide structured summary including objectives, setting, participants, sample size, predictors, outcome, statistical analysis, results, conclusions

### Introduction (Item 3)

- [ ] **3a. Background** - Explain the medical context and rationale for developing/validating the model
- [ ] **3b. Objectives** - Specify objectives, including whether developing and/or validating

### Methods (Items 4-12)

**Source of Data:**
- [ ] **4a. Study design** - Describe the study design or source of data
- [ ] **4b. Dates** - Specify key study dates (enrollment, follow-up)

**Participants:**
- [ ] **5a. Eligibility criteria** - Describe eligibility criteria for participants
- [ ] **5b. Treatments** - Give details of treatments received, if relevant
- [ ] **5c. Study setting** - Describe setting, locations, and number of centers

**Outcome:**
- [ ] **6a. Outcome definition** - Clearly define outcome, including how and when assessed
- [ ] **6b. Actions** - Report any actions to blind outcome assessment

**Predictors:**
- [ ] **7a. Candidate predictors** - Define all candidate predictors
- [ ] **7b. Assessment** - Report how predictors were measured

**Sample Size:**
- [ ] **8. Sample size** - Explain how sample size was determined

**Missing Data:**
- [ ] **9. Missing data** - Describe how missing data were handled

**Statistical Analysis:**
- [ ] **10a. Model building** - Describe how predictors were handled
- [ ] **10b. Selection method** - Specify type of model, method for internal validation
- [ ] **10d. Model performance** - Specify measures of model performance (discrimination, calibration) and how calculated
- [ ] **10e. Comparison** - Describe methods for comparing models if applicable

### Results (Items 13-17)

- [ ] **13a. Flow diagram** - Describe flow of participants through the study
- [ ] **13b. Characteristics** - Describe participant characteristics
- [ ] **14a. Sample characteristics** - Specify number of participants with missing data
- [ ] **14b. Outcome events** - Report number of participants with the outcome, by predictor categories
- [ ] **15a. Unadjusted associations** - Present unadjusted associations between predictors and outcome
- [ ] **15b. Full model** - Present the full prediction model (all regression coefficients, model intercept)
- [ ] **16. Model performance** - Report measures of model performance
- [ ] **17. Model updating** - If applicable, report results of model updating

### Discussion (Items 18-22)

- [ ] **18. Limitations** - Discuss limitations of study
- [ ] **19a. Validation** - Discuss results regarding validation
- [ ] **19b. Performance** - Give overall interpretation of results
- [ ] **20. Implications** - Discuss potential clinical use and implications

---

## TRIPOD+AI

### Additional Items for AI Prediction Studies
**Use for:** prognostic with AI component

- [ ] **AI-P1. Data preprocessing** - Describe data preprocessing and feature engineering
- [ ] **AI-P2. Model architecture** - Describe AI model architecture in detail
- [ ] **AI-P3. Hyperparameter tuning** - Report hyperparameter optimization approach
- [ ] **AI-P4. Class imbalance** - Describe handling of class imbalance
- [ ] **AI-P5. Interpretability** - Report interpretability/explainability methods used

---

## CLAIM 2024

### Checklist for Artificial Intelligence in Medical Imaging
**Use for:** segmentation_ai study type

### 1. Title/Abstract (6 items)

- [ ] **1.1** Identify as AI study in title
- [ ] **1.2** Structured abstract with all key elements
- [ ] **1.3** State if model is trained for binary or multiclass classification
- [ ] **1.4** Summarize training and test data sources
- [ ] **1.5** Report primary performance metric with CI
- [ ] **1.6** State conclusions regarding clinical applicability

### 2. Introduction (4 items)

- [ ] **2.1** Describe clinical context and motivation
- [ ] **2.2** Review relevant prior work
- [ ] **2.3** State study objectives clearly
- [ ] **2.4** Describe intended clinical use and deployment context

### 3. Methods - Data (10 items)

- [ ] **3.1** Describe data source(s) and acquisition protocol
- [ ] **3.2** Report eligibility criteria
- [ ] **3.3** Describe data preprocessing steps
- [ ] **3.4** Describe ground truth/reference standard
- [ ] **3.5** Report annotator qualifications and number
- [ ] **3.6** Report inter-annotator agreement
- [ ] **3.7** Describe data partitioning strategy
- [ ] **3.8** Ensure no data leakage between partitions
- [ ] **3.9** Describe handling of confounders
- [ ] **3.10** Report class distribution in each partition

### 4. Methods - Model (8 items)

- [ ] **4.1** Describe model architecture
- [ ] **4.2** Report input/output specifications
- [ ] **4.3** Describe training procedure
- [ ] **4.4** Report hyperparameter selection
- [ ] **4.5** Describe augmentation strategies
- [ ] **4.6** Report loss function and optimizer
- [ ] **4.7** Describe model selection criteria
- [ ] **4.8** Report handling of model failures/edge cases

### 5. Methods - Evaluation (6 items)

- [ ] **5.1** Define primary performance metric
- [ ] **5.2** Justify metric choice for clinical context
- [ ] **5.3** Report secondary metrics
- [ ] **5.4** Describe statistical analysis plan
- [ ] **5.5** Describe subgroup analysis plan
- [ ] **5.6** Define clinical equivalence thresholds if applicable

### 6. Results (8 items)

- [ ] **6.1** Report sample sizes for all partitions
- [ ] **6.2** Report participant characteristics
- [ ] **6.3** Report primary metric with CI
- [ ] **6.4** Report all secondary metrics
- [ ] **6.5** Report subgroup performance
- [ ] **6.6** Report failure case analysis
- [ ] **6.7** Compare to human performance if applicable
- [ ] **6.8** Report computational requirements

### 7. Discussion (6 items)

- [ ] **7.1** Summarize main findings
- [ ] **7.2** Discuss findings in context of literature
- [ ] **7.3** Address limitations comprehensively:
  - Data limitations
  - Model limitations
  - Evaluation limitations
- [ ] **7.4** Discuss generalizability
- [ ] **7.5** Discuss potential clinical impact
- [ ] **7.6** Suggest future research directions

---

## MI-CLEAR-LLM 2025

### Minimum Reporting Items for Clear Evaluation of Accuracy Reports of Large Language Models in Healthcare
**Use for:** llm_study type (LLM accuracy evaluation)
**Reference:** Park SH, Suh CH, Lee JH, Kahn CE Jr, Moy L. Korean Journal of Radiology, 2024 (2025 Updates)

### Item 1 - Model Identification and Specifications

- [ ] **1.1** Model name and version (e.g., GPT-4, Claude 3.5 Sonnet)
- [ ] **1.2** Model manufacturer/developer
- [ ] **1.3** Training data cutoff date (if known)
- [ ] **1.4** Retrieval-augmented generation (RAG) access status
- [ ] **1.5** Date(s) of querying
- [ ] **1.6** Access mode (web-based chatbot, API, or self-managed/open-source)

### Item 2 - Stochasticity Management

- [ ] **2.1** Number of query attempts per question/task
- [ ] **2.2** Method for synthesizing results across attempts
- [ ] **2.3** Reliability analysis across repeated attempts
- [ ] **2.4** Temperature and other generation parameter settings

### Item 3 - Complete Prompt Documentation

- [ ] **3.1** Full text of prompts with exact wording and syntax
- [ ] **3.2** Precise spellings, symbols, punctuation, spaces, and other details
- [ ] **3.3** System prompts or instructions (if applicable)
- [ ] **3.4** Few-shot examples provided (if applicable)

### Item 4 - Prompt Employment Details

- [ ] **4.1** Session structure (individual sessions vs. multi-query sessions)
- [ ] **4.2** Context provided within each session
- [ ] **4.3** Order of queries within sessions (if multi-query)

### Item 5 - Prompt Testing and Optimization

- [ ] **5.1** Description of prompt optimization steps
- [ ] **5.2** Rationale for prompt wording choices
- [ ] **5.3** Testing conducted during prompt development
- [ ] **5.4** Confirmation that test data was independent from optimization data

### Item 6 - Test Dataset Independence

- [ ] **6.1** Verification that test data was not used during prompt optimization
- [ ] **6.2** Assessment of whether test data was in the model's training data
- [ ] **6.3** For internet-sourced datasets: exact URLs provided
- [ ] **6.4** Measures taken to ensure data independence

---

## TRIPOD-LLM

### Transparent Reporting of a Multivariable Prediction Model for Individual Prognosis or Diagnosis - LLM Extension
**Use for:** llm_study type (LLM prediction/application studies)
**Reference:** Collins GS et al. Nature Medicine, 2024. 19 main items, 50 subitems.

### Title and Abstract (Items 1-2)

- [ ] **1. Title** - Identify as LLM study; specify LLM task category (e.g., text generation, classification, summarization)
- [ ] **2. Abstract** - Structured summary including LLM model used, task description, evaluation methodology, key results with uncertainty measures

### Introduction (Items 3-4)

- [ ] **3. Background** - Describe clinical/scientific context and rationale for using LLM approach
- [ ] **4. Objectives** - Specify study objectives, including LLM task and intended use case

### Methods (Items 5-12)

**Data and Task:**
- [ ] **5a. Data source** - Describe data sources, collection methods, and time periods
- [ ] **5b. Data characteristics** - Report data type (text, images, structured data), language, domain specificity
- [ ] **6. Task definition** - Clearly define LLM task (classification, generation, extraction, summarization, etc.)
- [ ] **7. Reference standard** - Define ground truth/reference standard and how it was established

**LLM Specification:**
- [ ] **8a. Model identification** - Report exact model name, version, and provider
- [ ] **8b. Model access** - Describe access method (API, web interface, local deployment)
- [ ] **8c. Model configuration** - Report temperature, top-p, max tokens, and other parameters
- [ ] **9a. Prompt design** - Provide complete prompts including system messages, few-shot examples
- [ ] **9b. Prompt optimization** - Describe prompt engineering methodology and validation

**Evaluation Design:**
- [ ] **10. Evaluation strategy** - Describe evaluation methodology (automated metrics, human evaluation, or both)
- [ ] **11. Human evaluation** - If applicable: evaluator qualifications, blinding, agreement measurement
- [ ] **12. Statistical methods** - Describe statistical analysis including uncertainty quantification

### Open Science (Item 13)

- [ ] **13a. Code availability** - Report availability of analysis code
- [ ] **13b. Data availability** - Report availability of evaluation data
- [ ] **13c. Prompt availability** - Provide complete prompts as supplementary material

### Patient and Public Involvement (Item 14)

- [ ] **14. PPI** - Report patient/public involvement in study design if applicable

### Results (Items 15-17)

- [ ] **15. Data description** - Report dataset characteristics, size, and distribution
- [ ] **16a. Overall performance** - Report primary performance metrics with confidence intervals
- [ ] **16b. Subgroup performance** - Report performance across relevant subgroups (demographics, task difficulty, etc.)
- [ ] **17. Failure analysis** - Analyze and characterize model failures and error patterns

### Discussion (Items 18-19)

- [ ] **18. Interpretation** - Discuss results in context of existing literature and clinical relevance
- [ ] **19. Limitations** - Address limitations including:
  - Model version dependency and reproducibility
  - Potential data contamination
  - Generalizability across populations/settings
  - Human-AI comparison limitations

---

## DEAL

### Development, Evaluation, and Assessment of Large Language Models Checklist
**Use for:** llm_study type (LLM development and applied research)
**Reference:** Tripathi S et al. NEJM AI, 2025. Registered with EQUATOR Network.

DEAL provides two pathways:
- **DEAL-A**: Advanced model development and fine-tuning
- **DEAL-B**: Applied research using pretrained models

### Common Items (Both DEAL-A and DEAL-B)

**Study Design:**
- [ ] **C1. Research question** - Clearly state the research question and clinical application
- [ ] **C2. Study design** - Describe study design and rationale for LLM approach
- [ ] **C3. Ethical considerations** - Report IRB approval, consent, and data privacy measures

**Model Specifications:**
- [ ] **C4. Model identification** - Report model name, version, developer, and access date
- [ ] **C5. Access method** - Specify API, web interface, or local deployment
- [ ] **C6. Configuration** - Report all generation parameters (temperature, top-p, etc.)

**Data Handling:**
- [ ] **C7. Data source** - Describe data origin, collection, and preprocessing
- [ ] **C8. Data quality** - Report data quality assessment methods
- [ ] **C9. Data splitting** - Describe train/validation/test split methodology

**Evaluation:**
- [ ] **C10. Metrics** - Define and justify evaluation metrics
- [ ] **C11. Statistical analysis** - Describe statistical methods and uncertainty quantification
- [ ] **C12. Comparison** - Report comparisons with baselines, human performance, or other models

**Transparency:**
- [ ] **C13. Reproducibility** - Report measures to ensure reproducibility
- [ ] **C14. Code/Data availability** - State availability of code, data, and prompts
- [ ] **C15. Limitations** - Discuss study limitations comprehensively

### DEAL-A Additional Items (Advanced Development)

- [ ] **A1. Architecture** - Describe model architecture modifications
- [ ] **A2. Training data** - Report training data characteristics, size, sources
- [ ] **A3. Fine-tuning procedure** - Describe fine-tuning methodology, hyperparameters
- [ ] **A4. Training infrastructure** - Report hardware, software, training time
- [ ] **A5. Model selection** - Describe model selection criteria and validation

### DEAL-B Additional Items (Applied Research)

- [ ] **B1. Prompt engineering** - Document complete prompt design process
- [ ] **B2. Prompt validation** - Describe prompt testing and optimization
- [ ] **B3. Stochasticity** - Report handling of output variability
- [ ] **B4. Context management** - Describe context window utilization and limitations
- [ ] **B5. Output processing** - Report post-processing of model outputs

---

## CHART

### Chatbot Assessment Reporting Tool
**Use for:** llm_study type (LLM chatbot clinical advice evaluation)
**Reference:** Huo B et al. (CHART Collaborative, 48 experts). BMJ Medicine, JAMA Network Open, BJS, BMC Medicine, Annals of Family Medicine, Artificial Intelligence in Medicine (simultaneous publication, 2025). 12 items, 39 subitems.

### 1. Title and Abstract

- [ ] **1.1** Identify as LLM chatbot evaluation study in title
- [ ] **1.2** Structured abstract with model, task, evaluation method, key results

### 2. Background

- [ ] **2.1** Clinical context and rationale for chatbot evaluation
- [ ] **2.2** Review of relevant prior chatbot studies

### 3. Model Identifier

- [ ] **3.1** Model name, version, release date
- [ ] **3.2** Open-source vs. proprietary status

### 4. Model Details

- [ ] **4.1** Base/novel/adapted/fine-tuned status
- [ ] **4.2** Customization or configuration details
- [ ] **4.3** Plugins, tools, or retrieval augmentation used

### 5. Prompt Engineering

- [ ] **5.1** Prompt development methodology
- [ ] **5.2** Source of prompts (clinical questions, exam banks, etc.)
- [ ] **5.3** Prompt testing and validation process
- [ ] **5.4** Role of domain experts in prompt design
- [ ] **5.5** Complete prompts provided (supplementary material)

### 6. Query Strategy

- [ ] **6.1** Access pathway (web interface, API, local deployment)
- [ ] **6.2** Date and location of queries
- [ ] **6.3** Session structure (new session per query vs. conversation)
- [ ] **6.4** Complete model outputs provided

### 7. Performance Evaluation

- [ ] **7.1** Reference standard definition
- [ ] **7.2** Evaluation process and criteria
- [ ] **7.3** Evaluator qualifications and number
- [ ] **7.4** Blinding of evaluators to model identity

### 8. Sample Size

- [ ] **8.1** Sample size justification

### 9. Data Analysis

- [ ] **9.1** Statistical analysis methods
- [ ] **9.2** Reproducibility assessment (repeated queries)

### 10. Results

- [ ] **10.1** Agreement with reference standard
- [ ] **10.2** Assessment of harmful/biased/misleading responses
- [ ] **10.3** Subgroup analysis if applicable

### 11. Discussion

- [ ] **11.1** Interpretation in clinical context
- [ ] **11.2** Comparison with prior studies
- [ ] **11.3** Limitations (model versioning, data contamination, generalizability)

### 12. Open Science

- [ ] **12.1** Data availability statement
- [ ] **12.2** Code/prompt availability
- [ ] **12.3** Funding disclosure
- [ ] **12.4** Conflict of interest disclosure
- [ ] **12.5** Ethics approval status

---

## Checklist Selection Logic

```
if study_type == "diagnostic_accuracy":
    if has_ai_component:
        use STARD 2015 + STARD-AI
    else:
        use STARD 2015

elif study_type == "prognostic":
    if has_ai_component:
        use TRIPOD + TRIPOD+AI
    else:
        use TRIPOD

elif study_type == "segmentation_ai":
    use CLAIM 2024

elif study_type == "screening":
    use STARD 2015 + screening-specific items

elif study_type == "interventional":
    use modified STARD for interventional

elif study_type == "llm_study":
    use MI-CLEAR-LLM 2025 + TRIPOD-LLM + DEAL
    if chatbot_evaluation:
        also use CHART
```

---

## Quick Reference

| Study Type | Primary Checklist | Items |
|------------|------------------|-------|
| Diagnostic Accuracy | STARD 2015 | 30 |
| Diagnostic Accuracy + AI | STARD 2015 + STARD-AI | 30 + 12 |
| Prognostic | TRIPOD | 22 |
| Prognostic + AI | TRIPOD + TRIPOD+AI | 22 + 5 |
| AI Segmentation | CLAIM 2024 | 48 |
| LLM Study | MI-CLEAR-LLM 2025 | 24 |
| LLM Study | + TRIPOD-LLM | + 50 |
| LLM Study | + DEAL | + 20 |
| LLM Chatbot Study | + CHART | + 39 |
