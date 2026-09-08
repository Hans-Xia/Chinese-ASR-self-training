# Chinese ASR Self-Training with WeNet & Whisper

Mandarin automatic speech recognition using **WeNet** and **Whisper**, with external language-model decoding and pseudo-label-based self-training.

## Overview

This project explores semi-supervised speech recognition using additional pseudo-labeled speech data.

The workflow combines:

- Supervised WeNet training
- N-gram language-model-assisted decoding
- AIShell-2 pseudo-label generation
- WeNet self-training
- Whisper self-training
- Cross-domain evaluation on AIShell and Common Voice zh

## Pipeline

```text
Labeled Speech
      │
      ▼
Supervised ASR
      │
      ├── Language Model Decoding
      │
      └── Pseudo-Label Generation
                  │
                  ▼
              AIShell-2
                  │
                  ▼
             Self-Training
                  │
          ┌───────┴───────┐
          ▼               ▼
      WeNet Model     Whisper Model
```

## Datasets

- **AIShell-1** — supervised training
- **AIShell-2** — pseudo-labeled speech for self-training
- **AIShell-2018** — external evaluation
- **Common Voice zh** — cross-domain evaluation

## Results

### WeNet and Language Model Decoding

| Model | AIShell-1 CER | AIShell-2018 CER | Common Voice zh CER |
| --- | ---: | ---: | ---: |
| Supervised WeNet | 5.10% | 12.67% | 35.21% |
| WeNet + LM | **4.62%** | **12.35%** | **34.29%** |

### Self-Training

| Model | AIShell-2018 CER | Common Voice zh CER |
| --- | ---: | ---: |
| WeNet baseline | 12.67% | 35.21% |
| Self-trained WeNet | 12.79% | **33.14%** |
| Whisper baseline | 13.00% | 35.87% |
| Self-trained Whisper | **7.11%** | **24.24%** |

Whisper showed the largest improvement after pseudo-label-based self-training, particularly on cross-domain speech.

## Tech Stack

- WeNet
- Whisper
- PyTorch
- Conformer
- SRILM
- OpenFST
- torchaudio


## Project Context

Research-oriented ASR project developed at **Budapest University of Technology and Economics**.
