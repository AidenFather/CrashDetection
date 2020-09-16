# Automated Road Crash Detection  

## Introduction

Prompt crash detection is essential to reduce incident congestion and increase road safety. To that end, I developed a cost-effective and reliable road crash detection model using XGBoost. The model was trained with the integrated dataset containing traffic, weather, road geometry, solar position, and population density. These data have both a direct and indirect impact on causing crashes, and most of the data are downloaded from the state DOT website. A total of six years crash and non-crash records were processed and used for the model train and test. The dataset for the model train and test includes 30 features and 54,643 observations. 

## Data 

We obtained and processed six years of crash data (occurred on NJ Route 3) and crash related data, including INRIX link speed, road vertical geometry, weather, and population density.
Below shows the download link.

- NJ road crash data [(download)](https://www.state.nj.us/transportation/refdata/accident/rawdata01-current.shtm)
- INRIX probe vehicle data for TMC links [(download)](https://ritis.org/login?r=Lw==)
- Road geometry [(download)](https://www.state.nj.us/transportation/refdata/sldiag/)
- Population density [(download)](https://nj.gov/health/fhs/primarycare/documents/Rural%20NJ%20density2015-revised%20municpalities.pdf)
- Weather data (Teterborough Station) [(download)](https://drive.google.com/open?id=1_OBGpLXJrTiC8SVJtSA7xWXSbxTZNlYz&authuser=kk64%40njit.edu&usp=drive_fs) 
- Road Traffic Volume (NJCMS NJ3 only) [(download)](https://drive.google.com/open?id=1Qmf8xlhdomH4PCHPdXWXEroE3_OB3StN&authuser=kk64%40njit.edu&usp=drive_fs)




