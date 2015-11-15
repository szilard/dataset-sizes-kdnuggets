
### Size of datasets used for analytics

With so much hype about "big data" and the industry pushing for "big data" tools for everyone,
the question arises who has big data and who really needs these tools (which are more complex and 
often more immature compared to the traditional tools for data analysis).

During the process of data analysis we typically start with some larger raw datasets, 
we transform, clean and prepare them for modeling (typically with SQL-like 
transformations) and then we use these refined and usually smaller datasets for
modeling.

In terms of computational resources needed I like to think in terms of the 
[pyramid of analytical tasks](https://github.com/szilard/datascience-latency#latency-numbers-every-data-scientist-should-know).
I'm mostly interested in tools for non-linear machine learning, the distribution of dataset sizes
practitioners have to deal with, and how this has changed over the last years.

[KDnuggets](http://www.kdnuggets.com/) has conducted surveys of "the largest dataset you 
analyzed/data mined" (yearly since 2006).
It surveys the largest dataset for a given practitioner (instead of the typical one), it
measures size in bytes (rather than the more informative number of records) and it surveys
raw data sizes (I would be more interested in the size of the refined datasets used for modeling).
Nevertheless, it provides data points interesting for a study. (One could also 
question the representativeness of the sample etc.)

The annual polls are available on various [URLs](data/survey-urls.txt) 
and I compiled the data into a [csv file](data/dataset-sizes.csv).
The cummulative distribution of dataset sizes for a few select years is plotted below:
![](figs/cumfq-size-few_yrs-clean-1.png)

The dataset sizes vary over many orders of magnitude with most users in the 10 Megabytes to
10 Terrabytes range (a huge range), but furthermore with some users in the many Pettabytes range.

It seems the cummulative distribution function in the `0.1-0.9` range follows a linear dependecy 
vs `log(size)`:
![](figs/cumfq-size-fit-1.png).

Fitting a linear regression `lm(log(size_GB, 10) ~ cum_freq + year, ...)` for that range,
one gets coefficients `year: 0.075` and `cum_freq: 6.0`. We can use this "model" as a smoother
in the discussion below.

The above results imply an annual rate of increase of datasets of `10^0.075 ~ 1.2` that is 20%. 

The median dataset size increases from 6 GB (2006) to 30 GB (2015). That's all tiny, even more for
raw datasets, and it implies that over 50% of analytics professionals work with datasets
that (even in raw form) can fit in the memory of a single machine, therefore it can be definitely dealt 
with using simple analytical tools.

However, the dataset sizes are distributed over many orders magnitudes, e.g. the larger quantiles
based on smoothing for 2015 are:

quantile  |  value
----------|---------
50%       |  30 GB
60%       |  120 GB
70%       |  0.5 TB
80%       |  2 TB
90%       |  8 TB

The Terrabyte range is the home turf of data warehouses, MPP/analytical databases and the like, but
many organizations are trying to use "big data" tools for those sizes. 

About 5% of uses are in the Pettabytes range and likely need big data tools like Hadoop or Spark. 
While the hype around big
data, "exponential growth" of sensors and internet-of-things (IoT) etc. suggests a more rapid growth
rate than 20% yearly, the simple linear fit used above does not extend over the 90% precentile and 
it's hard to tell either way directly from the data.

Unfortunately it is unclear from all this study what's the distribution of dataset sizes used for 
modeling/machine learning (my primary area of interest). Some informal surveys suggest that for 
at least 90% of non-linear supervised learning uses the data fits in the RAM of a single machine 
and can be processed by high-performant tools like xgboost or H2O
(see [this github repo](https://github.com/szilard/benchm-ml)
for a benchmark of the most commonly used open source tools for non-linear supervised learning).

TODO: max RAM vs year for high-end server/largest EC2




