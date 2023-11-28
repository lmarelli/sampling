![Beer Banner](/assets/images/beer_banner.jpg)

# Beer Sampling

## Beer Cluster Sampling 
[Beer Sampling Notebook](/assets/docs/beer_sampling_notebook.ipynb)


### Data
beer_reviews.csv\
Downloadable:\
https://www.kaggle.com/datasets/rdoume/beerreviews/data

```
import pandas as pd
import random

#1st Step: Load the CSV and take a peek
beer = pd.read_csv("beer_reviews.csv", dtype={"beer_style": 'category'})
print(beer.head(100))

style_count = beer['beer_style'].value_counts()

unique_beer = list(beer["beer_style"].unique())

#random.seed optional to replicate model
random.seed(222)
beer_sample = random.sample(unique_beer, k=5)

#2nd Step: Condition
beer_condition = beer['beer_style'].isin(beer_sample)
beer_cluster = beer[beer_condition]


beer_cluster['beer_style'] = beer_cluster['beer_style'].cat.remove_unused_categories()

#3rd Step:
cluster_sample = beer_cluster.groupby("beer_style").sample(n=5,random_state=2000) 
print(cluster_sample)
print(cluster_sample['beer_style'].value_counts())
```
