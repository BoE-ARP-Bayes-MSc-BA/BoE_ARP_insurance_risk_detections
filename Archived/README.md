

# BoE_ARP_insurance_risk_detections
In order to identify risks of the 59 insurer clients of the Bank of England, we implemented four main models: Latent Dirichlet Allocation (LDA), Dynamic Topic Models (DTM), Sentiment Analysis, and Generalized Additive Model (GAM).
We found that patterns of predictable events, such as catastrophes, disasters, and economic risks, could be seen for the common risks. There were, however, some unforeseeable events that did not frequently appear in history, such as political movements and pandemics. 
We successfully categorised relevant topics that could have impact on the stock price movements into 10 groups; Natural Damage & Catastrophe, Life Insurance, Financial Ratio Analysis, Corporate Transparency, Macroeconomic Uncertainty & Forex Exchange, Business Development, Stakeholders & Voting, Technology Investment, Internal & External Policy, and Japanese Insurers. 

## Methodology
<img width="814" alt="image" src="https://user-images.githubusercontent.com/61338647/188922377-e82457e3-8582-455e-8408-75587d0dd7e0.png">

## DTM and LDA model
- "get_topic_dist": Retrieves all topics probabilities of the paragraph. Instead of giving the label of a specific topic, a single paragraph might describe multiple topics, and retrieving all topics' probabilities would help present more detailed and precise data.
- "get_topic_word": Generates the words by sorted probability of each topic, and it would give us a general topic concept when seeking business insights.
<img width="492" alt="image" src="https://user-images.githubusercontent.com/61338647/188922579-58288aa1-6a67-40e2-9a99-f97bc4c3f39a.png">

## Result of dynamic topic modelling (DTM) 
- Analyzed extreme probability values at specific time points.
- Early time (2011 to 2014), Middle time (2015 to 2018), Recent time (2019 to 2021)
![MD-DTM_1](https://user-images.githubusercontent.com/61338647/188923939-82c06a13-7349-42bd-97e2-4f012529521c.png)
![QA-DTM_1](https://user-images.githubusercontent.com/61338647/188924055-1595298d-1be9-410b-bbe9-96f31c740cb9.png)

- Frequently raised in the earnings call:
  - general economy: basis points (BPS), currency, and inflation
  - Catastrophe: storms, floods, wire fires, and earthquakes
<img width="564" alt="Screenshot 2022-09-07 at 4 53 11 PM" src="https://user-images.githubusercontent.com/61338647/188923557-ff742280-40fe-4bee-8cc9-048159550e76.png">

- Covid, pandemic, and lockdown frequently appear in different topics
- Cross-topic and with a deep influence
<img width="795" alt="image" src="https://user-images.githubusercontent.com/61338647/188924239-1d9ac9b4-e694-477f-be45-308f9d663339.png">

## Result of latent Dirichlet allocation topic modeling (LDA)
- Removed special symbols and numbers.
- Converted all words to lowercase
- Applied tagger to extract only noun words in the lemmatization 
- Expanded the stopwords 
- Paragraph length and remove paragraphs with sentence lengths less than four
- Removed high-frequency tokens
- Processed the documents to join the tokens associated with bi-grams or tri-grams
- Wrapped the post-processed documents in corpus and input them into the LDA model 
<img width="836" alt="image" src="https://user-images.githubusercontent.com/61338647/188924815-4fd35d79-cafc-4661-8625-a85e64c957b4.png">

- words like “capital”, “growth”, and “company” are redundant  
- remove high-frequency words for:
  - Management Discussion of 50 words
  - Questions and Answers of 150 words
<img width="651" alt="image" src="https://user-images.githubusercontent.com/61338647/188924965-6825ef3a-fffd-47c5-97a2-32e7bdc710ea.png">

- Coherence score is not always a good indicator (Nikolenko, Koltcov and Koltsova, 2015)
- Can help us identify elbow points 

<img width="912" alt="image" src="https://user-images.githubusercontent.com/61338647/188925186-634b38db-c280-4a9a-aacd-b4173e487279.png">

## Sentiment model results
1. Accuracy score in 74%
2. Balance precision for both classes
3. High precision related to the low false positive rate
<img width="470" alt="Screenshot 2022-09-07 at 5 01 44 PM" src="https://user-images.githubusercontent.com/61338647/188925548-14ae8a23-fd7c-46ed-941d-54e1536a66e8.png">

## GAM regression model
- H0: There is no relationship between the sentiment score of the topic in earning call transcripts and stock price movement.
- H1: There is a significant relationship between the sentiment score of the topic in earning call transcripts and stock price movement.
<img width="429" alt="image" src="https://user-images.githubusercontent.com/61338647/188925778-f8de74cf-4d64-4f77-af5a-c10ea2ffe8ef.png">
<img width="380" alt="image" src="https://user-images.githubusercontent.com/61338647/188925808-836e9969-fa04-4514-a257-a1ed5d75c09c.png">

- After trying to interpret significant topics, this researcher would choose to study further more in stock price movement at two days after conference call as it has the highest Pseudo R2 and interpretable topics
- Results show that there are 23 significant variables (p_value < 0.05), which mean that our hypothesis has been proven.
- Partial dependence plots illustrated the non-linear relation between each significant sentiment topics and stock price movement





