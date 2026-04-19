# TCGA Survival Analysis Pipeline

> You found a gene that's upregulated in cancer. So what? The question that matters is: does it predict whether patients live or die? This pipeline connects gene expression to patient survival using TCGA data.

## Why Survival Analysis

A gene can be differentially expressed, part of an important pathway, even a known oncogene, but if its expression doesn't associate with patient outcomes, its clinical utility is limited.

## What This Pipeline Does

1. Downloads TCGA data for any cancer type via TCGAbiolinks
2. Splits patients into high/low expression groups (median split)
3. Kaplan-Meier curves with log-rank test
4. Multivariate Cox regression controlling for age, stage, and sex
5. Forest plots of hazard ratios

## How to Read a Kaplan-Meier Plot

X-axis = time. Y-axis = probability of being alive. Each step down = a death event. If the curves separate clearly, your gene predicts survival.

But the p-value doesn't tell you if the effect is clinically meaningful. A gene might split patients with p = 0.001 but only 2% difference in 5-year survival. Always look at the actual curves.

## Cox Regression Matters More Than KM

KM is univariate. Maybe high expression correlates with advanced stage, and stage is what's actually killing patients. Cox regression controls for confounders. If your gene survives adjustment, it's an independent prognostic marker.

## Usage
```bash
Rscript survival_analysis.R --cancer LUAD --genes "CXCL9,CXCL10,CCL5"
```

## A Warning About Multiple Testing

Test 50 genes against survival, 2-3 will be "significant" by chance. If you're testing a specific hypothesis, fine. If you're fishing, apply FDR correction.
