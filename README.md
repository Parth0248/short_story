# Short Story

Name: Parth Maradia

Course: CMPE-255 : Data Mining, Fall 2025

Professor: Vijay Eranti

# 🧠 In-Context Learning with Long-Context Models: An In-Depth Exploration

## 📄 Abstract

A new research paper titled **“In-Context Learning with Long-Context Models: An In-Depth Exploration”** challenges the traditional belief that Large Language Models (LLMs) only benefit from a *few* examples. As context windows expand to **32k**, **80k**, and beyond, a new paradigm is emerging: **Many-Shot In-Context Learning (ICL)**.

The researchers discovered that providing LLMs with thousands of examples (effectively the entire training set) inside the prompt can **match or even outperform traditional fine-tuning methods**. They further reveal the mechanism behind this success: the model isn’t *learning* a complex decision boundary in real time, but rather performing **in-context retrieval** — selectively attending to the most relevant examples to answer the query.

Tested on datasets like **Banking-77** and **TREC**, the study proves that **long-context ICL** is a robust, zero-training alternative to fine-tuning, offering a new workflow for data scientists where *“caching context” replaces “training weights.”*

This repository explores the limits of **Long-Context In-Context Learning (ICL)** and its viability as an alternative to **Parameter-Efficient Fine-Tuning (PEFT)**.

---

## 🧭 Overview

In the past, *Few-Shot Learning* meant giving an AI 5 or 10 examples.  
But what happens when we give it **2,000 examples**?

This project analyzes the behavior of LLMs (like **Llama-2-80k** and **Mistral**) when the context window is pushed to its limits. The paper investigates:

- **Scaling Laws:** Does performance plateau, or does it keep getting better with more examples?  
- **Mechanism:** Is the model reasoning over the whole sequence, or just “Ctrl+F”-searching for similar examples?  
- **Efficiency:** Is it better to *stuff the prompt* or *fine-tune the weights*?

> The study finds that massive context windows allow models to act as **Lazy Learners**, retrieving relevant patterns on the fly without the need for gradient updates.

---

## 🏗️ Architecture & Mechanism

### 🧩 The Retrieval Hypothesis

Instead of digesting 2,000 examples into a “task vector,” the model uses its **attention mechanism** to *retrieve* similar examples from the context window to answer the current query.

**Visualizing the Flow:**

1. **Input:** A massive prompt containing thousands of labeled examples (demonstrations)  
2. **Query:** The new, unseen question  
3. **Attention Heads:** The model attends specifically to demonstrations that are semantically similar to the query  
4. **Prediction:** The model copies the label style of the attended examples  

**Evidence:**  
Restricting the model to *Block-Sparse Attention* (forcing it to look only at local blocks of data + the query) barely hurts performance — proving the model doesn’t need to “read” the whole sequence linearly. It only needs to find the relevant *needles* (examples) in the *haystack* (context).

---

## ⚙️ Experiments & Ablation Studies

The researchers compared **Long-Context ICL** against **Fine-Tuning (Full & LoRA)** across multiple datasets.

| Strategy | Description | Outcome |
|-----------|--------------|----------|
| **Random Sampling ICL** | Stuffing random training examples into the context | ✅ Performance scales linearly; surprisingly robust to ordering |
| **Retrieval ICL** | Using BM25 to pick the best examples for the context | ✅ Superior at short contexts, but advantage vanishes at long contexts |
| **Fine-Tuning (LoRA)** | Training adapter weights on the dataset | ⚠️ ICL outperforms LoRA in low-data regimes; LoRA wins only with massive data |

**Key Finding on Ordering:**  
Unlike short-context models (which are highly sensitive to the order of examples — the “recency bias”), long-context models are **robust**.  
Shuffling 1,000 examples changes the output very little compared to shuffling 10 examples.

---

## 📊 Metrics & Evaluation

Evaluation was performed on **5 classification datasets** (TREC, Banking-77, Clinic-150, NLU, TREC-fine) and **1 generation task** (SAMSum).

| Dataset | Metric | ICL Result (2k+ examples) | vs. Fine-Tuning |
|----------|---------|---------------------------|----------------|
| **TREC** | Accuracy | ~90%+ | ICL matches Fine-Tuning |
| **Banking-77** | Accuracy | ~88% | ICL outperforms LoRA at <1000 examples |
| **Clinic-150** | Accuracy | ~93% | ICL outperforms LoRA consistently |

> **Verdict:** Performance continues to increase past 2,000 demonstrations without saturating, effectively utilizing the entire **80k context window**.

---

## 🧩 Key Deliverables

1. **Medium Article**  
   A simplified “Short Story” explaining why we might stop training models and start *contextualizing* them instead.  
   📖 [Read on Medium →](https://medium.com/@parthmaradia2002/the-end-of-fine-tuning-why-many-shot-learning-is-the-new-frontier-8d45d808e250)

2. **Slide Deck**  
   A presentation summarizing:
   - The shift from Few-Shot to Many-Shot  
   - The “Retrieval” discovery  
   - Cost/Benefit analysis of ICL vs. Fine-Tuning  
   📖 [Google Slides →](https://docs.google.com/presentation/d/1ieulwxAsiPPWtcnoeZmZWoZUrtOuVc_Ge5Fl-5SGKII/edit?usp=sharing)

3. **Video Presentation**  
   A video walkthrough explaining the paper’s findings.  
   🎥 [Watch Video →](https://drive.google.com/file/d/19CQKDNb8CS_P4cjY2Ty4zNpFVWTjVDUk/view?usp=sharing)

---

## 🔍 Data Mining Perspective

This paper fundamentally changes how we view **Classification** in Data Mining.

- **Lazy Learning:** Long-context ICL behaves like *k-Nearest Neighbors* — it doesn’t build a generalized function during training; it processes data only when a query arrives.  
- **Vector Similarity:** The mechanism relies on **semantic similarity search** via attention, mining the context window for patterns similar to the query.  
- **Information Retrieval:** Long-context ICL is mathematically similar to **Retrieval-Augmented Generation (RAG)**, but the *database* exists entirely within the prompt’s transient memory.

---

## 🧠 Applications Highlighted

- **Cold Start Problems:** Instant classification for new categories without training pipelines  
- **Dynamic Labeling:** Change class definitions by simply editing text in the prompt  
- **Data Annotation:** Use Many-Shot ICL to auto-label datasets for smaller models  
- **Personalized Agents:** Feed an agent a user’s entire history to predict future intent  

---

## 🚀 Future Directions

- **Context Caching:** Cache the Key-Value (KV) states of prompts to reduce inference cost  
- **Hybrid Models:** Use ICL for rare classes, fine-tuning for common ones  
- **Extreme Scale:** Explore contexts of 1M+ tokens (e.g., Gemini 1.5 Pro) to fit entire databases in-prompt  

---

## 📚 Research Paper Reference

**Bertsch, A., Alon, U., Neubig, G., & Gormley, M. R. (2025).**  
*In-Context Learning with Long-Context Models: An In-Depth Exploration.*  
Proceedings of the 2025 Conference of the Nations of the Americas Chapter of the Association for Computational Linguistics (**NAACL**).  
[arXiv / ACL Anthology Link](https://aclanthology.org/2025.naacl-long.605.pdf)

---

## 🙏 Acknowledgments

This project is based on open-access research from **NAACL 2025** and summarized for educational use in **CMPE 255 – Data Mining**.  
All figures and ideas are credited to the original authors (**Bertsch et al.**) and used here for academic commentary.
