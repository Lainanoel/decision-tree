# Statistical Test Decision Tree Logic for Shiny App

This document outlines the decision-tree logic for a Shiny application designed to guide students in selecting appropriate statistical tests based on their dataset characteristics. The logic is informed by the provided `task02.docx` file and the academic sources: "Choosing the right statistical test: a decision tree approach" by Iván Palomares Carrascosa [1] and "Which is the correct statistical test to use?" by Evie McCrum-Gardner [2].

## Core Decision Points

The decision tree will primarily branch based on the following key questions, presented to the user in a sequential manner:

1.  **What is the main goal of the analysis?**
    *   Compare groups
    *   Relationship between variables
    *   Prediction / Classification

2.  **What is the type of the response variable?**
    *   Continuous (Interval/Ratio)
    *   Count
    *   Binary
    *   Proportion
    *   Ordinal

3.  **What is the experimental structure or observation type?** (Relevant for 'Compare groups' and 'Relationship between variables')
    *   Completely Randomized Design (CRD)
    *   Randomized Complete Block Design (RCBD)
    *   Split-plot
    *   Repeated measures
    *   Nested design
    *   Independent observations
    *   Paired samples
    *   Multivariate community/ecological data

4.  **Are assumptions met?** (e.g., Normality, Homogeneity of Variance, Overdispersion, Covariance assumptions)

## Decision Tree Branches

### A. Main Goal: Compare Groups

#### A.1. Response Variable Type: Continuous

*   **Experimental Structure: Completely Randomized Design (CRD)**
    *   **Number of groups: 2 groups**
        *   Assumptions met? (Normality, Homogeneity of Variance)
            *   Yes → **Independent t-test** [2]
            *   No → **Mann–Whitney U test** [1] [2]
    *   **Number of groups: >2 groups**
        *   Assumptions met? (Normality, Homogeneity of Variance)
            *   Yes → **One-way ANOVA** [1] [2]
            *   No → **Kruskal–Wallis test** [1] [2]

*   **Experimental Structure: Randomized Complete Block Design (RCBD)**
    *   Assumptions met? (Normality, Homogeneity of Variance)
        *   Yes → **RCBD ANOVA**
        *   No → **Friedman test** [2]

*   **Experimental Structure: Split-plot**
    *   Assumptions met? (Normality, Homogeneity of Variance)
        *   Yes → **Split-plot ANOVA**
        *   No → **Mixed model with transformed/nonparametric approach**

*   **Experimental Structure: Repeated measures**
    *   Normality + covariance assumptions met?
        *   Yes → **Repeated Measures ANOVA** [2]
        *   No → **Linear Mixed Model**

*   **Experimental Structure: Nested design**
    *   Assumptions met?
        *   Yes → **Nested ANOVA**
        *   No → **Mixed model**

#### A.2. Response Variable Type: Count

*   Overdispersion present?
    *   No → **Poisson GLM**
    *   Yes → **Negative Binomial GLM**

#### A.3. Response Variable Type: Binary

*   Independent observations?
    *   Yes → **Logistic regression** [1] [2]
    *   No → **Generalized Linear Mixed Model (GLMM)**

#### A.4. Response Variable Type: Proportion

*   Values bounded between 0 and 1?
    *   Yes → **Beta regression**
    *   Binomial counts → **Binomial GLM**

#### A.5. Response Variable Type: Ordinal

*   Independent observations?
    *   Yes → **Ordinal logistic regression**
    *   No → **Ordinal mixed model**

### B. Main Goal: Relationship between variables

#### B.1. Variable Types: Continuous vs Continuous

*   Normality met?
    *   Yes → **Pearson correlation / Linear regression** [2]
    *   No → **Spearman correlation** [1] [2]

#### B.2. Variable Types: Multivariate community/ecological data

*   Goal?
    *   Ordination → **PCA / NMDS**
    *   Group comparison → **PERMANOVA**

#### B.3. Variable Types: Binary response

*   Independent observations?
    *   Yes → **Logistic regression** [1] [2]
    *   No → **GLMM**

#### B.4. Variable Types: Count response

*   Overdispersion?
    *   No → **Poisson regression**
    *   Yes → **Negative Binomial regression**

#### B.5. Variable Types: Ordinal response

*   Independent observations?
    *   Yes → **Ordinal logistic regression**
    *   No → **Ordinal mixed model**

### C. Main Goal: Prediction / Classification

#### C.1. Outcome Type: Continuous

*   Linear relationship?
    *   Yes → **Multiple linear regression**
    *   No → **GAM or nonlinear regression**

#### C.2. Outcome Type: Binary

*   Repeated/nested structure?
    *   No → **Logistic regression** [1] [2]
    *   Yes → **GLMM**

#### C.3. Outcome Type: Count

*   Overdispersion?
    *   No → **Poisson regression**
    *   Yes → **Negative Binomial regression**

#### C.4. Outcome Type: Time-to-event / mortality / failure

*   Censored observations?
    *   Yes → **Survival analysis (Cox model)**
    *   No → **Parametric survival model**

#### C.5. Outcome Type: High-dimensional multivariate data

*   Goal?
    *   Dimension reduction → **PCA**
    *   Classification → **Discriminant analysis / Random Forest**

## App Structure and User Experience Considerations

*   **Interactive Questions:** The Shiny app will present questions sequentially, guiding the user through the decision tree. Each answer will dynamically update the subsequent questions.
*   **Information Tooltips:** Brief explanations of statistical terms (e.g., types of variables, experimental designs) will be provided via tooltips or expandable sections to assist students.
*   **Assumptions:** For tests requiring assumptions (e.g., normality, homogeneity of variance), the app will prompt the user to confirm if these are met. It will also offer non-parametric alternatives when assumptions are not met.
*   **Output:** The final output will be the recommended statistical test, along with a brief description of its purpose and when it is appropriate to use.

## References

[1] [Choosing the Right Statistical Test: A Decision Tree Approach](https://www.statology.org/choosing-the-right-statistical-test-a-decision-tree-approach/) by Iván Palomares Carrascosa
[2] [Which is the correct statistical test to use?](http://oralpathol.dlearn.kmu.edu.tw/case/Journal%20reading-intern-08-12/statistical%20use-review-BJOMFS-2008.pdf) by Evie McCrum-Gardner
