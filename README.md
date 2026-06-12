# January 6th Capitol Twitter Analysis

A social media text analysis of Twitter data related to the January 6th,
2021 Capitol event, including longitudinal tweet volume analysis, AFINN,
VADER, and RoBERTa sentiment analysis.

Scripts for Python processing of VADER and RoBERTa can be found [here](https://github.com/sagefuentes/Sentiment-Analysis-Wrapper)

**Live analysis:** [View here](https://sagefuentes.github.io/Jan-6-Twitter-Data/)

## Data
Raw data is not included in this repository. The analysis pipeline reads
from parquet files generated during preprocessing. Data will be uploaded
at a future time using external methods as it exceeds GitHub's file size 
limits.

## Dependencies
R packages: tidyverse, arrow, lubridate, tidytext, textclean, gt,
textdata, scales, patchwork
