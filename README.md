# 🎓 Mute but not Silent: Visual Language Identification via Knowledge Distillation

Repository of the notebooks I've used for my Master Thesis' studies and experiments. 

---

## 🧠 Main Objective

The project explores the field of **Visual Language Identification (VLID)**, which aims to recognize the spoken language using only visual information from lip movements, without relying on audio signals. Leveraging state-of-the-art techniques such as cross-modal Knowledge Distillation, the approach introduces a Teacher model trained on audio data to guide a Student model that operates exclusively on video. The Student learns to classify the spoken language by analyzing the lip region, guided by a **Multi-Level Distillation** of the last two layers of the Teacher. The project also investigates how different levels of distillation strength affect the Student’s performance, evaluating which dosage provides the most robust visual-only classification.

---

## 🔍 Research Questions

1. **How can Knowledge Distillation enhance the performance of a video-only model in language recognition?**
2. **How does Knowledge Distillation support a video-only model in recognizing language when dealing with non-native speakers?**
3. **How do emotions affect visual language recognition, and how can Knowledge Distillation improve the robustness of video-only models in such scenarios?**

---

## 🗃️ Datasets

### 1. **BABELE** (Cascone et al., [2023](https://arxiv.org/pdf/2302.13902)).

### 2. **EMODB**

### 3. **VidTIMIT** (Sanderson and Lovell, [2009](https://conradsanderson.id.au/pdfs/sanderson_icb_2009.pdf))

---

## 🗂️ Notebooks
The languages explored for these experiments were Italian, English and Spanish.
| Notebook | Description |
|----------|-------------|
| [`whisper_multilang_distillation_babele.ipynb`](./whisper_multilang_distillation_babele.ipynb) | Model trained on BABELE dataset with both in-domain and cross-domain testing in respectively BABELE and EMODB dataset. |
| [`whisper_multilang_emodb.ipynb`](./whisper_multilang_emodb.ipynb) | Model trained on EMODB and VidTIMIT datasets with both in-domain and cross-domain testing in respectively EMODB+VidTIMIT and BABELE dataset. |
| [`emotional_impact_multilang.ipynb`](./emotional_impact_multilang.ipynb) | Model both trained and tested on EMODB to measure emotion impactness on visual language recognition. |

---

## ⚙️ Architecture and Pipeline

### Teacher (Audio)
- `Whisper-small` (OpenAI) → ASR + feature extraction  
- Extraction of the last two encoder layers (multi-level KD)  
- `detect_language` function used to annotate the videos  

### Student (Video‑only)
- Backbone: `r3d_18` (pretrained on Kinetics)  
- Two MLP heads: one for classification, one for alignment with Teacher logits  
- Robustness techniques: `MixUp`, `MixStyle`  

### Preprocessing
- Lip ROI extraction using `face_alignment`  
- Normalization to 32 frames, 64×64 px, grayscale  

### KD Setup
- Multi-level distillation (last and second-to-last encoder layers)  
- Tested variants:
  - KD on both second-to-last and last layer
  - Light KD on both second-to-last and last layer
  - KD on last layer only
  - KD on second-to-last layer only  
  - No KD

---

## 📈 Evaluation Metrics

- **Macro-F1**: average F1 score across all classes  
- **Accuracy**: correct predictions over total predictions  
- **ROC-AUC (Prob)**: probability score for the English class  
- **ROC-AUC (Margin)**: margin between English logit and next best class  

## 🧪 Experiments

- **BA configuration**: Train on BABELE, test on EMODB  
- **Unification configuration**: Train on EMODB + VidTIMIT (native speakers), test on BABELE  
- **Emotion‑based test**: Train/validate on EMODB with and without emotional speech  
- **Cross‑lingual**: native vs non‑native English speakers  

## 📊 Summary of Results

- Light KD → +20 % F1 in presence of emotions  
- KD on last layer → better separation between native and non‑native speakers  
- In‑domain F1 up to **65 %**, cross‑domain F1 up to **38 %**  

---

### Requirements
Please refer to `requirements.txt` file, before trying to run any of the notebooks.
Please also note, the datasets used are not available in the repository.

---

## License📝
  This project is licensed under the [MIT License](LICENSE).


