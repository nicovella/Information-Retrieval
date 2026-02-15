# [cite_start]Mitigating Echo Chambers in Financial Question Answering [cite: 1, 9]

## Overview
[cite_start]This project addresses the mitigation of echo chambers in the Financial Question Answering (QA) domain[cite: 9, 22]. [cite_start]Standard Information Retrieval (IR) systems typically optimize for topical relevance[cite: 18]. [cite_start]In the financial sector, high relevance scores often correlate with the dominant viewpoint, creating an "echo chamber" effect that obscures alternative market sentiments[cite: 18, 19]. [cite_start]The primary objective of this project is to develop a robust retrieval pipeline that balances relevance with sentiment diversity, ensuring users have access to unbiased information for effective risk management[cite: 21, 22].

## Dataset
* [cite_start]**Source:** The system operates on the FiQA (Financial Question Answering) dataset, which contains 57,638 documents[cite: 33, 93].
* [cite_start]**Characteristics:** This heterogeneous corpus integrates information from varying sources, including microblogs, news articles, and official financial reports[cite: 29].
* [cite_start]**Content Type:** It features a mix of objective factual data and subjective opinionated content, ranging from authoritative institutional reports to informal social media posts[cite: 30].

## Methodology 
[cite_start]The project was split into two phases to compare baseline models against advanced retrieval strategies[cite: 26]:

### Phase I: Baseline Systems
* [cite_start]**Lexical Baselines:** Implemented TF-IDF and BM25 to rank documents based on query term frequency and probabilistic scoring[cite: 38, 39, 40, 41].
* [cite_start]**Query Expansion:** Utilized the RM3 relevance model to extract and score terms from top documents to improve search results[cite: 42, 43].
* [cite_start]**Baseline Performance:** All initial baseline runs established a maximum Mean Average Precision (MAP) score of 0.21[cite: 10, 37].

### Phase II: Advanced Systems
* [cite_start]**Cross-Encoder Re-Ranking:** Implemented a Cross-Encoder pipeline to solve the "vocabulary mismatch" problem common in financial jargon[cite: 46, 47]. [cite_start]It uses the `ms-marco-MiniLM-L-6-v2` model to jointly encode query-document pairs, capturing deep semantic interactions that standard keyword matching misses[cite: 48, 49, 97].
* [cite_start]**LLM Query Expansion:** Leveraged the `Microsoft Phi-3-mini-4k-instruct` model to generate semantically relevant financial keywords directly from original user queries[cite: 66, 67, 104].
* [cite_start]**Sentiment Diversification:** Utilized the `FinTwitBERT-sentiment` model to classify candidate documents as bullish, bearish, or neutral[cite: 74, 75, 76, 110]. [cite_start]A Soft Zig-Zag Re-ranker was then applied to balance normalized relevance scores against a diversity penalty[cite: 76, 111]. 
* [cite_start]**Objective Function:** The re-ranker evaluates documents using the following formula[cite: 77, 78]:
  `Score(d) = (1 - lambda) * Relevance(d) + lambda * Diversity(d)`

## Key Results
* [cite_start]**Relevance Improvements:** The Cross-Encoder Re-Ranking configuration demonstrated significant efficacy, achieving a MAP of 0.290 (a 39.2% improvement over the baseline) and increasing NDCG@10 by 38.7%[cite: 115, 117].
* [cite_start]**Echo Chamber Mitigation:** The Sentiment Diversification approach (using a lambda parameter of 0.25) successfully disrupted sentiment homogeneity, increasing Shannon Entropy by 6.26%[cite: 79, 111, 159]. [cite_start]This was achieved with a negligible decrease in relevance (MAP dropped slightly from 0.2058 to 0.2032)[cite: 122]. 
* [cite_start]**LLM Limitations:** The LLM Query Expansion experiment underperformed compared to the RM3 baseline, resulting in a 14.6% lower MAP (0.180 vs 0.206) and poorer early precision[cite: 153]. [cite_start]General-knowledge LLMs produced less effective terms due to semantic drift compared to RM3's extraction from actual corpus documents[cite: 156, 157].
