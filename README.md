## Mitigating Echo Chambers in Financial Question Answering

### Overview

This project addresses the mitigation of echo chambers in the Financial Question Answering (QA) domain. Traditional Information Retrieval (IR) systems typically optimize for topical relevance. In the financial sector, high relevance scores often correlate with the dominant viewpoint, creating an "echo chamber" effect that obscures alternative market sentiments. The goal of this project is to develop a robust retrieval pipeline that balances relevance with sentiment diversity, ensuring users have access to unbiased information for effective risk management.

### Dataset

The system operates on the FiQA (Financial Question Answering) dataset, which contains 57,638 documents. This heterogeneous corpus integrates information from varying sources, including microblogs, news articles, and official financial reports. It features a mix of objective factual data and subjective opinionated content.


### Methodology

The project was split into two phases to compare baseline models against advanced retrieval strategies:

**Phase I: Baseline Systems**

* 
**Lexical Baselines**: Implemented TF-IDF and BM25 to rank documents based on query term frequency and probabilistic scoring.


* 
**Query Expansion**: Utilized the RM3 relevance model to extract and score terms from top documents to improve search results.


* 
**Baseline Performance**: All initial baseline runs established a maximum Mean Average Precision (MAP) score of 0.21.



**Phase II: Advanced Systems**

* 
**Cross-Encoder Re-Ranking**: Implemented to solve the "vocabulary mismatch" problem common in financial jargon. It uses the `ms-marco-MiniLM-L-6-v2` model to jointly encode query-document pairs, capturing deep semantic interactions that standard keyword matching misses.


* 
**LLM Query Expansion**: Leveraged the `Microsoft Phi-3-mini-4k-instruct` model to generate semantically relevant financial keywords directly from original user queries.


* 
**Sentiment Diversification**: Utilized the `FinTwitBERT-sentiment` model to classify candidate documents as bullish, bearish, or neutral. A Soft Zig-Zag Re-ranker was then applied to balance normalized relevance scores against a diversity penalty.


* 
**Objective Function**: The re-ranker evaluates documents using the following formula:





### Key Results

* 
**Relevance Improvements**: The Cross-Encoder Re-Ranking configuration demonstrated significant efficacy, achieving a MAP of 0.290 (a 39.2% improvement over the baseline) and increasing NDCG@10 by 38.7%.


* 
**Echo Chamber Mitigation**: The Sentiment Diversification approach (using ) successfully disrupted sentiment homogeneity, increasing Shannon Entropy by 6.26%. This was achieved with only a negligible decrease in relevance (MAP dropped slightly from 0.2058 to 0.2032).


* 
**LLM Limitations**: The LLM Query Expansion experiment underperformed compared to the RM3 baseline, resulting in a 14.6% lower MAP (0.180 vs 0.206) and poorer early precision. General-knowledge LLMs produced less effective terms due to semantic drift compared to RM3's extraction from actual corpus documents.
