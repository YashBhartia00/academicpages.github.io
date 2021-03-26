---
title: "NLRG at SemEval-2021 Task 5: Toxic Spans Detection Leveraging BERT-based Token Classification and Span Prediction Techniques"
collection: publications
permalink: /_publication/2021-02-24-NLRG-at-semeval-2021
# excerpt: 'News Propaganda, High granularity, Imbalanced Classification, Contextual Embeddings'
date: 2021-02-24
venue: 'Submited for review at SemEval-2021'
paperurl: 'https://arxiv.org/abs/2102.12254'
# citation: 'patil-etal-2020-bpgc'

---
Toxicity detection of text has been a popular NLP task in the recent years. In SemEval-2021 Task-5 Toxic Spans Detection, the focus is on detecting toxic spans within passages. Most state-of-the-art span detection approaches employ various techniques, each of which can be broadly classified into Token Classification or Span Prediction approaches. In our paper, we explore simple versions of both of these approaches and their performance on the task. Specifically, we use BERT-based models -- BERT, RoBERTa, and SpanBERT for both approaches. We also combine these approaches and modify them to bring improvements for Toxic Spans prediction. To this end, we investigate results on four hybrid approaches -- Multi-Span, Span+Token, LSTM-CRF, and a combination of predicted offsets using union/intersection. Additionally, we perform a thorough ablative analysis and analyze our observed results. Our best submission -- a combination of SpanBERT Span Predictor and RoBERTa Token Classifier predictions -- achieves an F1 score of 0.6753 on the test set. Our best post-eval F1 score is 0.6895 on intersection of predicted offsets from top-3 RoBERTa Token Classification checkpoints. These approaches improve the performance by 3% on average than those of the shared baseline models -- RNNSL and SpaCy NER.