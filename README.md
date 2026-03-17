# Herbal Article Creator — Research Data Repository

This repository contains all data, outputs, and analysis artifacts from an automated multi-agent AI pipeline designed to generate research-backed wellness articles about Thai medicinal herbs. The system orchestrates multiple LLMs through a structured 17-task workflow, producing publication-ready articles with rigorous quality evaluation.

---

## Repository Structure

```
crew-herbal-article-creator-data/
├── [95 articles] Herbal Article [selected]/     # Main dataset: 95 herb articles
├── [1 article - 1st time] Experiment Thesis Article - baseline/  # Initial baseline experiment
├── [1 article] blind model testing Herbal Article/              # Blind multi-model comparison
├── Model Exploration with Visualization/        # Jupyter notebooks & analysis CSVs
└── summary_report_95_articles.csv              # Master evaluation summary for all 95 articles
```

---

## Folders

### `[95 articles] Herbal Article [selected]/`

The main dataset. Contains 95 sub-folders, each named after a Thai medicinal herb (e.g., ขมิ้นชัน, ขิง, กระเทียม). Each herb folder contains two output runs:

```
[herb-name]/
├── outputs - 1/    # First run
│   ├── task_1_*.txt   ... task_17_*.txt   # Individual pipeline task outputs
│   └── research_paper.docx               # Final generated article
└── outputs - 2/    # Second run (same structure)
```

The `task_*.txt` files correspond to each stage of the 17-task generation pipeline (see [Pipeline](#article-generation-pipeline) below). The final `research_paper.docx` is the synthesized, publication-ready wellness article.

---

### `[1 article - 1st time] Experiment Thesis Article - baseline/`

The first baseline experiment — a single herb (Turmeric / ขมิ้นชัน) article used to validate the pipeline before scaling to 95 herbs. Contains:

- Full task output files (`task_1_*.txt` through `task_17_*.txt`)
- `research_paper.docx` — the baseline article
- `benchmark_meta_llama-3.3-70b-instruct.json` — performance benchmark for the LLaMA 3.3-70B model

Use this folder to understand the full pipeline output structure for a single end-to-end run.

---

### `[1 article] blind model testing Herbal Article/`

A controlled blind evaluation comparing five different LLM combinations on the same herb article task. Groups:

| Group | Models Used |
|-------|-------------|
| A | LLaMA 3.1 + LLaMA 3.3 |
| B | GPT + Claude |
| C | Gemini + LLaMA 3.3 |
| D | GPT + LLaMA 3.3 |
| E | LLaMA 3.3 + Claude |

Each group folder follows the same `task_*.txt` + `research_paper.docx` structure. Results were used to evaluate which model combinations produced the most accurate and high-quality articles, informing the model selection for the 95-article main run.

---

### `Model Exploration with Visualization/`

Analysis and visualization artifacts:

| File | Description |
|------|-------------|
| `data_visualization_kpi_summary_full_report.ipynb` | KPI distribution analysis across all 95 articles |
| `data_visualization_experimental.ipynb` | Experimental data exploration |
| `data_visualization_name_entity_5_models.ipynb` | Named entity analysis comparing 5 LLM outputs |
| `sorted_summary_report_v5_final.csv` | Comprehensive evaluation metrics (all articles) |
| `1763700679713-lf-traces-export-*.csv` | LangFuse trace export for pipeline monitoring |

---

### `summary_report_95_articles.csv` (Root)

Master evaluation spreadsheet covering all 95 herb articles. Each row corresponds to one herb article and includes the four KPI scores plus audit data and Go/No-Go decisions (see [Evaluation Metrics](#evaluation-metrics) below).

---

## Article Generation Pipeline

Each article is produced through a 17-task sequential pipeline:

| Task | Name | Description |
|------|------|-------------|
| 1 | Trends Data | Wellness market trends related to the herb |
| 3 | Science Facts | Research abstracts, lab findings, DOI citations |
| 5 | Raw Thai Data | Cultural and community context |
| 7 | Raw Thai Data | Community narratives and traditional processing methods |
| 8 | Compliance Facts | Thai food registration, safety dosages |
| 10 | Safety Facts | Active/inactive ingredients, warnings, directions |
| 11 | Master Fact Sheet | Consolidated facts from all upstream tasks |
| 12 | Fact Audit | Validation of all collected facts |
| 13 | Final Article | Synthesized wellness article |
| 14 | Robustness & Clarity Audit | Quality metric scoring |
| 15 | Efficacy & Feasibility Evaluation | Practical relevance scoring + Go/No-Go decision |
| 16–17 | Completion | Final output markers |

---

## Evaluation Metrics

Four KPIs are recorded per article (scored 0–100):

| KPI | Source Task | Measures |
|-----|-------------|----------|
| **Efficacy Score** | Task 15 | Practical relevance and usefulness of the article |
| **Feasibility Score** | Task 15 | Scalability and implementation readiness |
| **Robustness Score** | Task 14 | Data integrity, accuracy, and anti-hallucination |
| **Clarity Score** | Task 14 | Explanation clarity, readability, no hallucinated content |

Additionally, each article is evaluated for:
- **Go/No-Go Decision** — binary publication readiness decision
- **Dropped Entities** — facts present in the Master Sheet but missing from the final article
- **Hallucinated Entities** — fabricated or unsupported claims introduced by the model

Most articles in the 95-article dataset scored between **80–95** across all four KPIs.

---

## Technologies Used

- **LLMs:** Meta LLaMA 3.1/3.3 (70B), OpenAI GPT, Google Gemini, Anthropic Claude
- **Pipeline Orchestration:** CrewAI multi-agent framework
- **Monitoring & Tracing:** LangFuse
- **Analysis:** Python (Pandas, Matplotlib, Seaborn) in Jupyter Notebooks
- **Output Format:** DOCX articles, JSON benchmarks, CSV evaluation data
