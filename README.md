# 🔍 Semantic Information Retrieval

A Jupyter notebook project demonstrating how **medical domain knowledge** can be leveraged to significantly improve Information Retrieval (IR) systems. Built on the **TREC-COVID** and **PubMed** datasets using **PyTerrier**.

---

## 📌 Overview

Standard keyword-based search engines match documents by term frequency alone — they have no understanding of medical concepts. This project bridges that gap by integrating **MeSH (Medical Subject Headings)** — a structured biomedical vocabulary maintained by the U.S. National Library of Medicine — into the retrieval pipeline.

The notebook implements and compares four semantic enhancement strategies on top of a BM25 baseline:

| Strategy | Description |
|---|---|
| **Query Expansion (Bo1 PRF)** | Expands queries with terms from pseudo-relevant top documents |
| **Query Expansion (MeSH)** | Expands queries using MeSH terms from top-ranked documents |
| **Query Expansion (MeSH Index)** | Uses a dedicated MeSH index + Bo1 to select the most informative expansion terms |
| **Document Expansion (MeSH)** | Appends MeSH terms directly to indexed document text |
| **Semantic Ranking (BM25F)** | Field-weighted ranking across title, abstract, and MeSH fields |
| **Learning to Rank (LTR)** | Trains a supervised model to optimally combine retrieval features |

---

## 📁 Repository Structure

```
├── Semantic_Information_Retrieval.ipynb   # Main notebook with all experiments
├── Semantic_Information_Retrieval_html.html  # Rendered HTML version (view without running)
└── README.md
```

---

## 🧪 Methods in Detail

### 1. Data Preparation
- Loads the **TREC-COVID** document collection (`covid_df.csv`)
- Fetches **MeSH terms** for each paper via the [NCBI Entrez E-utilities API](https://www.ncbi.nlm.nih.gov/books/NBK25500/)
- Result: 306 documents annotated with MeSH terms (4,255 total, 1,446 unique), averaging ~14 terms per document

### 2. BM25 Baseline
Indexes the collection using **PyTerrier** with `Title + Abstract` as document text, establishing the reference retrieval model.

### 3. Query Expansion
- **Bo1 (Pseudo Relevance Feedback):** Assumes top-ranked documents are relevant and mines them for statistically informative expansion terms using Bose-Einstein statistics
- **MeSH-based Expansion:** Substitutes raw term statistics with controlled medical vocabulary from the MeSH dictionary
- **MeSH Index + Bo1:** Builds a separate MeSH-only index and applies Bo1 to select the most discriminative MeSH terms, avoiding noise from full term lists

### 4. Document Expansion
Appends MeSH terms to each document before indexing, so standard queries can match medical concepts without any query modification.

### 5. Semantic Relevance Ranking
- **BM25F:** Field-weighted retrieval model assigning different importance to `text`, `title`, `abstract`, and `mesh` fields
- **Learning to Rank:** Trains a supervised model on TREC-COVID relevance judgments using a 5-feature vector per document, evaluated with **Recall@K**

---

## 🛠️ Tech Stack

- **Python 3**
- [PyTerrier](https://pyterrier.readthedocs.io/) — IR experimentation framework (indexing, retrieval, evaluation)
- **BM25 / BM25F / Bo1** — retrieval and expansion models
- **NCBI Entrez E-utilities API** — MeSH term fetching
- **BeautifulSoup** — XML parsing
- **Pandas** — data manipulation
- **Google Colab** — original runtime environment

---

## 📊 Dataset

- **[TREC-COVID](https://ir.nist.gov/trec-covid/):** ~200,000 COVID-19 scientific publications with queries and official relevance judgments (327 documents subset used here)
- **PubMed / MeSH:** Medical Subject Headings fetched via the NCBI E-utilities API

> ⚠️ The dataset file `covid_df.csv` is **not included** in this repository. You can obtain TREC-COVID data from the [official TREC website](https://ir.nist.gov/trec-covid/).

---

## 🚀 Getting Started

### Option 1 — View without running
Open `Semantic_Information_Retrieval_html.html` directly in your browser to see the full notebook with all outputs pre-rendered.

### Option 2 — Run in Google Colab
1. Upload the notebook to [Google Colab](https://colab.research.google.com/)
2. Upload your `covid_df.csv` when prompted
3. Run all cells sequentially

### Option 3 — Run locally

```bash
pip install python-terrier pandas beautifulsoup4 lxml
jupyter notebook Semantic_Information_Retrieval.ipynb
```

> **Note:** PyTerrier requires Java. Make sure you have a JDK installed and `JAVA_HOME` set.

---

## 📈 Key Findings

- MeSH-based query expansion improves retrieval by grounding expansion in controlled medical vocabulary rather than raw term co-occurrence
- Document expansion allows standard queries to match medical concepts without any query-time modification
- BM25F with higher abstract weighting (`w.2 = 2`) reflects that abstracts carry the most descriptive content
- Learning to Rank automatically discovers the optimal combination of retrieval signals, outperforming manually tuned weights

---

## 📄 License

This project is for educational and research purposes.
