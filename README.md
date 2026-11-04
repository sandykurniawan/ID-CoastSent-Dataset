# ID-CoastSent: Indonesian Coastal-Tourism Reviews with Three Annotator Labels

ID-CoastSent contains **21,779 Indonesian Google Maps reviews of coastal-tourism destinations**, each labelled as **Positive**, **Negative** or **Neutral** by three annotators. The individual annotator labels are released alongside the aggregated labels, so the dataset can be used both for standard sentiment classification and for studies of annotator agreement and disagreement.

The dataset accompanies the following article (under review):

> Kurniawan, S., Tantyoko, H., Khadijah, Wibawa, H.A. *Annotator Agreement or Training-Data Quantity? A Controlled Comparison of Unanimous and Majority-Vote Training Data for Indonesian Coastal-Tourism Sentiment Classification.* Submitted to *Discover Artificial Intelligence*.

## Data collection and annotation

- **Source:** publicly posted Google Maps reviews of coastal-tourism destinations in several Indonesian regions. After removing empty, duplicate, non-Indonesian and very short reviews (< 20 characters), 253,422 reviews remained.
- **Sampling:** 4,386 reviews were drawn from each star-rating level (1–5), yielding 21,930 reviews for annotation. The rating served only as a stratification variable, because the textual sentiment of a review is unknown before annotation. As a consequence, the class distribution of this dataset does **not** reflect the natural distribution of sentiment in Google Maps reviews.
- **Annotators:** three undergraduate Informatics students, all native speakers of Indonesian. They labelled the reviews independently, each in a separate spreadsheet, and did not see one another's labels.
- **What annotators saw:** only the review identifier and the review text. Star rating, destination, province and date were hidden.
- **Instructions:** brief oral instructions to judge whether each review is positive, negative or neutral. There was no written guideline, no calibration round and no adjudication. The treatment of mixed reviews was left to each annotator.
- **Exclusions:** 151 reviews reported as non-Indonesian or uninterpretable were removed, leaving 21,779 reviews.

## Agreement

Fleiss' κ over all 21,779 reviews is **0.7096**, which corresponds to substantial agreement. Agreement differs strongly between classes, however:

| Agreement pattern | Reviews | % |
| :--- | ---: | ---: |
| 3-0 (all three labels identical) | 15,827 | 72.67 |
| 2-1 (two-to-one majority) | 5,708 | 26.21 |
| 1-1-1 (three different labels) | 244 | 1.12 |

| Majority label | Reviews | 3-0 | 2-1 | Category-specific κ |
| :--- | ---: | ---: | ---: | ---: |
| Negative | 8,691 | 82.3% | 17.7% | 0.791 |
| Positive | 8,479 | 83.7% | 16.3% | 0.792 |
| Neutral | 4,365 | 36.0% | 64.0% | 0.465 |

Of the 5,708 two-to-one cases, 94.0% involve the neutral label; only 6.0% are conflicts between positive and negative. Pairwise Cohen's κ between annotators ranges from 0.66 to 0.77. The annotators' neutral rates also differ considerably:

| Annotator | Positive | Negative | Neutral |
| :--- | ---: | ---: | ---: |
| Annotator 1 | 8,339 (38.3%) | 8,679 (39.9%) | 4,761 (21.9%) |
| Annotator 2 | 7,878 (36.2%) | 8,129 (37.3%) | 5,772 (26.5%) |
| Annotator 3 | 9,719 (44.6%) | 9,476 (43.5%) | 2,584 (11.9%) |

Disagreement in this dataset should therefore not be read as annotation error. Because no shared definition of the neutral category was given, a two-to-one label combines genuine textual ambiguity with differences in the annotators' thresholds for neutrality.

## Files

```text
data/
├── full_dataset_clean.csv     # all 21,779 reviews with the three individual labels
├── train_all_agree.csv        # Track A: unanimous (3-0) reviews, test set excluded
├── train_majority_vote.csv    # Track B: 3-0 and 2-1 reviews with majority label, test set excluded
└── test_set.csv               # fixed test set: 300 unanimous reviews, 100 per class
```

| File | Rows | Negative | Neutral | Positive | Content |
| :--- | ---: | ---: | ---: | ---: | :--- |
| `train_all_agree.csv` | 15,527 | 7,054 | 1,472 | 7,001 | 3-0 reviews only |
| `train_majority_vote.csv` | 21,235 | 8,591 | 4,265 | 8,379 | 3-0 and 2-1 reviews (26.9% are 2-1) |
| `test_set.csv` | 300 | 100 | 100 | 100 | 3-0 reviews only |
| `full_dataset_clean.csv` | 21,779 | – | – | – | all reviews, including the 244 1-1-1 reviews |

**Accounting:** 21,779 = 300 (test, 3-0) + 15,527 (3-0) + 5,708 (2-1) + 244 (1-1-1).

**Construction notes:**
- The test set was sampled after annotation and before the training files were built, by stratified random sampling from the unanimous reviews (100 per class, `random_state = 42`). It is disjoint from both training files.
- `train_all_agree.csv` is a subset of `train_majority_vote.csv`. A direct comparison of the two therefore confounds label agreement with training-set size and class distribution. To compare them at equal size and class distribution, draw subsets of `train_majority_vote.csv` with the per-class counts of `train_all_agree.csv` (7,054 / 1,472 / 7,001), as done in the accompanying article.
- Because the test set contains only unanimous labels, it measures agreement with clear-cut judgements, not performance on contested reviews.

## Data dictionary

| Column | Files | Description |
| :--- | :--- | :--- |
| `number_id` | all | Unique review identifier; links reviews across files. |
| `name` | all | Name of the coastal-tourism destination. |
| `province` | all | Indonesian province of the destination. |
| `published_at_date` | all | Publication date of the review. |
| `review_text` | all | Review text as posted. |
| `rating` | all | Google Maps star rating (1–5). Not shown to annotators. |
| `clean_review` | `full_dataset_clean.csv` | Review text with line breaks and repeated whitespace collapsed; this is the text the annotators labelled. |
| `Ann 1`, `Ann 2`, `Ann 3` | `full_dataset_clean.csv` | Labels assigned by each annotator (`Positive`, `Negative`, `Neutral`). |
| `All Agree` | `full_dataset_clean.csv` | The label if all three annotators agree; empty otherwise. |
| `Major Vote` | `full_dataset_clean.csv` | The majority label if at least two annotators agree; empty for the 244 1-1-1 reviews. |
| `sentiment` | training and test files | Aggregated label used for training and evaluation (`Positive`, `Negative`, `Neutral`). |

## Ethics and privacy

The dataset consists of publicly posted reviews analysed retrospectively. No identifying information about the reviewers was collected, and reviewer names are not included. Some review texts may nevertheless contain names or other personal details that the reviewers wrote themselves; please do not attempt to identify individuals from the data.

## License

The annotations, aggregated labels, data splits and documentation in this repository are released under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). The review texts were written by Google Maps users and remain subject to their authors' rights and to Google's terms of service; they are provided for non-commercial research purposes only.

## Citation

If you use this dataset, please cite the accompanying article (citation details will be updated after publication).
