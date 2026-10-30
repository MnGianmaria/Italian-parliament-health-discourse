# Semantic distance between parties

This folder compares the average semantic content of health-related parliamentary speeches across five source party categories: PD, FdI, M5S, Forza Italia and Lega.

## Sample

The main sample contains 2,400 speeches from BERTopic topic 0. An extended selection adds topic 14, covering end-of-life discourse, and contains 2,745 speeches.

The calendar periods are 2018–2019, 2020 and 2021–2022. The last period represents the later pandemic, not a post-pandemic period.

## Method

The notebook reuses the 384-dimensional speech embeddings generated in notebook 02.

For each party and period, it averages the speech embeddings with equal weight per speech and normalizes the resulting vector. Cosine distances compare these profiles within and across periods.

Hierarchical clustering uses cosine distances and average linkage. Uncertainty and clustering stability are assessed using 500 bootstrap repetitions that resample debate dates within each period, retaining all contributions from each selected date.

The extended selection provides a sensitivity check. Speeches close to the profiles provide illustrative examples.

## Contents

- `03_linguistic_analysis.ipynb`: analysis notebook.
- `outputs/`: sample composition, semantic profiles, distance tables, clustering figures, bootstrap results, selection sensitivity, illustrative speeches and run metadata.

`outputs/run_metadata.json` records input URLs and checksums, analysis settings and software versions.

## Run

Open the notebook in Google Colab and run the cells in order. A CPU runtime is sufficient. Inputs are downloaded from the repository; embeddings are reused without loading or fitting the language model.

The final cell downloads an archive containing the analysis outputs.

## Interpretation and limitations

Distances describe similarity in the model's representation of average speech content. They do not directly measure political agreement, ideological position or a causal effect of COVID-19.

Party categories are inherited from ItaParlCorpus without manual affiliation corrections and may differ from parliamentary affiliations at the date of a speech. Health selections are approximate.

Bootstrap intervals are pointwise and unadjusted for multiple comparisons. Resampling does not account for dependence across dates or uncertainty in the embedding model, party labels or topic selection. Clustering frequencies describe stability under this resampling procedure, not the probability that a grouping is correct.
