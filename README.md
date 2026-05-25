# Pearsons Four - EDA Data Science Job Salaries

**Exploratory Data Analysis on global Data Science salaries (2020–2024) + bias analysis with Stack Overflow Survey**

## Team

| Role | Name |
|------|------|
| Scrum Master / Developer | **Anas** |
| Product Owner / Developer | **Juan** |
| Developer | **Isabella** |
| Developer | **Vanesa (double s)** |

## Project Overview

DataTalent Solutions S.L., an HR consultancy specializing in tech profiles, needs empirical evidence on which technical skills are in demand in the Spanish data job market. This project performs a full EDA to answer:

- Most frequently demanded technical skills in data roles
- Salary distribution biases (gender, city, contract type)
- Industry sectors with more offers and better salaries
- Correlations between experience, skills, and salary
- How incomplete or biased data could lead to wrong business decisions

## Datasets

| Dataset | Source | Purpose |
|---------|--------|---------|
| **Data Science Job Salaries** | Kaggle | Main EDA: salaries, skills, experience, correlations |
| **Stack Overflow Developer Survey** | Stack Overflow | Bias analysis: demographic representation, MNAR |

## Repository Structure

```
├── data/                    # Datasets (or links)
├── notebooks/
│   └── EDA_completo.ipynb   # Single notebook with full analysis
├── docs/
│   └── GUIDE.md             # Team handbook (bilingual)
├── screenshots/             # Visualization captures
├── slides/                  # Executive presentation (10 min)
├── .github/
│   └── ISSUE_TEMPLATE/      # Issue templates
└── README.md
```

## Deliverables

- [x] Clean Jupyter notebook with full EDA (markdown between code blocks)
- [x] GitHub repository with README, dataset, notebook, screenshots
- [x] Executive presentation (slides, 10 min) for non-technical audience
- [x] Bias report section inside the notebook

## Methodology

- **GitHub Flow**: feature branches → PR → review → merge to main
- **Pair Programming**: rotating pairs, alternating driver/navigator every 30min
- **Single Colab**: all team members work on the same notebook in parallel sections
- **Daily standup**: 5 min, 3 questions

## License

Educational project - Module II: Data Analysis & Visualization
