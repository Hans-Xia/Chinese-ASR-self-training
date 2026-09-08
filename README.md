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

# 基于 WeNet 与 Whisper 的中文语音识别自训练

基于 **WeNet** 与 **Whisper** 的中文自动语音识别项目，结合外部语言模型解码与伪标签自训练方法。

## 项目概述

本项目主要研究如何利用额外的伪标注语音数据提升半监督语音识别性能。

整体流程包括：

- WeNet 有监督训练
- n-gram 语言模型辅助解码
- AIShell-2 伪标签生成
- WeNet 自训练
- Whisper 自训练
- 在 AIShell 与 Common Voice zh 上进行跨域评估

## 系统流程

```text
有标签语音
    │
    ▼
有监督 ASR
    │
    ├── 语言模型辅助解码
    │
    └── 伪标签生成
             │
             ▼
         AIShell-2
             │
             ▼
           自训练
             │
      ┌──────┴──────┐
      ▼             ▼
  WeNet 模型     Whisper 模型
```

## 数据集

- **AIShell-1**：用于有监督训练
- **AIShell-2**：用于生成伪标签并进行自训练
- **AIShell-2018**：用于外部评估
- **Common Voice zh**：用于跨域评估

## 实验结果

### WeNet 与语言模型解码

| 模型 | AIShell-1 CER | AIShell-2018 CER | Common Voice zh CER |
| --- | ---: | ---: | ---: |
| 有监督 WeNet | 5.10% | 12.67% | 35.21% |
| WeNet + LM | **4.62%** | **12.35%** | **34.29%** |

### 自训练结果

| 模型 | AIShell-2018 CER | Common Voice zh CER |
| --- | ---: | ---: |
| WeNet baseline | 12.67% | 35.21% |
| 自训练 WeNet | 12.79% | **33.14%** |
| Whisper baseline | 13.00% | 35.87% |
| 自训练 Whisper | **7.11%** | **24.24%** |

Whisper 在加入基于伪标签的自训练后提升最明显，尤其是在跨域数据上的识别效果有较大改善。

## 技术栈

- WeNet
- Whisper
- PyTorch
- Conformer
- SRILM
- OpenFST
- torchaudio

## 仓库结构

```text
chinese-asr-self-training/
├── configs/
├── data/
├── decoding/
├── pseudo_labels/
├── self_training/
├── whisper/
├── evaluation/
└── README.md
```

## 项目背景

布达佩斯技术与经济大学（BME）开展的研究型语音识别课题项目。
