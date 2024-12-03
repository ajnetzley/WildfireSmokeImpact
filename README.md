# Wildfire Smoke Impact - Norman, OK

## Overview
This repository contains the contents of an analysis into the impact of wildfire smoke on the city of Norman, OK. Specifically, we extract wildfire data from the [USGS repository](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81), historical AQI data from the US Environmental Protection Agency (EPA) Air Quality Service (AQS) [API](https://aqs.epa.gov/aqsweb/documents/data_api.html) build a predictive model to estimate smoke from 2025-2050, and ultimately create three figures:
- Figure 1: Histogram of Wildfire acreage by Proximity to Norman, OK
- Figure 2: Burn Acreage from 1961-2021 within 650 miles of Norman, OK
- Figure 3: Smoke and AQI Estimates from 1961-2021 in Norman, OK


## Licenses and API Documentation
### Wildfire Data Acquisition
To perform the wildfire smoke analysis, the complete [wildfire dataset](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81) was retrieved from a US government repository. The downloaded data schema can be found below in the data schema section labelled "USGS_Wildland_Fire_Combined_Dataset.json", however, this JSON was too large to store directly on GIT. Preliminary processing was performed to filter the wildfire data to wildfires occuring between 1961-2021, and within 650 (0r 1800) miles from Norman, OK. The distance was computed using a wildfire user module developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program.

### AQI Data Acquisition
The AQI Data Acquistion notebook extracted the historical AQI data from the US Environmental Protection Agency (EPA) Air Quality Service (AQS) API. The [documentation](https://aqs.epa.gov/aqsweb/documents/data_api.html) for the API provides definitions of the different call parameter and examples of the various calls that can be made to the API.
Specifically, this analyses makes API requests for AQI data from Cleveland county (home to Norman), and nearby Oklahoma county (home to Oklahoma City) in Oklahoma. 

### Contributions
Modeling ideation in "modeling.ipynb" was performed with collaboration from fellow student Jake Flynn, while AQI Estimate analysis was performed with collaboration from fellow student Sid Gurajala. Lastly, this assignment, and significant portions of the "data_acquisition_aqi.ipynb.ipynb" and "data_acquisition_wildfire.ipnyb" files were developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program. This code is provided under the [Creative Commons](https://creativecommons.org) [CC-BY license](https://creativecommons.org/licenses/by/4.0/). Revision 1.2 - September 16, 2024

## Reproducibility Guidelines
#TODO

## Repository Structure
Note that all files below denoted with (*) have been omitted from the actual repository as they are too large to upload on git. For the files in the data_intermediate, a subset denoted by the suffix "_SMALL" has been included for representation purposes. 
```markdown

├── data_clean/                                     # Folder containing the cleaned data
│   ├── asthma_non-smoker_survey_cleaned.csv            # CSV with the final, cleaned and processed survey non-smoker asthma data
│   ├── forecasted_smoke_estimates.csv                  # CSV with the forecasted smoke impact estimates from 2025-2050
│   ├── norman_aqi_yearly_average.csv                   # CSV with the final, cleaned and processed AQI estimate data
│   └── norman_wildfires_SI_yearly_average.csv          # CSV with the final, cleaned and processed widfire smoke estimate data
├── data_intermediate/                              # Folder containing the intermediate data (Note the real files were too large to be stored on GIT, so the files included here are smaller, sample files)
│   ├── *full_wildfires_SMALL.json                      # JSON with all of the extracted wildfire data
│   └── *norman_wildfires_SI_SMALL.json                 # JSON with the processed and filtered wildfire data
├── data_raw/                                       # Folder containing the raw data
│   ├── oklahoma_brfss_2000_raw.csv                     # CSV with raw survey asthma / smoker data from 2000
│   ├── oklahoma_brfss_2003-2010_raw.csv                # CSV with raw survey asthma / smoker data from 2003-2010
│   ├── oklahoma_brfss_2011-2023_raw.csv                # CSV with raw survey asthma / smoker data from 2011-2023
│   └── *USGS_Wildland_Fire_CombinedDataset.json        # JSON containing all of the raw wildfire data (Note this file was to large to upload, but can be downloaded directly from [here](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81))
├── notebooks/                                      # Source code
│   ├── data_acquisition_aqi.ipynb                      # Notebook to make the api calls to extract, store, and process the aqi data
│   ├── data_acquisition_wildfire.ipynb                 # Notebook to extract the wildfire data from the raw JSON
│   ├── data_processing_asthma.ipynb                    # Notebook to perform the data formatting, processing, and filtering of the asthma data
│   ├── data_processing_wildfire.ipynb                  # Notebook to perform the data processing and smoke estimates for the wildfire smoke data
│   ├── modeling_asthma.ipynb                           # Notebook to generate the linear regression prediction model, and forecast asthma from the smoke estimate
│   ├── modeling_smoke.ipynb                            # Notebook to perform the predictive modeling of the smoke estimates 
│   └── visualization_smoke.ipynb                       # Notebook to perform the plotting of the three smoke figures from the original analysis
├── LICENSE                                         # License documentation
├── .gitignore                                      # git ignore for the repo
└── README.md                                       # README for the repo
```

## Data Schema

### Cleaned Data
#### asthma_non-smoker_survey_cleaned.csv
```markdown

| Column Name            | Data Type | Data Description                                 
| ------------------------------------------
| 'Year'                 | 'int'     | The year  
| 'Percentage'           | 'float64' | The percentage of non-smoker survey respondants with asthma

```

#### forecasted_smoke_estimate.csv
```markdown

| Column Name            | Data Type | Data Description                                 
| ------------------------------------------
| 'Year'                 | 'int'     | The year  (since this is the forecast, from 2025-2050)
| 'Smoke_Estimate'       | 'float64' | The predicted Smoke Estimate score
| 'Upper_bound'          | 'float64' | The 95% CI upper bound of the Smoke Estimate score
| 'Lower_bound'          | 'float64' | The 95% CI lower bound of the Smoke Estimate score
```

#### norman_aqi_yearly_average.csv
```markdown

| Column Name            | Data Type | Data Description                                 
| ------------------------------------------
| 'Year'                 | 'int'     | The year  
| 'Average_AQI_Estimate' | 'float64' | The estimated average daily AQI for that year

```

#### norman_wildfires_SI_yearly_average.csv
```markdown

| Column Name      | Data Type | Data Description                                 
| ------------------------------------------
| 'Year'           | 'int'     | The year  
| 'Smoke_Estimate' | 'float64' | The estimated smoke score for that year

```

### Intermediate Data
#### full_wildfire(_SMALL).json
Note that this JSON contains a large number of fields, most of which were defined in the original dataset, and most of which we do not end up using. Listed below are all the relevant attribtues which were used during analysis.
```json
{
    "type": "object",
    "description": "Full fire data for a specific fire",
    "properties":{
            "attributes":{
                "type": "object",
                "description": "Full fire data for a specific fire",
                "properties":{
                        "items":{
                            "OBJECT_ID": {
                                "type": "int",
                                "description": "The ID for this specific fire"
                                },
                            "FIRE_YEAR": {
                                "type": "int",
                                "description": "The year the fire occured"
                                },
                            "GIS_Acres": {
                                "type": "float64",
                                "description": "The amount of burned GIS acres"
                                }
                            }
                    }
                },
            "geometry":{
                "type": "object",
                "description": "GIS Ring data for the fire location",
                "properties":{
                        "attributes":{
                            "rings": {
                                "type": "array",
                                "description": "The lat and lon of the point"
                                }
                            }
                        }
                }
            }    
}
```


#### norman_wildfires_SI(_SMALL).json
Note that this JSON contains 32 fields, 30 of which were defined in the original dataset, and most of which we do not end up using. This json also has a very similar structure to that of the full_wildfires.json described above, but with a few new attributes. Listed below are all the relevant attributes which were used during analysis.

```json
{
    "type": "array",
    "description": "Full fire data for a specific fire",
    "properties":{
            "items":{
                "OBJECT_ID": {
                    "type": "int",
                    "description": "The ID for this specific fire"
                    },
                "FIRE_YEAR": {
                    "type": "int",
                    "description": "The year the fire occured"
                    },
                "GIS_Acres": {
                    "type": "float64",
                    "description": "The amount of burned GIS acres"
                    },
                 "distance_from_norman": {
                    "type": "float64",
                    "description": "The distance of the fire from Norman, in miles"
                    },
                "Smoke_Impact": {
                    "type": "float64",
                    "description": "The computed Smoke Impact score"
                    }
            }
        }
    
}
```
### Raw Data
#### oklahoma_brfss_2000_raw.csv (with the same fromat as ...2003-2010, and ...2011-2023)
In their raw form as downloaded from the OK2Share website, these datasets were not in a clean tabular format, and therefore required extensive post processing (see data_processing_asthma.csv). Shown below is the sample input format (and actual raw data of 2000) to get a sense of data format. Visit the OK2Share [website](https://www.health.state.ok.us/stats/Health_Surveys/BRFSS/index.shtml) and select the relevant smoking and asthma fields in the BRFSS crosstab survey data request to obtain an html version.
```markdown

| Column 1                | Column 2 | Column 3 | Column 4          | Column 5       | Column 6                               
| ----------------------------------------------------------------------------------------------
| Year of interview 2000  |          |          |                   |                |
|                         |          |          | Currently smoking |                |
|                         |          |          | No                | Yes            | Total
| Reported current asthma |          |          |                   |                |
| No                      | n        |          | 761               | 194            | 955
|                         | N        |          | 521502            | 131652         | 653154
|                         | %        |          | 79.8              | 20.2           | 100
|                         | CI       |          | ( 77.0 - 82.7)    | ( 17.3 - 23.0) | n/a
| Yes                     | n        |          | 50                | 15             | 65
|                         | N        |          | 29322             | 10388          | 39710
|                         | %        |          | 73.8              | 26.2           | 100
|                         | CI       |          | ( 61.6 - 86.1)    | ( 13.9 - 38.4) | n/a
| Total                   | n        |          | 811               | 209            | 1020
|                         | N        |          | 550824            | 142040         | 692864
|                         | %        |          | 79.5              | 20.5           | 100
|                         | CI       |          | ( 76.7 - 82.3)    | ( 17.7 - 23.3) | n/a

```


#### USGS_Wildland_Fire_Combined_Dataset.json
Full data schema [overview](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81) and [details](https://www.sciencebase.gov/catalog/file/get/61aa537dd34eb622f699df81?f=__disk__d0%2F63%2F53%2Fd063532049be8e1bc83d1d3047b4df1a5cb56f15&transform=1&allowOpen=true)
