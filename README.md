# chicago_covid_19_zip_code_analysis

## File Directory

1. **census_data_prep.ipynb** - Gathering and cleaning ACS data from US Census.
2. **data_cleaning_for_mapping_regression.ipynb** - Gathering and cleaning Chicago COVID-19 data for mapping and regression notebooks.
3. **mapping_pos_rates.ipynb** - Mapping 2-week average poisitivity rates, testing per capita, and percent change in positivity rates.
4. **zip_code_regression_analysis.ipynb** - Using OLS regression to analyze the relationship between ZIP-code-level characteristics and 2-week average positivity rate.
5. **time_series_analysis.ipynb** - Cleaning daily case and testing data, EDA and time series modeling.

## Data
**US Census Data (API):**
- ACS 2018 Estimates (ZIP code analysis)
- Occupation Lookup Table S2401 (ZIP code analysis)

**Chicago Open Data:**
- [COVID-19 Cases, Tests, and Deaths by ZIP Code](https://data.cityofchicago.org/Health-Human-Services/COVID-19-Cases-Tests-and-Deaths-by-ZIP-Code/yhhz-zm2v) (ZIP code analysis)
- [COVID-19 Testing Sites](https://data.cityofchicago.org/Health-Human-Services/COVID-19-Testing-Sites/thdn-3grx) (ZIP code analysis)
- [COVID-19 Daily Cases, Deaths, and Hospitalizations](https://data.cityofchicago.org/Health-Human-Services/COVID-19-Daily-Cases-Deaths-and-Hospitalizations/naz8-j4nc) (Time Series Analysis)
- [COVID-19 Daily Testing by Test](https://data.cityofchicago.org/Health-Human-Services/COVID-19-Daily-Testing-By-Test/gkdw-2tgv) (Time Series Analysis)
- [Chicago ZIP Codes Shapefile](https://data.cityofchicago.org/Facilities-Geographic-Boundaries/Boundaries-ZIP-Codes/gdcf-axmw) (ZIP Code maps)

**NOAA Weather Data:**
- [Average Daily Temperature and Precipitation in Chicago](https://www.ncdc.noaa.gov/cdo-web/search) (Time Series Analysis)


## Background
Chicago is now in its second wave of the pandemic having approached a positivity rate of 15 percent at the start of November. With the massive uptick in positive cases, it’s important to *understand which communities are most affected by the virus* and *how certain mitigation efforts may be working to reduce the city’s overall positivity rate through the coming weeks.*

**Positivity rate** is the percentage of total tests administered in a given day that come back positive. This does not include rapid tests, or antibody tests.
The World Health Organization (WHO) recommends that the positivity rate in a particular area **remain below 5 percent for at least 2 weeks before loosening restrictions.** As shown below, the City is currently far above the 5 percent threshold.

![city_positivity_rate](https://user-images.githubusercontent.com/64563191/98833351-e75ffd80-240b-11eb-92a7-1750b09448bb.png)

On October 23, the City implemented a City-wide curfew for non-essential businesses from 10 pm to 6pm and mandated that bars without food licenses close all inoor services. The effects of these measures are yet to be seen, but are what I later forecaset using a SARIMAX Model.

## Analysis: 
- **ZIP Code Analysis:** Using a number of ZIP-code level econonomic and demographic variables to model the relatiosnhip between each respective variable and Zip Code Positivity Rate and Per-Capita Testing.
 - Model Type: Ordinary Least Squares
 - Variables:
  - Median Age
  - Median Household Income
  - Median Household Size
  - Percent of a ZIP Code that is Hispanic/Latinx
  - Percent Black
  - Percent White
  - Percent Undocumented & Percent Undocumented Foreign Born Latin America (FBLA)
  - Percent Uninsured
  - Percent Unemployed
  - Percent Healthcare Workers
  - Percent Essential Workers
  - Distance from Testing Site
  
  




![final_model_projection](https://user-images.githubusercontent.com/64563191/98833353-e7f89400-240b-11eb-9af6-20e144380894.png)
![positivity_rates_by_zip_map](https://user-images.githubusercontent.com/64563191/98833355-e7f89400-240b-11eb-9b66-4d95baf756a8.png)
![SARIMAX_model_train_test](https://user-images.githubusercontent.com/64563191/98833356-e8912a80-240b-11eb-8bfb-718eac02eaa7.png)
![testing_by_zip_map](https://user-images.githubusercontent.com/64563191/98833358-e8912a80-240b-11eb-90f4-83a37fa5fc87.png)
<img width="451" alt="time_series_modeling_metrics" src="https://user-images.githubusercontent.com/64563191/98833360-e929c100-240b-11eb-866c-6d4a340f1b39.png">
