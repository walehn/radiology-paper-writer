# Radiology Research Checklists

영상의학 연구 논문 작성을 위한 표준 체크리스트 모음

## Table of Contents
- [STARD 2015](#stard-2015) - Diagnostic Accuracy Studies
- [STARD-AI Extension](#stard-ai-extension) - AI Diagnostic Studies
- [TRIPOD](#tripod) - Prediction Model Studies
- [TRIPOD+AI](#tripod-ai) - AI Prediction Studies
- [CLAIM 2024](#claim-2024) - AI in Medical Imaging

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
