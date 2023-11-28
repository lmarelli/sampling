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

# Equal counts
In this variation, we sample 30 of each kind.

```
#Equal Counts
beer_sample_EC = beer.groupby('beer_style').sample(n=30)
sample_style_EC= beer_sample['beer_style'].value_counts()
print(sample_style_EC.head())
```

# Weighted
In this variation, we sample 30 of each kind.
