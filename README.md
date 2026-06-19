# January 6th Capitol Twitter Analysis

![R](https://img.shields.io/badge/R-4.x-276DC3?style=flat&logo=r&logoColor=white)
![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=flat&logo=python&logoColor=white)
![RoBERTa](https://img.shields.io/badge/Model-RoBERTa-orange?style=flat)
![License](https://img.shields.io/badge/License-Academic-lightgrey?style=flat)

A longitudinal social media text analysis of Twitter data related to the January 6th, 2021 Capitol events. The analysis covers tweet volume trends, multi-lexicon sentiment analysis (AFINN, NRC), syntactical (VADER), and transformer-based sentiment classification via RoBERTa, spanning a 48-hour window centred on the events of January 6th.

**Live analysis:** [View here](https://sagefuentes.github.io/Jan-6-Twitter-Data/)

> Python scripts for VADER and RoBERTa sentiment processing can be found in the companion repository: [Sentiment-Analysis-Wrapper](https://github.com/sagefuentes/Sentiment-Analysis-Wrapper)

---

## Project Overview

The original dataset was collected in 2021 under Twitter Academic Research Developer permissions using `snscrape`. It captures all tweets containing the word "Capitol" between January 5th, 2021 at 12:00 PM EST and January 7th, 2021 at 12:00 PM EST — a window chosen to capture:

- The 24 hours before and after Trump's 12 PM speech at the Ellipse
- The Capitol breach and simultaneous events across the country
- Reactions from US West Coast users well into the night
- The certification of Joe Biden as 46th President at 3:42 AM on January 7th

This project revisits the original 2021 analysis with updated methodology, replacing older tools with current NLP approaches while preserving the original data pipeline for transparency.

---

## Methods

### Text Preprocessing

Two separate text representations were prepared for different analytical purposes:

- **Lexicon-friendly tokens** (`token_text`) — URLs, mentions, emojis, and non-alphanumeric punctuation removed; normalised to lowercase for exact dictionary matching against AFINN and NRC lexicons
- **Syntactic context text** (`sentence_text`) — noise filtered but casing, sentence structure, and punctuation preserved; used for VADER and RoBERTa where linguistic nuance and structural modifiers carry meaning

### Sentiment Analysis

| Method | Type | Notes |
|---|---|---|
| **AFINN** | Lexicon (R) | Word-level valence scoring with a ±0.05 neutral band |
| **NRC** | Lexicon (R) | Emotion categorisation across 8 affect dimensions |
| **VADER** | Rule-based (Python) | Handles social media conventions; compound score used |
| **RoBERTa** | Transformer (Python) | `cardiffnlp/twitter-roberta-base-sentiment`; GPU inference via Google Colab T4 |

### Longitudinal Analysis

Tweet volume and sentiment scores are tracked across the 48-hour window at hourly resolution, with key event timestamps marked for contextual interpretation.

---

## Repository Structure

```
├── data_analysis.Rmd          # Main R Markdown analysis document
├── index.html                 # Rendered output (GitHub Pages)
├── data/
│   ├── interim/               # Preprocessed parquet files (not included — see below)
│   └── processed/             # VADER and RoBERTa results parquet files (not included)
└── README.md
```

---

## Data

The raw dataset is not included in this repository. The analysis pipeline reads from Parquet files generated during preprocessing. Due to file size constraints, processed data will be hosted externally at a future date.

**Original dataset fields:**

| Field | Description |
|---|---|
| `Date` | Datetime in UTC; converted to EST in preprocessing |
| `subject_id` | Anonymised user ID replacing original usernames |
| `Text` | Raw tweet text including HTML entity encoding |
| `Outlinks` | Outbound URLs present in the tweet |
| `ReplyCount` | Number of replies |
| `RetweetCount` | Number of retweets |
| `LikeCount` | Number of likes |
| `QuoteCount` | Number of quote tweets |

> **Privacy note:** Original usernames have been replaced with anonymised subject IDs. Some tweets from prominent public figures may still appear within direct discussion contexts.

---

## Dependencies

### R Packages

```r
tidyverse, arrow, lubridate, tidytext, textclean,
gt, textdata, scales, patchwork
```

### Python (companion repo)

```
vaderSentiment, transformers, torch, pandas, pyarrow
```

RoBERTa inference was performed on a GPU runtime (Google Colab T4) due to the computational requirements of transformer-based batch inference.

---

## Technical Notes

- Timezone handling uses `lubridate::with_tz()` to convert UTC timestamps to `America/New_York` — the original data was collected in PST encoded as UTC, requiring careful correction before any temporal analysis
- Parquet format used throughout via `arrow` for efficient storage and read performance on a dataset of this size
- VADER and RoBERTa were run via Python rather than R wrappers to ensure model fidelity and processing reliability; results were written to Parquet and read back into R for visualisation
- LDA topic modelling and hashtag analysis were considered but omitted to keep the scope focused on sentiment methodology
