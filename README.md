<img width="200" height="200" alt="image" src="https://github.com/user-attachments/assets/5019e8df-3f06-4291-beff-569e677d999c" />

# Uncovering Symbolism in User Poetry with LLMs

This repository fine-tunes GPT-3.5 on 979 user-submitted poems from r/OCPoem to detect literary symbolism using GPT-4.1-generated ground truth.

* **IMPORTANT:** This dataset was created using © Reddit API and OpenAI API. The original poem dataset is not distributed here to comply with terms of service. You must obtain your own API keys for both services and follow the provided pipelines in order to replicate. **This project cost $7.12**

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

Model training was conducted locally on a custom-built **Windows PC equipped with an AMD Ryzen 7 7700X CPU, RTX 4060 GPU, and 64 GB of DDR5 RAM**, using Jupyter Notebook. Labeling took 20 minutes, fine-tuning and evaluating the model both took 1 hour and 30 minutes.

**Challenges:** This was my first time working with LLMs, so there was a learning curve in understanding how to use the OpenAI API, manage prompts, handle responses, and navigate the fine-tuning process; including the cost and rate limits. Additionally, I initially forgot to separate a training set before labeling the full dataset, which turned the evaluation into more of a post-hoc analysis rather than a clean train/test split (which is still okay if low-stakes)

### GPT-3.5-Turbo Model Performance
Loss was grouped by 50 steps in order to reduce crowding. 2937 steps total.

<img width="852" height="547" alt="image" src="https://github.com/user-attachments/assets/80535aaa-3a93-4d29-be35-c417d895bf9d" />

**Evaluation Summary Table:**

<img width="519" height="53" alt="Screenshot 2025-08-06 at 8 08 53 PM" src="https://github.com/user-attachments/assets/e6a95610-4bc4-492e-a0c7-7e01c8ddfe0f" />

**Evaluation Metrics Used:**
* Exact Jaccard (Strict): Measures exact theme sets matched
* Token Jaccard (Forgiving): Measures partial overlap
* Precision/Recall: Of the predicted themes, how many were correct? Of the true themes, how many were successfully predicted?
* Embedding/Cosine Similarity: Compares semantic similarity (meaning)

<img width="989" height="690" alt="image" src="https://github.com/user-attachments/assets/c28eba34-f6d9-40a1-a0ff-9a59e1396a28" />

### Conclusions

-----------1qw12q

#### **Model Deployment**

*"Empty prayers, empty truth, empty faith. Dying hope, I no longer said His name. But He found me and said He loved me just the same because it was the day true love bloomed. No, it had always been that way."*

* Fine-Tuned GPT-3.5-turbo Themes: ['emptiness', 'faith and doubt', 'hope and despair', 'love's persistence', 'truth and perception']
* GPT-4.1-mini Themes: ['emptiness and despair', 'lost and found love', 'enduring faith', 'renewal and blooming', 'unspoken connection']

**Human Analysis:** There are **several surface-level overlaps between the two sets of themes**, such as "emptiness", "faith", and "love", suggesting that the fine-tuned **GPT-3.5 model captured the core emotional threads of the poem.** However, **GPT-4.1-mini demonstrates a deeper interpretive ability**, identifying more nuanced concepts like "renewal and blooming" and "unspoken connection." In contrast, **GPT-3.5's themes are more literal and generalized**, such as "truth and perception" and "love’s persistence." **Overall, while GPT-3.5-turbo successfully learned to associate symbolic ideas with the text, GPT-4.1-mini appears more capable of abstract reasoning and subtle emotional inference.**

### Future Work
 
* **Increase dataset size** to support deeper and more expressive models
* **No post-hoc evaluation** to test generalization on unseen data. Next time, split the data beforehand into train_df and eval_df, fine-tune using only train_df, and then evaluating on eval_df

### Overview of Files in Repository

The list below follows the chronological order in which each component of the project was developed:

* **LLM_Proposal_Cornelio:** This project's proposal which includes background information and an abstract
* **create_poem_dataset.ipynb:** Create poem dataset using Reddit API
    * **Output Files:** All files minus original poem datasets (to respect terms of service)
* **clean_dataset.ipynb:** Clean and pre-process poem dataset before labeling
* **data_labeling.ipynb:** Label poem dataset using GPT-4.1-mini for baseline
* **train_and_fine_tune.ipynb:** Train and fine tune using GPT-3.5-turbo
* **evaluation_and_deployment.ipynb:** Evaluation and deployment of fine tuned model

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
  * precision_score, recall_score, PCA
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

[1] --------

[2] --------





