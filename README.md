# Wildfire Smoke Impact - Norman, OK

## NEED TO UPDATE EVERYTHING BELOW
## Overview
This repository contains the contents of an analysis into the quality rating of wikipedia articles on politicians from countries across the globe. Specifically, we use an API to scrape articles from wikipedia, and then pass these articles to the ORES ML model called LiftWing, which gives the articles their ratings. After collecting the data, this analysis generates the following 6 tables:
- Table 1: Top 10 countries by coverage
- Table 2: Bottom 10 countries by coverage
- Table 3: Top 10 countries by high quality
- Table 4: Bottom 10 countries by high quality
- Table 5: Geographic regions by total coverage
- Table 6: Geographic region by high quality coverage

## Licenses and API Documentation
To generate the list of politicians by nationality ("politicians_by_country_AUG.csv"), the Wikipedia [Category:Politiciens](https://en.wikipedia.org/wiki/Category:Politicians_by_nationality) by nationality was crawled. The list of population by country ("population_by_country_AUG.2024.csv"), was downloaded from the [world population data sheet](https://www.prb.org/international/indicator/population/table/) published by the Population Reference Bureau.

To access the page info data, we use the [MediaWiki REST API for the EN Wikipedia](https://www.mediawiki.org/wiki/API:Main_page), with API documentation [here](https://www.mediawiki.org/wiki/API:Info).

To score the articles, we use the Wikimedia ML service infrastructure called [LiftWing](https://wikitech.wikimedia.org/wiki/Machine_Learning/LiftWing). Specifically, we use the LiftWing version of [ORES](https://www.mediawiki.org/wiki/ORES). here we additionally link to the [ORES API documentation](https://ores.wikimedia.org) and the [ORES LiftWing documentation](https://wikitech.wikimedia.org/wiki/Machine_Learning/LiftWing/Usage).

Lastly, this assignment, and significant portions of the "access_page_info.ipynb" and "ores_liftwing_scoring.ipnyb" files were developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program. This code is provided under the [Creative Commons](https://creativecommons.org) [CC-BY license](https://creativecommons.org/licenses/by/4.0/). Revision 1.2 - September 16, 2024

## Repository Structure
```markdown

├── data_clean/                                           # Folder containing the cleaned data
│   ├── wp_countries-no_match.txt                           # txt file of all country names not present in BOTH the politician-country and population-country datasets
│   └── wp_politicians_by_country.csv                       # CSV with the final, cleaned and merged data of politician, article score, country, and population
├── data_intermediate/                                    # Folder containing the intermediate data
│   ├── rev_ids.json                                        # JSON with the revision ids for the latest wikipedia pages
│   └── scores_dict.json                                    # JSON with the ORES LiftWing score associated with each article
├── data_raw/                                             # Folder containing the raw data
│   ├── politicians_by_country_AUG.2024.csv                 # CSV with the list of politicians and their corresponding countries
│   └── population_by_country_AUG.2024.csv                  # CSV with the list of countries and their populations
├── notebooks/                                            # Source code
│   ├── analysis.ipynb                                      # Notebook to perform the data analysis for generating the 6 tables
│   ├── data_acquisition.ipynb                              # Notebook to perform the API calls to get the revision IDs of the wikipedia articles
│   ├── data_processing.ipynb                               # Notebook to perform the data processing and merging of the scores datasets and population/politician countries
│   └── data_scoring.ipynb                                  # Notebook to perform the ORES LiftWing calls to score the accessed articles
├── LICENSE                                               # License documentation
├── .gitignore                                            # git ignore for the repo
└── README.md                                             # README for the repo
```

## Data Schema
### rev_ids.json
```json
{
    "type": "object",
    "properties":{
        "politician_name": {
            "type": "array",
            "description": "Name of politician",
            "items":{
                "revision_id": {
                    "type": "int",
                    "description": "Revision ID of wikipedia page"}
            }
        }
    }
}
```

### scores_dict.json

```json
{
    "type": "object",
    "properties":{
        "politician_name": {
            "type": "array",
            "description": "Name of politician",
            "items":{
                "revision_id": {
                    "type": "int",
                    "description": "Revision ID of wikipedia page"
                    },
                "quality_score": {
                    "type": "string",
                    "description": "LiftWing given quality score for the article"
                    }
            }
        }
    }
}
```

### wp_countries-no_match.txt
```markdown

| Column Name | Data Type | Data Description                                 
| ------------------------------------------
| 'Country'   | 'string'  | The name of the country  

```

### wp_politicians_by_country.csv
```markdown

| Column Name       | Data Type | Data Description                                 
| ------------------------------------------
| 'revision_id'     | 'int'     | The revision ID of the wikipedia article
| 'article_quality' | 'string'  | The ORES LiftWing quality rating
| 'article_title'   | 'string'  | The title of the article (the politician's name)  
| 'country'         | 'string'  | The country of the politician  
| 'population'      | 'float'   | The population of the country 
| 'region'          | 'string'  | The region associated with the country
```

## Research Implications
### Learning Reflections
Throughout the analyses, I learned a lot about API calling, especially for using a hosted ML model like ORES LiftWing, as well as how something even as simple as a data merge can be quite cumbersome when there are issues with the data. In the analysis of the article qualities, I found that in general, economically disadvantaged countries tended to have articles that were rated as lower quality. For example, many of the countries that had zero high quality ratings were located in Africa, which was further cemented by many Africa regions being low on the region-based high-quality score analysis. One thing that surprised me is that there was no absolute trend in the ratings; for example, Norway and China are well-developed countries, but still had very poor article quality ratings (in fact both had 0 high-quality rated articles). To draw any sort of strong opinions about biases present in the wikimedia dataset would involve a much more thoughtfully derived investigation, but based on the limited analysis provided, there does appear to be a bit of bias towards African and Asian countries having less articles in general, on a per-capita basis. However, this could possibly be due to individuals in these continents spending a lot less time on wikipedia than individuals from other places -- spending less time on wikipedia likely means that less articles have been created, and the ones that have are of lower quality.

### Q1: What might your results suggest about (English) Wikipedia as a data source?
Similar to what I discussed above, the results showing that African and Asian countries have less per-capita articles and high quality per-capita articles could provide evidence that English Wikipedia does not have a fair spread of data across all regions of the world. This is not particularly shocking, as I alluded to earlier, I expect that English Wikipedia is more often used by predominantly English speaking countries. Therefore, perhaps the African and Asian countries have different online sources that they use to access information about their politicians.

### Q2: Can you think of a realistic data science research situation where using these data might create biased or misleading results, due to the inherent gaps and limitations of the data?
Absolutely. If we were planning to do any sort of INTER-country analysis, we would likely face misleading results. For example, if we were planning to appoint a world leader of an international coalition, we might think to use the data contained on wikipedia as a good list of politicians to form our applicant pool. However, by analyzing this data, we would see that world leaders from many African and Asian countries would likely have much more limited, and perhaps less accurate information about them. This could lead decision makers to unfairly ignore potential applicants from these countries, simply due to lack of high-quality information.

### Q3: On the flip side, can you think of a realistic data science research situation where using these data might still be appropriate and useful, despite its inherent limitations and biases?
Yes. If we were planning to do a INTRA-country analysis, despite the limitations of the data, we could still get appropriate and useful results (of course, this is just based on the comparison between countries -- we didn't look into other potential biases that might be present even within countries, perhaps biases on gender, race, etc.). For example, if we were a business, we could use this data to investigate potential politicians in Spain, to predict who might be most likely to take office and what the implications might be on our industry. However, as with all forms of bias, it is critical to take a proactive approach in attempting to limit biases whenever possible, and being sure to always be transparent about all of your data sources and analysis techniques, to showcase all potential sources.

