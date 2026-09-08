# Chinese ASR Self-Training with WeNet & Whisper

A Mandarin automatic speech recognition project exploring end-to-end ASR, external language-model decoding, pseudo-label generation, and semi-supervised self-training using **WeNet** and **Whisper**.

The project investigates whether unlabeled speech data can improve recognition robustness across both in-domain and cross-domain evaluation sets.

## Highlights

- Trained a supervised Mandarin ASR baseline using **WeNet** with a Conformer-based architecture.
- Integrated an external **n-gram language model** using SRILM and OpenFST.
- Used **AIShell-2** as pseudo-labeled data for semi-supervised self-training.
- Implemented self-training experiments using both **WeNet** and **Whisper**.
- Evaluated generalization on **AIShell-2018** and **Common Voice zh**.
- Reduced WeNet AIShell-1 CER from **5.10% to 4.62%** using language-model-assisted decoding.
- Reduced Whisper CER from **13.00% to 7.11%** on AIShell-2018 after self-training.
- Reduced Whisper CER from **35.87% to 24.24%** on Common Voice zh.

## Motivation

Modern end-to-end ASR systems simplify the traditional speech-recognition pipeline by directly mapping acoustic inputs to text.

However, their performance still depends heavily on labeled speech data, which is expensive to obtain.

This project explores **self-training**, a semi-supervised learning strategy in which an existing ASR model generates pseudo-labels for additional speech data. These automatically generated labels are then incorporated into the training process.

The experiments study the approach across two different ASR systems:

- WeNet / Conformer
- OpenAI Whisper

## System Pipeline

```text
                       AIShell-1
                     labeled speech
                          │
                          ▼
                 Supervised ASR Model
                          │
             ┌────────────┴────────────┐
             │                         │
             ▼                         ▼
       LM-assisted decoding       AIShell-2 speech
             │                         │
             │                  pseudo-label generation
             │                         │
             └────────────┬────────────┘
                          ▼
                     Self-Training
                          │
                          ▼
                  Updated ASR Model
                          │
             ┌────────────┴────────────┐
             ▼                         ▼
       AIShell-2018              Common Voice zh
        evaluation                 evaluation
```

## Datasets

### AIShell-1

AIShell-1 is used as the main labeled Mandarin corpus for supervised baseline training.

### AIShell-2

AIShell-2 is used as the additional speech corpus for pseudo-label generation and self-training.

The trained seed models decode AIShell-2 and generate pseudo-transcriptions, which are then incorporated into subsequent fine-tuning.

### AIShell-2018

Used as an external evaluation set to assess generalization beyond the original training distribution.

### Common Voice zh

The Mandarin subset of Common Voice is used as a cross-domain evaluation dataset with different speakers and recording conditions.

Audio is standardized to **16 kHz** and converted into the manifest structure required by the ASR pipeline.

## WeNet Baseline

The initial supervised system is implemented using the **WeNet** framework.

The acoustic model uses a Conformer-based architecture, combining convolutional modeling of local acoustic patterns with Transformer-style self-attention for long-range dependencies.

The baseline is trained using AIShell-1 and evaluated using Character Error Rate (**CER**).

## External Language Model

A traditional n-gram language model is trained using **SRILM** and integrated into the decoding pipeline.

**OpenFST** is used as part of the language-model decoding infrastructure.

During implementation, a compatibility problem between the updated WeNet environment and OpenFST prevented the decoding pipeline from compiling correctly.

The build configuration was corrected and OpenFST was recompiled, restoring language-model-assisted decoding.

## Effect of Language-Model Integration

| Model | AIShell-1 CER | AIShell-2018 CER | Common Voice zh CER |
| --- | ---: | ---: | ---: |
| Supervised WeNet | 5.10% | 12.67% | 35.21% |
| WeNet + LM | **4.62%** | **12.35%** | **34.29%** |

The external language model provides the largest improvement on AIShell-1.

The gains on the external evaluation sets are smaller, which indicates the effect of domain mismatch between the training corpus and cross-domain speech.

## WeNet Self-Training

The supervised WeNet model is used to generate pseudo-labels for AIShell-2.

The pseudo-labeled speech is then combined with labeled data to continue model training.

| Model | AIShell-2018 CER | Common Voice zh CER |
| --- | ---: | ---: |
| Supervised WeNet | 12.67% | 35.21% |
| Self-trained WeNet | 12.79% | **33.14%** |

The self-trained system improves performance on Common Voice zh, suggesting improved cross-domain robustness.

The WeNet self-training run was computationally constrained and did not represent a fully converged large-scale training experiment.

## Whisper Self-Training

A parallel self-training workflow is implemented using **Whisper** to investigate whether the same strategy transfers across different ASR architectures.

| Model | AIShell-2018 CER | Common Voice zh CER |
| --- | ---: | ---: |
| Supervised Whisper | 13.00% | 35.87% |
| Self-trained Whisper | **7.11%** | **24.24%** |

Whisper shows a substantial improvement after self-training on both external evaluation sets.

The result demonstrates that pseudo-label-based adaptation can be effective beyond a single ASR framework.

## Evaluation Metric

Recognition performance is measured using **Character Error Rate (CER)**:

```text
CER = (Substitutions + Deletions + Insertions) / Number of Reference Characters
```

Lower CER indicates better recognition performance.

## Environment

Main tools used in the project include:

- Python 3.10
- PyTorch 2.0.1
- WeNet
- Whisper
- SRILM
- OpenFST
- torchaudio
- librosa
- sox
- TensorBoard

Training and decoding experiments were performed using NVIDIA GPU resources.

## Project Structure

```text
chinese-asr-self-training/
├── configs/                 # WeNet and training configurations
├── data/                    # Dataset preparation scripts
├── decoding/                # Decoding and language-model integration
├── pseudo_labels/           # Pseudo-label generation utilities
├── self_training/           # Self-training pipeline
├── whisper/                 # Whisper experiments
├── evaluation/              # CER evaluation scripts
├── scripts/
├── requirements.txt
└── README.md
```

## Key Takeaways

The experiments highlight three main findings:

1. External language models can improve end-to-end ASR decoding, particularly for in-domain speech.
2. Pseudo-labeled speech can improve cross-domain recognition without additional manual annotation.
3. Self-training is applicable across different ASR architectures, although its effectiveness depends strongly on the seed model, pseudo-label quality, and training conditions.

## Project Context

This work was developed as a research-oriented project at the **Budapest University of Technology and Economics (BME)** with a focus on semi-supervised end-to-end speech recognition.
