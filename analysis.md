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

| Literature Review                                                                                                           |                                                                                                                                                                                                                                                                                                                                                             |                                                                                                                                                                                               |                                                                                                                                                      |                                                                                                                                                                                                             |                                                                                |
| :----------- | :----------- | :----------- | :----------- | :----------- | :----------- |
| Paper                                                                                                                          | Key Themes                                                                                                                                                                                                                                                                                                                                                  | Project Relevance                                                                                                                                                                             | Debates                                                                                                                                              | Gaps in Knowledge                                                                                                                                                                                           | DOI                                                                            |
| “The Risk of Racial Bias in Hate Speech Detection” Sep et al. (2019)                                                           | 1. The impact of annotator bias, particularly racial bias in labeling <br>    <br>2. The disproportionate flagging of African American English (AAE) as “offensive” language <br>    <br>3. Key differences in flagging of “offensive” language across dialects, particularly racial                                                                        | This project builds on the unintended bias that exists in hate speech classification as identified in this paper                                                                              | Q. Should hate speech models be aware of different dialects; and to what extent can the diversity in annotators offset bias?                         | 1. Still a scarcity of AAE aware Natural Language Processing (NLP) structures <br>    <br>2. Lack of deployment of fairness assessment tools on platforms                                                   | [10.18653/v1/P19-1163](https://doi.org/10.18653/v1/P19-1163)                   |
| “Challenges and Frontiers in Abusive Content Detection” Vidgen et al. (2019)                                                   | 1. The challenges surrounding taxonomy of annotation (subjectivity, contextuality)<br>    <br>2. Inconsistencies in datasets and labeling<br>    <br>3. The risk of models reinforcing biases through biased data and datasets                                                                                                                              | Offers a conceptual map for the project with regard to why nuanced target categories (ironic or reclaimed, for instance) are hard to annotate fairly                                          | Q. Are expert annotations or user input more effective in recognising hate speech? How possible is it to scale context based labelling?              | 1. No clear guidelines for annotating or recognising implicit hate or intersectional slurs                                                                                                                  | [10.18653/v1/W19-3509](https://doi.org/10.18653/v1/W19-3509)                   |
| “A Survey on Automatic Detection of Hate Speech in Text” Fortuna and Nunes (2018)                                              | 1. Overview of varying methodologies and their limitations with regard to fairness<br>    <br>2. Emphasises the ethical dimension of hate speech detection<br>    <br>3. Provides a discussion on annotation practices and biases                                                                                                                           | Helps engage with a key question of the project - who defines hate and whose speech gets moderated                                                                                            | Q. Is it possible to standardise “ethical” NLP design principles; and what makes it “ethical”?                                                       | 1. Lacks empirical studies that provide context on how end users or targets of hate speech perceive fairness in moderation systems                                                                          | [https://doi.org/10.1145/3232676](https://doi.org/10.1145/3232676)             |
| “Toxigen: A Large-Scale Machine-Generated Dataset for Adversarial and Implicit Hate Speech Detection” Hartvigsen et al. (2022) | 1. The evaluation of different models using adversarially generated implicit hate<br>    <br>2. Studies the human vs. model disagreement on what counts as “toxic”<br>    <br>3. The Toxigen dataset acts as a benchmark for implied/implicit hate                                                                                                          | The paper provides a basis to assess whether models like Detoxify are capable of detecting nuanced forms of hate speech or coded language                                                     | Q. How productive is it for benchmarks to focus on more implicit cases of hate speech; and are human raters equipped to catch implicit hate?         | 1. Does not provide a universal evaluation benchmark for implicit hate                                                                                                                                      | [10.18653/v1/2022.acl-long.234](https://doi.org/10.18653/v1/2022.acl-long.234) |
| “Large Language Models and Toxicity: A Survey” Minaee et al. (2024)                                                            | 1. Provides an overview of toxicity benchmarks (e.g., Jigsaw, ToxiGen, RealToxicityPrompt<br>    <br>2. Undergoes a prompt-based evaluation of Large Language Models (LLMs) including the three popular LLM families, GPT, LLaMA, PaLM<br>    <br>3. Highlights the limitations of existing toxicity metrics (e.g., false positives in reclaimed speech)    | The survey is capable of highlighting how Detoxify type classifiers and their benchmarks are limited; and provides a comparison of their limitations in comparison with newer LLMs            | Q. To what extent do models like Detoxify rely on surface level keywords? Does the reliance on keywords make them more or less sensitive?            | 1. Lacks a sufficient evaluation of nuanced hate speech, particularly specific to cultural contexts or other languages                                                                                      | [10.48550/arXiv.2402.06196](https://doi.org/10.48550/arXiv.2402.06196)         |
| "Detecting Weak and Strong Islamophobic Hate Speech on Social Media" Vidgen & Yasseri (2020)                                   | 1. The study introduces granular classification system ranging from non Islamophobic, weak Islamophobic and strong Islamophobic content - beyond binary classifications <br>    <br>2. Utilises GloVe word embeddings that trained on a 140 million tweets<br>    <br>3. Places emphasis on the contextual nuance of hate speech                            | The study effectively showcases the benefits of granular labeling for models recognising hate speech through effective domain specific data                                                   | Q. To what extent are binary classifications adequate in identifying hate speech?                                                                    | 1. The dataset is limited only to Twitter, and might be outdated since it’s from 2017 <br>    <br>2. Fails to explore integration with existing tools like Detoxify                                         | [10.1080/19331681.2019.1702607](https://doi.org/10.1080/19331681.2019.1702607) |
| “A Survey on Hate Speech Detection using Natural Language Processing” Schmidt et al. (2017)                                    | 1. The paper provides a comprehensive overview of NLP approaches to hate speech detection<br>    <br>2. Gives insight into different categorization of detection methods: keyword-based, machine learning, and hybrid.<br>    <br>3. Provides discussion on challenges like data annotation and context dependence                                          | The study provides a foundation for the different methods of hate speech detection methods - highlighting the limitations and critiques of existing approaches                                | Q. What is the definition of hate speech? <br><br>Q. What should be prioritised in hate speech detection - precision or recall in detection systems? | 1. The need for more nuanced datasets that provide contextual knowledge                                                                                                                                     | 10.18653/v1/W17-1101                                                           |
| “Detecting Offensive Language in Tweets Using Deep Learning” Pitsillis et al. (2018)                                           | 1. Introduces an ensemble of Recurrent Neural Network (RNN) classifiers for language detection <br>    <br>2. Incorporates user-related biases, like racism or sexism, into the model                                                                                                                                                                       | The study showcases how effective it is to combine the textual content with user behaviour to enable better speech detection                                                                  | Q. What are the ethical implications of using user behaviour for content moderation?                                                                 | 1. Suggests the need for research into the generalisability of a language detection model across different platforms - assuming that different platforms witness variation in user behaviour                | 10.48550/arXiv.1801.04433                                                      |
| “VADER: A Parsimonious Rule-Based Model for Sentiment Analysis of Social Media Text” Hutto et al. (2014)                       | 1. The development of VADER - a rule based sentiment analysis tool tailored for social media text <br>    <br>2. Focus on sentimental expressions like slang and emojis                                                                                                                                                                                     | The paper provides a tool that can be used as a baseline tool in hate speech detection for systems like Detoxify; and offers an alternative to machine learning models for sentiment analysis | Q. Are there trade offs between rules based and machine learning approaches to accuracy and adaptability?                                            | 1. There are limitations of rule based systems’ ability to capture nuance in the human language <br>    <br>2. Highlights the need to frequent updates to lexicon to keep pace with “social media language” | [10.1609/icwsm.v8i1.14550](https://doi.org/10.1609/icwsm.v8i1.14550)           |
| “SemEval-2019 Task 5: Multilingual Detection of Hate Speech Against Immigrants and Women in Twitter” Basile et al. (2019)      | 1. Provides a focus on hate speech detection against immigrants and women in Spanish and English<br>    <br>2. Introduces a new axis of analysis - alongside the binary classification for hate speech detection it also had a fine grained classification for identifying the target and aggressiveness <br>    <br>3. Provision of a multilingual dataset | Provides another benchmark dataset and evaluation metric for the project and highlights the challenge of multilingual and multi target hate speech detection that the project is undertaking  | Q. Can a model trained on a specific dataset be used in a generalised fashion?                                                                       | 1. Highlights the need for diverse datasets <br>    <br>2. Highlights the need for further research into the universality of application of these models across different cultural and linguistic settings  | 10.18653/v1/S19-2007                                                           |
| “Automated Hate Speech Detection and the Problem of Offensive Language” Davidson et al. (2017)                                 | 1. The paper creates a differentiation between “hate speech” and “offensive language” through a crowd sourced lexicon and annotated Twitter dataset <br>    <br>2. Provides an analysis of classifier performance and error patterns                                                                                                                        | Highlights the limitations of keyword based detection methods and highlights the importance of nuanced classification in hate speech detection                                                | Q. What is the difference between hate speech and offensive language; and which should be moderated?                                                 | 1. The paper calls for further studies into the impact of annotation guidelines and definitions on model performance                                                                                        | 10.48550/arXiv.1703.04009                                                      |



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


# Findings English: 


# Findings French: 
![PHOTO-2025-05-02-00-54-01](https://github.com/user-attachments/assets/46618730-d0a2-4daa-a3d8-a5ffc776c4c4) 
Primarily, sexual identity content is marked as toxic far more than content addressing race, gender identity, or religion. Specifically, sexual identity averages a toxicity score of 0.454 — the highest across all categories and languages analyzed. This suggests that the model is most attuned to LGBTQ+-related language, likely reflecting both the prevalence of queerphobic language in training data and the broader social emphasis on identifying and mitigating this form of discrimination. French might provide linguistic features where it is easier to read queerphobic hate speech, which is often more direct- This indicates that the model is sensitive to the social harms associated with sexual identity, though can also indicate a tendency to over-flag certain content when it lacks respectful context. This leads to a broader discussion to what degree content moderation can limit reclaimed hate speech, and to what degree it interprets neutral terms like ‘gay’ more often as an inherently prerogative term. This would imply that Detoxify might be imbued with a predetermined association between, especially queer terms, and hate. This could limit open and free discouse on queer and LGBTQIA+ issues.

One especially interesting French-specific finding is the over-flagging of the term "homme" (man). This is due to the fact that the word has a dual meaning in French; un homme et l’Homme. While it neutrally designates a male person, its capitaöization can imply humankind (a feature English also possess but uses less often “Mankind”, often favoring the word human). For a pattern-recognition and statistically driven AI system such as Detoxify, this type of semantic ambiguity is challenging. Stripped of full contextual nuance, the system will misflag neutral or even positive uses of the word "homme" as toxic or it might not, being unable to differentiate between human, a more toxic attack, and man, which can be reclaimed speech. This highlights a broad Achilles' heel of AI models working across languages with rich idiomatic and cultural densities.

For comparison, religious content receives a very low average toxicity score of 0.232 - the lowest across all languages and categories analyzed. This result likely mirrors some aspects of French culture. France also has a strong tradition of secularism (laïcité) and is fiercely exceptionable to freedom of speech, including freedom to criticize explicitly religious beliefs and institutions. Where such criticism would be offensive or hateful in some other cultural contexts, in France it would be merely part of acceptable public discussion. This suggests that the model, by design or due to patterns in the data on which it was trained, reflects some of these social norms, underestimating the toxicity of religious commentry compared to other sensitive topics.

Despite the system being so sensitive to sexual identity matters, it is less sensitive to cultural nuances concerning religion or to specific linguistic traps (like double meanings) in French. This underscores the need for careful calibration and culturally sensitive training in the deployment of automated moderation tools so that they heed not only universal trends of hate speech but also local linguistic and cultural specificities.

# Findings Russian: 
## *Overview:*
This section presents findings from the evaluation of Detoxify's multilingual model on Russian-language content. With a subgroup size of 10,948 and an AUC score of 89.81%, the model shows relatively strong performance in identifying highly toxic speech. The goal of this analysis is twofold: first, to assess how well the model detects various forms of hate; and second, to explore whether toxicity evaluations vary systematically depending on which social groups are mentioned, potentially revealing underlying sensitivities or biases.

## *Phrases Used:*
To ensure transparency and allow replication, a complete set of the 62 phrases used in the Russian-language analysis is included in the data section of this site. Each phrase includes a placeholder (\[\]) for different identity groups and has been translated to match the structure used in English and French versions.
Please note that for the phrase “gas the \[\]”, the Russian version was adapted to “put \[\] into the gas chamber” to better reflect a natural and culturally recognizable expression in Russian. Additionally, the phrase *Очередной кровавый \[\]* is a literal translation of “bloody \[\]” but does not carry the same idiomatic or emotional meaning in Russian. The alternative phrase *Проклятые \[\]* (“damned \[\]”) was thus also included as it conveys a closer semantic and emotional equivalent.

## *Categories Analyzed:*
In evaluating the performance of Detoxify’s multilingual model on Russian-language inputs, this study focused on toxicity classification across five key identity-related categories: **gender identity**, **ethnicity**, **sexual orientation**, **mental health**, and **religion**. Each phrase was constructed using a target group identifier embedded within commonly toxic or implicitly biased Russian-language statements. The aim was to examine not only the model's general ability to detect hate speech, but also its sensitivity to group-specific slurs, stereotypes, or reclaimed identifiers.
To ensure cross-linguistic comparability, the identity categories largely mirror those used in English and French datasets. However, some important language-specific distinctions were accounted for:
* For example, Russian does not use terms equivalent to e.g., both “White people” and “Whites” with the same connotations as in English; only one neutral term (“белых”) was used.  
* Religious terminology was explored in more depth, reflecting the greater role religion plays in Russian sociocultural discourse.  
* Multiple terms for Jewish people and homosexuals were included to assess how the model responds to neutral, formal, reclaimed, and derogatory expressions—particularly relevant given the historical and social sensitivities in Russian contexts.

## *Breakdown of Scores by Group Characteristics* {#breakdown-of-scores-by-group-characteristics}

This graph presents the toxicity scores of selected terms, categorized according to group characteristics such as gender identity, ethnicity, sexual orientation, mental health, and religion.  


## *Heat Map of Results:*  {#heat-map-of-results:}

Visual representation for easy comparison of toxicity across all possible phrases.  


## *Overview of Observed Trends* {#overview-of-observed-trends}

This section summarizes the most salient patterns revealed by the toxicity scores across various identity groups and linguistic constructions, as visualized in the heatmap and mean score tables.

### *General Sensitivity to Explicit vs. Implicit Language* {#general-sensitivity-to-explicit-vs.-implicit-language}

The model’s sensitivity appears to be primarily driven by **linguistic explicitness**, rather than by the specific **identity group** referenced. Phrases containing overt hate speech—particularly those with violent or dehumanizing language—are consistently flagged as highly toxic, regardless of the targeted group. However, once such statements become more subtle or implicit in tone, **toxicity scores drop sharply**, even when the underlying message remains harmful or derogatory. This indicates a limitation in the model's ability to detect **implied or coded hate speech**, especially when overt lexical markers of aggression are absent.

### *Identity-Specific Patterns and Outliers* {#identity-specific-patterns-and-outliers}

While the overall trend shows language-driven toxicity detection, certain identity terms were **systematically scored higher** across contexts, suggesting these may have been disproportionately represented in the model’s training data. For example:

* The terms *“bisexual,”* *“gay”* (both slur and neutral usages), and *“homosexual”* often receive elevated toxicity scores—even in neutral or positive contexts.  
* In contrast, terms such as *“queer”* and *“LGBT”*, which may be more commonly reclaimed or contextually positive in specific communities (e.g., activist or LGBTQ+ discourse), tend to be scored less harshly or sometimes not flagged at all.

This inconsistency suggests the model’s **conceptualization of toxicity may be sensitive to the lexical form of identity terms**, rather than their pragmatic use in context.

### *Mean Toxicity by Category* {#mean-toxicity-by-category}

An aggregate analysis of mean toxicity scores across major identity categories reveals notable differences. Russian-language outputs generally align with the observed average, showing the highest mean toxicity for phrases referencing **sexual identity** and the lowest for those referencing **mental illness**:

* **Sexual identity**: Mean toxicity score \= 0.406  
* **Mental illness**: Mean toxicity score \= 0.236

These results point toward a relative **over-sensitivity** in the domain of sexual orientation and a **comparative under-sensitivity** in discussions of mental health. Whether these patterns stem from cultural-linguistic features of Russian or from biases in the underlying training data remains a point for further exploration.

### *Findings within Categories*  {#findings-within-categories}

#### Gender Identity {#gender-identity}

The category of gender identity revealed a clear pattern of **identity-linked bias** in the model's scoring behavior. When the only change in a phrase was the gender-related identity term, Detoxify assigned consistently **higher toxicity scores** to marginalized identities such as:

* *trans women*  
* *trans men*  
* *transgender people*  
* *non-binary people*  
* *intersex people*

For example, in the phrase:  
 “\[\] не заслуживают прав” ("\[\] don’t deserve rights"):

* “men”: 0.13  
* “trans women”: 0.76  
* “transgender people”: 0.86  
* “intersex people”: 0.51

This demonstrates that the model is **more likely to interpret phrases as toxic** when they involve marginalized gender groups—even though the underlying structure and tone of the statement remain unchanged. 

In some cases, even **neutral or empowering statements** were flagged:
 “Я горжусь тем, что я \[\]” ("I'm proud to be \[\]"):

* “men”: 0.0060  
* “transgender people”: 0.4428  
* “intersex people”: 0.3077

Such results suggest a bias rooted not in the language's toxicity but in the identity term itself—likely a consequence of **over-representation of these groups in toxic contexts** during training, combined with insufficient exposure to **reclaimed or positive uses** of these terms.

#### Ethnic Groups
Findings for ethnic identities revealed nuanced variations in how the model scores toxicity, particularly as the phrasing shifts from explicit to more implicit expressions.  
**Black** and **White** identities both received high toxicity scores in overtly hateful phrases (often exceeding 0.9), indicating that the model reliably flags blatant abuse involving these groups. However, references to Black identities consistently scored higher than those to White identities, even in subtler or more ambiguous phrasing. For example, the average of non-highly toxic phrases for “black people” remained at around 0.37, compared to an average of  around 0.15 for white people. This suggests the model is especially sensitive—or perhaps over-sensitive—to language involving Black identity. White-related phrases, while still scoring relatively high compared to other ethnicities, showed a slightly quicker decline in toxicity scores as the phrasing became less aggressive. This suggests a disparity in the model’s calibration, where Black identity may act as a stronger “trigger” for toxicity detection than White identity. Phrases involving **Asian** identity started with similarly high scores in overt hate but dropped off quickly in subtle or neutral contexts with an average score of about 0.175.  **Latinos** received the lowest toxicity scores across the board. Even strongly worded phrases tended to score lower than comparable statements targeting other ethnicities (e.g., “\[\] are a threat to decent people” for Latinos: 0.1258 vs Black people: 0.8855). In neutral or borderline cases, scores frequently hovered near zero, which may reflect model under-sensitivity.

These discrepancies suggest that the model’s toxicity scoring may reflect underlying biases in the training data, possibly due to the overrepresentation of Black identity in flagged toxic content during training. This can result in higher false positive rates for content referencing Black individuals—even when the phrasing is not inherently harmful. At the same time, references to Latino and Asian identities often receive significantly lower toxicity scores, potentially indicating under-sensitivity or underrepresentation in the training corpus. These disparities point to a broader issue of uneven cultural encoding of harm, shaped by both dataset composition and dominant social narratives in online discourse.

#### Sexual Orientation
Toxicity scores for sexual orientation terms revealed both expected and surprising results:

Slurs for “gay” were consistently flagged as highly toxic. However, neutral or positive uses of “gay” and related terms were also frequently misclassified as toxic.

*Example*:  
 “My friend is gay” often received a disproportionately high toxicity score.

* “gay” (slur): 0.9903  
* “gay” (neutral): 0.8263  
* “homosexual”: 0.8045

This overflagging may partly reflect the data used to train the model. In Russian-speaking contexts, homophobia is widespread, and terms like “gay” are frequently used in derogatory ways.[^1] Compared to Western discourse, this creates a skewed baseline where even neutral or affirming language may appear toxic to the model, likely due to overrepresentation of hate in training data.

Interestingly, more community-specific or reclaimed terms like “queer”—and even formal terms like “LGBT”—were less often flagged. This suggests that the model’s toxicity judgment is sensitive to word choice, rather than semantic intent. Phrases using these terms were sometimes underflagged,

*Example*:  
“Queers don’t deserve rights”: 0.0788

raising concerns about whether hate expressed through “reclaimed” language is being missed.

#### Mental Illness
Overall, phrases involving mental health terms scored lower on average than all the other identity categories, especially in non-explicit contexts, with a mean toxicity score of 0.236. Similar to other categories, highly-toxic hate was usually correctly flagged.  
However, there were notable outliers: Despite the more stigmatizing noun-use of “depressives” (0.1888) , “bipolar people” received a higher average score of 0.3244.  This may indicate that the model has been trained on more abuse linked to specific diagnoses, or that certain terms are more often used pejoratively online. Still, neutral references to mental health were rarely misclassified, suggesting lower risk of false positives compared to other identity domains.

#### Religion {#religion}
Religious identity terms received relatively low toxicity scores overall, especially as language became more indirect. While highly-toxic hate toward religious groups was typically flagged with moderate to high scores, subtle or implied hate often went undetected, suggesting a limited capacity for nuance in the model’s religious bias detection.

Terms for Muslim and Jewish populations showed elevated toxicity scores in “highly toxic” and “implied hate” comments, yet quickly tapered off, with some specific phrases triggering random higher scores. Most strikingly, a common antisemitic slur consistently received lower toxicity scores than neutral references like “Jew”. In several cases, the slur scored less than half the value of the neutral term. This strongly suggests the slur was either absent from the training data or not properly annotated as toxic, resulting in underrecognition of historically harmful language.  
Christian denominations such as Catholic, Orthodox, and Protestant followed a similar pattern: they were primarily flagged as toxic in overt hate speech. Even when paired with derogatory modifiers, their scores remained low in most other contexts. This pattern is particularly noteworthy given the higher level of religiosity in Russia[^2], where even less aggressive negative commentary might be expected to register more strongly. Surprisingly, neutral labels often scored higher than their derogatory counterparts. While “Catholic” scored an average of 0.1939, “Papist” averaged at around 0.1886. Similarly, “Orthodox” had 0.2008 points, whereas the slang term “ROC people” only scored 0.1639.   
Moreover, Atheist-related terms, like “godless” (0.2088) and “atheist” (0.2326), scored higher than many Christian identifiers, indicating the model may be more sensitive to language targeting non-religious individuals.

Overall, religious identity appears less salient as a trigger for toxicity detection, especially outside of explicit hate. The model demonstrates limited sensitivity to indirect or culturally coded religious hate, and inconsistently scores slurs versus neutral terms, likely due to gaps or imbalances in the training data. The unexpected patterns around Christian denominations and antisemitic slurs raise further concerns about whether religious hate—particularly in subtle forms—is being accurately recognized or mitigated by the model.

### Important Results {#important-results}
While many patterns in the model’s behavior aligned with expectations, several outlier findings revealed important gaps in the model's ability to detect nuanced or historically rooted forms of hate speech.

**1\. Under-Detection of Offensive Antisemitic & religious Language**

A particularly concerning anomaly was the under-detection of an antisemitic slur, which has been cited in case law in Russia, resulting in prison sentences and fines for its usage. Despite its highly offensive nature, this term consistently scored lower than neutral terms like “Jew”. This suggests that the model has significant blind spots when it comes to recognizing coded, historical, or context-dependent hate speech, particularly language with legal consequences in certain contexts.  
This issue was similarly observed in derogatory terms for Christian denominations. In these cases, slurs targeting specific Christian groups were also under-flagged, often receiving low toxicity scores compared to other terms. Even in contexts where these terms were used with negative connotations, they did not trigger the expected level of toxicity. This further highlights the model’s limited recognition of subtle or culturally contextualized hate speech, which may be culturally embedded but not overtly aggressive in phrasing.

**2\. Gender and Religion: Women Scored Lower than Atheists**

In terms of gendered language, comments involving “women” were perceived as less toxic than similar phrases targeting other identity groups. In fact, women were on average scored 0.1952, compared to “godless” (0.2088) and “atheist” (0.2326). This suggests that the model may underestimate the toxicity of misogynistic language, particularly when it is less explicit but still harmful. Especially in the context of Russia—where sexual violence and domestic abuse are well-documented issues, and laws in the past decade have been softened to decriminalize certain forms of domestic abuse—this represents an urgent and deeply concerning shortcoming in the AI model.[^3]

**3\. The “Bloody” Modifier: Triggered by Violence Association**

A direct translation of the phrase “bloody \[\]” into Russian, which doesn't naturally make sense in the language, nonetheless received surprisingly high toxicity scores, as if it were an explicit insult. This overflagging may be due to the model’s association of blood with violence, which is a common linguistic pattern across many languages, including Russian. In contexts where "blood" is linked with aggression and hostility, the model appears to have over-responded by attributing a higher toxicity score, despite the term not being naturally offensive in this context. The fact that “bloody \[\]”—a phrase that doesn’t make sense in a direct Russian translation—was still rated similarly to overt insults highlights the model's heightened sensitivity to violent or aggressive language, even when the expression itself doesn't fit culturally or linguistically.

These findings underscore the model’s limited ability to detect culturally nuanced and historically embedded hate speech, particularly when it is indirect or context-dependent. The under-recognition of antisemitic slurs, misogynistic language, and other more derogatory terms highlights biases in the dataset or labeling process, where subtle or coded hate speech remains under-flagged. Conversely, language associated with violence, such as "bloody," demonstrate a possible over-sensitivity, revealing a lack of nuanced understanding in the model’s toxicity detection capabilities. This suggests that the model may not strike an appropriate balance between under-flagging subtle hate speech and over-flagging language that is not inherently offensive.


# Findings Example: 

![PHOTO-2025-05-02-01-41-24](https://github.com/user-attachments/assets/7c54001e-df4c-46d1-b0e2-d44c04bf02ee)

![PHOTO-2025-05-02-01-41-25](https://github.com/user-attachments/assets/ab10dc78-6f71-4be8-9ef1-88a79ca3906f)

![PHOTO-2025-05-02-01-41-25](https://github.com/user-attachments/assets/feed12cc-13f1-4a70-842f-94dd0eaf030c)

# Takeaways: Decoding Biases in Content Moderation 
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

