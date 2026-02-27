# BoE ARP Insurance Risk Detections

This project identifies risk signals for **59 insurer clients of the Bank of England** using four main models:

- Latent Dirichlet Allocation (LDA)
- Dynamic Topic Models (DTM)
- Sentiment Analysis
- Generalized Additive Model (GAM)

Key findings:

- Predictable events (for example catastrophes, disasters, and economic risks) show recurring patterns.
- Some low-frequency but high-impact events are harder to forecast from historical text alone (for example political movements and pandemics).
- Relevant earnings-call risk content was categorized into 10 topic groups:
  - Natural Damage & Catastrophe
  - Life Insurance
  - Financial Ratio Analysis
  - Corporate Transparency
  - Macroeconomic Uncertainty & Forex Exchange
  - Business Development
  - Stakeholders & Voting
  - Technology Investment
  - Internal & External Policy
  - Japanese Insurers

## Methodology

<img width="814" alt="image" src="https://user-images.githubusercontent.com/61338647/188922377-e82457e3-8582-455e-8408-75587d0dd7e0.png">

## DTM and LDA Model Components

- `get_topic_dist`: Retrieves topic probabilities for each paragraph. A paragraph can represent multiple topics, so full distributions provide richer signals than single labels.
- `get_topic_word`: Generates words sorted by topic probability to support topic interpretation and business insight extraction.

<img width="492" alt="image" src="https://user-images.githubusercontent.com/61338647/188922579-58288aa1-6a67-40e2-9a99-f97bc4c3f39a.png">

## Dynamic Topic Modeling (DTM) Results

- Extreme topic-probability values were analyzed at specific time points.
- Time windows used:
  - Early: 2011 to 2014
  - Middle: 2015 to 2018
  - Recent: 2019 to 2021

![MD-DTM_1](https://user-images.githubusercontent.com/61338647/188923939-82c06a13-7349-42bd-97e2-4f012529521c.png)
![QA-DTM_1](https://user-images.githubusercontent.com/61338647/188924055-1595298d-1be9-410b-bbe9-96f31c740cb9.png)

Frequently raised earnings-call themes:

- General economy: basis points (BPS), currency, and inflation
- Catastrophe: storms, floods, wildfires, and earthquakes

<img width="564" alt="Screenshot 2022-09-07 at 4 53 11 PM" src="https://user-images.githubusercontent.com/61338647/188923557-ff742280-40fe-4bee-8cc9-048159550e76.png">

- Covid, pandemic, and lockdown terms appear repeatedly across multiple topics.
- These terms have cross-topic and deep influence.

<img width="795" alt="image" src="https://user-images.githubusercontent.com/61338647/188924239-1d9ac9b4-e694-477f-be45-308f9d663339.png">

## Latent Dirichlet Allocation (LDA) Results

Preprocessing pipeline:

- Removed special symbols and numbers
- Converted all text to lowercase
- Applied a tagger and kept noun words in lemmatization
- Expanded stopwords
- Filtered short paragraphs (sentence length < 4)
- Removed high-frequency tokens
- Built bi-grams and tri-grams
- Wrapped post-processed documents into corpus and trained the LDA model

<img width="836" alt="image" src="https://user-images.githubusercontent.com/61338647/188924815-4fd35d79-cafc-4661-8625-a85e64c957b4.png">

- Words like `capital`, `growth`, and `company` were considered redundant.
- High-frequency word removal thresholds:
  - Management Discussion: 50 words
  - Questions and Answers: 150 words

<img width="651" alt="image" src="https://user-images.githubusercontent.com/61338647/188924965-6825ef3a-fffd-47c5-97a2-32e7bdc710ea.png">

- Coherence score is not always a good indicator (Nikolenko, Koltcov and Koltsova, 2015).
- It is still helpful for identifying elbow points.

<img width="912" alt="image" src="https://user-images.githubusercontent.com/61338647/188925186-634b38db-c280-4a9a-aacd-b4173e487279.png">

## Sentiment Model Results

1. Accuracy score: 74%
2. Balanced precision for both classes
3. High precision associated with low false-positive rate

<img width="470" alt="Screenshot 2022-09-07 at 5 01 44 PM" src="https://user-images.githubusercontent.com/61338647/188925548-14ae8a23-fd7c-46ed-941d-54e1536a66e8.png">

## GAM Regression Model

Hypotheses:

- **H0**: There is no relationship between topic sentiment in earnings-call transcripts and stock price movement.
- **H1**: There is a significant relationship between topic sentiment in earnings-call transcripts and stock price movement.

<img width="429" alt="image" src="https://user-images.githubusercontent.com/61338647/188925778-f8de74cf-4d64-4f77-af5a-c10ea2ffe8ef.png">
<img width="380" alt="image" src="https://user-images.githubusercontent.com/61338647/188925808-836e9969-fa04-4514-a257-a1ed5d75c09c.png">

Interpretation highlights:

- For follow-up analysis, stock-price movement at two days after conference calls was selected due to the highest Pseudo R2 and topic interpretability.
- There are 23 significant variables (`p_value < 0.05`), supporting the alternative hypothesis.
- Partial dependence plots show non-linear relationships between significant sentiment topics and stock-price movement.

## Repository Structure

- `input/`: source inputs
- `meeting_transcript/`: transcript files
- `output_MD/`, `output_QA/`, `output_MDQA/`: model outputs
- `main_df_output/`: transformed outputs used downstream
- `regression_df_input/`: regression-ready inputs
- `plots/`: generated plots
- `data_cleaning.py`: preprocessing script
- `participant_function.py`: participant-related utility functions
- `*_LDA.ipynb`, `*_DTM.ipynb`, `sentiment_analysis_model.ipynb`, `regression_final.ipynb`: modeling and analysis notebooks

## How to Use

1. Start with `data_cleaning.py` and source data under `input/` and `meeting_transcript/`.
2. Run topic modeling notebooks (`MD_LDA.ipynb`, `QA_LDA.ipynb`, `MDQA_LDA.ipynb`, `MD_DTM.ipynb`, `QA_DTM.ipynb`).
3. Run sentiment pipeline (`sentiment_analysis_model.ipynb`) and build regression inputs (`MD_Regression_input.ipynb`, `QA_Regression_input.ipynb`).
4. Run `regression_final.ipynb` for GAM-based relationship analysis.

## Notes

- Existing visualizations and result graphs are intentionally preserved in this README.
- Notebook execution environment and package versions should be aligned to your local setup for reproducibility.
