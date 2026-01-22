# mt-pattern-preservation

This repository contains the code and experiments for a research project investigating whether modern **Neural Machine Translation (NMT)** systems preserve **stylistic and genre-specific language patterns**, rather than merely producing fluent text. The project was developed  during my 2nd year of *MSc in Artificial Intelligence at the Faculty of Mathematics and Computer Science, University of Bucharest.*

> **Do machine translation models preserve language patterns, or do they flatten them into generic text?**

To answer this, we:
1. Train a genre classifier on original English news articles.
2. Translate the same articles into multiple languages: French, German, Polish, Russian.
3. Train and evaluate classifiers on each translated dataset.
4. Compare performance against the English baseline.

If patterns are preserved, a classifier should perform similarly across languages.

## Dataset

- **Task**: SemEval 2023 – Task 3, Subtask 1  
  *News Genre Classification*
- **Genres**:
  - Opinion
  - Reporting
  - Satire
- **Source Language**: English
- **Topics**: COVID-19, climate change, abortion, migration, war, elections, etc.

## Translation 

To translate we used a multilingual encoder-decoder transformer `facebook/m2m100_418M`, and we evaluate the resulted translations using **reference-free COMET** (`Unbabel/wmt20-comet-qe-da`):

| Language | COMET Score |
|--------|-------------|
| French | 0.3829 |
| German | 0.4156 |
| Spanish | 0.4213 |
| Polish | **0.4514** |
| Russian | 0.4504 |

## Classification

To classify, we used an XLM-RoBERTa Large model, that was finetuned for a few epochs on 80% of the dataset, and tested on the other 20%.

| Language | Precision | Recall | Macro F1 |
|--------|-----------|--------|----------|
| English | 0.7395 | 0.7700 | 0.7407 |
| French | 0.8023 | 0.7063 | 0.7172 |
| German | 0.7684 | 0.7607 | 0.7257 |
| Spanish | 0.7984 | 0.6671 | **0.6510** |
| Polish | **0.8175** | **0.8174** | **0.8135** |
| Russian | 0.8285 | 0.7999 | 0.7820 |

## Key Observations
- **Polish and Russian outperform the English baseline**, despite being translations.
- Spanish exhibits the highest information loss.
- Better translation quality (COMET) aligns with stronger pattern preservation.
- Simplification introduced by MT may make genres more linearly separable.
