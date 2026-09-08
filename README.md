
---

# `chinese-asr-self-training`

```markdown
# Chinese ASR Self-Training with WeNet & Whisper

Mandarin automatic speech recognition using **WeNet** and **Whisper**, with external language-model decoding and pseudo-label-based self-training.

## Overview

This project explores semi-supervised ASR using unlabeled speech data.

The workflow combines:

- Supervised WeNet training
- n-gram language-model decoding
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
