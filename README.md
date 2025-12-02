Student Performance — Data Exploration & Analysis (Assignment README)

A focused exploratory data analysis pipeline that ingests two student datasets, cleans and merges them, inspects score distributions by demographic groups, visualises performance, and performs non-parametric hypothesis testing to check for differences between age groups.

Table of contents:

- Overview
- What the code does 
- Data
- Key findings 
- Requirements
- How to run
- Files & code structure
- Detailed pipeline / steps performed
- Visualisations produced
- Statistics & tests
- Decisions, assumptions & notes
- Next steps / suggestions
- License & contact

 --- Overview

This repository contains a reproducible analysis of student performance. The pipeline reads two CSV files (part_2a.csv with student demographics and scores; part_2b.csv with click-event activity), cleans and merges them, explores distributions and group differences, draws relevant visualisations (histograms, boxplots, heatmap) and runs statistical tests (normality + Kruskal–Wallis).

The analysis focuses on how scores vary by highest_education and age_band, plus a short consideration of click activity after merging.

 --- What the code does

Loads libraries: numpy, scipy, pandas, matplotlib, seaborn.
Reads part_2a.csv and part_2b.csv.
Drops a useless indexing column and all rows with missing values.
Removes duplicate rows from part_2a.
Left-joins the datasets on id_student.
Produces descriptive summaries, group means (by education and age), score bins, and counts.
Visualises distributions and outliers; preserves outliers (deliberate).
Tests normality (Anderson–Darling) and uses Kruskal–Wallis to compare age-band score distributions.
Prints conclusions and plots.

 --- Data

Put your CSV files in the location the script expects, or update the paths:
part_2a.csv — demographic & score data (columns include id_student, gender, region, highest_education, age_band, disability, final_result, score, etc.)
part_2b.csv — click activity per student (id_student, click_events)
Example paths used in the script (change to relative paths for portability):
C:/Users/aldom/Msc Data Science/Modules/Module 3 - Programming for Data Science/Assessment/Assessment resources/part_2a.csv
C:/Users/aldom/Msc Data Science/Modules/Module 3 - Programming for Data Science/Assessment/Assessment resources/part_2b.csv

 --- Key findings 

After cleaning and merging: 25,300 students remain in the merged dataset (click_events non-null count = 23,984).
Removed 1,427 duplicate rows from part_2a.
Education groups mean scores (highest → lowest):
Post Graduate Qualification: 82.16
HE Qualification: 75.75
A Level or Equivalent: 74.09
Lower Than A Level: 71.51
No Formal quals: 66.49
Age-band mean scores:
0-35: 72.61
35-55: 75.02
55<=: 77.27
Students scoring < 50: 1,876
Fail: 5,654; Withdrawn: 4,848
Outliers in score (using 1.5×IQR rule): 520 (kept intentionally)

Normality test (Anderson–Darling): the score distribution is not normal (statistic ≫ critical values).
Kruskal–Wallis test across age bands: H ≈ 196.26, p ≈ 2.41e-43 → reject null: at least one age group differs in score distribution.

 --- Requirements

Create an environment and install required packages:

# Recommended: create a venv
python -m venv .venv
source .venv/bin/activate          # macOS / Linux
.venv\Scripts\activate             # Windows PowerShell

pip install -r requirements.txt

Example requirements.txt (minimum):

pandas>=1.5
numpy
scipy
matplotlib
seaborn

 --- How to run

If the code is in a single script (e.g. analysis.py), run:

python analysis.py


If you run in a Jupyter Notebook, run the notebook cells in order. Update CSV file paths at the top of the script/notebook to match your machine or make them relative.

 --- Files & code structure

├── data/
│   ├── part_2a.csv
│   └── part_2b.csv
├── notebooks/
│   └── eda.ipynb           # exploratory notebook (optional)
├── src/
│   └── analysis.py         # main script (the code you provided)
├── outputs/
│   ├── figures/            # plots exported here (png/pdf)
│   └── reports/            # any summary tables
├── requirements.txt
└── README.md

 --- Detailed pipeline / steps performed

Load libraries: pandas, numpy, scipy, matplotlib, seaborn.

Read CSVs using pd.read_csv.

Initial inspection: head(), describe() — look for missing values.

Drop Unnamed: 0 column (index artefact) and dropna() to remove rows with nulls.

Check duplicates and remove full-row duplicates (drop_duplicates()).

Merge with pd.merge(..., how='left') on id_student.

Compute group statistics (mean scores by highest_education and age_band).

Bin scores into 5-point ranges and create pivot/heatmap counts.

Outlier detection: IQR method; count and visualise but keep them.

Normality test: Anderson–Darling.

Group comparison: Kruskal–Wallis (non-parametric ANOVA alternative) across age bands.

Visualisations: histogram with category overlay, countplot for age bands, boxplots by age, heatmap of binned scores.

 --- Visualisations produced

Histogram of scores (with KDE)

Histogram of scores grouped by >=50 / <50

Countplot of age_band

Boxplots of score by age_band (outliers visible)

Horizontal heatmap of student counts per 5-point score bin

Boxplot with IQR boundary lines showing outliers

Tip: Save plots to outputs/figures/ by calling plt.savefig("outputs/figures/plot_name.png", bbox_inches='tight') after each plt.show() if you want exportable images.

 --- Statistics & tests

Normality: Anderson–Darling; the score distribution strongly rejects normality.

Group difference (age_band): Kruskal–Wallis H test used because distribution is non-normal; result indicates statistically significant differences in score distributions across age bands (p ≪ 0.05).

Interpretation: there are meaningful differences between age groups in student scores; follow-up pairwise tests with multiple-comparison correction (e.g., Dunn’s test + Bonferroni/Benjamini–Hochberg) are recommended to identify which specific pairs differ.

 --- Decisions, assumptions & notes

Dropped rows with any null values — this is simple and safe for this assignment, but consider imputation for click_events (or using fillna(0)) if you want to keep more rows for analyses involving activity.

Removed Unnamed: 0 because it was redundant and could cause merge/index issues.

Duplicates: full-row duplicates were removed for part_2a. If duplicates in id_student are expected (e.g., multiple records per student over time), a different de-duplication strategy would be needed.

Outliers: intentionally kept — they represent real high/low scoring students, not measurement errors (per your notes).

Normality assumption violated, hence use of non-parametric testing for comparing groups.

 --- License & contact

This README and the accompanying code can be used under the MIT License
