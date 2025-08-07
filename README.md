<img width="200" height="200" alt="image" src="https://github.com/user-attachments/assets/5019e8df-3f06-4291-beff-569e677d999c" />

# Uncovering Symbolism in User Poetry with LLMs

This repository fine-tunes GPT-3.5 on 979 user-submitted poems from r/OCPoem to detect literary symbolism using GPT-4.1-generated ground truth.

* **IMPORTANT:** This dataset was created using © Reddit API and OpenAI API. The original poem dataset is not distributed here to comply with terms of service. You must obtain your own API keys for both services and follow the provided pipelines in order to replicate. **This project costs $7.12**

## OVERVIEW

This project detects and classifies literary symbolism in user-submitted poems. Framed as a multi-label text classification problem, 5 common symbolic themes were given for each entry. To ensure the model was trained on unseen data, 979 original poems from Reddit’s r/OCPoem were scraped. **GPT-4.1-mini was used to generate ground truth labels, and GPT-3.5-turbo was fine-tuned on this labeled dataset.** Model performance was evaluated by comparing each model's labels, achieving a **token-level Jaccard score of 0.418**, **precision and recall of 0.202**, and a **cosine embedding similarity of 0.678**.

## SUMMARY OF WORK DONE

### Data

  * **Type:**
    * Input: CSV of 979 poems
    * Output: CSV containing 5 symbolism themes for each poem
  * **Source:**
    * Custom-built data set using Reddit and OpenAI API
  * **Splits:**
    * 979 training, 197 validation
   
### Compiling Data and Pre-processing

* **Data Collection** using Reddit API to scrape poems from r/OCPoem
* **Data Cleaning**
    * Normalized all text to lowercase
    * Fixed common Unicode artifacts and removed non-ASCII characters
    * Removed URLs, bracketed numbers, and the words “feedback” and “link”
    * Stripped duplicated title formatting and unwanted symbols while preserving useful punctuation
    * Removed newlines and normalized whitespace
    * Filtered out non-English poems using language detection
* **Data Labeling** using OpenAI API to label poem dataset with GPT-4.1-mini

### Problem Formulation

* **Goal:** Evaluate whether GPT-3.5-turbo can learn to detect symbolic themes in poetry by imitating GPT-4.1-mini's labeling, effectively transferring symbolic reasoning through fine-tuning
* **Models Used:**
  * **gpt-4.1-mini:** labeling, used to assign high-quality ground truth theme labels
  * **gpt-3.5-turbo:** fine-tuned model, lower cost and faster training time
  
### Training

Model training was conducted locally on a custom-built **Windows PC equipped with an AMD Ryzen 7 7700X CPU, RTX 4060 GPU, and 64 GB of DDR5 RAM**, using Jupyter Notebook. Labeling took 20 minutes, fine-tuning the model took 1 hour, 30 minutes, and evaluation took 1 hour.

**Challenges:**

### GPT-3.5-Turbo Model Performance
<img width="852" height="547" alt="image" src="https://github.com/user-attachments/assets/80535aaa-3a93-4d29-be35-c417d895bf9d" />

Loss was grouped by 50 steps in order to reduce crowding. 2937 steps total.

<img width="543" height="80" alt="Screenshot 2025-08-06 at 7 26 37 PM" src="https://github.com/user-attachments/assets/0c803bf1-730e-48d0-b735-bfe0c0b848b6" />

* **Exact Jaccard (Strict):**
* **Token Jaccard (Lenient):**
* **Precision/Recall:**
* **Embedding/Cosine Similarity:**

### Conclusions

#### **Model Deployment**

**WHAT THIS MEANS:** 

### Future Work
 
* **Increase dataset size** to support deeper and more expressive models
* **No post-hoc evaluation** to test generalization on unseen data. Next time, split the data beforehand into train_df and eval_df, fine-tune using only train_df, and then evaluating on eval_df

### Overview of Files in Repository

The list below follows the chronological order in which each component of the project was developed:

* **LLM_Proposal_Cornelio:** This project's proposal which includes background information and an abstract
* **create_poem_dataset.ipynb:** 
    * **Output Files:**
* **clean_dataset.ipynb:** 
* **data_labeling.ipynb:** 
* **train_and_fine_tune.ipynb:**
* **evaluation_and_deployment.ipynb:**

### Software Setup

* praw
  * RequestException, ResponseException, ServerError
* csv
* time
* pandas
* re
* html
* langdetect
    * detect, LangDetectException
* unicodedata
* openai
* json
* os
* matplotlib
* from collections import defaultdict
* sklearn
  * precision_score, recall_score
* ast

### Data

* **Websites Used:**
    * Reddit API: https://www.reddit.com/prefs/apps/
    * OpenAI API: https://openai.com/api/

***For reference, see create_poem_dataset.ipynb***

### Training

* Install required packages in notebook
* Create, prepare, and train/fine-tune the data using the provided pipelines

***For reference, see clean_dataset.ipynb, data_labeling.ipynb, and train_and_fine-tune.ipynb***

### Performance Evaluation

* After training, model performance can be evaluated using the validation set (20%)
* Run the evaluation script to compute and visualize Jaccard similarities, precision/recall, and embedding/cosine similarity

***For reference, see evaluation_and_deployment.ipynb***

## CITATIONS

[1] 

[2] 





