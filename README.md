# Improving Multimodal Topic Modeling with VLM2Vec and Semantic Component Analysis (SCA)

**Contacts**  
- [yurui.zhang@tum.de](mailto:yurui.zhang@tum.de)  
- [amr.s.mohamed@tum.de](mailto:amr.s.mohamed@tum.de)  
- [mahdi.bayouli@tum.de](mailto:mahdi.bayouli@tum.de)

This repository presents our project on **enhancing Multimodal Topic Modeling**. We build on the original work _“Multimodal Topic Modeling Using Pretrained Embeddings”_ by our advisor **Alexander Kowsik**, reusing the same datasets and baseline experiments. We compare **Semantic Component Analysis (SCA)** vs. **BERTopic** for **multimodal topic modeling**, and introduce **VLM2Vec** and **VLM2VecComb** embeddings into the pipeline.

---

## Introduction

Topic modeling helps uncover hidden themes in large text datasets, making analysis more efficient. It is used for organizing corpora around “latent topics,” providing:

- Automated **document clustering** (grouping by semantic themes).  
- High-level **insights and exploration** of unstructured data.  
- **Trend analysis** and categorization for social media, reviews, etc.

However, traditional topic modeling (e.g., **LDA** or **BERTopic**) typically deals with **text-only** data, missing valuable cues from **images** or **captions**.  
**Multimodal topic modeling** addresses this gap by:
- Integrating **images, text, and optional captions** into a shared semantic space.  
- Capturing **richer contextual patterns** overlooked by purely text-based approaches.

### Our Project Scope
1. **SCA (Semantic Component Analysis)**  
   An **iterative extension** of BERTopic that can discover multiple topics per document, reducing outliers and capturing more nuanced “semantic components.”

2. **VLM2Vec**  
   A new embedding model that can handle **any combination of images and text** to produce **task-specific, fixed-dimensional vectors**—beneficial for clustering-based topic modeling.

3. **Comparisons**  
   We replicate experiments on the same datasets from Alexander Kowsik’s work and compare **BERTopic vs. SCA** using **CLIP**, **ImageBind**, **PaLM**, **VLM2Vec**, and **VLM2VecComb**.

---

## Methodology

![image](https://github.com/user-attachments/assets/e6c4d0ff-10cb-4978-850c-6a490bf69605)

### Datasets

We focus on the same three multimodal datasets from Kowsik’s original work:

- **RCF (Retail Customer Feedback)**: Real-world retail reviews with images.  
- **MEIR (Multimodal Entity Image Repurposing)**: Pairs of text and images describing various entities.  
- **WIT (Wikipedia-based Image-Text)**: Subset of Wikipedia data containing text and images.

Each dataset includes text-image pairs; some also include captions.

### Embedding Models & Combinations

1. **VLM2Vec & VLM2VecComb**  
   - Trained via a contrastive framework ([VLM2Vec Repo](https://tiger-ai-lab.github.io/VLM2Vec/)).  
   - **Instruction-based** representation (helpful for domain/task-specific embeddings).  
   - Handles **any resolution** image and **variable-length** text.

   ![image](https://github.com/user-attachments/assets/e72963d5-006d-46ef-8dec-ac1dc298c5fd)


2. **CLIP, ImageBind, PaLM**  
   - Baseline multimodal embeddings to compare text-image alignment (these are the same embedding models used in the original work).  

3. **Combination Strategies**  
   - **Sum & Concat**:  
     - **Sum**: Simple vector addition.  
     - **Concat**: Vector concatenation.  
   - **Modalities**: `Text_Image`, `Text_ImageCaption`, `Image_ImageCaption`, `Text_Image_ImageCaption`  
   - **VLM2VecComb**: Direct combined embedding output.  

   ⇒ In total, **9 embedding combinations** (including `CombinedEmbedding`) were tested across the datasets.

### Topic Modeling Approaches

1. **BERTopic** ([paper](https://arxiv.org/abs/2203.05794))  
   - Density-based clustering (UMAP + HDBSCAN).  
   - **c-TF-IDF** for topic representations.  
   - **Limitation**: Each document is assigned **one** topic.

2. **Semantic Component Analysis (SCA)** ([paper](https://arxiv.org/abs/2410.21054#:~:text=We%20introduce%20Semantic%20Component%20Analysis,clustering%2Dbased%20topic%20modeling%20framework.))  
   - An **iterative extension** of BERTopic that decomposes embeddings to uncover multiple overlapping “semantic components.”  
   - **Advantages**:  
     - **Lower outlier rate**.  
     - Captures **multi-faceted** topics, especially in short texts.

![image](https://github.com/user-attachments/assets/3e9ea377-e20a-4372-803b-64f11259e62b)
![image](https://github.com/user-attachments/assets/19a27f23-d0ce-49cd-b10c-b86a0e17454b)

---

## Experiments & Results

We ran **BERTopic** and **SCA** on all three datasets, testing each embedding + combination strategy. Our metrics:

- **Text Topic Coherence** (NPMI, CV, or UMass).  
- **Text Topic Diversity** (proportion of unique tokens across topics).  
- **Image Topic Coherence** (e.g., image intruder tests).  
- **Image Topic Diversity** (distinctiveness of image clusters).

### Highlights

- **VLM2VecComb + SCA** often yields a stronger balance between **text coherence** and **image diversity**, especially if text-image alignment is robust.
- **SCA** typically **reduces outliers** more effectively than BERTopic, though it can fragment topics slightly and lower average coherence (due to finer-grained topics).
- Detailed tables and metrics can be found in these CSVs:
  - [BERTopic Results CSV](https://docs.google.com/spreadsheets/d/1RYO81cRGVMvY-l9qnLHmBW8jvlZ2YfmtgXjVCiu2V7Q/edit?usp=sharing)  
  - [SCA Results CSV](https://docs.google.com/spreadsheets/d/1h6qcMCLeWMEtM-rE-vizIpniOvOwV-gagZVgOXAeNhk/edit?usp=sharing)

---

## Conclusion & Future Work

Our findings confirm that **Semantic Component Analysis (SCA)** can uncover richer, more fine-grained topics from multimodal data than conventional single-topic methods (like BERTopic), especially when paired with **VLM2Vec** embeddings for flexible, instruction-driven representations.  

**Future Directions**:
- Add more modalities (audio, video).  
- Explore advanced instruction-based tuning of VLM2Vec.  
- Investigate improved evaluation metrics for multimodal topic coherence.

We welcome contributions, suggestions, and issues from the community. Feel free to create a pull request or open an issue in this repository!

---

## References

1. **Kowsik, A.**: _Multimodal Topic Modeling using Pretrained Embeddings._  
2. **Grootendorst, M.**: _BERTopic: Neural topic modeling with a class-based TF-IDF procedure._  
3. **Eichin, F. et al.**: _Semantic Component Analysis: Discovering Patterns in Short Texts Beyond Topics._  
4. **Jiang, Z. et al.**: _VLM2Vec: Training Vision-Language Models for Massive Multimodal Embedding Tasks._  

Thank you for checking out our project! For any questions or to report issues, please contact us.  
