# data-512-homework_2

## Considering Bias in Data
The goal of this analysis is to explore the concept of bias in data using Wikipedia articles. This analysis will consider articles on political figures from different countries. We will combine a dataset of Wikipedia articles with a dataset of country populations, and use a machine learning service called ORES to estimate the quality of each article.

We will also perform an analysis of how the coverage of politicians on Wikipedia and the quality of articles about politicians varies among countries. It will consist of a series of tables that show:
1. The countries with the greatest and least coverage of politicians on Wikipedia compared to their population.
2. The countries with the highest and lowest proportion of high quality articles about politicians.
3. A ranking of geographic regions by articles-per-person and proportion of high quality articles.


## Licenses
### Code and APIs
1. Code used in `analysis.ipynb` - This code example extended by the code snippets developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program. This code is provided under the [Creative Commons](https://creativecommons.org) [CC-BY license](https://creativecommons.org/licenses/by/4.0/). Revision 1.3 - August 16, 2024
2. This dataset was created in accordance with the [Wikimedia Foundation Terms of Use](https://foundation.wikimedia.org/wiki/Policy:Terms_of_Use), which states that we are good to create this dataset and use it for our analysis as long as we don't violate their UCoC and other terms and policies. Summary of the Terms of Use: According to Wikimedia's terms, any data extracted from Wikipedia must attribute the source and be shared under the same or compatible license. For this project, any dataset created must acknowledge the Wikimedia Foundation and must not be used in a manner that violates the privacy of Wikipedia's contributors or users.

### Source data
1. The Wikipedia [Category:Politicians by nationality](https://en.
wikipedia.org/wiki/Category:Politicians_by_nationality) was crawled to generate a list of Wikipedia article pages about politicians from a wide range of countries. It has the list of information about the article name, url, and country the politician is/was associated to. Can be found in the repository as `politicians_by_country.AUG.2024.csv`.
2. The population data is available in CSV format as `population_by_country_AUG.2024.csv` in the repository. This dataset was downloaded from the [world population data sheet](https://www.prb.org/international/indicator/population/table) published by the Population Reference Bureau. It contains rows that provide cumulative regional population counts. These rows are distinguished by having ALL CAPS values in the 'geography' field (e.g. AFRICA, OCEANIA).

## API Documentation/References
1. Revision id data was fetched using the [MediaWiki GET API](https://en.wikipedia.org/w/api.php). Careful consideration was given to adhere to not bombard their platform with too many requests - The GET API asks that we not exceed 100 requests per second, we add a small delay to each request.
2. Python's [urllib3](https://urllib3.readthedocs.io/en/stable/) to fetch the data
3. [Pandas](https://pandas.pydata.org/docs/) to handle data processing
4. [ORES API](https://www.mediawiki.org/wiki/ORES) - Object Revision Evaluation API gives us the prediction of the quality of a Wikipedia article.
5. [urllib](https://docs.python.org/3/library/urllib.request.html) is a python library to fetch data from API and load it in our scripts.


## Data Files
### Input

### Generated

## Research Implications
We found some interesting patterns. The number and quality of articles varied a lot depending on where you looked. This suggests that Wikipedia might have some biases in how it covers political figures.

Why might that be? Well, to start with, having access to the internet and knowing how to use it makes a big difference in how much people can contribute to Wikipedia. Countries with fewer people online or who aren't as comfortable with technology might have fewer articles. Second, a country's politics and culture can influence whether people want to write or edit articles about their politicians. Restrive governments like North Korea and China might have fewer people willing to write articles, fearing negative consequences for any mistakes. And finally, how well people in different countries understand English can also affect the quality and quantity of articles.

Some of the findings were also very interesting. For example, the huge difference in coverage between rich and poor countries shows that Wikipedia doesn't represent everyone equally. This could lead to a skewed view of global politics, since countries with fewer articles might not be heard as much. This might need some looking into to ensure equitable coverage and access to political information across the globe.

To make Wikipedia more fair and balanced, we need to make the articles more accessible to the people in less developed countries and understand why Wikipedia is important. We should also encourage more people from different backgrounds to contribute to Wikipedia. This will help us create a more complete and accurate picture of political figures and issues around the world.