![Beer Banner](/assets/images/beer_banner.jpg)

# Beer Sampling
### Data
The data set is: beer_reviews.csv\
To proper run the code, you may download it at:\
https://www.kaggle.com/datasets/rdoume/beerreviews/data

## Intro 

In this example, we will compare several stratified sampling methods. For the second part, we will demonstrate sampling distributions.

## Proportional

In this example we grab a sample of 30% of each subgroup, notice the `frac = .3` argument on sample.

```
import pandas as pd

# 1st Step: Load the CSV and take a peek
beer = pd.read_csv("beer_reviews.csv", dtype={"beer_style": 'category'})
style_count = beer['beer_style'].value_counts(normalize = True)
print(style_count)

# Sample the 30% of each subgroup
beer_sample = beer.groupby('beer_style').sample(frac=.30)
sample_style_count = beer_sample['beer_style'].value_counts(normalize = True)
```
If we compare the proportions of the population and the sample, they will be very similar.

```
# Compare proportions
print(style_count.head())
print(sample_style_count.head())
```

## Analize population

Let's use the entire population data as a gold standard, since it may not happen in the real world.\
We can make a histogram of the Beer's ABV.\
ABV, or alcohol by volume, is the standard measurement, used worldwide, to assess the strength of a particular beer.\
The lowest ABV in this dataset is 0.01 and the highest is 57.7. Most beer styles are clustered in between 2 and 7.

```
# Population
beer['beer_abv'].hist(bins=np.arange(0, 59, .5))
beer_population_mean = np.mean(beer['beer_abv'])
plt.axvline(beer_population_mean, color='red', linestyle='dashed', linewidth=2, label=f'Mean = {beer_population_mean:.2f}')
plt.xlabel('Beer AVB')
plt.ylabel('Frequency')
plt.title('Population AVB Histogram')
plt.legend()
plt.show()
print(beer_population_mean)
```

## Weighted
We repeat the same procedure and create a weighted sample this time.\
In this variation, we sample **5** of each kind, but we assign a weight by its AVB.\
If it has a higher ABV, it is more likely to get sampled.\
Finally, we plot the results.

```
# Sample 50 beers weighted by ABV
beer_sample_w = beer.sample(n=5, weights="beer_abv")
beer_sample_w_mean = np.mean(beer_sample_w['beer_abv'])
    #sample_style_w = beer_sample['beer_style'].value_counts()
    #print(sample_style_w.head())

# Plot Beer AVB from beer_sample_w as a histogram
beer_sample_w['beer_abv'].hist(bins=np.arange(0, 21, .25))
plt.axvline(beer_sample_w_mean, color='red', linestyle='dashed', linewidth=2, label=f'Mean = {beer_sample_w_mean:.2f}')
plt.xlabel('Beer AVB')
plt.ylabel('Frequency')
plt.title('Weighted Sample Histogram')
plt.legend()
plt.show()
print(beer_sample_w_mean)
```

## Equal counts
Same thing but with an equal count variation, let's sample 50 of each and plot it.

```
# Sample 50 beers EqualCounts by ABV
beer_sample_EC = beer.groupby('beer_style').sample(n=50)
beer_sample_EC_mean = np.mean(beer_sample_EC['beer_abv'])
    #sample_style_EC= beer_sample['beer_style'].value_counts()
    #print(sample_style_EC.head())

# Plot Beer AVB from beer_sample_w as a histogram
beer_sample_EC['beer_abv'].hist(bins=np.arange(0, 21, .25))
plt.axvline(beer_sample_w_mean, color='red', linestyle='dashed', linewidth=2, label=f'Mean = {beer_sample_EC_mean:.2f}')
plt.xlabel('Beer AVB')
plt.ylabel('Frequency')
plt.title('Equal Count Histogram')
plt.legend()
plt.show()
print(beer_sample_EC_mean)
```

## Sampling distribution
So we now have 2 different samples one with 5 samples and the other has 50.\
A consequence of the **Central Limit Theorem** is that the means of independent samples will have distributions that approximate a normal distribution.\
As the sample size increases, the distribution becomes closer to a normal distribution.\

We can run a simple for loop on the weighted sample.\ 
* Take a sample of 5, calculate the mean, and append it to an empty list.
* Repeat the process 500 times
* The result of this procedure is a `list` of means that we will later use on other calculations.
* Plot the results

```
# Sampling distribution
mean_beers_w = []
# Loop 500 times to create 500 sample means
for i in range(500):
	mean_beers_w.append(
    	beer.sample(n=5, weights="beer_abv")['beer_abv'].mean()
	)

# Create a histogram of the 500 sample means
plt.hist(mean_beers_w)
plt.xlabel('Beer AVB')
plt.ylabel('Frequency')
plt.title('500 means a sample of 5')
plt.legend()
plt.show()
plt.show()
```

* Repeat the process but with a different method and a larger (50) sample.

```
mean_beers_EC = []
# Loop 500 times to create 500 sample means
for i in range(500):
	mean_beers_EC.append(
    	beer.sample(n=50)['beer_abv'].mean()
	)

# Create a histogram of the 500 sample means
plt.hist(mean_beers_EC)
plt.xlabel('Beer AVB')
plt.ylabel('Frequency')
plt.title('500 means a sample of 50')
plt.legend()
plt.show()

print("Beer population mean: " + str(beer_population_mean))
print("Weighted Sample mean: " + str(np.mean(mean_beers_w)))
print("Equal Count Sample mean: " + str(np.mean(mean_beers_EC)))
```

## Standard Deviation
[Standard Deviation](https://en.wikipedia.org/wiki/Standard_deviation)
"In statistics, the standard deviation is a measure of the amount of variation or dispersion of a set of values.\
A low standard deviation indicates that the values tend to be close to the mean (also called the expected value) of the set, while a high standard deviation indicates that the values are spread out over a wider range."

To get a quick estimate of the standard deviation of a sample we can calculate the standard deviation of the population and divide that by the square root of the **sample size**
Since we have the mean of different samples we can run the `std` on those as well and then compare to the estimate.

```
# Population Distribution
sd_of_beer_ABV = beer['beer_abv'].std(ddof=0)
sd_of_w_means = np.std(mean_beers_w, ddof=1)
sd_of_EC_means = np.std(mean_beers_EC, ddof=1)

print("Beer population STD: " + str(sd_of_beer_ABV))
 
print("Weighted STD: " + str(sd_of_w_means))
print("Weighted estimate STD: " + str(sd_of_beer_ABV / np.sqrt(5)))

print("Equal Count StdDev: " + str(sd_of_EC_means))
print("Equal Count estimate STD: " + str(sd_of_beer_ABV / np.sqrt(50)))
```


