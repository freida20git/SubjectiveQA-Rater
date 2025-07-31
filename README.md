# SubjectiveQA-Rater

Predicting subjective scores for StackExchange Q&amp;A using NLP and human-labeled data.



**Motivation:**

One of the most fascinating and challenging tasks in Natural Language Processing (NLP) is handling subjective content – human interpretations that can vary greatly between individuals.

In this project, we tackle subjective question–answer (QA) evaluation using data from platforms like
StackExchange and other QA forums.
Each QA pair is annotated with multiple subjective metrics (e.g., Is the question well-written?, Is the 
answer
helpful?, Does it contain instructions?).

Building a model for this task is complex because:
* Labels are continuous and subjective, not discrete classes.
* Understanding whether an answer truly satisfies a question requires contextual alignment between them.
* Traditional classification or regression alone is often insufficient to capture the nuanced human judgment.

Our goal: Automatically evaluate QA pairs to reduce reliance on human raters and enable scalable, efficient, 
and consistent subjective content evaluation.

---

📁 Repo Structure:

as we explored many models, the training notebooks of the different models are included as well as some of 
the model files themselves.

__Data and EDA notebook:__

  data/

    EDA_qa_data.ipynb 

__Model training notebooks:__

  notebooks/
  
    CrossAttention_siameseRoBERTa.ipynb
    
    RoBERTa_Net.ipynb
    
    siamiseNet.ipynb
    
    BERT_Multi_head.ipynb
    
    simple_regressor.ipynb
  
__Run Inference:__

    Inference_QArater.ipynb

__Model files (.pth):__

    RoBERTa_5epochs.pth 
  
    Siamese_CrossAttention_RoBERTa_10epochs.pth
  
---

📊 Results Overview:

We iteratively improved our models and achieved low loss and strong correlations with human ratings:

* Best Loss (MSE): ~0.04

* High Pearson & Spearman correlations(between prediction and real rating for metric) on most metrics(most 
arround 0.5 and maximum of 0.75)

* Inference examples make sense, especially with the Siamese + Cross-Attention model

---

🔄 Model exploration and Workflow

We explored several architectures step by step:

1. __Simple BERT regressor – single output layer predicting 30 metrics directly.__

✅ Low loss

❌ Extremely low correlation with ground truth

2. __BERT multi-head regressor (30 outputs)__
   
✅ Loss ~0.02 (on train only!)

❌ Fine correlations in some metrics but many of them low!

3. __Siamese BERT – question and answer encoded separately but with shared weights__
   
✅ Good correlations

⚠ Inference examples were less convincing

4. __Metric grouping (from 30 → 9)__
   
Grouped related metrics to simplify the learning target (e.g., answer helpfulness, question quality)

✅ Improved generalization and interpretability

5. __Roberta-based Siamese model__
   
✅ Lower loss and better correlations

✅ More robust inference

6. __Siamese + Cross-Attention model__

Cross-attention lets question tokens attend to answer tokens (q2a, or a2q)

✅ Strong inference results and more intuitive behavior

✅ Best-performing configuration overall

---

How to Use:
1. Runing inference with the model.pth file in the repo, or your own;) and the 
  Inference_QArater.ipynb notebook.
2. To train models, check the notebooks/ folder.

  **Inference Example:**
<img width="1004" height="430" alt="image" src="https://github.com/user-attachments/assets/08d8bad3-4256-4b92-a229-4bdab9eb93ff" />

---

**Data:**

The dataset contains ~6K QA pairs, mostly from StackExchange.
Each QA pair has 30 human-labeled subjective metrics, including:
- Question clarity, writing quality, and conversational tone
- Answer helpfulness, relevance, and level of detail
- Opinion-seeking and instruction/procedure indicators

Categories:

Most questions come from technical topics, as shown in the distribution below:
<img width="961" height="512" alt="Screenshot 2025-07-31 142542" src="https://github.com/user-attachments/assets/d18013f6-9c9c-43a6-8720-51bcfd76bbf5" />

Metric Correlation:

Many of the 30 metrics are inter-correlated, which motivated grouping into 9 composite metrics:
<img width="929" height="779" alt="Screenshot 2025-07-31 130149" src="https://github.com/user-attachments/assets/b5297e34-fede-4711-bd2b-f83e531f0175" />

Data available at: https://drive.google.com/file/d/1FXPgFkR0fTZz31ZaI3RivjNBtsTfH0S7/view?usp=drive_link 

