# Wildfire Smoke Impact - Norman, OK

## Overview
This repository contains the contents of an analysis into the impact of wildfire smoke on the city of Norman, OK. Specifically, we extract wildfire data from #TODO, AQI data from #TODO, build a predictive model to estimate smoke from 2025-2050, and ultimately create three figure:
- Figure 1: Histogram of Wildfire acreage by Proximity to Norman, OK
- Figure 2: Burn Acreage from 1961-2021 within 650 miles of Norman, OK
- Figure 3: Smoke and AQI Estimates from 1961-2021


## Licenses and API Documentation
#TODO Fill out API Documentation

Modeling inspiration was taken from fellow student Jake Flynn.

Lastly, this assignment, and significant portions of the "data_acquisition_aqi.ipynb.ipynb" and "data_acquisition_wildfire.ipnyb" files were developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program. This code is provided under the [Creative Commons](https://creativecommons.org) [CC-BY license](https://creativecommons.org/licenses/by/4.0/). Revision 1.2 - September 16, 2024

## Repository Structure
```markdown

├── data_clean/                                           # Folder containing the cleaned data
│   ├── #TODO Move norman_wildfires_SI_yearly_average.csv                           # CSV with the final, cleaned and processed AQI estimate data
│   └── #TODO.csv                       # CSV with the final, cleaned and processed widfire smoke estimate data
├── data_intermediate/                                    # Folder containing the intermediate data #TODO note a handful of intermediate files were too large to be stored on GIT
│   ├── rev_ids.json                                        # JSON with the revision ids for the latest wikipedia pages
│   └── scores_dict.json                                    # JSON with the ORES LiftWing score associated with each article
├── data_raw/                                             # Folder containing the raw data
│   └── USGS_WIldland_Fire_CombinedDataset.json                  # JSON containing all of the raw wildfire data
├── notebooks/                                            # Source code
│   ├── data_acquisition_aqi.ipynb                              # Notebook to make the api calls to extract,s tore, and process the aqi data
│   ├── data_acquisition_wildfire.ipynb                              # Notebook to extract the wildfire data from the raw JSON
│   ├── data_processing_wildfire.ipynb                               # Notebook to perform the data processing and smoke estimates for the wildfire smoke data
│   ├── modeling.ipynb                               # Notebook to perform the predictive modeling of the smoke estimates 
│   └── visualization.ipynb                                  # Notebook to perform the plotting of the three final figures
├── LICENSE                                               # License documentation
├── .gitignore                                            # git ignore for the repo
└── README.md                                             # README for the repo
```

## Data Schema
### norman_wildfires_SI_yearly_average.csv
```markdown

| Column Name      | Data Type | Data Description                                 
| ------------------------------------------
| 'Year'           | 'int'     | The year  
| 'Smoke_Estimate' | 'float64' | The estimated smoke score for that year

```

### scores_dict.json #TODO add any from the intermediate files?

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