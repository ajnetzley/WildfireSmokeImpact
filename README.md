# Wildfire Smoke Impact - Norman, OK

## Overview
This repository contains the contents of an analysis into the impact of wildfire smoke on the city of Norman, OK. Specifically, we extract wildfire data from #TODO, AQI data from #TODO, build a predictive model to estimate smoke from 2025-2050, and ultimately create three figure:
- Figure 1: Histogram of Wildfire acreage by Proximity to Norman, OK
- Figure 2: Burn Acreage from 1961-2021 within 650 miles of Norman, OK
- Figure 3: Smoke and AQI Estimates from 1961-2021


## Licenses and API Documentation
### Wildfire Data Acquisition
To perform the wildfire smoke analysis, the complete [Wildfire dataset](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81) was retrieved from a US government repository. The downloaded data schema can be found below in the data schema section labelled "USGS_Wildland_Fire_Combined_Dataset.json", however, this JSON was too large to store directly on GIT. Preliminary processing was performed to filter the wildfire data to wildfires occuring between 1961-2021, and within 650 (0r 1800) miles from Norman, OK. The distance was computed using a wildfire user module developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program.

### AQI Data Acquisition
The AQI Data Acquistion notebook extracted the historical AQI data from the US Environmental Protection Agency (EPA) Air Quality Service (AQS) API. The [documentation](https://aqs.epa.gov/aqsweb/documents/data_api.html) for the API provides definitions of the different call parameter and examples of the various calls that can be made to the API.
Specifically, this analyses makes API requests for AQI data from Cleveland county (home to Norman), and nearby Oklahoma county (home to Oklahoma City) in Oklahoma. 

### Contributions
Modeling ideation in "modeling.ipynb" was performed with collaboration from fellow student Jake Flynn, while AQI Estimate analysis was performed with collaboration from fellow student Sid Gurajala. Lastly, this assignment, and significant portions of the "data_acquisition_aqi.ipynb.ipynb" and "data_acquisition_wildfire.ipnyb" files were developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program. This code is provided under the [Creative Commons](https://creativecommons.org) [CC-BY license](https://creativecommons.org/licenses/by/4.0/). Revision 1.2 - September 16, 2024

## Repository Structure
Note that all files below denoted with (*) have been omitted from the actual repository as they are too large to upload on git. For the files in the data_intermediate, a subset denoted by the suffix "_SMALL" has been included for representation purposes. 
```markdown

├── data_clean/                                # Folder containing the cleaned data
│   ├── norman_aqi_yearly_average.csv                       # CSV with the final, cleaned and processed AQI estimate data
│   └── norman_wildfires_SI_yearly_average.csv                       # CSV with the final, cleaned and processed widfire smoke estimate data
├── data_intermediate/                                    # Folder containing the intermediate data (Note the real files were too large to be stored on GIT, so the files included here are smaller, sample files)
│   ├── *full_wildfires_SMALL.json                                        # JSON with all of the extracted wildfire data
│   └── *norman_wildfires_SI_SMALL.json                                    # JSON with the processed and filtered wildfire data
├── data_raw/                                             # Folder containing the raw data
│   └── *USGS_WIldland_Fire_CombinedDataset.json                  # JSON containing all of the raw wildfire data (note this file was to large to upload, but can be downloaded directly from [here](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81))
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