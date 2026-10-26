# TitleGuard

## Online Title Similarity Verification

TitleGuard helps reviewers find publication titles that resemble existing registry entries. It combines text preprocessing, TF-IDF retrieval, several similarity measures, and machine learning classification to identify title pairs that need closer examination.

The project addresses **SIH1782: An online system to automatically verify new title submissions by checking similarities with existing titles**. It was developed as a Data Science and Ecosystem project using Python and Jupyter Notebook.

TitleGuard supports preliminary screening. A similarity flag is a reason for human review, not proof of plagiarism.

## The Problem

An exact-text search can miss related titles when spelling, punctuation, capitalization, or word order changes. Checking every submission manually also becomes repetitive as a title registry grows. TitleGuard retrieves likely matches and explains the evidence behind its predictions to help reviewers assess these cases.

## Features

- Normalize titles before comparison.
- Retrieve candidate matches using word-level and character-level TF-IDF.
- Compare title pairs through five complementary similarity features.
- Flag potential matches using Logistic Regression.
- Compare classification performance with Decision Tree and K-Nearest Neighbors.
- Display feature values, model contributions, and the log-odds margin.
- Check individual titles through interactive Jupyter widgets.
- Process multiple titles from CSV files and export results.

## How It Works

1. **Prepare the registry:** Load existing titles from `registry.csv`.
2. **Normalize the input:** Apply Unicode NFKC normalization, case folding, and regular-expression-based alphanumeric filtering.
3. **Retrieve candidates:** Rank registry entries using a weighted combination of word-level and character-level TF-IDF similarity.
4. **Extract features:** Calculate five similarity measures for each candidate pair.
5. **Classify the pair:** Use Logistic Regression to predict whether the titles form a potential match.
6. **Review the evidence:** Present matching records, predictions, and feature-based explanations for manual assessment.

### Retrieval Configuration

| Representation | N-gram sizes | Weight |
| --- | --- | --- |
| Word-level TF-IDF | 1–2 | 0.4 |
| Character-level TF-IDF | 2–5 | 0.6 |

### Similarity Features

| Feature | What it measures |
| --- | --- |
| Normalized exact match | Whether the normalized titles are identical |
| Token Jaccard similarity | Overlap between the sets of words in each title |
| Sequence similarity | Resemblance between the titles as character sequences |
| Relative length ratio | How closely the title lengths correspond |
| Character-trigram Jaccard similarity | Overlap between sets of three-character sequences |

The Logistic Regression model uses balanced class weights. Related examples are grouped by title family when separating training, validation, and testing data to reduce overlap between related variants. The reported classifier review threshold is **0.75** and is configurable.

## Technology

| Component | Technology |
| --- | --- |
| Programming language | Python |
| Development environment | Jupyter Notebook |
| Retrieval and classification | scikit-learn |
| Interactive controls | Jupyter widgets |
| Registry and batch input | CSV files |

## Using the Notebook

1. Open the project notebook in Jupyter Notebook or Google Colab.
2. Install the dependencies specified by the notebook.
3. Make `registry.csv` available at the path expected by the data-loading code.
4. Run the cells in order to prepare the data, build retrieval representations, train the models, and evaluate them.
5. Use the interactive controls to check a title or process a batch CSV.
6. Inspect the retrieved matches and explanations before making a verification decision, and export results when needed.

Use the notebook's loading code to determine the required CSV columns and input format.

## Reported Evaluation

The project report evaluates Logistic Regression on **90 synthetic test pairs**.

| Classification metric | Reported result |
| --- | --- |
| Accuracy | 93.3% |
| Precision | 100% |
| Recall | 90% |
| F1-score | 0.947 |

The reported retrieval evaluation produced:

| Retrieval metric | Reported result |
| --- | --- |
| Recall@5 | 100% |
| MRR@5 | 1.0 |

Recall@5 measures whether the relevant title appears within the first five results. MRR@5 measures the reciprocal rank of the first relevant result within those positions; a value of 1.0 means the relevant result ranked first for every evaluated query.

These are experimental results on the reported evaluation data. They do not establish performance on a larger real-world registry or measure the plagiarism percentage of a document.

## Limitations

- Synthetic title pairs may not represent the full variety of real publication titles.
- Titles with similar meanings but substantially different vocabulary may be missed.
- The review threshold may need adjustment for a different dataset or verification policy.
- Strong results across comparison models may reflect patterns in the synthetic data.
- Title similarity alone cannot determine plagiarism; human review remains necessary.

## Future Work

- Add semantic comparisons using sentence-transformer models such as Sentence-BERT.
- Evaluate larger collections of real publication and academic project titles.
- Use verified reviewer feedback to improve the training data.
- Explore vector databases such as Qdrant or pgvector for larger registries.
- Extend explanations with matching-word highlights and clearer confidence information.
- Investigate multilingual title verification.
- Develop an optional web application around the notebook workflow.

