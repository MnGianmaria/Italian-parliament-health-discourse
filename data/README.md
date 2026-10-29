# Corpus preparation

This folder contains the notebook and prepared corpus used for the analysis of Italian parliamentary health discourse.

## Files

- `01_prepare_corpus.ipynb`: downloads the source data and prepares the corpus.
- `processed/corpus_prepared.parquet`: prepared corpus, stored using Git LFS.

## Source

Cova, Joshua (2024). *ItaParlCorpus*, version 3.0. Harvard Dataverse.
https://doi.org/10.7910/DVN/KUARWD

The notebook uses [`camera_2006-2022.csv`](https://dataverse.harvard.edu/file.xhtml?fileId=10876411&version=3.0), downloaded directly from Harvard Dataverse. The original CSV is not included in this repository.

## Preparation

The notebook selects legislature XVIII within 2018–2022 and excludes interventions marked as chair contributions. It preserves the original text and metadata, creates a cleaned text column, and tokenizes and lemmatizes Italian speeches using spaCy.

Records are eligible for topic modelling when they:

- Are not flagged as transcription headings.
- Contain at least 20 whitespace-separated words after cleaning.
- Contain at least five retained lexical tokens.

All records remain in the prepared table, with explicit eligibility flags and exclusion reasons.

Five source party categories are mapped to PD, FdI, M5S, FI and Lega for the subsequent party comparison. Other eligible speeches remain available for topic modelling.

## Prepared corpus

The dataset contains 61,985 records dated from 29 March 2018 to 28 September 2022:

- 49,629 records eligible for topic modelling.
- 39,072 eligible records assigned to the five selected party categories.

The main derived columns are:

| Column | Description |
| --- | --- |
| `source_record` | Record position in the source CSV, starting at 1 |
| `date_parsed` | Parsed speech date |
| `quarter` | Calendar quarter |
| `text_clean` | Text after removing matched speaker headers and stage directions |
| `n_words_clean` | Whitespace-separated word count |
| `text_lexical` | Retained lowercase lemmas, separated by spaces |
| `n_lexical_tokens` | Number of retained lexical tokens |
| `flag_heading` | Possible transcription heading |
| `flag_empty_text` | Empty cleaned text |
| `flag_short_text` | Fewer than 20 cleaned words |
| `flag_few_lemmas` | Fewer than five retained lexical tokens |
| `eligible_for_models` | Eligibility for topic modelling |
| `exclusion_reason` | Reasons for ineligibility |
| `analysis_party` | Selected party abbreviation, where applicable |
| `in_five_party_comparison` | Membership in a selected source party category |

Health-related speeches are identified in the subsequent stage of the project.

## Reproduction

Open `01_prepare_corpus.ipynb` in Google Colab and run its cells in order. The notebook downloads the source CSV and exports `corpus_prepared.parquet`.

Files created under `/content` are temporary. Download the exported corpus before ending the Colab session.

To retrieve the prepared dataset from a local clone with Git LFS installed, run this command from the repository root:

    git lfs pull
