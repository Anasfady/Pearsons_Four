# Pearsons Four — EDA Data Science Job Salaries

<p align="center">
  <img src="screenshots/banner.png" alt="Pearsons Four Banner" width="800">
</p>

**Exploratory Data Analysis: LinkedIn job postings + Stack Overflow bias analysis for DataTalent Solutions S.L.**

---

## Team

| Rol | Nombre | GitHub |
|-----|--------|--------|
| **Responsable de Ingeniería de Datos** (Data Wrangler) — Fases 1 & 2 | **Juan** | @Juan de la Fuente Larrocca |
| **Responsable de Análisis Estadístico** — Fase 3 | **Isabela** | @Isabela |
| **Responsable de Visualización** (Data Storyteller) — Fase 4 | **Anas** | @Anas28 |
| **Consultora de Estrategia y Ética de Datos** — Sesgos | **Vanessa** | @Vanessa |

> **Scrum Master:** Anas | **Product Owner:** Juan

---

## Project Context

DataTalent Solutions S.L., an HR consultancy specializing in tech profiles, needs **empirical evidence** on which technical skills are in demand in the Spanish data job market. This project performs a full EDA across **two datasets** to answer:

1. Most frequently demanded technical skills in data roles
2. Salary distribution biases (experience, remote work, location)
3. Industry sectors with more offers and competitive salaries
4. Correlations between experience, skills, and salary
5. How incomplete or biased data could lead to wrong business decisions

---

## Datasets

| Dataset | Source | Rows | Purpose |
|---------|--------|------|---------|
| **LinkedIn Job Postings** | Kaggle (arshkon) | 123,849 | Main EDA: job market, salaries, skills, industries |
| **Stack Overflow Developer Survey 2025** | Stack Overflow | 49,191 | Bias analysis: demographic representation, MNAR, conditional probability |

---

## Repository Structure

```
Pearsons_Four/
├── screenshots/                 # Banner + graph captures
│   └── banner.png
├── docs/
│   ├── GUIDE.md                 # Team handbook with task distribution
│   └── trello_template.json
├── notebooks/
│   ├── Pearsons_Four_EDA_Linkedin.ipynb              # LinkedIn EDA (full, Juan + Isabela + Anas)
│   └── VGGPearsonsFour.ipynb                         # Bias analysis (Stack Overflow + Cross-dataset, Vanessa)
├── data/
│   └── linkedin_data_roles_procesed.csv              # Cleaned LinkedIn data roles
├── slides/
│   └── presentacion_pearsons_four.pdf                # Executive presentation
├── .github/
│   └── ISSUE_TEMPLATE/
│       └── task.md
└── README.md
```

---

## Key Findings

### 1. Data Wrangling (Juan)

| Metric | LinkedIn Dataset |
|--------|-----------------|
| Rows / Columns | 123,849 × 31 |
| Total nulls | 1,269,564 (70.87% in salaries) |
| Duplicates | 0 |
| Data role postings | 1,831 (1.5% of total) |
| Outliers (IQR) | 14 (2.3% of salaried) |
| Outliers (z-score) | 1 (0.2%) |
| **Decision** | Removed salaries < $1K and > $2M |

**Key normalization applied:**
- `.str.lower().str.strip()` on `title`, `location`, `company_name`
- Experience levels mapped to numeric: Internship=0 through Executive=5
- City extracted from location field
- IQR filter: kept 607 clean records with reliable salaries

### 2. Statistical Analysis (Isabela)

| Metric | LinkedIn (Data Roles) |
|--------|----------------------|
| Clean records for analysis | 607 |
| **Mean salary** | $139,844 |
| **Median salary** | $135,588 |
| Salary range (IQR-cleaned) | $35,360 – $265,000 |
| **Remote premium** | +45.1% (Remote: $112,500 vs On-site: $77,500 median) |

**Experience vs Salary:**
| Level | Mean Salary |
|-------|-------------|
| Internship | $54,126 |
| Entry Level | $122,322 |
| Associate | $101,394 |
| Mid-Senior | $144,213 |
| Director | $201,739 |
| Executive | $227,000 |

**Hypothesis Testing (ANOVA):**
- p-value < 0.0001 → **Experience level significantly affects salary** (reject H₀)
- Pearson correlation (Experience → Salary): **r = 0.43**

**Conditional Probability (LinkedIn):**
- P(High Salary | Python) = **8.00%**
- P(High Salary | No Python) = **7.09%**
- Python mention increases high-salary probability by ~13%

### 3. Correlations (Isabela)

| Variable Pair | Correlation |
|--------------|-------------|
| Experience level → Salary | **0.43** (moderate positive) |
| Remote allowed → Salary | **0.18** (weak positive) |
| Views → Applies | **0.62** (moderate positive) |

### 4. Stack Overflow — Bias Analysis (Vanessa)

| Bias Type | Finding | Impact on AI |
|-----------|---------|-------------|
| **Geographic** | US/UK overrepresented; 0 postings from Spain in LinkedIn data | Model would not generalize to Spanish market |
| **Salary MNAR** | 70.87% missing salaries; 47.33% of juniors hide salary vs 23.86% seniors | Model would **underestimate** junior salaries |
| **Selection Bias** | 83.81% Senior/Experienced vs 16.19% Junior/Early-Career in sample | Algorithm penalizes junior profiles by lack of training data |
| **Skills sparsity** | 98.03% missing `skills_desc` column | Cannot reliably extract skills → use `job_skills` table instead |

**Conditional Probability (Stack Overflow):**
- P(High Salary | Senior/Experienced) = **53.42%**
- P(High Salary | Junior/Early-Career) = **22.57%**

### 5. Cross-Dataset Comparison (Vanessa)

| Metric | Stack Overflow (Community) | LinkedIn (Marketplace) |
|--------|---------------------------|----------------------|
| Clean sample | 18,981 | 607 |
| Median salary | $85,000 | **$135,588** |
| Mean salary | $109,735 | **$139,844** |
| Perspective | Developer self-reported | Corporate real offers |

**Key insight:** LinkedIn offers reflect real market rates (+59% median vs SO survey). Programs must be priced on LinkedIn data, not inflated SO self-reports.

---

## Visualization Previews

> *Add screenshots of key graphs here:*
> - `screenshots/histogram_kde.png` — Salary Distribution
> - `screenshots/boxplot_experience.png` — Salary by Experience
> - `screenshots/top_roles.png` — Top Data Roles
> - `screenshots/top_industries.png` — Top Industries
> - `screenshots/correlation_heatmap.png` — Correlation Matrix
> - `screenshots/bias_mnar.png` — MNAR Bias Evidence
> - `screenshots/cross_dataset_comparison.png` — SO vs LinkedIn KDE

---

## Methodology

- **GitHub Flow**: feature branches → PR → review → merge to main
- **Pair Programming**: rotating pairs every 30 min
- **Single Colab**: all team members work on the same notebook
- **Daily standup**: 5 min, 3 questions (led by SM)

---

## Deliverables Status

| Deliverable | Owner | Status |
|------------|-------|--------|
| LinkedIn Job Postings EDA notebook | Juan + Isabela + Anas | ✅ Complete |
| Stack Overflow bias analysis notebook | Vanessa | ✅ Complete |
| Cross-dataset comparison | Vanessa | ✅ Complete |
| Executive presentation (10 min slides) | Vanessa (leads) + Juan | 🔄 In progress |
| README with results | Anas | ✅ Complete |
| GUIDE.md with task distribution | Anas | ✅ Complete |

---

## License

Educational project — Module II: Data Analysis & Visualization  
*Pearsons Four — May 2026*
