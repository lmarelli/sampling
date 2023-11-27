# Sampling in Python
## Intro

In statistics, a population refers to the entire group that is the subject of a study, while a sample is a subset of the population.

The goal of statistical analysis is often to make inferences or draw conclusions about a population based on the characteristics observed in a sample.

When to Use Populations and Samples:

### Population:

* Use Case: When you want to analyze an entire group or collect data from every individual in a defined group.
* Example: If you are studying the average income of all households in a city, the population is all households in that city.

### Sample:

* Use Case: When it's impractical or too costly to study the entire population, so you select a representative subset.
* Example: If you want to estimate the average height of students in a university, you might measure the heights of a sample of students.

### Benefits of Using Samples:

1. Cost-Effective: Studying an entire population can be time-consuming and expensive. Sampling allows you to achieve similar results with less effort and cost.
2. Efficiency: Analyzing a sample can be more efficient, especially when the population is large or geographically dispersed.
3. Feasibility: In some cases, it's impossible to study an entire population due to logistical constraints.

## Sampling Techniques

### Simple Random Sampling

Consider a deck of playing cards as your population. If you want to select a few cards randomly, you might shuffle the deck and draw cards without looking. The randomness in the drawing process ensures that each card has an equal chance of being selected.

Simple random sampling is a basic sampling method where every individual or element in the population has an equal chance of being included in the sample. The selection of each unit is independent of the selection of other units, and each possible sample has an equal probability of being chosen.

1. Homogeneous Population:

* Scenario: When the population is relatively homogeneous, meaning that there is not much variation in the characteristics of individuals.
* Rationale: Since every individual has an equal chance of being selected, this method ensures that the sample is representative of the entire population.

2. Unbiased Representation:

* Scenario: When there is a need for an unbiased representation of the population in the sample.
* Rationale: Simple random sampling avoids systematic biases, providing an unbiased way to select a subset of individuals.

3. Practicality:

* Scenario: When the population is not too large and it is feasible to list all individuals.
* Rationale: This method becomes more practical when the population size is manageable, and a complete list of individuals can be easily obtained.

4. Randomization Requirement:

* Scenario: When randomization is a key requirement for the study.
* Rationale: Simple random sampling is the most straightforward method to achieve randomization, ensuring that each individual has an equal chance of being part of the sample.

**OK, So how do we do it in Python?**
Using `pandas` we can just call the `.sample` on a `DataFrame`
```
import pandas as pd
simple_random_sample = df.sample(n=5)
```

### Stratified Sampling:

Suppose you want to study TV preferences. Rather than randomly selecting shows from all genres, you categorize shows into genres (comedy, drama, documentary). You then randomly select shows from each genre to create a sample that reflects the diversity of TV preferences.

Stratified sampling is a statistical sampling technique where the population is divided into subgroups or strata based on certain characteristics, and then samples are randomly selected from each stratum. This method ensures that each subgroup is adequately represented in the final sample.

1. Dividing into Strata: The population is divided into distinct subgroups or strata based on specific attributes or characteristics that are relevant to the study.
2. Random Sampling within Strata: Samples are then randomly selected independently from each stratum. This helps ensure that each subgroup is proportionally represented in the final sample.

When to Use Stratified Sampling:

1. Heterogeneous Population: When the population is diverse and exhibits significant variability, and you want to ensure representation from each subgroup.
2. Reducing Variability: It is particularly useful when there is a significant variability within the strata, and you want to control or reduce the overall variability of the sample.
3. Improved Precision: When you want to increase the precision of your estimates by ensuring that each subgroup is adequately represented.
4. Comparative Studies: In situations where you want to compare specific subgroups, ensuring that each subgroup is included in the sample can provide more meaningful insights.



**OK, So how do we do it in Python?**
We have a few options depending on what is the use case. 
They all start the same way.
1.  `groupby` by strata.
2.  Then sample.
* For equal counts from each subgroup:
  ```
  import pandas as pd
  stratified_sample = df.groupby('stratum_column').sample(n=10)
  ```
* Proportional stratified sampling 40% of each stratum_column group counts from each subgroup:
  ```
  import pandas as pd
  proportional_stratified_sample = df.groupby('stratum_column').sample(frac=.4)
  ```
* weighted stratified sampling. Assign weight to the sampling:
  In this case we take a sample of `n=200 `, we dont group by the stratum_column but we assing it to the `weights` argument from the `.sample` method
  ```
  import pandas as pd
  import numpy as np
  
  weighted_stratified_sample = df.sample(n=200, weights="stratum_column")
  ```

### Systematic Sampling

Description: Every kth item is selected from a list after a random starting point.
Library/Function: Implemented manually.

```
k = 2
start_point = random.randint(0, k - 1)
systematic_sample = [population[i] for i in range(start_point, len(population), k)]
print(systematic_sample)
```



### Cluster Sampling:

* Description: Divide the population into clusters, randomly select some clusters, and then sample all individuals within those clusters.
* Example: If studying a country's population, you might use states as clusters and randomly select a few states to study.
How it Works:

Dividing into Clusters: The population is divided into clusters, often based on geographical or administrative boundaries.
Random Selection of Clusters: A subset of clusters is randomly selected from the entire population.
Inclusion of All Elements: All individuals within the selected clusters are included in the sample.
When to Use Cluster Sampling:

* Geographically Dispersed Population: When the population is spread out over a large geographical area, and it is more practical to sample entire clusters than individual elements.
* Cost-Effective: Cluster sampling can be more cost-effective than other sampling methods, especially when it is expensive or logistically challenging to sample individual elements.
* Natural Groupings: When the population naturally falls into identifiable groups or clusters, and these clusters are a relevant unit of analysis.
* Time and Resource Constraints: In situations where time and resources are limited, and it is more feasible to randomly select and sample entire clusters.

Example Scenario:
Consider a study on public health in different regions of a country. Instead of sampling individuals from each household, you might choose to randomly select specific neighborhoods or towns (clusters) and include all individuals within those clusters in your sample. This approach can simplify data collection and reduce costs.
