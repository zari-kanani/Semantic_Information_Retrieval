# Semantic Information Retrieval

A Jupyter notebook project demonstrating how **medical domain knowledge** can be leveraged to significantly improve Information Retrieval (IR) systems. Built on the **TREC-COVID** and **PubMed** datasets using **PyTerrier**.

---

## Overview

Standard keyword-based search engines match documents by term frequency alone — they have no understanding of medical concepts. This project bridges that gap by integrating **MeSH (Medical Subject Headings)** into the retrieval pipeline.

The notebook implements and compares four semantic enhancement strategies on top of a BM25 baseline:

| Strategy | Description |
|---|---|
| **Query Expansion (Bo1 PRF)** | Expands queries with terms from pseudo-relevant top documents |
| **Query Expansion (MeSH)** | Expands queries using MeSH terms from top-ranked documents |
| **Query Expansion (MeSH Index)** | Uses a MeSH index + Bo1 to select the most informative expansion terms |
| **Document Expansion (MeSH)** | Appends MeSH terms directly to indexed document text |
| **Semantic Ranking (BM25F)** | Field-weighted ranking across title, abstract, and MeSH fields |
| **Learning to Rank (LTR)** | Trains a supervised model to optimally combine retrieval features |


## Key Findings

- MeSH-based query expansion improves retrieval by grounding expansion in controlled medical vocabulary rather than raw term co-occurrence
- Document expansion allows standard queries to match medical concepts without any query-time modification
- BM25F with higher abstract weighting (`w.2 = 2`) reflects that abstracts carry the most descriptive content
- Learning to Rank (Random Forest, 5 features) scored **lower** than the plain BM25 baseline across all Recall@K cutoffs — likely due to overfitting on the small training set (~30 queries after splitting TREC-COVID's 50 topics)

