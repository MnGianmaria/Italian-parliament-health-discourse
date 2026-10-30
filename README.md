# Health Discourse in Transition

Semantic similarities in Italian parliamentary health discourse before and during COVID-19.

This project examines how health-related speeches assigned to five party categories — Partito Democratico (PD), Fratelli d’Italia (FdI), Movimento 5 Stelle (M5S), Forza Italia (FI) and Lega — differ across **2018–2019, 2020 and 2021–2022**.

It extends an earlier group project, shifting the focus from lexical comparisons between political blocs to contextual speech embeddings and party-level semantic profiles.

**[Read the research report](health_discourse_report.pdf)**

## Research question

How did the semantic similarity of health-related parliamentary discourse across the five party categories evolve before and during the COVID-19 pandemic?

The analysis compares parties within each period and follows changes within each party over time. It describes semantic similarities rather than directly measuring political agreement or identifying a causal effect of the pandemic.

## Data and sample

The source is **ItaParlCorpus v3.0**, using `camera_2006-2022.csv` from [Harvard Dataverse](https://doi.org/10.7910/DVN/KUARWD).

The preparation pipeline selects legislature XVIII within 2018–2022, excludes recorded chair interventions, cleans transcription artifacts and creates an Italian lemmatized representation. Original texts and metadata are preserved.

| Stage | Records |
|---|---:|
| Legislature XVIII, excluding chair interventions | 61,985 |
| Eligible for topic modelling | 49,629 |
| Main health selection, all party categories | 3,415 |
| Main five-party sample | 2,400 |
| Extended five-party sample | 2,745 |

Modelling eligibility requires at least 20 whitespace-separated words and five retained lexical tokens, excluding empty texts and flagged transcription headings. Exclusion flags remain available in the prepared dataset.

The main health selection uses BERTopic topic 0. The extended selection adds topic 14, concerning end-of-life discourse. Topic identifiers refer to the saved model run.

## Method

1. **Speech embeddings.** Cleaned texts are encoded with `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`. Long speeches are split into passages within the 128-token input limit. Passage vectors are combined using token-count weights and normalized, producing one 384-dimensional vector per speech.

2. **Health identification.** BERTopic clusters all eligible speech embeddings using UMAP and HDBSCAN. Lemmatized texts provide topic keywords. Keywords and speech examples inform the health selection.

3. **Party-period comparisons.** Speech embeddings are averaged with equal weight per speech and normalized into 15 party-period profiles. Cosine distances measure their proximity; average-linkage hierarchical clustering describes their groupings. These comparisons use the original embeddings, not the reduced UMAP coordinates.

4. **Uncertainty and sensitivity.** Five hundred bootstrap repetitions resample debate dates within each period, retaining contributions from all parties on each selected date. Additional checks broaden the health selection and give equal weight to individual speakers.

## Main findings

- **2018–2019:** the principal partition is reproduced in 40.4% of date-bootstrap samples and changes under equal speaker weighting.
- **2020:** the PD–M5S versus FI–Lega–FdI partition is reproduced in 96.4% of bootstrap samples.
- **2021–2022:** FdI forms a separate principal branch from the other four categories in 99.8% of bootstrap samples.
- Both later-period partitions persist under the extended health selection and equal speaker weighting.
- Five first-to-last-period pairwise distance changes have pointwise bootstrap intervals entirely below zero. All observed increases involving FdI have intervals crossing zero.

Stable partitions do not imply unchanged distances: some pairwise estimates change substantially under the extended health selection.

## Repository structure

| Location | Contents |
|---|---|
| [`data/`](data/) | Corpus preparation notebook, documentation and prepared dataset |
| [`02_health_identification/`](02_health_identification/) | Health-selection notebook, speech embeddings, topic assignments and selection outputs |
| [`03_linguistic_analysis/`](03_linguistic_analysis/) | Semantic comparisons, bootstrap analysis, sensitivity checks, tables and figures |
| [`health_discourse_report.pdf`](health_discourse_report.pdf) | Research report with methods, results, discussion and references |

Each analysis folder contains a README describing its contents.

## Reproduce the analysis

Run the notebooks in this order:

1. [`data/01_prepare_corpus.ipynb`](data/01_prepare_corpus.ipynb)
2. [`02_health_identification/02_identify_health_speeches.ipynb`](02_health_identification/02_identify_health_speeches.ipynb)
3. [`03_linguistic_analysis/03_linguistic_analysis.ipynb`](03_linguistic_analysis/03_linguistic_analysis.ipynb)

The notebooks are designed for Google Colab. Run cells sequentially and follow their setup instructions. Notebook 02 requires a GPU for embedding generation; notebook 03 reuses the saved embeddings and runs on a CPU.

Prepared inputs and embeddings are provided, so the semantic analysis can be run without repeating corpus preparation or embedding generation. Files created under `/content` are temporary; download outputs before ending a Colab session.

The prepared corpus and speech embeddings are stored through **Git LFS**. With Git LFS installed, clone the repository using:

```bash
git lfs install
git clone https://github.com/MnGianmaria/Italian-parliament-health-discourse.git
cd Italian-parliament-health-discourse
git lfs pull
