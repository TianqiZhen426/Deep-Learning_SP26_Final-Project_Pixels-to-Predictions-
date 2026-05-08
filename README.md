# Pixels to Predictions: DL Vision Challenge

This project explores parameter-efficient multimodal reasoning for the ScienceQA benchmark using the **SmolVLM-500M-Instruct** vision-language model.

The system was developed under strict competition constraints:

- Maximum **5M trainable parameters**
- Google Colab compute limitations
- Only competition-provided data allowed

The final solution combines:

- 4-bit QLoRA fine-tuning
- LoRA adaptation on attention + MLP layers
- Deterministic choice-scoring inference
- Warmup scheduling
- Category-aware hard-example oversampling

Final Kaggle leaderboard score:

> **77.6% Accuracy**

---

# Model

Base model:

- SmolVLM-500M-Instruct

Quantization:

- 4-bit NF4 QLoRA
- Double quantization
- float16 compute

Final LoRA configuration:

| Parameter | Value |
|---|---|
| Rank | 8 |
| Alpha | 16 |
| Dropout | 0.05 |
| Target Modules | q, k, v, o, up, down |

Trainable parameters:

- 4,784,128
- 1.56% of total parameters

---

# Dataset

Dataset used:

- ScienceQA

Dataset splits:

| Split | Samples |
|---|---|
| Train | 3109 |
| Validation | 1048 |
| Test | 1008 |

Each example includes:

- Image
- Question
- Multiple-choice answers
- Optional lecture/hint context
- Metadata (subject, topic, category, grade)

---

# Prompt Format

```text
<image>

Context:
{lecture + hint}

Question:
{question}

Choices:
A. ...
B. ...
C. ...

Answer:
```

The model predicts answer letters using token-level scoring instead of free-form generation.

---

# Training

Main training settings:

| Setting | Value |
|---|---|
| Optimizer | AdamW |
| Learning Rate | 1e-4 |
| Scheduler | Linear Warmup |
| Warmup Ratio | 5% |
| Batch Size | 2 |
| Training Steps | 500 |

---

# Main Experiments

The project includes controlled ablation studies on:

- LoRA target modules
- DoRA adaptation
- Learning rate tuning
- Prompt engineering
- Multi-seed training
- Training duration
- Dual-prompt inference
- Hard-category oversampling

Most important finding:

> Data-centric optimization produced larger gains than prompt engineering.

---

# Results

Best local validation accuracy:

- 0.7075

Best Kaggle leaderboard score:

- 77.6%

Main improvements came from:

- Attention + MLP LoRA adaptation
- Physics-focused hard-example oversampling
- Stable choice-scoring inference

---

# Error Analysis

The model performs well on:

- Geography
- Social science
- Science-and-engineering practices

Hardest categories:

- Genes to Traits
- Particle Motion and Energy
- Solutions
- Weather and Climate

The system also struggles with 5-choice questions.

---

# Future Improvements

Potential future directions:

- Higher image resolution
- Task-aware image augmentation
- Better ensemble scoring
- Dynamic hard-example sampling
- More advanced parameter-efficient adaptation

---

# Author

Tianqi Zhen  
New York University
