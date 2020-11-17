# chicago_covid_19_zip_code_analysis

## File Directory

1. **census_data_prep.ipynb** - Gathering and cleaning ACS data from US Census.
2. **data_cleaning_for_mapping_regression.ipynb** - Gathering and cleaning Chicago COVID-19 data for mapping and regression notebooks.
3. **mapping_pos_rates.ipynb** - Mapping 2-week average poisitivity rates, testing per capita, and percent change in positivity rates.
4. **zip_code_regression_analysis.ipynb** - Using OLS regression to analyze the relationship between ZIP-code-level characteristics and 2-week average positivity rate.
5. **time_series_analysis.ipynb** - Cleaning daily case and testing data, EDA and time series modeling.
6. **Chicago_COVID_presentation.pdf** - PDF of the presentation of findings.

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

![pos_rate_citywide](https://user-images.githubusercontent.com/64563191/99413491-d7409600-28c3-11eb-83fd-a5aabd6a33ef.png)

On October 23, the City implemented a City-wide curfew for non-essential businesses from 10 pm to 6am and mandated that bars without food licenses close all inoor services. The effects of these measures are yet to be seen, but are what I later forecaset using a SARIMAX Model.

## ZIP Code Analysis: 
Used a number of ZIP-code level econonomic and demographic variables to model the relatiosnhip between each respective variable and Zip Code Positivity Rate and Per-Capita Testing.
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
  
**Findings:**

<img width="716" alt="pos_rates_rlm" src="https://user-images.githubusercontent.com/64563191/99413492-d7409600-28c3-11eb-83f4-d4c665b6c3fe.png">
  
Zip codes with a high percentage of Hispanic residents, essential workers, and with larger mean houshold sizes are tend to have significantly higher positivity rates controlling for all other variables. High positivity rates in these areas could imply that these areas lack adequate testing, or simply, that there is an actual surge of cases in that area assuming testing is readily available. From the descriptive anlayses (see mapping_pos_rates.ipynb), it would appear testing is lagging in these areas. 

Conversely, however, areas with a higher percentage of healthcare workers and uninsured individuals are more likely to have substantially lower positivity rates in their zip codes, controlling for all other variables in the model. For healthcare workers, testing is readliy available at their workplace and it is a reuquirement that they get tested frequently which would drive down the positivity rates in their respective zip codes. For ZIP codes with a high percentage of uninsured individuals, the narrative is rather unclear as it is also the case that these individuals are less likely to be tested, which would, in turn, reduce positivity rates. However, we see that these ZIP codes have a significantly lower positivity rate than others, controlling for all other variables in the model. This could mean that these uninsured individuals may be more risk averse than others.
  
 
<img width="718" alt="test_rates_rlm" src="https://user-images.githubusercontent.com/64563191/99413495-d7409600-28c3-11eb-89a4-3627ebfbf607.png">

Testing, on the other hand, seems to be targeted at younger and whiter communities surrounding universities (see mapping_pos_rates.ipynb). In this analysis, controlling for all other variables in the model, ZIP codes with a higher percentage of younger people, with lower incomes, with larger household sizes, with a larger percentage of uninsured individuals, and with a higher percentage of unemployed individuals are all significantly less likely to get tested than other ZIP codes in the model. While most of these make sense, percent uninsured, again, sticks out as one variable that seems to contradict the findings from the positivity rate ZIP code analysis. Again, it could be that while these individuals are getting tested at lower rates, they are also being more risk averse, although additional investigation is needed to better understand the behaviors of individuals in this group.

Another caveat here is that unemployed and uninsured estimates are from the ASC 2018 estimates and it is highly likely that the percentage of uninsured and unemployed individuals is mugh higher in some of the more vulnerable communities in the City. 
  
## Time Series Analysis:
In response to the City's tightening of restrictions, I wanted to project how the overall positivity rate would respond in the coming 2 weeks. In order to do this I created a number of models displayed below, as well as nearly 20 different iterations of the SARIMAX model before settling on the best performing one (see time_series_analysis.ipynb). It should be noted that I trained each model on the full set of data from 03/01/2020 to 10/20/2020 and tested each model on a 2-week period from 10/20/2020 to 11/3/2020. 

<img width="393" alt="model_metrics_updated" src="https://user-images.githubusercontent.com/64563191/99414402-c47a9100-28c4-11eb-8086-13fb3018c93e.png">

In optimizing the SARIMAX model, I shortened the training set to start on 5/15/2020 and end on 10/20/2020. I believe this removed some of the noise from the March data resulting from a lack of testing and much uncertainty around the actual number of positive cases in the City as a whole. Shortening the training set helped minimize the RMSE in the final SARIMAX model (shown below). This model incorporates five 10-day-lagged variables including temperature, precipitation, holidays, loosening restrictions, and tightening restrictions. That is, assuming the effect of gathering on a holiday will not be observed for roughly 5-10 days, I shifted the indicator variable to 10 days later to better capture the effect of a given variable in the pattern.

![final_model_train_test](https://user-images.githubusercontent.com/64563191/99413488-d6a7ff80-28c3-11eb-9073-5c39f10514ce.png)

For the final forecast, I trained the model on the training and test set (05/15/2020 - 11/03/2020) and forecasted the next two weeks' daily positivity rates. As can be seen in the chart below, it appears that the positivity rate in Chicago will continue to increase to roughly 16 percent by November 17 and likely continue on that upward path through the Holiday seeason. On November 17, however, the City implemented a City-wide lockdown advisory recommending that individuals stay at home as much as possible. It is uncertain the effect this will have on the City's positivity rate, but it's likely that the positivity rate will taper off and decrease to 5 percent in the coming weeks if City residents follow these new guidelines.

![final_projection](https://user-images.githubusercontent.com/64563191/99413490-d6a7ff80-28c3-11eb-9952-946da30646b8.png)

## Summary and Recommendatons:
Throughout the month of October, the City of Chicago experienced a substantial uptick in daily cases with the positivity rate nearing 15 percent. The regression analysis shows that this uptick in cases primarily affected Latinx communities and areas with a large percentage of essential workers (see mapping_pos_rates.ipynb and zip_code_regression_analysis.ipynb).


Testing, however, seems to be targeted primarily at whiter and younger areas surrounding the City’s universities. This could explain why the positivity rate for college-age students is lower than most. Similarly, communities with a higher percentage of uninsured or unemployed individuals are less likely to be tested as well. This could imply that individuals may not be aware of free testing available to them regardless of their insurance coverage. 

**Recommendations:**
- Work with community leaders and city stakeholders to ensure that all community residents are aware of free testing regardless of insurance coverage.
- Consider mandating regular testing for all essential and non-essential workers
- Continue stressing the importance of social distancing in mitigating the spread of the virus particularly in communities with higher positivity rates (i.e., Hispanic communities)

