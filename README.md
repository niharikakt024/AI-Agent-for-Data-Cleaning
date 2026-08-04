# 🧹 AI Data-Cleaning & EDA Agent

An autonomous agent that inspects a messy, real-world dataset, reasons about what's wrong with it using an LLM, and automatically applies fixes — producing a clean dataset and a plain-English summary report.

Built as a hands-on project to develop practical, agentic AI skills for data analysis: profiling data, reasoning with an LLM, executing structured fix plans, and validating results.

---

## 🎯 What it does

Given any messy CSV, the agent:

1. **Profiles** every column — data type, missing values, uniqueness, sample values
2. **Reasons** about each column's issues using an LLM (Groq / Llama 3.3 70B), which returns a structured JSON cleaning plan
3. **Executes** that plan automatically — standardizing text casing, parsing inconsistent date formats, cleaning currency strings into numeric values, and filling missing values (median for numeric, mode for categorical)
4. **Detects and removes duplicate records**
5. **Generates a summary report** documenting exactly what was changed and why

## 🧠 Why this matters

Data cleaning is estimated to take up 60–80% of a real analyst's time. This project demonstrates the ability to automate that process end-to-end — combining traditional pandas-based data engineering with LLM-driven decision-making, which is increasingly how modern data tooling works.

## 🛠️ Tech stack

| Component | Tool |
|---|---|
| Language | Python |
| Data handling | pandas, numpy |
| LLM reasoning | Groq API (Llama 3.3 70B) |
| Environment | Google Colab |

All tools used are free tier.

## 📊 Example: before → after

**Input issues detected and fixed:**

| Column | Issue | Fix Applied |
|---|---|---|
| `Department` | Inconsistent casing (`sales`, `Sales`, `SALES`) + missing values | Standardized to consistent categories, missing filled with mode |
| `Gender` | Mixed formats (`M`, `Male`, `F`, `female`) | Mapped to consistent `Male`/`Female` values |
| `JoinDate` | Multiple date formats in the same column | Parsed into a single standardized datetime format |
| `Salary` | Stored as text, mixed with `$` and commas | Cleaned and converted to numeric |
| `Age` | Missing values | Filled with column median |
| `EmployeeID` | 40 exact duplicate rows | Detected and removed |

**Result:** 520 raw rows → 500 clean, de-duplicated rows with zero missing values and fully consistent formatting.

## 🚀 How it works (architecture)

```
Raw CSV
   │
   ▼
[Profiler] ── inspects each column (dtype, missing %, samples)
   │
   ▼
[LLM Agent] ── receives profile, returns a structured JSON fix plan with reasoning
   │
   ▼
[Executor] ── applies the plan: case standardization, date parsing,
              currency cleaning, missing-value imputation
   │
   ▼
[Validator] ── detects and removes duplicate records
   │
   ▼
[Reporter] ── generates a before/after summary + downloadable clean CSV
```

## 📁 Files

- `data_cleaning_agent.ipynb` — the full Colab notebook, runnable end-to-end
- `cleaned_data.csv` — example cleaned output
- `cleaning_report.txt` — example generated report

## ▶️ How to run it yourself

1. Open the notebook in [Google Colab](https://colab.research.google.com)
2. Get a free API key from [Groq Console](https://console.groq.com)
3. Add it as a Colab secret named `GROQ_API_KEY`
4. Upload any messy CSV and run all cells
5. Download your cleaned dataset and report

## 🔮 Next steps

- Add outlier detection (IQR / z-score based) with LLM-suggested handling strategies
- Extend the agent to handle multiple file formats (Excel, JSON)
- Turn this into a Streamlit app for a live, no-code interface
- Add a self-correction loop where the agent re-profiles after cleaning and flags anything it missed

---

*Built as part of a self-directed learning project exploring agentic AI workflows for data analysis.*
