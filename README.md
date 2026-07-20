# Remote Work and Productivity: The Role of Home Office Quality

**Author:** Nuria García Ruiz de Alarcón · **Last compiled:** April 12, 2026

A statistical analysis examining whether the number of days an employee works from home each week is associated with their productivity, and whether that relationship depends on the quality of their home office setup. The analysis combines descriptive comparisons with a sequence of linear regression models (simple → multiple → home-office-quality → interaction) to isolate the effect of home office quality from WFH frequency itself.

## Research question

Does the relationship between work-from-home (WFH) frequency and employee productivity depend on the quality of the employee's home office environment — or does home office quality matter on its own, independent of how many days someone works remotely?

## Key finding

The **Home Office Quality Model** was selected as the best-fitting model (lowest AIC, highest adjusted R²: 0.317). It shows that `wfh_days_per_week` has a small, non-significant effect on productivity once other factors are controlled for, while employees with a **Good/Excellent** home office score about **3.35 points higher** on productivity than those with **Poor/Average** setups — an effect that holds regardless of how many days they work from home. The interaction model found no meaningful interaction between WFH frequency and home office quality, so the two do not appear to compound.

## Data source

| File | Description |
|---|---|
| `data/remote_work_productivity_analysis.csv` | 1,500 employee-level observations, 17 variables (demographics, job info, work patterns, wellbeing, and the productivity outcome). Employees are identified only by synthetic IDs (`EMP0001`–`EMP1500`), no names or other direct identifiers. |

This dataset was derived from a broader remote-work project shared in the [`RemoteWorkAnalysis`](https://github.com/betsysavage/RemoteWorkAnalysis) GitHub repository, which in turn is based on the New South Wales Productivity Commission's *Remote Working Insight Surveys* (2020–2021) of Australian employees whose jobs could be performed remotely.

> **Before republishing `remote_work_productivity_analysis.csv`:** the source repository's license wasn't clearly stated at the time this README was written. Since the data only contains synthetic employee IDs (no real names/emails), the re-identification risk is low, but confirm the source repo's/survey's terms permit redistribution before including the raw CSV here — or link to the original source instead of re-hosting it.

## Variables used in the analysis

| Variable | Type | Description |
|---|---|---|
| `productivity_score` | Continuous | Main outcome variable |
| `wfh_days_per_week` | Discrete numeric | Main predictor — days worked from home per week (0–5) |
| `home_office_quality` / `home_office_group` | Categorical | Poor / Average / Good / Excellent, later collapsed into Poor-Average vs Good-Excellent |
| `work_hours_per_week`, `meetings_per_week` | Numeric | Workload controls |
| `stress_level`, `job_satisfaction`, `work_life_balance` | Numeric | Wellbeing controls |
| `years_experience`, `job_level`, `department`, `age` | Numeric/Categorical | Background controls |

## Methodology

Five models were compared using AIC and adjusted R²:

1. Descriptive group comparison (Low / Hybrid / High WFH)
2. Simple linear regression (`productivity_score ~ wfh_days_per_week`)
3. Multiple linear regression (+ workload, wellbeing, experience, job level controls)
4. Home Office Quality model (+ `home_office_group`) — **selected as final model**
5. Interaction model (`wfh_days_per_week * home_office_group`)

This is an explanatory (not predictive) analysis, so no train/test split was used.

## Project structure

```
.
├── remote_work_productivity_analysis.Rmd    # Full R Markdown analysis (EDA, modeling, diagnostics)
├── remote_work_productivity_analysis.pdf    # Rendered/compiled report
├── remote_work_productivity.pptx            # Slide deck presenting the study
├── data/
│   └── remote_work_productivity_analysis.csv
    └── remote_work_productivity.csv
├── requirements.txt
├── LICENSE
└── README.md
```

## Getting started

```bash
# 1. Clone the repo
git clone <this-repo-url>
cd <this-repo>

# 2. Install R dependencies (see requirements.txt)
Rscript -e 'install.packages(c("tidyverse", "janitor", "knitr"))'

# 3. Render the R Markdown report
Rscript -e 'rmarkdown::render("remote_work_productivity_analysis.Rmd")'
```

The `.Rmd` reads its input from `data/remote_work_productivity_analysis.csv` — keep it inside a `data/` folder alongside the file, or the `read_csv()` call will fail.

## Deliverables

- **`remote_work_productivity_analysis.pdf`** — the full compiled report: introduction, data description, EDA, methodology, results, discussion, and appendix with diagnostic plots.
- **`remote_work_productivity.pptx`** — slide deck summarizing the study for a general audience.

## Limitations & future work

- Adjusted R² of the final model (~0.32) means a majority of variation in productivity is unexplained by the included variables.
- Residual diagnostics show some non-constant variance and a few influential/outlier observations (see Appendix in the PDF).
- Future iterations could add collaboration/manager-support variables, break results down by department or job level, and validate the model against real-world outcomes.

## License

The code and analysis in this repository (R Markdown, rendered report, slides) are released under the MIT License — see [`LICENSE`](./LICENSE). This does not extend to the underlying survey data, which originates from the NSW Productivity Commission surveys and the `RemoteWorkAnalysis` repository — see the note under [Data source](#data-source).
