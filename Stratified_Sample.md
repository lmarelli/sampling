

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
