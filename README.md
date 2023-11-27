## Sampling in Python
#Intro
The population is the complete dataset
Typically, don't know what the whole population is so we take a "sample" which is the subset of data you calculate on.


#Simple Random Sampling:
Description: Randomly select a specified number of items from a DataFrame.
Method: Use the sample method with the n parameter.
'import pandas as pd
simple_random_sample = df.sample(n=5)'

#Stratified Sampling:
Description: Randomly sample from each group (stratum) in a DataFrame.
Method: Use the groupby and apply functions.
'import pandas as pd
stratified_sample = df.groupby('stratum_column').apply(lambda x: x.sample(n=1))'
