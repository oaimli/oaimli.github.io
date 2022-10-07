---
type: article
title: Mathematical Modeling in Sequence-to-sequence Learning
key: 2018011101
tags: MachineLearning
comment: false
mathjax: true
---

Text generation, such as text summarization and story generation, usually aims to generate the target text $Y=(y_0, ..., y_i, ..., y_{n-1}), y_i \in \mathcal{V}$ based on the input $X$ which could be different in different text generation sub-tasks. For example, $X$ is the source text in text summarization, while it is the given title in story generation. 

To generate the target text $Y$, there are two types of generation modes: autoregressive and non-autoregressive generation. Autoregressive generation generates words based on the input $X$ and also words generated before, while non-autoregressive generaion generate all words of $Y$ independently at the same time. Here we only talk about autoregressive generation, which is mostly used in the text generation community.

Sequence-to-sequence text generation is formulated as Given the input $X$, auto-regressive generation is 