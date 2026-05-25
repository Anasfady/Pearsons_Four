# Pearsons Four — Team Guide / Guía del Equipo

**Bilingual — English / Español**

---

## 1. Team & Roles / Equipo y Roles

| Person | Primary Role | Also a Developer? |
|--------|-------------|-------------------|
| **Anas** | Scrum Master | Yes |
| **Juan** | Product Owner | Yes |
| **Isabella** | Developer | Yes |
| **Vanesa** | Developer | Yes |

**Scrum Master (Anas):** Facilitates the process, removes blockers, ensures Agile practices, manages the repo.
**Product Owner (Juan):** Prioritizes backlog, validates business questions are answered, prepares executive presentation.
**Developers (Everyone):** Write code in the Colab, create visualizations, document findings.

---

## 2. Workflow / Flujo de Trabajo

### GitHub Flow
```
main (protected — no direct pushes)
  └── feature/eda-carga           → PR → review → merge
  └── feature/analisis-sesgos     → PR → review → merge
  └── feature/slides-readme       → PR → review → merge
```

**Rules / Reglas:**
- Never push directly to `main` / Nunca hagas push directo a `main`
- Every change = a branch → Pull Request → someone reviews → merge
- Branch naming: `feature/lo-que-sea` or `fix/lo-que-sea`
- Commits in English, descriptive: `feat: load dataset and inspect`, `fix: handle null salaries`

### Single Colab Workflow
- **One notebook** shared by everyone (`EDA_completo.ipynb`)
- Each person writes ONLY their assigned sections (cells)
- Don't edit someone else's cells without asking
- At the end → one person downloads `.ipynb` and pushes to repo

### Daily Standup (5 min)
- What did I do yesterday? / ¿Qué hice ayer?
- What will I do today? / ¿Qué haré hoy?
- Any blockers? / ¿Algún impedimento?
- Led by Anas (SM), rotates if desired

---

## 3. Pair Programming

**How it works / Cómo funciona:**
- **Driver:** writes code, explains what they're typing
- **Navigator:** reviews, thinks ahead, spots errors, researches
- **Swap every 30 minutes** — no exceptions

**Pairs for this project:**
```
Days 1-2:    Pair A (Anas + Isabella)      Pair B (Juan + Vanesa)
Days 2-3:    Swap to cross-pollinate
```

---

## 4. Project Structure / Estructura del Proyecto

```
Proyecto_I_EDA/
├── data/
│   ├── ds_salaries.csv              ← Data Science Job Salaries
│   └── stack_overflow_survey.csv    ← Stack Overflow Survey
├── notebooks/
│   └── EDA_completo.ipynb           ← Single notebook (Colab)
├── docs/
│   └── GUIDE.md                     ← This file
├── screenshots/                     ← PNG captures of graphs
├── slides/                          ← Executive presentation
├── .github/
│   └── ISSUE_TEMPLATE/
│       └── task.md
└── README.md
```

---

## 5. Colab Setup / Configuración del Colab

1. **Anas creates** the Colab notebook and shares with everyone (edit access)
2. **Name:** `Pearsons_Four_EDA.ipynb`
3. Each person writes **only their sections**
4. Use text cells (Markdown) between each code block to explain findings in **English**

### Cell formatting:
```python
# === SECTION 1.1 ===
# by: Anas
# date: 2026-05-25
import pandas as pd
df = pd.read_csv('/content/ds_salaries.csv')
```

---

## 6. Task Distribution / Distribución de Tareas

### Dataset 1: Data Science Job Salaries (Main EDA)

#### Section 1 — Carga e inspección / Load & Inspect
**Owner: Anas**

| Cell | Code |
|------|------|
| Imports | `import pandas as pd`, `import numpy as np`, `import matplotlib.pyplot as plt`, `import seaborn as sns` |
| Load | `df = pd.read_csv('ds_salaries.csv')` |
| Inspect | `df.shape`, `df.dtypes`, `df.info()`, `df.describe()` |
| Preview | `df.head(10)` |
| MD | Brief interpretation: rows, columns, data types, salary range |

#### Section 2 — Limpieza / Cleaning (nulos + outliers)
**Owner: Anas**

| Cell | Code |
|------|------|
| Nulls | `df.isnull().sum()` — quantify nulls per column |
| Impute | Impute with median (salary) or mode (categorical), or justify removal |
| Outliers IQR | `Q1, Q3 = df['salary_in_usd'].quantile([0.25, 0.75])`; `IQR = Q3 - Q1`; filter `[Q1-1.5*IQR, Q3+1.5*IQR]` |
| Outliers Z-score | `from scipy import stats`; `np.abs(stats.zscore(df['salary_in_usd'])) < 3` |
| MD | Justify each decision: why impute vs delete, how many outliers removed |

#### Section 3 — Normalización de textos / Text Normalization
**Owner: Juan**

| Cell | Code |
|------|------|
| Lowercase | `df['job_title'] = df['job_title'].str.lower().str.strip()` |
| Clean | `df['employee_residence'] = df['employee_residence'].str.lower().str.strip()` |
| Group | Group similar titles (e.g., "data scientist", "data science" → "data scientist") |
| Country group | `df['pais_grupo'] = df['employee_residence'].apply(lambda x: 'Spain' if x == 'es' else 'Other')` |
| MD | What was normalized and why |

#### Section 4 — Feature Engineering
**Owner: Juan**

| Cell | Code |
|------|------|
| Salary range | `df['rango_salarial'] = pd.cut(df['salary_in_usd'], bins=[0,50000,100000,200000,500000], labels=['bajo','medio','alto','muy_alto'])` |
| Level numeric | `df['nivel_experiencia_num'] = df['experience_level'].map({'EN':1, 'MI':2, 'SE':3, 'EX':4})` |
| Employment type | `df['employment_type_num'] = df['employment_type'].map({'FT':1, 'PT':2, 'CT':3, 'FL':4})` |
| MD | Explain what features were created and their potential use in a predictive model |

#### Section 5 — Estadística descriptiva / Descriptive Statistics
**Owner: Isabella**

| Cell | Code |
|------|------|
| Mean | `df[['salary_in_usd', 'nivel_experiencia_num', 'remote_ratio']].mean()` |
| Median | `.median()` |
| Std | `.std()` |
| Percentiles | `.describe(percentiles=[.25, .5, .75, .9])` |
| MD | Interpret: mean vs median (skewed distribution), what it means for DataTalent |

#### Section 6 — Correlaciones + Heatmap / Correlations
**Owner: Vanesa**

| Cell | Code |
|------|------|
| Corr matrix | `corr = df[['salary_in_usd','nivel_experiencia_num','remote_ratio','employment_type_num']].corr()` |
| Heatmap | `sns.heatmap(corr, annot=True, cmap='coolwarm')` + title, labels |
| MD | Which variables correlate most with salary, business interpretation |

#### Section 7 — GroupBy / Group Comparison
**Owner: Vanesa**

| Cell | Code |
|------|------|
| By level | `df.groupby('experience_level')['salary_in_usd'].agg(['mean','median','count'])` |
| By country | `df.groupby('employee_residence')['salary_in_usd'].mean().sort_values(ascending=False).head(10)` |
| By contract | `df.groupby('employment_type')['salary_in_usd'].mean()` |
| Pivot table | `pd.pivot_table(df, values='salary_in_usd', index='experience_level', columns='employment_type', aggfunc='mean')` |
| MD | Salary differences by level, country, contract type. Key insights for DataTalent |

#### Section 8 — Visualizations (Anas)
**Owner: Anas**

| Cell | Code |
|------|------|
| **Graph 1** — Histogram + KDE | `sns.histplot(df['salary_in_usd'], kde=True, bins=30)` — title: "Salary Distribution in Data Science (2020-2024)" |
| **Graph 2** — Boxplot by level | `sns.boxplot(x='experience_level', y='salary_in_usd', data=df)` |
| MD | Interpretation of each graph |

#### Section 9 — Visualizations (Isabella)
**Owner: Isabella**

| Cell | Code |
|------|------|
| **Graph 3** — Countplot top countries | `df['employee_residence'].value_counts().head(10).plot(kind='bar')` |
| **Graph 4** — Salary by employment type | `sns.barplot(x='employment_type', y='salary_in_usd', data=df)` |
| MD | Interpretation |

#### Section 10 — Visualizations (Vanesa)
**Owner: Vanesa**

| Cell | Code |
|------|------|
| **Graph 5** — Top job titles | `df['job_title'].value_counts().head(10).plot(kind='barh')` |
| **Graph 6** — Scatter exp vs salary | `sns.scatterplot(x='nivel_experiencia_num', y='salary_in_usd', data=df, alpha=0.5)` |
| MD | Interpretation |

#### Section 11 — Visualizations (Juan)
**Owner: Juan**

| Cell | Code |
|------|------|
| **Graph 7 (extra)** — Wordcloud skills | (install wordcloud) `WordCloud().generate(' '.join(df['job_title']))` |
| **Graph 8 (extra)** — Salary by year | `sns.boxplot(x='work_year', y='salary_in_usd', data=df)` |
| MD | Interpretation |

---

### Dataset 2: Stack Overflow Survey (Bias Analysis)

#### Section 12 — Sesgos / Bias Report
**Owner: Juan + Everyone**

| Cell | Code |
|------|------|
| Load | `so = pd.read_csv('stack_overflow_survey.csv')` |
| Explore | `so.shape`, `so.info()`, `so.head()` |
| **Bias 1: Geographic underrepresentation** | `so['Country'].value_counts(normalize=True).head(10)` — show developed countries overrepresented |
| MD | Explain: this dataset overrepresents US/UK, underrepresents Spain/LATAM. DataTalent would make wrong reskilling decisions for Spain using this data. |
| **Bias 2: MNAR (Missing Not At Random)** | `so['salary'].isnull().sum()` — compare null rates by country |
| MD | Explain: high earners don't report salary. A model trained only on reported salaries would underestimate real salaries. |
| Conditional probability | P(skill|high_salary), P(gender|role) — applied to DataTalent's case |
| **Impact on predictive model** | Text: If DataTalent trained a salary predictor with this data, it would underestimate salaries in underrepresented groups and high ranges. Mitigation: use local data or weight adjustments. |

---

## 7. Definition of Done (DoD)

A task is complete when:

- [ ] Code runs without errors
- [ ] All code cells have a corresponding **Markdown interpretation** in English
- [ ] Graphs have: title, axis labels, legend (if applicable)
- [ ] Peer has reviewed and approved
- [ ] PR merged to main

---

## 8. Final Checklist / Lista de Verificación

| Item | Who verifies |
|------|-------------|
| Minimum 6 graphs with title + labels + interpretation | **Anas** |
| At least 1 distribution graph (histogram + KDE) | **Anas** |
| 2+ biases identified with origin and impact | **Juan** |
| groupby() used and interpreted | **Vanesa** |
| Correlations matrix + heatmap | **Vanesa** |
| Mean/median/std of 3+ key variables | **Isabella** |
| Every cleaning decision justified | **Anas** |
| Nulls quantified and treated | **Anas** |
| Executive presentation (10 min slides) | **Juan** |
| Notebook uploaded to GitHub + README updated | **Anas** |

---

## 9. Glossary / Glosario

| English | Spanish | Definition |
|---------|---------|------------|
| **EDA** | Análisis Exploratorio de Datos | Process of examining datasets to summarize main characteristics |
| **NaN** | Valor nulo | Missing value in a dataset |
| **Outlier** | Valor atípico | Data point far from others; may be error or genuine extreme |
| **Standard Deviation** | Desviación estándar | How spread out values are from the mean |
| **Correlation** | Correlación | How two variables change together (-1 to +1) |
| **Bias** | Sesgo | Systematic error that makes certain groups over/underrepresented |
| **MNAR** | Missing Not At Random | Missing data where absence depends on the unobserved value itself |
| **IQR** | Rango Intercuartílico | Q3 - Q1, used to detect outliers |
| **Conditional Probability** | Probabilidad condicional | P(A\|B): probability of A given B occurred |
| **Pair Programming** | Programación en pareja | Driver writes code, Navigator reviews in real time |
| **Reskilling** | Recapacitación | Training someone in new skills for a different role |

---

*Pearsons Four — Module II: Data Analysis & Visualization*
*May 2026*
