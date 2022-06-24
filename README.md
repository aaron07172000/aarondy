# aarondy
introduction to final project
Installing and loading required packages
install.packages("rlang")
## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.2'
## (as 'lib' is unspecified)
install.packages("tidymodels")
## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.2'
## (as 'lib' is unspecified)
install.packages("tidyverse")
## Installing package into '/cloud/lib/x86_64-pc-linux-gnu-library/4.2'
## (as 'lib' is unspecified)
library(tidymodels)
## ── Attaching packages ────────────────────────────────────── tidymodels 0.2.0 ──
## ✔ broom        0.8.0     ✔ recipes      0.2.0
## ✔ dials        1.0.0     ✔ rsample      0.1.1
## ✔ dplyr        1.0.9     ✔ tibble       3.1.7
## ✔ ggplot2      3.3.6     ✔ tidyr        1.2.0
## ✔ infer        1.0.2     ✔ tune         0.2.0
## ✔ modeldata    0.1.1     ✔ workflows    0.2.6
## ✔ parsnip      0.2.1     ✔ workflowsets 0.2.1
## ✔ purrr        0.3.4     ✔ yardstick    1.0.0
## ── Conflicts ───────────────────────────────────────── tidymodels_conflicts() ──
## ✖ purrr::discard() masks scales::discard()
## ✖ dplyr::filter()  masks stats::filter()
## ✖ dplyr::lag()     masks stats::lag()
## ✖ recipes::step()  masks stats::step()
## • Dig deeper into tidy modeling with R at https://www.tmwr.org
library(tidyverse)
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
## ✔ readr   2.1.2     ✔ forcats 0.5.1
## ✔ stringr 1.4.0
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ readr::col_factor() masks scales::col_factor()
## ✖ purrr::discard()    masks scales::discard()
## ✖ dplyr::filter()     masks stats::filter()
## ✖ stringr::fixed()    masks recipes::fixed()
## ✖ dplyr::lag()        masks stats::lag()
## ✖ readr::spec()       masks yardstick::spec()
Downloading the NOAA weather dataset
For this assignment we analysed data from a subset of the NOAA JFK weather dataset. This sample dataset contained 9 columns called:

DATE
HOURLYDewPointTempF
HOURLYRelativeHumidity
HOURLYDRYBULBTEMPF
HOURLYWETBULBTEMPF
HOURLYPrecip
HOURLYWindSpeed
HOURLYSeaLevelPressure
HOURLYStationPressure
Dataset glossary:

DATE: Date (day of month given in two digits) and Time of observation
HOURLYDewPointTempF: This is the dew point temperature. It is given here in whole degrees Fahrenheit.4
HOURLYRelativeHumidity: This is the relative humidity given to the nearest whole percentage.* HOURLYDRYBULBTEMPF: This is the dry-bulb temperature and is commonly used as the standard air temperature reported. It is given here in whole degrees Fahrenheit.
HOURLYWETBULBTEMPF: This is the wet-bulb temperature. It is given here in whole degrees Fahrenheit.
HOURLYPrecip: Amount of precipitation in inches to hundredths over the past hour. For certain automated stations, precipitation will be reported at sub-hourly intervals (e.g. every 15 or 20 minutes) as an accumulated amount of all precipitation within the preceding hour. A “T” indicates a trace amount of precipitation.
HOURLYWindSpeed: Speed of the wind at the time of observation given in miles per hour (mph). HOURLYSeaLevelPressure: Sea level pressure given in inches of Mercury (in Hg).
HOURLYStationPressure: Atmospheric pressure observed at the station during the time of observation. Given in inches of Mercury (in Hg).
Here I downloaded the dataset:

url <- 'https://dax-cdn.cdn.appdomain.cloud/dax-noaa-weather-data-jfk-airport/1.1.4/noaa-weather-sample-data.tar.gz'

download.file(url, destfile = "noaa-weather-sample-data.tar.gz")

untar("noaa-weather-sample-data.tar.gz", tar = "internal")
## Warning in untar2(tarfile, files, list, exdir, restore_times): using pax
## extended headers
NOAA_weather <- read_csv("noaa-weather-sample-data/jfk_weather_sample.csv", 
                                                            col_types= cols("DATE" = col_number(),
                                                                           "HOURLYDewPointTempF" = col_number()))
To get a look of the dataset I was to analyse, I performed the head and glimpse function

head(NOAA_weather)
## # A tibble: 6 × 9
##    DATE HOURLYDewPointTempF HOURLYRelativeHum… HOURLYDRYBULBTE… HOURLYWETBULBTE…
##   <dbl>               <dbl>              <dbl>            <dbl>            <dbl>
## 1  2015                  60                 46               83               68
## 2  2016                  34                 48               53               44
## 3  2013                  33                 89               36               35
## 4  2011                  18                 48               36               30
## 5  2015                  27                 61               39               34
## 6  2013                  35                 79               41               38
## # … with 4 more variables: HOURLYPrecip <chr>, HOURLYWindSpeed <dbl>,
## #   HOURLYSeaLevelPressure <dbl>, HOURLYStationPressure <dbl>
glimpse(NOAA_weather)
## Rows: 5,727
## Columns: 9
## $ DATE                   <dbl> 2015, 2016, 2013, 2011, 2015, 2013, 2014, 2014,…
## $ HOURLYDewPointTempF    <dbl> 60, 34, 33, 18, 27, 35, 4, 14, 51, 71, 76, 19, …
## $ HOURLYRelativeHumidity <dbl> 46, 48, 89, 48, 61, 79, 51, 65, 90, 94, 79, 37,…
## $ HOURLYDRYBULBTEMPF     <dbl> 83, 53, 36, 36, 39, 41, 19, 24, 54, 73, 83, 44,…
## $ HOURLYWETBULBTEMPF     <dbl> 68, 44, 35, 30, 34, 38, 15, 21, 52, 72, 78, 35,…
## $ HOURLYPrecip           <chr> "0.00", "0.00", "0.00", "0.00", "T", "0.00", "0…
## $ HOURLYWindSpeed        <dbl> 13, 6, 13, 14, 11, 6, 0, 11, 11, 5, 21, 7, 17, …
## $ HOURLYSeaLevelPressure <dbl> 30.01, 30.05, 30.14, 29.82, NA, 29.94, 30.42, 3…
## $ HOURLYStationPressure  <dbl> 29.99, 30.03, 30.12, 29.80, 30.50, 29.92, 30.40…
Subsetting the data
In this assignment, I subsetted the data to be able to inspect the columns. I stored the columns HOURLYRelativeHumidity, HOURLYDRYBULBTEMPF, HOURLYPrecip, HOURLYWindSpeed, HOURLYStationPressure into a new dataframe.

subset_NOAA_weather <- select(NOAA_weather, c("HOURLYRelativeHumidity",
"HOURLYDRYBULBTEMPF",
"HOURLYPrecip",
"HOURLYWindSpeed",
"HOURLYStationPressure"))
I then performed the head function on the new dataframe to get a look at the data

head(subset_NOAA_weather, n = 10)
## # A tibble: 10 × 5
##    HOURLYRelativeHumidity HOURLYDRYBULBTEMPF HOURLYPrecip HOURLYWindSpeed
##                     <dbl>              <dbl> <chr>                  <dbl>
##  1                     46                 83 0.00                      13
##  2                     48                 53 0.00                       6
##  3                     89                 36 0.00                      13
##  4                     48                 36 0.00                      14
##  5                     61                 39 T                         11
##  6                     79                 41 0.00                       6
##  7                     51                 19 0.00                       0
##  8                     65                 24 0.00                      11
##  9                     90                 54 0.06                      11
## 10                     94                 73 <NA>                       5
## # … with 1 more variable: HOURLYStationPressure <dbl>
Cleaning the HOURLYPrecip column
From the glimpse it was clear that the HOURLYPrecip column contained NA and T values. Additionally some values ended with an s.

The T value specifies trace amounts of precipitation (meaning essentially no precipitation), NA means not available, and is commonly used to denote a missing value. The ‘s’ at the end of some means that the precipitation was snow.

In order to see all of the aformentioned values the unique function was performed on the HOURLYPrecip column

unique(subset_NOAA_weather$HOURLYPrecip)
##  [1] "0.00"  "T"     "0.06"  NA      "0.03"  "0.02"  "0.08"  "0.01"  "0.07" 
## [10] "0.16"  "0.09"  "0.22"  "0.02s" "0.24"  "0.18"  "0.05"  "0.04"  "0.09s"
## [19] "0.11"  "0.14"  "0.25"  "0.10"  "0.01s" "0.58"  "0.12"  "0.13"  "0.46" 
## [28] "1.07"  "1.19"  "0.34"  "0.20"  "0.36s" "0.42"  "0.17"  "0.27"  "0.35" 
## [37] "0.31"  "0.33"  "0.23"  "0.26"  "0.28"  "0.75"  "0.19"  "0.36"  "0.03s"
## [46] "0.07s" "0.54"  "0.59"  "0.21"
As the ‘T’ and ‘s’ values would impact model development the values were removed

subset_NOAA_weather$HOURLYPrecip[subset_NOAA_weather$HOURLYPrecip == "T"] <- 0

subset_NOAA_weather$HOURLYPrecip <- str_remove(subset_NOAA_weather$HOURLYPrecip, pattern = "s$")
NOAA_weather$HOURLYPrecip <- subset_NOAA_weather$HOURLYPrecip
I then checked the dataset to ensure the ‘T’ and ‘s’ values had been removed.

unique(NOAA_weather$HOURLYPrecip)
##  [1] "0.00" "0"    "0.06" NA     "0.03" "0.02" "0.08" "0.01" "0.07" "0.16"
## [11] "0.09" "0.22" "0.24" "0.18" "0.05" "0.04" "0.11" "0.14" "0.25" "0.10"
## [21] "0.58" "0.12" "0.13" "0.46" "1.07" "1.19" "0.34" "0.20" "0.36" "0.42"
## [31] "0.17" "0.27" "0.35" "0.31" "0.33" "0.23" "0.26" "0.28" "0.75" "0.19"
## [41] "0.54" "0.59" "0.21"
Checking coloumn types
glimpse(NOAA_weather)
## Rows: 5,727
## Columns: 9
## $ DATE                   <dbl> 2015, 2016, 2013, 2011, 2015, 2013, 2014, 2014,…
## $ HOURLYDewPointTempF    <dbl> 60, 34, 33, 18, 27, 35, 4, 14, 51, 71, 76, 19, …
## $ HOURLYRelativeHumidity <dbl> 46, 48, 89, 48, 61, 79, 51, 65, 90, 94, 79, 37,…
## $ HOURLYDRYBULBTEMPF     <dbl> 83, 53, 36, 36, 39, 41, 19, 24, 54, 73, 83, 44,…
## $ HOURLYWETBULBTEMPF     <dbl> 68, 44, 35, 30, 34, 38, 15, 21, 52, 72, 78, 35,…
## $ HOURLYPrecip           <chr> "0.00", "0.00", "0.00", "0.00", "0", "0.00", "0…
## $ HOURLYWindSpeed        <dbl> 13, 6, 13, 14, 11, 6, 0, 11, 11, 5, 21, 7, 17, …
## $ HOURLYSeaLevelPressure <dbl> 30.01, 30.05, 30.14, 29.82, NA, 29.94, 30.42, 3…
## $ HOURLYStationPressure  <dbl> 29.99, 30.03, 30.12, 29.80, 30.50, 29.92, 30.40…
From the glimpse you can see the HOURLYPrecip is of type char when it should be a double. So I converted HOURLYPrecip to type dbl and then checked the dataset to ensure the conversion had occured.

NOAA_weather$HOURLYPrecip <- (as.numeric(NOAA_weather$HOURLYPrecip))
glimpse(NOAA_weather)
## Rows: 5,727
## Columns: 9
## $ DATE                   <dbl> 2015, 2016, 2013, 2011, 2015, 2013, 2014, 2014,…
## $ HOURLYDewPointTempF    <dbl> 60, 34, 33, 18, 27, 35, 4, 14, 51, 71, 76, 19, …
## $ HOURLYRelativeHumidity <dbl> 46, 48, 89, 48, 61, 79, 51, 65, 90, 94, 79, 37,…
## $ HOURLYDRYBULBTEMPF     <dbl> 83, 53, 36, 36, 39, 41, 19, 24, 54, 73, 83, 44,…
## $ HOURLYWETBULBTEMPF     <dbl> 68, 44, 35, 30, 34, 38, 15, 21, 52, 72, 78, 35,…
## $ HOURLYPrecip           <dbl> 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00,…
## $ HOURLYWindSpeed        <dbl> 13, 6, 13, 14, 11, 6, 0, 11, 11, 5, 21, 7, 17, …
## $ HOURLYSeaLevelPressure <dbl> 30.01, 30.05, 30.14, 29.82, NA, 29.94, 30.42, 3…
## $ HOURLYStationPressure  <dbl> 29.99, 30.03, 30.12, 29.80, 30.50, 29.92, 30.40…
Renaming columns
The below columns were renamed and the dataframe was stored in a new variable.

‘HOURLYRelativeHumidity’ to ‘relative_humidity’
‘HOURLYDRYBULBTEMPF’ to ‘dry_bulb_temp_f’
‘HOURLYPrecip’ to ‘precip’
‘HOURLYWindSpeed’ to ‘wind_speed’
‘HOURLYStationPressure’ to ‘station_pressure’
NOAA_weather2 <- NOAA_weather %>%
                rename(relative_humidity = HOURLYRelativeHumidity,
                       dry_bulb_temp_f = HOURLYDRYBULBTEMPF,
                       precip = HOURLYPrecip, 
                       wind_speed = HOURLYWindSpeed,
                       station_pressure = HOURLYStationPressure)
The new dataframe

glimpse(NOAA_weather2)
## Rows: 5,727
## Columns: 9
## $ DATE                   <dbl> 2015, 2016, 2013, 2011, 2015, 2013, 2014, 2014,…
## $ HOURLYDewPointTempF    <dbl> 60, 34, 33, 18, 27, 35, 4, 14, 51, 71, 76, 19, …
## $ relative_humidity      <dbl> 46, 48, 89, 48, 61, 79, 51, 65, 90, 94, 79, 37,…
## $ dry_bulb_temp_f        <dbl> 83, 53, 36, 36, 39, 41, 19, 24, 54, 73, 83, 44,…
## $ HOURLYWETBULBTEMPF     <dbl> 68, 44, 35, 30, 34, 38, 15, 21, 52, 72, 78, 35,…
## $ precip                 <dbl> 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00,…
## $ wind_speed             <dbl> 13, 6, 13, 14, 11, 6, 0, 11, 11, 5, 21, 7, 17, …
## $ HOURLYSeaLevelPressure <dbl> 30.01, 30.05, 30.14, 29.82, NA, 29.94, 30.42, 3…
## $ station_pressure       <dbl> 29.99, 30.03, 30.12, 29.80, 30.50, 29.92, 30.40…
Exploratory Data Analysis
The next stage was to split the data into training and testing set in a 80:20 ratio.

set.seed(1234)
NOAA_weather_split <- initial_split(NOAA_weather2, prop = 0.8)
train_data <- training(NOAA_weather_split)
test_data <- testing(NOAA_weather_split)
The next task was to visualise variables relative_humidity, dry_bulb_temp_f, precip, wind_speed, station_pressure. Using the training data set. I did this using a simple histogram

ggplot(train_data, aes(x = relative_humidity))+
geom_histogram(color = "darkblue", fill = "lightblue")
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
## Warning: Removed 114 rows containing non-finite values (stat_bin).


ggplot(train_data, aes(x = dry_bulb_temp_f))+
geom_histogram(color = "darkblue", fill = "lightblue")
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
## Warning: Removed 114 rows containing non-finite values (stat_bin).


ggplot(train_data, aes(x = precip))+
geom_histogram(color = "darkblue", fill = "lightblue")
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
## Warning: Removed 1349 rows containing non-finite values (stat_bin).


ggplot(train_data, aes(x = wind_speed))+
geom_histogram(color = "darkblue", fill = "lightblue")
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
## Warning: Removed 119 rows containing non-finite values (stat_bin).


ggplot(train_data, aes(x = station_pressure))+
geom_histogram(color = "darkblue", fill = "lightblue")
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
## Warning: Removed 121 rows containing non-finite values (stat_bin).


Linear Regression
After visualising the data I then created a simple linear regression model using precip as the response variable and each of relative_humidity, dry_bulb_temp_f,wind_speed or station_pressure as the predictor variable. As the goal of this assignment was to predict precipitation based on certian variables. The models were then visualised using a scatter plot.

linear_model_humidity <- lm(precip ~ relative_humidity, data = train_data)

ggplot(train_data, aes(x = relative_humidity, y = precip)) +
geom_point() +
stat_smooth(method = "lm", col = "purple")
## `geom_smooth()` using formula 'y ~ x'
## Warning: Removed 1349 rows containing non-finite values (stat_smooth).
## Warning: Removed 1349 rows containing missing values (geom_point).


linear_model_drybulbtempf <- lm(precip ~ dry_bulb_temp_f, data = train_data)

ggplot(train_data, aes(x = dry_bulb_temp_f, y = precip)) +
geom_point() +
stat_smooth(method = "lm", col = "purple")
## `geom_smooth()` using formula 'y ~ x'
## Warning: Removed 1349 rows containing non-finite values (stat_smooth).
## Warning: Removed 1349 rows containing missing values (geom_point).


linear_model_windspeed <- lm(precip ~ wind_speed, data = train_data)
ggplot(train_data, aes(x = wind_speed, y = precip)) +
geom_point() +
stat_smooth(method = "lm", col = "purple")
## `geom_smooth()` using formula 'y ~ x'
## Warning: Removed 1349 rows containing non-finite values (stat_smooth).
## Warning: Removed 1349 rows containing missing values (geom_point).


linear_model_stationpressure <- lm(precip ~ station_pressure, data = train_data)
ggplot(train_data, aes(x = station_pressure, y = precip)) +
geom_point() +
stat_smooth(method = "lm", col = "purple")
## `geom_smooth()` using formula 'y ~ x'
## Warning: Removed 1353 rows containing non-finite values (stat_smooth).
## Warning: Removed 1353 rows containing missing values (geom_point).


I then outputted a summary of the different models

summary(linear_model_humidity)
## 
## Call:
## lm(formula = precip ~ relative_humidity, data = train_data)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -0.02044 -0.01107 -0.00496  0.00197  1.17119 
## 
## Coefficients:
##                     Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       -2.030e-02  2.598e-03  -7.816 7.34e-15 ***
## relative_humidity  4.074e-04  3.775e-05  10.794  < 2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.04265 on 3230 degrees of freedom
##   (1349 observations deleted due to missingness)
## Multiple R-squared:  0.03481,    Adjusted R-squared:  0.03451 
## F-statistic: 116.5 on 1 and 3230 DF,  p-value: < 2.2e-16
summary(linear_model_drybulbtempf)
## 
## Call:
## lm(formula = precip ~ dry_bulb_temp_f, data = train_data)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -0.00795 -0.00698 -0.00646 -0.00598  1.18305 
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)  
## (Intercept)     4.752e-03  2.575e-03   1.845   0.0651 .
## dry_bulb_temp_f 3.226e-05  4.435e-05   0.727   0.4671  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.04341 on 3230 degrees of freedom
##   (1349 observations deleted due to missingness)
## Multiple R-squared:  0.0001638,  Adjusted R-squared:  -0.0001458 
## F-statistic: 0.5291 on 1 and 3230 DF,  p-value: 0.4671
summary(linear_model_windspeed)
## 
## Call:
## lm(formula = precip ~ wind_speed, data = train_data)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -0.01482 -0.00758 -0.00568 -0.00453  1.17937 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)   
## (Intercept) 0.0022478  0.0015811   1.422  0.15523   
## wind_speed  0.0003811  0.0001230   3.099  0.00196 **
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.04334 on 3230 degrees of freedom
##   (1349 observations deleted due to missingness)
## Multiple R-squared:  0.002965,   Adjusted R-squared:  0.002657 
## F-statistic: 9.606 on 1 and 3230 DF,  p-value: 0.001956
summary(linear_model_stationpressure)
## 
## Call:
## lm(formula = precip ~ station_pressure, data = train_data)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -0.02585 -0.00912 -0.00573 -0.00167  1.17658 
## 
## Coefficients:
##                   Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       0.684392   0.094889   7.213 6.81e-13 ***
## station_pressure -0.022599   0.003163  -7.144 1.12e-12 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.04309 on 3226 degrees of freedom
##   (1353 observations deleted due to missingness)
## Multiple R-squared:  0.01557,    Adjusted R-squared:  0.01527 
## F-statistic: 51.04 on 1 and 3226 DF,  p-value: 1.115e-12
Improving the model
After creating a simple linear regression model the next task was to improve the model by adding new features.

Based on the results of the previous models. relative_humidity appeared to have the strongest predictior of precip. However, the results were non-linear and so I created a 10th order polynomial regression model.

polynomial_relativehumidity <- lm(precip ~ poly(relative_humidity, 10, raw = TRUE), data = train_data)

ggplot(data = train_data, aes(relative_humidity, precip))+
       geom_point() +
       geom_smooth(method = "lm", formula = y ~ poly(x,10))
## Warning: Removed 1349 rows containing non-finite values (stat_smooth).
## Warning: Removed 1349 rows containing missing values (geom_point).


Showing a summary of the 10th order polynomial regression model

summary(polynomial_relativehumidity)
## 
## Call:
## lm(formula = precip ~ poly(relative_humidity, 10, raw = TRUE), 
##     data = train_data)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -0.03602 -0.00498 -0.00008  0.00005  1.15495 
## 
## Coefficients:
##                                             Estimate Std. Error t value
## (Intercept)                                8.840e-02  3.103e+00   0.028
## poly(relative_humidity, 10, raw = TRUE)1  -2.057e-02  7.657e-01  -0.027
## poly(relative_humidity, 10, raw = TRUE)2   1.983e-03  8.034e-02   0.025
## poly(relative_humidity, 10, raw = TRUE)3  -1.043e-04  4.744e-03  -0.022
## poly(relative_humidity, 10, raw = TRUE)4   3.309e-06  1.754e-04   0.019
## poly(relative_humidity, 10, raw = TRUE)5  -6.551e-08  4.263e-06  -0.015
## poly(relative_humidity, 10, raw = TRUE)6   7.989e-10  6.921e-08   0.012
## poly(relative_humidity, 10, raw = TRUE)7  -5.494e-12  7.437e-10  -0.007
## poly(relative_humidity, 10, raw = TRUE)8   1.442e-14  5.078e-12   0.003
## poly(relative_humidity, 10, raw = TRUE)9   4.421e-17  1.995e-14   0.002
## poly(relative_humidity, 10, raw = TRUE)10 -2.718e-19  3.432e-17  -0.008
##                                           Pr(>|t|)
## (Intercept)                                  0.977
## poly(relative_humidity, 10, raw = TRUE)1     0.979
## poly(relative_humidity, 10, raw = TRUE)2     0.980
## poly(relative_humidity, 10, raw = TRUE)3     0.982
## poly(relative_humidity, 10, raw = TRUE)4     0.985
## poly(relative_humidity, 10, raw = TRUE)5     0.988
## poly(relative_humidity, 10, raw = TRUE)6     0.991
## poly(relative_humidity, 10, raw = TRUE)7     0.994
## poly(relative_humidity, 10, raw = TRUE)8     0.998
## poly(relative_humidity, 10, raw = TRUE)9     0.998
## poly(relative_humidity, 10, raw = TRUE)10    0.994
## 
## Residual standard error: 0.04218 on 3221 degrees of freedom
##   (1349 observations deleted due to missingness)
## Multiple R-squared:  0.05828,    Adjusted R-squared:  0.05536 
## F-statistic: 19.93 on 10 and 3221 DF,  p-value: < 2.2e-16
I also created a multiple linear regression model that included all the predictor variables to attempt to create a more accurate predictor variable by adding more factors.

mlr_all <- lm(precip ~ relative_humidity + dry_bulb_temp_f + wind_speed + station_pressure, data = train_data)

ggplot(train_data, aes(x = relative_humidity + dry_bulb_temp_f + wind_speed + station_pressure, y = precip)) +
geom_point() +
stat_smooth(method = "lm", col = "purple")
## `geom_smooth()` using formula 'y ~ x'
## Warning: Removed 1353 rows containing non-finite values (stat_smooth).
## Warning: Removed 1353 rows containing missing values (geom_point).


showing a summary of the MLR model

summary(mlr_all)
## 
## Call:
## lm(formula = precip ~ relative_humidity + dry_bulb_temp_f + wind_speed + 
##     station_pressure, data = train_data)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -0.03672 -0.01091 -0.00438  0.00221  1.16126 
## 
## Coefficients:
##                     Estimate Std. Error t value Pr(>|t|)    
## (Intercept)        2.651e-01  1.082e-01   2.451  0.01431 *  
## relative_humidity  4.250e-04  4.118e-05  10.321  < 2e-16 ***
## dry_bulb_temp_f   -6.467e-06  4.524e-05  -0.143  0.88635    
## wind_speed         6.045e-04  1.365e-04   4.429 9.76e-06 ***
## station_pressure  -9.767e-03  3.534e-03  -2.764  0.00574 ** 
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.04241 on 3223 degrees of freedom
##   (1353 observations deleted due to missingness)
## Multiple R-squared:  0.0475, Adjusted R-squared:  0.04632 
## F-statistic: 40.18 on 4 and 3223 DF,  p-value: < 2.2e-16
Which model was better?
The final task in the assignment was to determine which model was better by evaluating the models on the test data. To do this I used the metric R-squared. As this shows the proportion of the variation in the dependent variable that is predictable from the independent variable(s).

To do this I first had to create a matrix that contained the predicted and true values. I first did this with the multiple linear regression model and the polynomial regression model.

MLR model

MLR <- linear_reg() %>%
  set_engine(engine = "lm")

train_fit <- MLR %>% 
    fit(precip ~ relative_humidity + dry_bulb_temp_f + wind_speed + station_pressure, data = train_data)

MLR_results <- train_fit %>%
  predict(new_data = test_data) %>%
  mutate(truth = test_data$precip)
Taking a look at the matrix

head(MLR_results)
## # A tibble: 6 × 2
##       .pred truth
##       <dbl> <dbl>
## 1 -0.000983     0
## 2  0.00977      0
## 3  0.00275      0
## 4 -0.0107       0
## 5  0.00598     NA
## 6 -0.00363     NA
Polynomial model

polynomial <- linear_reg () %>%
    set_engine(engine = "lm")
    
train_fit2 <- polynomial %>% 
      fit(precip ~ poly(relative_humidity, 10, raw = TRUE), data = train_data)

poly_relative_humidity <- train_fit2 %>%
  predict(new_data = test_data) %>%
  mutate(truth = test_data$precip)
Taking a look at the matrix

head(poly_relative_humidity)
## # A tibble: 6 × 2
##        .pred truth
##        <dbl> <dbl>
## 1  0.0000583     0
## 2  0.00498       0
## 3 -0.0000692     0
## 4 -0.0000790     0
## 5  0.000746     NA
## 6 -0.0000763    NA
Creating a matrix to present the best model
After calculating the predicted and true values for both models I then calculated their R squared.

rsq_MLR <- rsq(MLR_results, truth = truth, estimate = .pred)

rsq_MLR
## # A tibble: 1 × 3
##   .metric .estimator .estimate
##   <chr>   <chr>          <dbl>
## 1 rsq     standard       0.110
rsq_poly_humiditity <- rsq(poly_relative_humidity, truth = truth, estimate = .pred)

rsq_poly_humiditity
## # A tibble: 1 × 3
##   .metric .estimator .estimate
##   <chr>   <chr>          <dbl>
## 1 rsq     standard       0.112
I then fitted the values into a dataframe for comparison including the R-squared calculated based on the training data.

model_names <- c("Relative_humidity_poly", "MLR_all")
train_error <- c("0.05536", "0.04632")
test_error  <- c("0.06346145", "0.06314133")

comparison_df <- data.frame(model_names, train_error, test_error)
Comparison dataframe

comparison_df
##              model_names train_error test_error
## 1 Relative_humidity_poly     0.05536 0.06346145
## 2                MLR_all     0.04632 0.06314133
