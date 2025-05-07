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
1. [What is Detoxify? - Understanding Toxicity Detection in Language](#What-is-Detoxify?---Understanding-Toxicity-Detection-in-Language)
	1. [How does it work?](#How-Does-It-Work?)
		1. [What does it detect?](#What-Does-It-Detect?)
		2. [Available models](#Available-Models)
		3. [Input and Output](#Input-and-Output)
	2. [Language Support](#Language-Support)
	3. [Tech Specs](#Technical-Specifications)
2. [Methodology](#Methodology)
	1. [Test Phrases](#Test-Phrases)
		1. [Template Phrases](#Template-Phrases)
		2. [Key Words](#Key-Words)
	2. [Technical Implementation](#Technical-Implementation)

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


| Model Name | Base Transformer | Task & Dataset | Purpose |
| :------ |:--- | :--- | :--- |
| original | bert-base-uncased | Jigsaw 2018 Toxic Comment Classification<br><br>(Wikipedia Comments) | Detect multiple forms of toxicity like insults, identity hate, and threats in English |
| unbiased | roberta-base | Jigsaw 2019 Unintended Bias in Toxicity (Civil Comments) | Trained to reduce bias across identity groups while detecting toxicity |
| multilingual | xlm-roberta-base | Jigsaw 2020 Multilingual Toxic Comments (Wikipedia + Civil Comments) | Designed to detect toxicity in seven languages, including French and Russian |

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

## Technical Specifications
- Built using **PyTorch** and optionally **PyTorch Lightning**  
- Integrated with **Hugging Face Transformers**  
- Can be installed via pip (pip install detoxify)  
- GitHub: [Detoxify on GitHub](https://github.com/unitaryai/detoxify)

# Literature Review

|Literature Review|   |   |   |   |   |
|:-----------|:---|:-----------|:-----------|:-----------|:-----------|
|Paper|DOI|Key Themes|Project Relevance|Debates|Gaps in Knowledge|
|“The Risk of Racial Bias in Hate Speech Detection” Sep et al. (2019)|[10.18653/v1/P19-1163](https://doi.org/10.18653/v1/P19-1163)|1. The impact of annotator bias, particularly racial bias in labeling <br>    <br>2. The disproportionate flagging of African American English (AAE) as “offensive” language <br>    <br>3. Key differences in flagging of “offensive” language across dialects, particularly racial|This project builds on the unintended bias that exists in hate speech classification as identified in this paper|Q. Should hate speech models be aware of different dialects; and to what extent can the diversity in annotators offset bias?|1. Still a scarcity of AAE aware Natural Language Processing (NLP) structures <br>    <br>2. Lack of deployment of fairness assessment tools on platforms|
|“Challenges and Frontiers in Abusive Content Detection” Vidgen et al. (2019)|[10.18653/v1/W19-3509](https://doi.org/10.18653/v1/W19-3509)|1. The challenges surrounding taxonomy of annotation (subjectivity, contextuality)<br>    <br>2. Inconsistencies in datasets and labeling<br>    <br>3. The risk of models reinforcing biases through biased data and datasets|Offers a conceptual map for the project with regard to why nuanced target categories (ironic or reclaimed, for instance) are hard to annotate fairly|Q. Are expert annotations or user input more effective in recognising hate speech? How possible is it to scale context based labelling?|1. No clear guidelines for annotating or recognising implicit hate or intersectional slurs|
|“A Survey on Automatic Detection of Hate Speech in Text” Fortuna and Nunes (2018)|[https://doi.org/10.1145/3232676](https://doi.org/10.1145/3232676)|1. Overview of varying methodologies and their limitations with regard to fairness<br>    <br>2. Emphasises the ethical dimension of hate speech detection<br>    <br>3. Provides a discussion on annotation practices and biases|Helps engage with a key question of the project - who defines hate and whose speech gets moderated|Q. Is it possible to standardise “ethical” NLP design principles; and what makes it “ethical”?|1. Lacks empirical studies that provide context on how end users or targets of hate speech perceive fairness in moderation systems|
|“Toxigen: A Large-Scale Machine-Generated Dataset for Adversarial and Implicit Hate Speech Detection” Hartvigsen et al. (2022)|[10.18653/v1/2022.acl-long.234](https://doi.org/10.18653/v1/2022.acl-long.234)|1. The evaluation of different models using adversarially generated implicit hate<br>    <br>2. Studies the human vs. model disagreement on what counts as “toxic”<br>    <br>3. The Toxigen dataset acts as a benchmark for implied/implicit hate|The paper provides a basis to assess whether models like Detoxify are capable of detecting nuanced forms of hate speech or coded language|Q. How productive is it for benchmarks to focus on more implicit cases of hate speech; and are human raters equipped to catch implicit hate?|1. Does not provide a universal evaluation benchmark for implicit hate|
|“Large Language Models and Toxicity: A Survey” Minaee et al. (2024)|[https://doi.org/10.48550/arXiv.2402.06196](https://doi.org/10.48550/arXiv.2402.06196)|1. Provides an overview of toxicity benchmarks (e.g., Jigsaw, ToxiGen, RealToxicityPrompt<br>    <br>2. Undergoes a prompt-based evaluation of Large Language Models (LLMs) including the three popular LLM families, GPT, LLaMA, PaLM<br>    <br>3. Highlights the limitations of existing toxicity metrics (e.g., false positives in reclaimed speech)|The survey is capable of highlighting how Detoxify type classifiers and their benchmarks are limited; and provides a comparison of their limitations in comparison with newer LLMs|Q. To what extent do models like Detoxify rely on surface level keywords? Does the reliance on keywords make them more or less sensitive?|1. Lacks a sufficient evaluation of nuanced hate speech, particularly specific to cultural contexts or other languages|
|"Detecting Weak and Strong Islamophobic Hate Speech on Social Media" Vidgen & Yasseri (2020)|[https://doi.org/10.1080/19331681.2019.1702607](https://doi.org/10.1080/19331681.2019.1702607)|1. The study introduces granular classification system ranging from non Islamophobic, weak Islamophobic and strong Islamophobic content - beyond binary classifications <br>    <br>2. Utilises GloVe word embeddings that trained on a 140 million tweets<br>    <br>3. Places emphasis on the contextual nuance of hate speech|The study effectively showcases the benefits of granular labeling for models recognising hate speech through effective domain specific data|Q. To what extent are binary classifications adequate in identifying hate speech?|1. The dataset is limited only to Twitter, and might be outdated since it’s from 2017 <br>    <br>2. Fails to explore integration with existing tools like Detoxify|
|“A Survey on Hate Speech Detection using Natural Language Processing” Schmidt et al. (2017)|10.18653/v1/W17-1101|1. The paper provides a comprehensive overview of NLP approaches to hate speech detection<br>    <br>2. Gives insight into different categorization of detection methods: keyword-based, machine learning, and hybrid.<br>    <br>3. Provides discussion on challenges like data annotation and context dependence|The study provides a foundation for the different methods of hate speech detection methods - highlighting the limitations and critiques of existing approaches|Q. What is the definition of hate speech? <br><br>Q. What should be prioritised in hate speech detection - precision or recall in detection systems?|1. The need for more nuanced datasets that provide contextual knowledge|
|“Detecting Offensive Language in Tweets Using Deep Learning” Pitsillis et al. (2018)|10.48550/arXiv.1801.04433|1. Introduces an ensemble of Recurrent Neural Network (RNN) classifiers for language detection <br>    <br>2. Incorporates user-related biases, like racism or sexism, into the model|The study showcases how effective it is to combine the textual content with user behaviour to enable better speech detection|Q. What are the ethical implications of using user behaviour for content moderation?|1. Suggests the need for research into the generalisability of a language detection model across different platforms - assuming that different platforms witness variation in user behaviour|
|“VADER: A Parsimonious Rule-Based Model for Sentiment Analysis of Social Media Text” Hutto et al. (2014)|[https://doi.org/10.1609/icwsm.v8i1.14550](https://doi.org/10.1609/icwsm.v8i1.14550)|1. The development of VADER - a rule based sentiment analysis tool tailored for social media text <br>    <br>2. Focus on sentimental expressions like slang and emojis|The paper provides a tool that can be used as a baseline tool in hate speech detection for systems like Detoxify; and offers an alternative to machine learning models for sentiment analysis|Q. Are there trade offs between rules based and machine learning approaches to accuracy and adaptability?|1. There are limitations of rule based systems’ ability to capture nuance in the human language <br>    <br>2. Highlights the need to frequent updates to lexicon to keep pace with “social media language”|
|“SemEval-2019 Task 5: Multilingual Detection of Hate Speech Against Immigrants and Women in Twitter” Basile et al. (2019)|10.18653/v1/S19-2007|1. Provides a focus on hate speech detection against immigrants and women in Spanish and English<br>    <br>2. Introduces a new axis of analysis - alongside the binary classification for hate speech detection it also had a fine grained classification for identifying the target and aggressiveness <br>    <br>3. Provision of a multilingual dataset|Provides another benchmark dataset and evaluation metric for the project and highlights the challenge of multilingual and multi target hate speech detection that the project is undertaking|Q. Can a model trained on a specific dataset be used in a generalised fashion?|1. Highlights the need for diverse datasets <br>    <br>2. Highlights the need for further research into the universality of application of these models across different cultural and linguistic settings|
|“Automated Hate Speech Detection and the Problem of Offensive Language” Davidson et al. (2017)|10.48550/arXiv.1703.04009|1. The paper creates a differentiation between “hate speech” and “offensive language” through a crowd sourced lexicon and annotated Twitter dataset <br>    <br>2. Provides an analysis of classifier performance and error patterns|Highlights the limitations of keyword based detection methods and highlights the importance of nuanced classification in hate speech detection|Q. What is the difference between hate speech and offensive language; and which should be moderated?|1. The paper calls for further studies into the impact of annotation guidelines and definitions on model performance|


# Methodology
We sought to create a methodology which would allow us to test, across the three available languages – English, French, and Russian, the toxicity confidence scores produced by Detoxify from a variety of test input phrases. This involved three key steps: the creation of template phrases, the creation of key words, and the technical implementation of these with the Detoxify API.

## Test Phrases
Each test phrase was constructed from a template phrase, and a key word.

### Template Phrases
Whilst it was tempting to scrape real existing comments from websites, such comments would not have allowed a meaningful comparison between categories of hate, as analysis would necessarily entail attempting to compare very different phrases and to provide a normative judgement of comparative hatefulness across hate categories. Instead, in order to compare the scores created by Detoxify across categories of hate (ie between hate based on, for instance, an individual’s gender versus their race), we created set template phrases which could be utilised for a wider number of categories of hate. Each phrase contained a space for a varying key word (representing a different category of hate) to be inserted into. For instance, the phrase ‘I hate all [X].’ allows for [X] to be replaced by a variety of key words representing hate categories. These phrases were kept short and inspired from online perusal of hate comments in order to retain as much authenticity as possible, at the same time as allowing comparison between categories.

Using short and standard template phrases had the further advantage of allowing simpler translation across the three languages of our experiment – English, French, and Russian. We took advantage of the linguistic abilities of our team, in addition to valuable input from colleagues, to produce authentic, rather than word-for-word translations of such phrases, which were then held constant for tests across different categories of hate in that language.

In order to compare the performance of Detoxify across different levels of hate, we categorised template phrases by the level of hate expressed. **Highly toxic** comments were examined first, expressing clear and direct hate. **Implied hate** offered a more opaque, although still hateful, sentiment. **Stereotypes** and **irony**, not necessarily in themselves hateful, but very context-dependent, were also analysed. We further tested **neutral** phrases, expressing a non-contentious statement which was not hateful in tone to test whether simply using the key word could be flagged as hateful. Finally, we examined so-called **reclaimed** phrases, where members of a community consciously take previously hateful content, and seek to use it for positive, rather than negative, means.

Altogether, we created and tested 62 template phrases, across all six categories. These, in all three languages, are recorded in our data, also provided.

### Key Words
Detoxify attempts to detect hate across five categories of hate – gender identity, ethnicity, sexual orientation, mental illness, and religion. In order to test our template phrases across each of these, we used key words which were inserted into template phrases as above. Such key words were principally intended to be neutral, theoretically not affecting the toxicity score for neutral phrases, and not adding or taking away from the scores in other categories. As such, we started with factual and descriptive phrases such as ‘men’, and ‘women’.

However, it very quickly became obvious that limiting the experiment to solely the most neutral key words risked not fully assessing the performance of Detoxify with real-life comments, where particularly the most hateful are unlikely to use neutral and non-hateful terms. Furthermore, identifying neutral language became significantly harder particularly with ethnicity, sexual orientation, and religion, where subtle differences such as the use of [adjective + “people”] versus solely [noun] (i.e. ‘white people’ and ‘whites’) can affect the hatefulness perceived from a comment due to complex societal and historical reasons. As such, we have also endeavoured to include key words which are less neutral for comparative reasons.

Translation also posed a difficulty in this respect. Sometimes, particularly for Russian, it was not possible to provide a direct translation – for instance with the lack of a distinction between using [adjective + “people”] versus [noun] as above. We were keen to retain this difference in English and, where applicable, French, given the different patterns of use in hateful comments – that is to say that hateful comments do not always use the nicest language to describe their chosen target! However, where a translation was not possible, this was left blank.

Altogether, we tested 53 key words in English, 45 in French, and 48 in Russian, the differences being due to the non-translatability of certain key words. These, in all three languages, are recorded in our data, also provided.

## Technical Implementation
The technical implementation of our experiment was largely carried out using the Python interface of Detoxify, run through a Google Colab workbook with a T4 (Graphical Processing Unit) runtime.
Detoxify allows access to its API through Python. Data, in the form of a matrix of template phrases and key words, was first imported into Python from CSV format using Pandas, and UTF-8 encoding to allow the French accents and Russian alphabet to be properly read.

Python was instructed to iterate through each prompt, constructed individually of every template phrase plus every key word for each category of hate, in one language at a time. This was completed separately for each of the three languages to avoid cross-contamination.

The iterated constructed phrases were then sent to Detoxify’s API, using the ‘original’ model for English prompts, and the ‘multilingual’ model for French and Russian, as per the instructions provided by Detoxify.
The toxicity score from Detoxify was received, and separated from the additional information on confidence for each category of hate, which was not utilised for our experiment. Each phrase was then assigned a score in a Pandas dataframe, and in turn exported to a matrix in CSV format.

Finally, Google Sheets was used to read the CSV file and to provide first-level analysis, namely a gradient colour mapping of toxicity scores allowing easy-to-read and simple analyses to be made before more in-depth charts were produced.

## Comparison 
![PHOTO-2025-05-02-01-20-38](https://github.com/user-attachments/assets/1bd0399f-8a1c-4abd-b7a2-27d97710097e)

![PHOTO-2025-05-02-01-40-33](https://github.com/user-attachments/assets/142ef0fd-70b0-426b-b993-fb22229d0cdc)

![PHOTO-2025-05-02-01-39-57](https://github.com/user-attachments/assets/082e281b-9c92-4dd7-a12e-bcbe83fb2c84)


## Findings English: 


## Findings French: 
![PHOTO-2025-05-02-00-54-01](https://github.com/user-attachments/assets/46618730-d0a2-4daa-a3d8-a5ffc776c4c4) 
Primarily, sexual identity content is marked as toxic far more than content addressing race, gender identity, or religion. Specifically, sexual identity averages a toxicity score of 0.454 — the highest across all categories and languages analyzed. This suggests that the model is most attuned to LGBTQ+-related language, likely reflecting both the prevalence of queerphobic language in training data and the broader social emphasis on identifying and mitigating this form of discrimination. French might provide linguistic features where it is easier to read queerphobic hate speech, which is often more direct- This indicates that the model is sensitive to the social harms associated with sexual identity, though can also indicate a tendency to over-flag certain content when it lacks respectful context. This leads to a broader discussion to what degree content moderation can limit reclaimed hate speech, and to what degree it interprets neutral terms like ‘gay’ more often as an inherently prerogative term. This would imply that Detoxify might be imbued with a predetermined association between, especially queer terms, and hate. This could limit open and free discouse on queer and LGBTQIA+ issues.

One especially interesting French-specific finding is the over-flagging of the term "homme" (man). This is due to the fact that the word has a dual meaning in French; un homme et l’Homme. While it neutrally designates a male person, its capitaöization can imply humankind (a feature English also possess but uses less often “Mankind”, often favoring the word human). For a pattern-recognition and statistically driven AI system such as Detoxify, this type of semantic ambiguity is challenging. Stripped of full contextual nuance, the system will misflag neutral or even positive uses of the word "homme" as toxic or it might not, being unable to differentiate between human, a more toxic attack, and man, which can be reclaimed speech. This highlights a broad Achilles' heel of AI models working across languages with rich idiomatic and cultural densities.

For comparison, religious content receives a very low average toxicity score of 0.232 - the lowest across all languages and categories analyzed. This result likely mirrors some aspects of French culture. France also has a strong tradition of secularism (laïcité) and is fiercely exceptionable to freedom of speech, including freedom to criticize explicitly religious beliefs and institutions. Where such criticism would be offensive or hateful in some other cultural contexts, in France it would be merely part of acceptable public discussion. This suggests that the model, by design or due to patterns in the data on which it was trained, reflects some of these social norms, underestimating the toxicity of religious commentry compared to other sensitive topics.

Despite the system being so sensitive to sexual identity matters, it is less sensitive to cultural nuances concerning religion or to specific linguistic traps (like double meanings) in French. This underscores the need for careful calibration and culturally sensitive training in the deployment of automated moderation tools so that they heed not only universal trends of hate speech but also local linguistic and cultural specificities.

## Findings Russian: 


## Findings Example: 

![PHOTO-2025-05-02-01-41-24](https://github.com/user-attachments/assets/7c54001e-df4c-46d1-b0e2-d44c04bf02ee)

![PHOTO-2025-05-02-01-41-25](https://github.com/user-attachments/assets/ab10dc78-6f71-4be8-9ef1-88a79ca3906f)

![PHOTO-2025-05-02-01-41-25](https://github.com/user-attachments/assets/feed12cc-13f1-4a70-842f-94dd0eaf030c)

## Takeaways: Decoding Biases in Content Moderation 
1) *Toxicity is nuanced and language-specific*:
Toxicity is not universal; it is conditioned on the manner in which each language encodes meaning and the manner in which cultural norms shape what is deemed to be hateful or offensive. Linguistic features such as double meanings (e.g., "homme" having both man and human meanings) and local conventions for politeness and criticism make the same terms or phrases have very different toxic loads across languages.

2) *Cultural norms and freedom of speech matter*:
French, for example, has rich histories of laïcité (secularism) and religious critique freedom, which presumably operates to get religion speech flagged as less toxic. Conversely, the societal consensus that heavily condones queerphobia can potentially explain LGBTQ+ - oriented insult being flagged more. Offending speech and insults are thus contextually interpreted. What counts as "hate speech" in one society might count as biting debate or fair critique in another.

3) *Concerns for AI and language technologies*:
This raises significant concerns for AI instruments aimed at automating moderation, translation, or toxicity detection. Machines lack the human sensitivity to navigate subtle, context-dependent meaning and are completely dependent on the data and assumptions they are trained with. Automated processes like translation or content moderation risk flattening these nuances and applying one-size-fits-all standards that don't respect local meanings.

4) *Discriminatory impact on vulnerable groups*:
Automated content moderation disproportionately censors marginalized communities, particularly when they reclaim their own language or speak about their own identity and freedom struggles. Uplifting words or community language may be wrongly categorized as toxic since the algorithm does not comprehend context, irony, or reappropriation. This silencing ability limits marginalized groups' ability to speak on their own and inhibits their linguistic and political agency.

5) *Who are the moderators?*:
Behind all AI systems is the question: who writes the rules? Content moderation is not neutral; AI systems are "regimes of discourse" that decide what kinds of language, knowledge, and expressions are valid or acceptable. This decides not just what is acceptable online but also what kind of human expression is heard or silenced, quietly controlling linguistic possibility and curtailing the range of human sensibilities and epistemologies.

6) *Linked to broader political debates*:
These issues naturally extend into broader political debates over censorship, insult, freedom of speech, and the boundaries of public discourse. As societies grapple with what may be permitted or banned on the internet, AI increasingly occupies a central  position in defining the terms of that discourse.

