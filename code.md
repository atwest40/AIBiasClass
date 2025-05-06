---
layout: page
title: Code
subtitle: Read our code
gh-repo: daattali/beautiful-jekyll
gh-badge: [star, fork, follow]
tags: [test]
comments: true
mathjax: true
author: Annemarie Sommer, Varushka Bhushan, Richard Rybar, Moritz Schwenger, Aron West
---

# Our code used to test Detoxify - written in Python
## Code was executed using Google Colab with a T4 runtime (GPU needed for processing).

```
#Run me with T4 enabled
#Adapted from Detoxify
#@misc{Detoxify,
#  title={Detoxify},
#  author={Hanu, Laura and {Unitary team}},
#  howpublished={Github. https://github.com/unitaryai/detoxify},
#  year={2020}
#}
from detoxify import Detoxify
results = Detoxify('original').predict("I hate all [group]")

#Installs and preparation
!pip install detoxify
import pandas as pd
from detoxify import Detoxify
import csv

#testing upload
df1 = pd.read_csv('input_table.csv', encoding='utf-8', header=[0,1], index_col=[0, 1])
display(df1)

# upload CSV with title headers and columns
df1 = pd.read_csv('input_table.csv', encoding='utf-8', header=[0,1], index_col=[0, 1])
#display(df1)

# iterate through and create prompts
prompts = df1.values.flatten()
#Filter out NaN values
prompts = [prompt for prompt in prompts if pd.notna(prompt)]
#print(list(prompts))

# For each prompt in df1, replace with score from Detoxify
# Run Detoxify on the prompts
results = Detoxify('multilingual').predict(prompts) #NB 'original' model also exists.

# Create a DataFrame from the results
results_df = pd.DataFrame(results)

# Replace the text with the toxicity values in the original DataFrame
#We need to iterate through the original dataframe and replace values
k = 0
for i in df1.index:
    for j in df1.columns:
        if pd.notna(df1.loc[i,j]):
            df1.loc[i,j] = results_df.iloc[k]['toxicity']
            k += 1
            
#display(df1)

# save detailed results to CSV as vertical list:
#results_df.to_csv('results-detailed.csv')

# save regulat results as CSV:
df1.to_csv('results1.csv')


```
