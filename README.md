# Road Crash Detection  

## Introduction

Prompt crash detection is essential to reduce incident congestion and increase road safety. Conventional road crash detections have been performed with various Intelligent Transportation Systems (ITS) devices. However, the deployment of ITS devices along a corridor requires tremendous amount of money and time. To that end, I developed a cost-effective and reliable road crash detection model using XGBoost. The model was trained with the integrated dataset containing traffic, weather, road geometry, solar position, and population density. These data have both a direct and indirect impact on causing crashes, and most of the data are downloaded from the state DOT website. A total of six years crash and non-crash records were processed and used for the model train and test. The dataset for the model train and test includes 30 features and 54,643 observations. 

## Data 

We obtained and processed six years of crash data (occurred on NJ Route 3) and crash related data, including INRIX link speed, road geometry, weather, and population density. 
Below shows the download link.

- NJ road crash data [(download)](https://www.state.nj.us/transportation/refdata/accident/rawdata01-current.shtm)
- INRIX probe vehicle data for TMC links [(download)](https://ritis.org/login?r=Lw==)
- Road geometry [(download)](https://www.state.nj.us/transportation/refdata/sldiag/)
- NJ Route 3 vertical profile [(download)](https://drive.google.com/open?id=1-WhEbYTr6nix6FYc3N61KozXpRO4Cfgs&authuser=kk64%40njit.edu&usp=drive_fs)
- Population density [(download)](https://nj.gov/health/fhs/primarycare/documents/Rural%20NJ%20density2015-revised%20municpalities.pdf)
- Weather data (Teterborough Station) [(download)](https://drive.google.com/open?id=1_OBGpLXJrTiC8SVJtSA7xWXSbxTZNlYz&authuser=kk64%40njit.edu&usp=drive_fs) 
- Road Traffic Volume (NJCMS NJ3 only) [(download)](https://drive.google.com/open?id=1Qmf8xlhdomH4PCHPdXWXEroE3_OB3StN&authuser=kk64%40njit.edu&usp=drive_fs)

Note that the weather data was pulled from the [NOAA](https://www.ncdc.noaa.gov/data-access/quick-links) and the vertical profile was obtained from the Google Earth.


## Data Process & Integration

Data processing of the collected data is shown in [NJ3_Crash_Data_Pre-processing.ipynb](NJ3_Crash_Data_Pre-processing.ipynb).
Then, the processed datasets were merged together by time and location, which is shown in [Join_CrashData_RoadLink_INRIX-TMC.ipynb](Join_CrashData_RoadLink_INRIX-TMC.ipynb)

Overall, the features used for the model training are summarized below.

| Category | Feature | Type | Description | 
|---:|---|---|---|
| Traffic | VOL | Continuous | Car volume (veh/hr) |
|         | TRK | Continuous | Truck volume (veh/hr) |
|         | spd |	Continuous | Speed (mph) |
|         | avg_spd_5min | Continuous	| Avg. speed for the previous 5-min interval (mph) |
|   	  | avg_spd_10min |	Continuous	| Avg. speed for the previous 10-min interval (mph) |
|   	  | avg_spd_15min |	Continuous	| Avg. speed for the previous 15-min interval (mph) |
|		  | dn1_spd_diff |	Continuous	| Speed difference between crash link and downstream link (mph) |
|		  | up1_spd_diff |	Continuous	| Speed difference between crash link and    upstream link (mph) |
|		  | Std¬_5min |	Continuous	| STD of speed for the previous 5-min (mph) |
|		  | Std_10min |	Continuous	| STD of speed for the previous 10-min (mph) |
|		  | Std_15min |	Continuous	| STD of speed for the previous 15-min (mph) |
|		  | cv_15min | Continuous | CV of speed for the previous 5-min |
|		  | cv_10min | Continuous | CV of speed for the previous 10-min |
|		  | cv_5min  | Continuous | CV of speed for the previous 15-min |
|		  | CAPLINK	| Continuous | Link Capacity (veh/hr) |
|	      | avg_spd_diff_up1dn1_5min | Continuous| Avg. speed difference between upstream and downstream link for the previous 5-min (mph)|
| Geometry | MP | Continuous | Milepost |
|		     |Lanes |	Categorical |	Number of Lanes |
|		     | RAMP_prox |	Categorical	| Ramp Proximity |
|		     | Grade |	Continuous	| Vertical Road Grade (%) |
|		     | RAMP_shop |	Categorical	| Shopping Mall Entrance Existence (y/n) |
| Weather	 |	Temp | Continuous |	Temperature (F)|
|		     | Precip. | Continuous	| Precipitation Depth (inch) |
|		     | Snow D |	Continuous	| Snow Depth (inch) |
| Solar	| solar_altitude |	Continuous	| Angular Solar Elevation (degree) |
|		     | solar_azimuth |	Continuous	| Angular Solar Azimuth (degree) |
| Time     | Month | Categorical	| Month |
|			 | hour |	Categorical	| Hour |
|			 | Weekday	| Categorical |	Day of Week |
| Demographic | Pop_Dense | Continuous | Town Population Density (person/mi^{2}) |


## Model 

We used [XGBoost](http://xgboost.readthedocs.io/en/latest/) library for the supervised machine
learning model. The model was trained using the following tuned parameters.

- `max_depth` : 6.0
- `min_child_weight`: 5.0
- `reg_lambda`: 1.0
- `reg_alpha` : 0.0
- `scale_pos_weight`: 1.0
- `eval_metric`: auc
- `eta`: 0.5

The model train and test is shown in [XGBoostModeling.ipynb](XGBoostModeling.ipynb)

## HeatMap
<svg width="100" height="100" xmlns="heatmap2014_time.html">
<foreignObject width="100" height="100">
    <div xmlns="http://www.w3.org/1999/xhtml">
        <ul>
            <li>text</li>
        </ul>
        <!-- Other embed HTML element/text into SVG -->
    </div>
</foreignObject>
</svg>

## SHAP Analysis

With the feature importance analysis, we have more insight into what features contributed, and on what scale, to mapping the test data to an output. A limitation of the feature importance plot is that it does not provide insight into the interactions between features, and it does not show how the results and features are correlated (e.g., negatively or positively). To address the shortcomings of the feature importance analysis, SHAP (Shapley Additive exPlanations) was adopted to interpret the output of the trained model. 
Below shows the primary features that affect positively (or negatively) on crash detection.

![SHAP Summary Plot](https://nycdsa-blog-files.s3.us-east-2.amazonaws.com/2020/09/chris-kitae-kim/shap3-131271-hrBQBOGK.png)

## Notebooks
- NJ3_Crash_Data_Pre-processing.ipynb
- Join_CrashData_RoadLink_INRIX-TMC.ipynb
- XGBoostModeling.ipynb


