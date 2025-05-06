---
layout: page
title: Analysis
subtitle: Read our study
gh-repo: daattali/beautiful-jekyll
gh-badge: [star, fork, follow]
tags: [test]
comments: true
mathjax: true
author: Annemarie Sommer, Varushka Bhushan, Richard Rybar, Moritz Schwenger, Aron West
---

# Table of Contents
1. [What is Detoxify? - Understanding Toxicity Detection in Language](#section1)
	1. [How does it work?](#section1)
		1. [What does it detect?](#example2)
		2. [Available models](#example2)
		3. [Input and Output?](#example2)
	2. [Language Support](#third-example)
	3. [Tech Specs](#fourth-examplehttpwwwfourthexamplecom)

# What is Detoxify? - Understanding Toxicity Detection in Language
Detoxify is an open-source AI tool - developed by Laura Hanu at Unitary that detects toxic or harmful language in online comments by using advanced natural language processing (NLP) techniques. Trained on real-world data (downloaded via Kaggle API), it helps researchers and platforms identify hate speech, bias, and offensive language across different languages and contexts. 

It aims ultimately to mitigate online harm through intelligent content moderation

## How Does It Work?
Detoxify is a deep learning-based tool for detecting toxic language in text. It uses pretrained Transformer models fine-tuned on labelled data from various Jigsaw Toxic Comment Classification challenges. Here's how it works in detail:
### What Does It Detect?
Each model outputs a toxicity probability score (0 to 1) for one or more of the following categories:
- toxicity
- severe_toxicity
- obscene
- threat
- insult
- identity_attack
- sexual_explicit
(Some labels vary depending on the model used.)

These scores indicate the **likelihood that a given comment falls into each category**, assisting in the identification and moderation of harmful content.
### Available Models
Detoxify provides three main models, each fine-tuned for a different classification task and use case:

| Model Name   | Base Transformer  | Task & Dataset                                                       | Purpose                                                                               |
| ------------ | ----------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| original     | bert-base-uncased | Jigsaw 2018 Toxic Comment Classification<br><br>(Wikipedia Comments) | Detect multiple forms of toxicity like insults, identity hate, and threats in English |
| unbiased     | roberta-base      | Jigsaw 2019 Unintended Bias in Toxicity (Civil Comments)             | Trained to reduce bias across identity groups while detecting toxicity                |
| multilingual | xlm-roberta-base  | Jigsaw 2020 Multilingual Toxic Comments (Wikipedia + Civil Comments) | Designed to detect toxicity in seven languages, including French and Russian          |
### Input and Output
Detoxify takes in either a single string or a list of strings and returns a dictionary of scores:
```

from detoxify import Detoxify


results = Detoxify('original').predict("I hate all [group]")

  

Output (example)

{

  "toxicity": 0.97,

  "insult": 0.92,

  "identity_attack": 0.88,

  ...

}
```

You can also use pandas for easy result formatting.
## Language Support
The multilingual model supports toxicity detection in 7 languages:
- **English**
- **French** (AUC: 89.61%)
- **Russian** (AUC: 89.81%)
- **Spanish** (AUC: 92.74%)
- **Italian** (AUC: 89.18%)
- **Portuguese** (AUC: 91.00%)
- **Turkish** (AUC: 97.19%)

English remains the most fine-tuned overall. The AUC (Area Under the Curve) score measures how well the model distinguishes between toxic and non-toxic content — a higher AUC means better accuracy. Performance varies due to differences in dataset size, cultural context, and linguistic structure.

## Tech Specs:
- Built using **PyTorch** and optionally **PyTorch Lightning**  
- Integrated with **Hugging Face Transformers**  
- Can be installed via pip (pip install detoxify)  
- GitHub: [Detoxify on GitHub](https://github.com/unitaryai/detoxify)
