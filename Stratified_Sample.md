![Beer Banner](/assets/images/beer_banner.jpg)

# Beer Sampling
### Data
beer_reviews.csv\
Downloadable:\
https://www.kaggle.com/datasets/rdoume/beerreviews/data

## Proportional

```
import pandas as pd

#1st Step: Load the CSV and take a peek
beer = pd.read_csv("C:/Users/Luciano/Documents/Python Scripts/Sampling/beer_reviews.csv", dtype={"beer_style": 'category'})
style_count = beer['beer_style'].value_counts(normalize = True)
print(style_count)

#Sample the 30% of each subgroup
beer_sample = beer.groupby('beer_style').sample(frac=.30)
sample_style_count = beer_sample['beer_style'].value_counts(normalize = True)

#Compare proportions
print(style_count.head())
print(sample_style_count.head())
```

## Equal counts
In this variation, we sample 30 of each kind.

```
#Equal Counts
beer_sample_EC = beer.groupby('beer_style').sample(n=30)
sample_style_EC= beer_sample['beer_style'].value_counts()
print(sample_style_EC.head())
```

## Weighted
In this variation, we sample 30 of each kind, but we assign a weight by its AVB
if it has a higher ABV, it is more likely to get sampled.

```
import matplotlib.pyplot as plt 
import numpy as np

beer['beer_abv'].hist(bins=np.arange(0, 21, .5))
plt.show()

print(beer['beer_abv'].mean())

# Sample 30 beers weighted by ABV
beer_sample_w = beer.sample(n=30, weights="beer_abv")
sample_style_w = beer_sample['beer_style'].value_counts()
print(sample_style_w.head())

# Plot YearsAtCompany from attrition_weight as a histogram
beer_sample_w['beer_abv'].hist(bins=np.arange(0, 21, .5))
plt.show()

print(beer_sample_w['beer_abv'].mean())
```
