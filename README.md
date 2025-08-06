<img width="200" height="200" alt="image" src="https://github.com/user-attachments/assets/5019e8df-3f06-4291-beff-569e677d999c" />

# Uncovering Symbolism in User Poetry with LLMs

This repository fine-tunes GPT-3.5 on 979 user-submitted poems from r/OCPoem to detect literary symbolism using GPT-4.1-generated ground truth.

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
    * 979 training, 196 validation 


