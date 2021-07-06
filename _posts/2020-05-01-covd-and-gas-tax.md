---
layout: post
title: COVID-19 and Maine Gas Tax
date: 2020-05-01 11:11:22 PDT
description: Initial economic impact time series analysis in R.
---

<br> 

##### **Summary**
This report covers the attempt to estimate the effect of COVID-19 pandemic on gasoline tax revenue in Maine. Around the world, most economies have responded with lockdowns of varying degrees, in order to curtail the spread of the virus. Following this pattern, the state of Maine has imposed mandatory closures of non-essential businesses and reduced access to public areas. An inevitable outcome of this approach has been a significant reduction in traffic levels. Given that most activities require a certain amount of motor vehicle travel, these closures resulted in less distance being traveled and therefore less gasoline being purchased. For the past decade, the state has relied on a steady revenue stream from the tax on gasoline. This has accounted for nearly 70% percent of the Highway Fund and around 5% of the total tax revenue for a given fiscal[^1] year.

In order to predict the expected losses of decreased traffic volume this reported is broken up into three steps. First, a seasonal ARIMA model is used in order to forecast the general revenue trend the state would have expected to see, had there been no major shock to the economy. Second, the relationship between traffic volume and gasoline tax revenue is estimated through a linear regression model and the expected revenue is predicted using known traffic data. Finally, the entire model is tested within the last fiscal year with available data (which is fiscal year 2019). This allows for comparison of values calculated within steps one and two with actual data.

##### Step 1: Forecasting

Looking at Figure 1, which shows monthly gasoline tax revenue between 2007 and 2018, several issues need to be addressed before any modelling is to occur. Within the red boxes are monthly values that drastically diverge from the expected trend and the observed mean. This series should be considered a proxy for the real variable of interest. The actual information we could say the state is interested in is the amount of revenue generated each month. However, the state is aware only of the taxes it actually collects, when it collects them. This means that collection processes and lags between the point in time when revenue is generated and when it is collected, need to be taken into account. With this in mind, these points are deemed outliers and reverted to the mean of the remainder of the series.

<br>

*Figure 1 - Gas Tax Revenue Jan 2007 – Jun 2018*

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/gas_tax_revenue.png" data-zoomable>
    </div>
</div>

<br>

Furthermore, there is clear seasonality in the data. Each fiscal year resembles the one before. Also, there is a slight upward trend in the data: although barely discernable, it is still worth noting. An augmented Dickey-Fuller (ADF) test, one that accounts for seasonality with a trend, failed to reject the presence of unit roots with a statistic of -1.04. To detrend the data, remove the seasonal pattern and lose the unit root issue, the series was differenced monthly and seasonally. That is, each month’s revenue was subtracted from the one a month away and a year away. Running an ADF test on this series, gives the statistic of -6.79 which strongly indicates no more unit root issues. The final series, which now appears to be stationary, can be seen in Figure 2.

<br>

*Figure 2 - Cleaned-up Gas Tax Revenue Jan 2007 – Jun 2018*

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/gas_tax_revenue_clean.png" data-zoomable>
    </div>
</div>

<br>

Figure 3 shows what the autocorrelations (both partial and complete) look like for the series. Lines colored red represent the seasonal lags (at 12, 24, etc. to account for the monthly nature of the series). Looking at the complete autocorrelation plot (ACF), there is no significant tailing off within a season. However, two red lines above the line of significance suggest that it would be wise to include a seasonal autoregressive term. Looking at the partial autocorrelation plot (PACF), there is some tailing off within a season and no seasonal pattern of significance, suggesting that a moving average term should be added.

<br>

*Figure 3 - ACF (top) and PACF (bottom), seasonal lags in red*

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/acf_pacf.png" data-zoomable>
    </div>
</div>

<br>

With this in mind (and a few minor alterations around each term), the model with the lowest reported AIC and BIC is SARIMA$$(0,1,1)(1,1,0)_{12}$$ or

$$(1-\phi_1B^{12})(1-B)(1-B^{12})Y_t=(1-\theta_1B)\varepsilon_t$$

where the moving average is 1, seasonal autoregressive lag is 1 and the data has been differenced once monthly and once seasonally. As can be noted by Figure 4, residuals seem to be distributed randomly and independently. Table 1 reports the estimates of the model. Finally, Figure 5 shows what the expected forecast 12 months ahead is. This forecast covers fiscal year 2019, for which data is available.

<br>

*Figure 4 - Analysis of model residuals*

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/model_residuals.png" data-zoomable>
    </div>
</div>

_Table 1. SARIMA estimation results_

<table>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>estimate</th>
<th>std. error</th>
<th>t.statistic</th>
<th>p.value</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="span">ma1</td>
<td markdown="span">-0.8469</td>
<td markdown="span">0.0557</td>
<td markdown="span">-15.218</td>
<td markdown="span">0.0000</td>
</tr>
<tr>
<td markdown="span">sar1</td>
<td markdown="span">-0.3528</td>
<td markdown="span">0.0903</td>
<td markdown="span">-3.9064</td>
<td markdown="span">0.0002</td>
</tr>
</tbody>
</table>

*Figure 5 - Forecast for 2019 fiscal year*

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/sarima.png" data-zoomable>
    </div>
</div>

<br>

##### Step 2: Revenue vs. Traffic

Plotting traffic and revenue over time, on the same plot (as can be seen in Figure 7), reveals a few interesting things. They both follow the same seasonal pattern over time (with a month lag between the two) and both have quarterly trends that can be individually considered. First of all, the points are separated into quarterly sets and then shifted over by a month so that the highest and lowest points of each series sync up. This lag is assumed to exist because it must take the state some time (presumably a month) to collect the tax after the traffic levels have been observed.

<br>

*Figure 6 - OLS estimates*

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/ols_estimates.png" data-zoomable>
    </div>
</div>

_Table 2. OLS estimates_

<table>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>Quarter 1</th>
<th>Quarter 2</th>
<th>Quarter 3</th>
<th>Quarter 4</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="span">intercept</td>
<td markdown="span">__8.168\*\*\*__ <br> (0.667)</td>
<td markdown="span">__10.616\*\*\*__ <br> (2.63)</td>
<td markdown="span">__2.559\*__ <br> (2.93)</td>
<td markdown="span">__7.744\*__ <br> (3.689)</td>
</tr>
<tr>
<td markdown="span">log(traffic)</td>
<td markdown="span">__0.539\*\*\*__ <br> (0.042)</td>
<td markdown="span">__0.383\*\*__ <br> (0.167)</td>
<td markdown="span">__0.899\*\*\*__ <br> (0.188)</td>
<td markdown="span">__0.561\*\*__ <br> (0.234)</td>
</tr>
</tbody>
</table>

_\* - 90 % significance / \*\* - 95 % significance / \*\*\* - 99 % significance_

<br>

Now, as they are synced up, quarterly slope signs of each series are identical and therefore a straightforward linear regression should be all that is necessary. However, to further capture the possible quarterly idiosyncrasy of the data and further reduce variation, regressions are performed within each quarter. Additionally, future data for state traffic levels and tax revenue used for economic impact analysis of COVID-19 closures will concern the last quarter of fiscal year 2020, starting with April. Therefore, it seemed prudent to isolate quarterly effects in order to obtain the most accurate estimates.

Figure 6 and Table 2 show the results of the linear regression. As can be noted, the relationships are fairly straightforward and statistically significant. For each 1% increase in traffic volume, there is 54, 38, 90, and 56 percent increase in revenue for quarters 1 through 4, respectively. This data can now be used to estimate revenue from traffic. In this case, revenue is simply

<br>

_revenue = intercept + coefficient \* traffic_

<br>

and traffic volume is obtained from monthly turnpike data across Maine.

<br>

*Figure 7 - Organizing traffic (bottom) and revenue (top) data*

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/traffic_revenue_data.png" data-zoomable>
    </div>
</div>

<br>

##### Step 3: Putting it all together

This one is straightforward, as Figures 8 and 9 illustrate. Fiscal year 2019 contains three lines: actual revenue (yellow), OLS predicted revenue (green) and time series forecast of expected revenue (red). Furthermore, Table 3 shows the difference between expected and predicted revenue. For fiscal year 2019, this doesn’t provide new information, because there were no unexpected shocks, and all the models arrive at the same conclusion (mostly). However, the purpose of running the model across 2019 was only to determine its validity. Matching results were expected. Fiscal year 2020 is where it is actually going to be used to make predictions.

<br>

_Table 3. Tax revenue comparison for fiscal year 2019, in US\$_

<table>
<colgroup>
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
<col width="10%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th> </th>
<th>OLS</th>
<th>OLS</th>
<th>OLS</th>
<th>SARIMA</th>
<th>SARIMA</th>
<th>SARIMA</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="span"> </td>
<td markdown="span">Actual Data</td>
<td markdown="span">Min</td>
<td markdown="span">Average</td>
<td markdown="span">Max</td>
<td markdown="span">Min</td>
<td markdown="span">Average</td>
<td markdown="span">Max</td>
</tr>
<tr>
<td markdown="span">Quarter 1</td>
<td markdown="span">58,311,032</td>
<td markdown="span">56,724,305</td>
<td markdown="span">59,275,087</td>
<td markdown="span">61,825,870</td>
<td markdown="span">55,836,036</td>
<td markdown="span">58,001,488</td>
<td markdown="span">60,166,940</td>
</tr>
<tr>
<td markdown="span">Quarter 2</td>
<td markdown="span">51,606,718</td>
<td markdown="span">44,014,749</td>
<td markdown="span">52,863,879</td>
<td markdown="span">61,713,010</td>
<td markdown="span">49,853,379</td>
<td markdown="span">52,084,273</td>
<td markdown="span">54,315,167</td>
</tr>
<tr>
<td markdown="span">Quarter 3</td>
<td markdown="span">47,028,837</td>
<td markdown="span">40,009,428</td>
<td markdown="span">49,259,003</td>
<td markdown="span">58,508,579</td>
<td markdown="span">45,395,921</td>
<td markdown="span">47,652,095</td>
<td markdown="span">49,908,270</td>
</tr>
<tr>
<td markdown="span">Quarter 4</td>
<td markdown="span">50,225,926</td>
<td markdown="span">36,792,179</td>
<td markdown="span">48,887,326</td>
<td markdown="span">60,982,473</td>
<td markdown="span">47,774,515</td>
<td markdown="span">50,034,520</td>
<td markdown="span">52,294,526</td>
</tr>
</tbody>
</table>

<br>

*Figure 8 - Tax revenue data (yellow), OLS prediction (green), SARIMA forecast (red); shaded region represents 99% CI*

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/final_figure.png" data-zoomable>
    </div>
</div>

<br>

*Figure 9 - Tax revenue data (yellow), OLS prediction (green), SARIMA forecast (red); shaded region represents 99% CI, (close up of 2019 fiscal year)*

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/final_figure_zoom.png" data-zoomable>
    </div>
</div>

<br>

##### Appendix 1: Data Cleanup Code in R

{% highlight r linenos %} 

#-------- packages --------
library(tidyverse)
library(tidylog)
library(rebus)
library(readxl)
library(lubridate)

#-------- data and directory --------
directory <- paste0(here::here(), "/data")
setwd(directory)

tab_2020 <- read_xlsx("Maine Traffic - DOT Sensor.xlsx", sheet = "2020", skip = 2)
tab_2019 <- read_xlsx("Maine Traffic - DOT Sensor.xlsx", sheet = "2019", skip = 2)
classifications_2019 <- read_xlsx("Maine Traffic - DOT Sensor.xlsx", range = "'2019 Classifications'!A1:CA37")
classifications_2020 <- read_xlsx("Maine Traffic - DOT Sensor.xlsx", sheet = "2020 Classifications")
other_class_data <- read_xlsx("Maine Traffic - DOT Sensor.xlsx", sheet = "Other Class Data")

class <- read_csv("class temp.csv")
interchange <- read_csv("interchange temp.csv")
comparison_1 <- read_csv("comparison1 temp.csv")
comparison_2 <- read_csv("comparison2 temp.csv")

tax_revenues <- read_csv("tax_revenues.csv") %>%
  select(-X18)

unemployment_claims <- read_csv("unemployment_claims.csv") %>%
  filter(!is.na(State))
unemployment <- read_csv("unemployment.csv") 
monthly_inflation <- read_csv("monthly_inflation.csv")
fuel_efficiency <- read_csv("fuel_efficiency.csv")

#-------- cleaning tab_2019 --------
fix <- c("3/31/19",	"4/1/19",	
         "4/2/19",	"4/3/19",	
         "4/4/19",	"4/5/19",	
         "4/6/19",	"4/7/19",	
         "4/8/19",	"4/9/19",	
         "4/10/19",	"4/11/19",	
         "4/12/19",	"4/13/19") %>%
  mdy() %>% as.character()
  

dates <- tibble(date = unlist(tab_2019[1, 5:41]),
                day = colnames(tab_2019)[5:41])

dates[24:37, 1] <- fix

dates <- dates %>%
  mutate(date = ymd(date))

tab_2019 <- tab_2019[-c(1, 66, 67), ] %>%
  select(-(42:46))

for(i in 5:41){
  vector <- tab_2019[,i] %>%
    as.character() %>%
    str_split(pattern = ",") %>%
    unlist()
  
  numbers <- vector %>%
    str_detect(pattern = one_or_more(DGT)) %>%
    which()
  
  NAs <- vector %>%
    str_detect(pattern = one_or_more("N")) %>%
    which()
  
  vector[numbers] <- str_extract(vector[numbers], pattern = one_or_more(DGT))
  
  vector[NAs] <- NA
  
  vector <- as.numeric(vector)
  
  tab_2019[, i] <- vector
}

tab_2019 <- tab_2019 %>%
  pivot_longer(cols = 5:41, names_to = "day", values_to = "traffic")

tab_2019 <- left_join(tab_2019, dates, by = "day") %>%
  mutate(day = str_remove_all(tab_2019$day, 
                              pattern = "..." %R% one_or_more(DGT)))

days_short <- c("Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat")
days_long <- c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday")

for(i in seq_along(days_short)) {
  tab_2019$day[which(str_detect(tab_2019$day, 
                                pattern = days_short[i]))] <- days_long[i]
}

tab_2019$code <- str_extract_all(tab_2019$`Site Name`, pattern = SPACE %R% one_or_more(DGT) %R% END) %>% unlist %>%
  str_remove_all(pattern = " ") %>% as.numeric()

tab_2019$`Site Name` <- str_remove_all(tab_2019$`Site Name`, 
                                       pattern = SPACE %R% one_or_more(DGT) %R% END) %>% 
  str_to_title()

colnames(tab_2019) <- c("id", "town", "county", "description", "day", "traffic", "date", "code")

tab_2019 <- tab_2019 %>%
  select(id, code, county, town, description, date, day, traffic)

#-------- cleaning tab_2020 --------
fix <- c("3/17/20",	"3/18/20",	"3/19/20",	
         "3/20/20",	"3/21/20",	"3/22/20",	
         "3/23/20",	"3/24/20",	"3/25/20",	
         "3/26/20",	"3/27/20",	"3/28/20",	
         "3/29/20",	"3/30/20",	"3/31/20",	
         "4/1/20", "4/2/20", "4/3/20", 
         "4/4/20",	"4/5/20",	"4/6/20",	
         "4/7/20",	"4/8/20",	"4/9/20",	
         "4/10/20", "4/11/20") %>%
  mdy() %>% as.character()

dates <- tibble(date = unlist(tab_2020[1, 5:41]),
                day = colnames(tab_2020)[5:41])

dates[12:37, 1] <- fix

dates <- dates %>%
  mutate(date = ymd(date))

tab_2020 <- tab_2020[-c(1, 67, 68), ]

for(i in 5:41){
  vector <- tab_2020[,i] %>%
    as.character() %>%
    str_split(pattern = ",") %>%
    unlist()
  
  numbers <- vector %>%
    str_detect(pattern = one_or_more(DGT)) %>%
    which()
  
  NAs <- vector %>%
    str_detect(pattern = one_or_more("N")) %>%
    which()
  
  vector[numbers] <- str_extract(vector[numbers], pattern = one_or_more(DGT))
  
  vector[NAs] <- NA
  
  vector <- as.numeric(vector)
  
  tab_2020[, i] <- vector
}

tab_2020 <- tab_2020 %>%
  pivot_longer(cols = 5:41, names_to = "day", values_to = "traffic")

tab_2020 <- left_join(tab_2020, dates, by = "day") %>%
  mutate(day = str_remove_all(tab_2020$day, 
                              pattern = "..." %R% one_or_more(DGT)))

days_short <- c("Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat")
days_long <- c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday")

for(i in seq_along(days_short)) {
  tab_2020$day[which(str_detect(tab_2020$day, 
                                pattern = days_short[i]))] <- days_long[i]
}

tab_2020$code <- str_extract_all(tab_2020$`Site Name`, pattern = SPACE %R% one_or_more(DGT) %R% END) %>% unlist %>%
  str_remove_all(pattern = " ") %>% as.numeric()

tab_2020$`Site Name` <- str_remove_all(tab_2020$`Site Name`, 
                                       pattern = SPACE %R% one_or_more(DGT) %R% END) %>% 
  str_to_title()

colnames(tab_2020) <- c("id", "town", "county", "description", "day", "traffic", "date", "code")

tab_2020 <- tab_2020 %>%
  select(id, code, county, town, description, date, day, traffic)

#-------- cleaning classifications_2019 --------
types <- tibble(type = unlist(classifications_2019[1, -1]),
                place = colnames(classifications_2019)[-1])

types$type[which(str_detect(types$type, pattern = "Pass"))] <- "Pass. Veh"
types$type[which(str_detect(types$type, pattern = "SU"))] <- "SU Trucks"

classifications_2019 <- classifications_2019[-1, ] 

classifications_2019$day <- str_extract_all(classifications_2019$...1,
                                            pattern = START %R% 
                                              one_or_more(WRD) %R% 
                                              SPACE) %>% 
  unlist() %>% 
  str_remove(pattern = " ")

classifications_2019$...1 <- str_remove_all(classifications_2019$...1, 
                                            pattern = START %R% 
                                              one_or_more(WRD) %R% 
                                              SPACE)

classifications_2019$...1 <- dmy(classifications_2019$...1)

for(i in 2:79){
  vector <- classifications_2019[,i] %>%
    as.character() %>%
    str_split(pattern = ",") %>%
    unlist()
  
  numbers <- vector %>%
    str_detect(pattern = one_or_more(DGT)) %>%
    which()
  
  NAs <- vector %>%
    str_detect(pattern = one_or_more("N")) %>%
    which()
  
  vector[numbers] <- str_extract(vector[numbers], pattern = one_or_more(DGT))
  
  vector[NAs] <- NA
  
  vector <- as.numeric(vector)
  
  classifications_2019[, i] <- vector
}

index <- str_detect(colnames(classifications_2019), pattern = DOT %R% DOT %R% DOT) %>%
  which()

names <- colnames(classifications_2019)[-index] %>% head(n = -1)

for(i in seq_along(names)) {
  number <- which(colnames(classifications_2019) == names[i])
  colnames(classifications_2019)[number + 1] <- paste0(names[i], "_1")
  colnames(classifications_2019)[number + 2] <- paste0(names[i], "_2")
}

colnames(classifications_2019)[1] <- "date"

types$place <- colnames(classifications_2019)[-c(1, 80)]

classifications_2019 <- pivot_longer(classifications_2019, cols = 2:79, names_to = "place", values_to = "traffic")

classifications_2019 <- left_join(classifications_2019, types, by = "place")

classifications_2019$place <- str_remove_all(classifications_2019$place, pattern = "_" %R% DGT)

classifications_2019 <- separate(classifications_2019, col = place, into = c("town", "description"), sep = "\\(")

classifications_2019$description <- str_remove_all(classifications_2019$description, pattern = "\\)")

#-------- cleaning classifications_2020 --------
types <- tibble(type = unlist(classifications_2020[1, -1]),
                place = colnames(classifications_2020)[-1])

types$type[which(str_detect(types$type, pattern = "Pass"))] <- "Pass. Veh"
types$type[which(str_detect(types$type, pattern = "SU"))] <- "SU Trucks"

classifications_2020 <- classifications_2020[-1, ] 

classifications_2020$day <- str_extract_all(classifications_2020$...1,
                                            pattern = START %R% 
                                              one_or_more(WRD) %R% 
                                              SPACE) %>% 
  unlist() %>% 
  str_remove(pattern = " ")

classifications_2020$...1 <- str_remove_all(classifications_2020$...1, 
                                            pattern = START %R% 
                                              one_or_more(WRD) %R% 
                                              SPACE)

classifications_2020$...1 <- dmy(classifications_2020$...1)

for(i in 2:79){
  vector <- classifications_2020[,i] %>%
    as.character() %>%
    str_split(pattern = ",") %>%
    unlist()
  
  numbers <- vector %>%
    str_detect(pattern = one_or_more(DGT)) %>%
    which()
  
  NAs <- vector %>%
    str_detect(pattern = one_or_more("N")) %>%
    which()
  
  vector[numbers] <- str_extract(vector[numbers], pattern = one_or_more(DGT))
  
  vector[NAs] <- NA
  
  vector <- as.numeric(vector)
  
  classifications_2020[, i] <- vector
}

index <- str_detect(colnames(classifications_2020), pattern = DOT %R% DOT %R% DOT) %>%
  which()

names <- colnames(classifications_2020)[-index] %>% head(n = -1)

for(i in seq_along(names)) {
  number <- which(colnames(classifications_2020) == names[i])
  colnames(classifications_2020)[number + 1] <- paste0(names[i], "_1")
  colnames(classifications_2020)[number + 2] <- paste0(names[i], "_2")
}

colnames(classifications_2020)[1] <- "date"

types$place <- colnames(classifications_2020)[-c(1, 80)]

classifications_2020 <- pivot_longer(classifications_2020, cols = 2:79, names_to = "place", values_to = "traffic")

classifications_2020 <- left_join(classifications_2020, types, by = "place")

classifications_2020$place <- str_remove_all(classifications_2020$place, pattern = "_" %R% DGT)

classifications_2020 <- separate(classifications_2020, col = place, into = c("town", "description"), sep = "\\(")

classifications_2020$description <- str_remove_all(classifications_2020$description, pattern = "\\)")

#-------- cleaning other_class_data --------
types <- tibble(type = unlist(other_class_data[1, -1]),
                place = colnames(other_class_data)[-1])

other_class_data <- other_class_data[-c(1, 9, 10, 11, 19), ] 

other_class_data$day <- str_extract_all(other_class_data$...1,
                                            pattern = START %R% 
                                              one_or_more(WRD) %R% 
                                              SPACE) %>% 
  unlist() %>% 
  str_remove(pattern = " ")

other_class_data$...1 <- str_remove_all(other_class_data$...1, 
                                            pattern = START %R% 
                                              one_or_more(WRD) %R% 
                                              SPACE)

other_class_data$...1 <- dmy(other_class_data$...1)

for(i in 2:55){
  vector <- other_class_data[,i] %>%
    as.character() %>%
    str_split(pattern = ",") %>%
    unlist()
  
  numbers <- vector %>%
    str_detect(pattern = one_or_more(DGT)) %>%
    which()
  
  NAs <- vector %>%
    str_detect(pattern = one_or_more("N")) %>%
    which()
  
  vector[numbers] <- str_extract(vector[numbers], pattern = one_or_more(DGT))
  
  vector[NAs] <- NA
  
  vector <- as.numeric(vector)
  
  other_class_data[, i] <- vector
}

index <- str_detect(colnames(other_class_data), pattern = DOT %R% DOT %R% DOT) %>%
  which()

names <- colnames(other_class_data)[-index] %>% head(n = -1)

for(i in seq_along(names)) {
  number <- which(colnames(other_class_data) == names[i])
  colnames(other_class_data)[number + 1] <- paste0(names[i], "_1")
  colnames(other_class_data)[number + 2] <- paste0(names[i], "_2")
}

colnames(other_class_data)[1] <- "date"

types$place <- colnames(other_class_data)[-c(1, 56)]

other_class_data <- pivot_longer(other_class_data, cols = 2:55, names_to = "place", values_to = "traffic")

other_class_data <- left_join(other_class_data, types, by = "place")

other_class_data$place <- str_remove_all(other_class_data$place, pattern = "_" %R% DGT)

other_class_data <- separate(other_class_data, col = place, into = c("town", "description"), sep = "\\(")

other_class_data$description <- str_remove_all(other_class_data$description, pattern = "\\)")

#-------- cleaning class data --------
class <- class %>%
  filter(!is.na(Vehicle)) %>%
  pivot_longer(cols = 2:159, names_to = "date", values_to = "transactions")

class <- class %>%
  mutate(date = mdy(date))

#-------- cleaning interchange data --------
interchange$X7 <- str_extract_all(interchange$plaza, 
                                  pattern = START %R% one_or_more(DGT)) %>% 
  unlist() %>% as.numeric()

interchange$plaza <- str_remove_all(interchange$plaza, 
                                    pattern = START %R% one_or_more(DGT) %R% SPACE)

interchange <- interchange %>%
  select(year, code = X7, plaza, revenue, transactions, 
         average_fare, average_daily_transactions = average_daily_trans)

#-------- cleaning comparison data --------
comparison_1 <- comparison_1 %>%
  select(date, revenue, type, subtype) %>%
  filter(!is.na(date)) %>%
  mutate(date = mdy(date))

comparison_2 <- comparison_2 %>%
  select(date, transactions, vehicle) %>%
  filter(!is.na(date)) %>%
  mutate(date = mdy(date))

#-------- cleaning tax revenue data --------
tax_revenues <- tax_revenues %>%
  pivot_longer(cols = 6:17, names_to = "month", values_to = "revenue") %>%
  mutate(year = fiscal_year)

change <- which(tax_revenues$month %in% c("July", "August", 
                                           "September", "October", 
                                           "November", "December"))

tax_revenues$year[change] <- tax_revenues$year[change] - 1

tax_revenues <- tax_revenues %>%
  mutate(date = ymd(paste0(year, "-", month, "-", "01")),
         quarter = recode(month, "July" = "Quarter 1",
                                 "August" = "Quarter 1",
                                 "September" = "Quarter 1",
                                 "October" = "Quarter 2",
                                 "November" = "Quarter 2",
                                 "December" = "Quarter 2",
                                 "January" = "Quarter 3",
                                 "February" = "Quarter 3",
                                 "March" = "Quarter 3",
                                 "April" = "Quarter 4",
                                 "May" = "Quarter 4",
                                 "June" = "Quarter 4")) %>%
  select(date, fiscal_year, quarter, month, fund, category, code, tax, revenue)

#-------- cleaning unemployment claims data --------
unemployment_claims <- unemployment_claims %>%
  mutate(date = mdy(`Filed week ended`),
         month = month(date),
         year = year(date))%>%
  group_by(year, month) %>%
  summarise(claims = sum(`Initial Claims`)) %>%
  ungroup() %>%
  mutate(date = ymd(paste0(year, "-", month, "-01"))) %>%
  select(date, claims)

#-------- cleaning unemployment data --------
unemployment <- unemployment %>%
  pivot_longer(cols = 3:14, names_to = "month", values_to = "value") %>% 
  mutate(date = ymd(paste0(year, "-", month, "-01"))) %>%
  filter(!is.na(value)) %>%
  select(date, category, value)
  

#-------- cleaning inflation data --------
monthly_inflation <- monthly_inflation %>%
  filter(!is.na(date)) %>%
  select(date, inflation) %>%
  mutate(date = mdy(date))

#-------- merging data --------
traffic_summary <- bind_rows(tab_2019, tab_2020)

traffic_breakdown <- bind_rows(classifications_2019, 
                                    classifications_2020,
                                    other_class_data)

rm(classifications_2019, classifications_2020, other_class_data, dates,
   types, tab_2019, tab_2020, days_long, days_short, i, fix, index, names,
   NAs, number, numbers, vector, change)

#-------- saving data --------
save(traffic_summary, file = "traffic_summary.RData")
save(traffic_breakdown, file = "traffic_breakdown.RData")
save(class, file = "class.RData")
save(interchange, file = "interchange.RData")
save(comparison_1, file = "comparison1.RData")
save(comparison_2, file = "comparison2.RData")
save(tax_revenues, file = "tax_revenues.RData")
save(unemployment_claims, file = "unemployment_claims.RData")
save(monthly_inflation, file = "monthly_inflation.RData")
save(unemployment, file = "unemployment.RData")
save(fuel_efficiency, file = "fuel_efficiency.RData")

write_csv(traffic_summary, "traffic_summary.csv")
write_csv(traffic_breakdown, "traffic_breakdown.csv")
write_csv(class, "class.csv")
write_csv(interchange, "interchange.csv")
write_csv(comparison_1, "comparison1.csv")
write_csv(comparison_2, "comparison2.csv")

{% endhighlight %}

##### Appendix 1: Data Analysis Code in R

{% highlight r linenos %} 

#-------- packages --------
library(tidyverse) 
library(ggplot2) 
library(lubridate)
library(directlabels) 
library(ggrepel) 
library(ggthemes) 
library(scales) 
library(xts)
library(astsa) 
library(forecast) 
library(tseries) 
library(broom)

#-------- data and directory --------
directory <- paste0(here::here(), "/data") 
setwd(directory)
load("traffic_breakdown.RData") 
load("traffic_summary.RData") 
load("class.RData") 
load("comparison1.RData") 
load("comparison2.RData") 
load("interchange.RData") 
load("tax_revenues.RData") 
load("unemployment.RData")

directory <- paste0(here::here(), "/output") setwd(directory)
options(scipen = "100")

#-------- functions --------
linear_reg <- function(data, formula){ 
    lm(formula, data)
}

#-------- analysis --------
gas <- tax_revenues %>%
    filter(revenue != 0, !is.na(revenue)) %>%
    filter(code == "0321") %>%
    filter(date < "2020-01-01", date >= "2007-01-01") %>% filter(fiscal_year < 2019) %>%
    group_by(date) %>%
    summarise(revenue = sum(revenue, na.rm = TRUE)) %>% 
    ungroup()

gas <- gas %>%
    mutate(revenue = ifelse(revenue > 22000000, NA, revenue),
    revenue = ifelse(revenue < 13000000, NA, revenue),
    revenue = ifelse(is.na(revenue), mean(revenue, na.rm = T), revenue))

gas <- xts(gas$revenue, order.by = gas$date)
lgas = log(gas)
dgas <- na.locf(diff(gas), fromLast = T)
ddgas <- na.locf(diff(dgas, lag = 12), fromLast = T)
data <- ddgas length(data)
adf.test(data)
plot(data)
acf2(data, max.lag = 100, main = '')

sarima(gas, 0, 1, 1, 1, 1, 0, 12)
sarima.for(gas, n.ahead = 12, 0, 1, 1, 1, 1, 0, 12)
auto.arima(gas, trace = TRUE, ic = "bic", seasonal = TRUE, seasonal.test = "ocsb")

ols <- tax_revenues %>%
    filter(code == "0321", fiscal_year %in% c(2016:2019)) %>% filter(revenue != 0, !is.na(revenue)) %>%
    filter(date < "2020-04-01") %>%
    left_join(comparison_2 %>%
                group_by(date) %>%
                summarise(traffic = sum(transactions, na.rm = TRUE)) %>%
                mutate(month = month(date) + 1,
                       year = year(date),
                       year = if_else(month > 12, year + 1, year),
                       month = if_else(month > 12, month - 12, month), date_shifted = ymd(paste0(year, "-", month, "-01"))),
    by = "date")

plot <- ols %>%
    ggplot(aes(x = date, y = revenue, color = quarter)) +
    geom_line() +
    geom_point() +
    geom_line(aes(y = lag(traffic, 1))) +
    geom_point(aes(y = lag(traffic, 1))) +
    facet_wrap(. ~ fiscal_year, scales = "free_x", nrow = 1) + scale_color_ptol() +
    theme_linedraw() +
    scale_y_continuous(label = comma) +
    theme(axis.text.x = element_blank(),
    axis.title.x = element_blank(), axis.text.y = element_blank(), axis.title.y = element_blank(), legend.position = "none")

ggsave("plot.png", plot, height = 3, width = 7)

ols %>%
    ggplot(aes(x = log(lag(traffic,1)), y = log(revenue), colour =
    quarter)) +
    geom_point() +
    stat_smooth(method = "lm") + facet_wrap(. ~ quarter, scales = "free") + scale_color_ptol() +
    theme_par() +
    xlab("log(traffic)") + ylab("log(revenue)") + theme(legend.position = "none")

ols %>%
    ggplot(aes(x = lag(traffic,1), y = revenue, colour = quarter)) + geom_point() +
    stat_smooth(method = "lm") +
    scale_y_continuous(label = comma) + scale_x_continuous(label = comma) +
    facet_wrap(. ~ quarter, scales = "free") + scale_color_ptol() +
    theme_par() +
    xlab("traffic") +
    ylab("revenue") + theme(legend.position = "none")

ols_output <- map(c("Quarter 1", "Quarter 2", "Quarter 3", "Quarter 4"), 
                  function(x) {
                        bind_rows( ols%>%
                        filter(quarter == x) %>%
                        linear_reg(log(revenue) ~ log(lag(traffic, 1))) %>% tidy() %>%
                        mutate(type = "log",
                        quarter = x), ols %>%
                        filter(quarter == x) %>% linear_reg(revenue ~ lag(traffic, 1)) %>% tidy() %>%
                        mutate(type = "normal",
                        quarter = x) )
                    }) %>% data.table::rbindlist()

ols_output_log <- ols_output %>% filter(type == "log")
ols_output <- ols_output %>% filter(type == "normal")
ols_output <- left_join( ols_output %>%
    filter(term == "lag(traffic, 1)") %>% rename(slope = estimate,
    slope_se = std.error) %>% select(slope, slope_se, quarter),
    ols_output %>%
    filter(term != "lag(traffic, 1)") %>% rename(intercept = estimate,
    intercept_se = std.error) %>% select(intercept, intercept_se, quarter),
    by = "quarter")

expectation <- sarima.for(gas, n.ahead = 12, 6, 0, 1, 1, 0, 0, 12) 
expectation <- tibble(expectation$pred,
                      expectation$se, 
                      tax_revenues %>%
                        filter(fiscal_year == 2019) %>%
                        select(date) %>% 
                        unique()
                    ) 

colnames(expectation) <- c("expectation", "error", "date")

prediction <- tax_revenues %>% 
                filter(code == "0321") %>% 
                filter(revenue != 0, !is.na(revenue)) %>% 
                filter(date < "2020-04-01") %>% 
                left_join(comparison_2 %>%
                            group_by(date) %>%
                            summarise(traffic = sum(transactions, na.rm = TRUE)) %>%
                            mutate(month = month(date) + 1, 
                                   year = year(date),
                                   year = if_else(month > 12, year + 1, year),
                                   month = if_else(month > 12, month - 12, month), 
                                   date_shifted = ymd(paste0(year, "-", month, "-01"))),
                by = "date") %>%
                left_join(ols_output, by = "quarter") %>%
                left_join(expectation, by = "date") %>%
                mutate(traffic = lag(traffic, 1),
                       prediction = ifelse(fiscal_year == 2019, intercept + slope*traffic, NA),
                       prediction_upper = ifelse(fiscal_year == 2019, (intercept + intercept_se) + (slope + slope_se) * traffic, NA), 
                       prediction_lower = ifelse(fiscal_year == 2019, (intercept- intercept_se) + (slope - slope_se) * traffic, NA), 
                       prediction_error = revenue - prediction, 
                       expectation_upper = expectation + error, 
                       expectation_lower = expectation - error)

prediction %>%
    filter(fiscal_year == 2019) %>%
    ggplot(aes(x = date)) +
    geom_point(aes(y = revenue), colour = "#DDAA33") + 
    geom_line(aes(y = revenue), colour = "#DDAA33", size=1.5) +
    geom_point(aes(y = expectation), colour = "#BB5566") + 
    geom_line(aes(y = expectation), colour = "#BB5566", size=1.5) +
    geom_point(aes(y = prediction), colour = "#228833") + 
    geom_line(aes(y = prediction), colour = "#228833", size=1.5) +
    geom_ribbon(aes(ymin = prediction_lower, 
                    ymax =prediction_upper), 
                fill = "#228833", 
                colour = "transparent", 
                alpha = 0.1) +
    geom_ribbon(aes(ymin = expectation_lower, 
                    ymax = expectation_upper),
                fill = "#BB5566", 
                colour = "transparent", 
                alpha = 0.3) +
    scale_y_continuous(label = comma) + 
    theme_par() +
    theme(axis.title.x = element_blank())
    
prediction %>%
    filter(fiscal_year == 2019) %>% 
    group_by(quarter) %>%
    summarize(revenue = sum(revenue, na.rm = T),
            prediction = sum(prediction, na.rm = T), 
            prediction_upper = sum(prediction_upper, na.rm = T), 
            prediciton_lower = sum(prediction_lower, na.rm = T), 
            expectation = sum(expectation, na.rm = T), 
            expectation_upper = sum(expectation_upper, na.rm = T),
            expectation_lower = sum(expectation_lower, na.rm = T)) %>% 
    View()

{% endhighlight %}

[^1]: in Maine, fiscal year spans July of last year to June of current year. That is, fiscal year 2018 starts in July 2017 and ends in June 2018.
