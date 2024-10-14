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
1. The Wikipedia [Category:Politicians by nationality](https://en.wikipedia.org/wiki/Category:Politicians_by_nationality) was crawled to generate a list of Wikipedia article pages about politicians from a wide range of countries. It has the list of information about the article name, url, and country the politician is/was associated to. Can be found in the repository as `politicians_by_country.AUG.2024.csv`.
2. The population data is available in CSV format as `population_by_country_AUG.2024.csv` in the repository. This dataset was downloaded from the [world population data sheet](https://www.prb.org/international/indicator/population/table) published by the Population Reference Bureau. It contains rows that provide cumulative regional population counts. These rows are distinguished by having ALL CAPS values in the 'geography' field (e.g. AFRICA, OCEANIA).

## API Documentation/References
1. Revision id data was fetched using the [MediaWiki GET API](https://en.wikipedia.org/w/api.php). Careful consideration was given to adhere to not bombard their platform with too many requests - The GET API asks that we not exceed 100 requests per second, we add a small delay to each request.
2. Python's [urllib3](https://urllib3.readthedocs.io/en/stable/) to fetch the data
3. [Pandas](https://pandas.pydata.org/docs/) to handle data processing
4. [ORES API](https://www.mediawiki.org/wiki/ORES) - Object Revision Evaluation API gives us the prediction of the quality of a Wikipedia article.
5. [urllib](https://docs.python.org/3/library/urllib.request.html) is a python library to fetch data from API and load it in our scripts.

### Access Token and ORES API

To use the ORES API, you'll need an access token. Here's how to get one:

1. Create a Wikimedia Account: If you don't have one already, sign up for a Wikimedia account. This is the same account you use for Wikipedia.
2. Get an Access Token: Follow the steps in this guide to generate an access token: [Link to guide](https://api.wikimedia.org/wiki/Authentication).

Important: When you create your access token, you'll get a Client ID, Client secret, and Access token. Save these carefully, as they won't be shown again. If you lose any of them, you can create a new access token.


### Input
The below csv files are available in the resources folder of this repository

1. [politicians_by_country_AUG.2024.csv](politicians_by_country_AUG.2024.csv): List of Wikipedia articles about politicians
```csv
name, url, country
"Majah Ha Adrif","https://en.wikipedia.org/wiki/Majah_Ha_Adrif","Afghanistan"
"Haroon al-Afghani","https://en.wikipedia.org/wiki/Haroon_al-Afghani","Afghanistan"
...


About the data -
1. name (string): full name of the person
2. url (string): URL of the corresponding Wikipedia article for each person
3. country (string): country of origin for each person

```
2. [population_by_country_AUG.2024.csv](population_by_country_AUG.2024.csv): Population data by country
```csv
Geography,Population
WORLD,8009
AFRICA,1453
NORTHERN AFRICA,256
Algeria,46.8
Egypt,105.2
Libya,6.9
...


About the data -
1. geography (string): geographic region or country
2. population (integer): population count for the corresponding geography

```


### Generated
The below csv files are present in the generated_files folder of this repository -
1. [wp_politicians_by_country.csv](wp_politicians_by_country.csv): Combined data on politicians, countries, and article quality
```csv
,article_title,country,revision_id,article_quality,region,population
0,Majah Ha Adrif,Afghanistan,1233202991,Start,SOUTH ASIA,42.4
1,Haroon al-Afghani,Afghanistan,1230459615,B,SOUTH ASIA,42.4
...


About the data -
1. article_title (string): Stores the title of the Wikipedia article.
2. country (string): Stores the country associated with the article.
3. revision_id (integer): Stores the unique identifier for the article revision.
4. article_quality (string): Stores the assessed quality of the article (e.g., "Start", "B").
5. region (string): Stores the geographic region of the country.
6. population (float): Stores the population of the country (with decimal points).
```
2. [wp_countries-no_match.txt](wp_countries-no_match.txt): Countries with no matches between datasets
```txt
San Marino
Reunion
Western Sahara
Andorra
Curacao
Ireland
Sao Tome and Principe
Korean
Brunei
Jamaica
...


About the data -
Contains list of countries
```
3. [wiki_page_info.json](wiki_page_info.json): Contains the data dump of articles and their latest revision ids, based on info extracted from MediaWiki API.
```json
{
  "article_title_1": {
    "pageid": 12345, # int - Page ID on Wikipedia
    "ns": 0, # int - Namespace ID
    "title": "Article Title 1", # string - Title of the article
    "lastrevid": 67890, # int - Last revision ID
    "touched": "YYYY-MM-DDTHH:MM:SSZ" # string - Timestamp when the page was last edited
  },
  "article_title_2": {
    "pageid": 67890,
    "ns": 0,
    "title": "Article Title 2",
    "lastrevid": 13579,
    "touched": "YYYY-MM-DDTHH:MM:SSZ"
  },
  ... 
}

About the data -
  - pageid:  The unique identifier for the Wikipedia page.
  - ns: The namespace of the page (typically 0 for articles).
  - title: The title of the Wikipedia article.
  - lastrevid: The ID of the last revision of the article.
  - touched: The timestamp indicating when the page was last modified.
```
- [ores_info.json](ores_info.json): Contains a list of articles with their latest revision ids and corresponding article rating, based on the response from ORES API.
```json
{
    "Abdul Baqi Turkistani": {
        "httpCode": 504,
        "httpReason": "upstream request timeout"
    },
    "Abdul Ghani Ghani": {
        "enwiki": {
            "models": {
                "articlequality": {
                    "version": "0.9.2"
                }
            },
            "scores": {
                "1227026187": {
                    "articlequality": {
                        "score": {
                            "prediction": "Stub",
                            "probability": {
                                "B": 0.011385918433177827,
                                "C": 0.01746636720083123,
                                "FA": 0.0023603522965185463,
                                "GA": 0.005156605723717649,
                                "Start": 0.0518527973277106,
                                "Stub": 0.9117779590180441
                            }
                        }
                    }
                }
            }
        }
    },
    ...
}


About the data -
1. article_title (string): Stores the name of the Wikipedia article (e.g., "Abdul Baqi Turkistani", "Abdul Ghani Ghani").
2. httpCode (integer): Stores the HTTP status code for the article's data retrieval (e.g., 504).
3. httpReason (string): Stores the reason for the HTTP status code (e.g., "upstream request timeout").
4. enwiki (object): Contains data specific to the English Wikipedia.
models (object): Contains information about the models used for analysis.
articlequality (object): Contains data related to the article's quality assessment.
5. version (string): Stores the version of the article quality model.
scores (object): Contains quality scores for specific article revisions.
6. revision_id (integer): Stores the unique identifier for the article revision.
7. articlequality (object): Contains the detailed article quality assessment.
8. score (object): Contains the final quality prediction and probability distribution.
9. prediction (string): Stores the predicted article quality class (e.g., "Stub").
10. probability (object): Stores the probability distribution for different quality classes.
11. quality_class (string): Stores the individual quality class (e.g., "B", "C", "FA", "GA", "Start", "Stub").
12. probability_value (float): Stores the probability value associated with the quality class.
```

## Gotchas / Issues / Special considerations
1. When fetching data from ORES API, we hit 504 Gateway Timeout errors randomly, and this might lead to slightly different analysis results every time you run the scripts. Should not be a major difference.
2. The ORES API calls to all 7k+ articles  takes approx. 2 hours to run, so sit back and grab some more popcorn while this runs. This is [exactly how long the process will run for](https://www.youtube.com/watch?v=oMrfhk-MXRg).
3. We will need access token to make the API calls, and since this analysis was done in Google Colab, it has an internal mechanism to implement sensitive ENV variables. Refer `1. Setup` section.
4. In the analysis part of `7.1`, Monaco and Tuvalu seem to have Inf values. This is because the original data has population values as 0 for them. This was not fixed as just modifying those values would not be the right way to proceed with the analysis.
5. When we are combining the two datasets (population and politicians) in `step 6`, we use an Inner Join to perform the merge. Inner join selects records that have matching values in both tables, which is exaclty what we need.
6. It seems like there are 7155 - 7111 = 44 duplicate values for politicians, but they belong to different countries. Care must be taken to not discard them as it is as they are associated with different countries. To highlight - a politician is associated with two different countries, but there is only one Wiki article with the name of the politician. It is impossible to have two wiki articles with same title name as it would break the URL of the webpage. Looking into Wikipedia naming strategies, it is aparent that they include additional details in the article title for similar names (usually extra information in round brackets). Refer the sample articles below - 

    - Jack Sparrow - https://en.wikipedia.org/wiki/Jack_Sparrow
    - Jack Sparrow (song) - https://en.wikipedia.org/wiki/Jack_Sparrow_(song)
7. The above does not affect our analysis, as Wikipedia always has unique titles. So for rows where same politician is associated with 2 or more countries, same data needs to be applied.


## Research Implications
We found some interesting patterns. The number and quality of articles varied a lot depending on where you looked. This suggests that Wikipedia might have some biases in how it covers political figures. Additionally, the top countries by article count don’t always match those with the highest-quality articles, proving that more coverage doesn’t always mean better content.

Why might that be? Well, to start with, having access to the internet and knowing how to use it makes a big difference in how much people can contribute to Wikipedia. Countries with fewer people online or who aren't as comfortable with technology might have fewer articles. Second, a country's politics and culture can influence whether people want to write or edit articles about their politicians. Restrive governments like North Korea and China might have fewer people willing to write articles, fearing negative consequences for any mistakes. And finally, how well people in different countries understand English can also affect the quality and quantity of articles.

Some of the findings were also very interesting. For example, the huge difference in coverage between rich and poor countries shows that Wikipedia doesn't represent everyone equally. This could lead to a skewed view of global politics, since countries with fewer articles might not be heard as much. This might need some looking into to ensure equitable coverage and access to political information across the globe.

To make Wikipedia more fair and balanced, we need to make the articles more accessible to the people in less developed countries and understand why Wikipedia is important. We should also encourage more people from different backgrounds to contribute to Wikipedia. This will help us create a more complete and accurate picture of political figures and issues around the world.