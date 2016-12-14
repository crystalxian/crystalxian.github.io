---
layout: post
title:  "hello ASR!"
date:  2016-12-12
categories: ASR
tags: ASR
---

* content
{:toc}

语音是由调节动态系统中相对少的参数来产生的。这意味着隐藏在语音特征下的真正结构的复杂度，比直接描述现有特征的模型要小得多。

## 简介ASR
ASR(Automatic speech recognition)　speech-to-text transcription

### ASR实现难点
* Variability in speech recognition

1. Size Number of word types in vocabulary, perplexity
2. Speaker Tuned for a particular speaker, or speaker-independent? Adaptation to speaker characteristics and accent
3. Acoustic environment Noise, competing speakers, channel conditions (microphone, phone line, room acoustics)
4. Style Continuously spoken or isolated? Planned monologue or spontaneous conversation?

* Data-driven machine learning: Construct simple models of speech which can be learned from large amounts of data
(thousands of hours of speech recordings)
