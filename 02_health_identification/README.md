# Health speech identification

This folder contains the notebook and outputs used to construct a topic-based selection of health-related parliamentary speeches.

## Method

The notebook processes the 49,629 eligible speeches in the prepared corpus.

Cleaned texts are encoded with `paraphrase-multilingual-MiniLM-L12-v2`. Long speeches are split into non-overlapping passages within the model's 128-token limit. Passage vectors are combined using a token-weighted mean and normalized to produce one 384-dimensional vector per speech.

BERTopic clusters these vectors using UMAP and HDBSCAN. Topic keywords are extracted from lemmatized texts.

The fitted model produces 69 topics, with 20,629 speeches left unassigned.

## Selections

- **Main:** topic 0, concerning healthcare and health policy — 3,415 speeches.
- **Extended:** topics 0 and 14, adding end-of-life care and related ethical debates — 3,876 speeches.

All party categories are retained. The main selection contains 2,400 speeches assigned to the five party categories used in the subsequent analysis.

Topic choices were informed by keywords and exploratory inspection of sampled speeches, including unassigned records. These checks identified both off-topic inclusions and relevant speeches outside the selected topics. The selections are approximate, rather than manually validated health classifications.

## Files

| File | Contents |
| --- | --- |
| `02_identify_health_speeches.ipynb` | Corpus loading, embeddings, topic modelling and export |
| `outputs/speech_embeddings.npz` | Embeddings, aligned speech IDs and model name; stored with Git LFS |
| `outputs/bertopic_assignments.parquet` | Topic assignment for every eligible speech |
| `outputs/bertopic_topics.csv` | Topic counts and keywords |
| `outputs/bertopic_selection.parquet` | Main and extended selection flags for every eligible speech |
| `outputs/corpus_health_bertopic.parquet` | Main subset with metadata and prepared texts |
| `outputs/run_metadata.json` | Software versions, model settings and selection definitions |

To reconstruct the extended subset, join the selection flags to `data/processed/corpus_prepared.parquet` using `row_id`, then filter on `health_bertopic_extended`.

## Reproduction

Open the notebook in Google Colab, enable a GPU runtime and run the cells in order. The prepared corpus is downloaded from the repository.

The final optional cell downloads the outputs in one archive. Files under `/content` are temporary.

Software versions used for the saved outputs are recorded in `outputs/run_metadata.json`. Topic assignments and numbering may differ across environments; inspect topic keywords before reusing the selected topic numbers after refitting.

To retrieve Git LFS files from a local clone, run `git lfs pull` from the repository root.
