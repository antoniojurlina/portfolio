---
layout: post
title: Predictive Power - Kriging and Machine Learning
date: 2020-12-04 09:37:17 PDT
description: Comparing the predictive power of kriging and Random Forests in R.
---

<br> 

##### 1. Objective

The purpose of this project is to assess the predictive power of kriging (along with a generalized additive model) relative to that of a simple machine learning method like Random Forests, with ordinary least squares serving as the simplest (and most biased) approach. Furthermore, this project is specifically interested in the effect geographic location has on expected earnings for cab drivers in Manhattan. **If drivers pick passengers up in downtown Manhattan, they will earn a higher wage on average, everything else equal, implying that geographic latitude is the most relevant factor when determining where to start**. This is not a project that seeks to determine which specific variable contributes the most (on average with everything else held equal) to the final fares earned by cab drivers. Rather, it seeks to discover relevant relationship between latitude, longitude and fares collected. All other (known) effects are going to be accounted for and stripped away.

<br>

*Figure 1 - Spatial distribution of data*

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/spatial_distribution.png" data-zoomable>
    </div>
</div>

_Table 1. First 6 (out of 50,000) rows of the data_

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
<col width="10%" />
<col width="10%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>log(total earnings)</th>
<th>longitude</th>
<th>latitude</th>
<th>passenger count</th>
<th>trip distance</th>
<th>wday</th>
<th>hour</th>
<th>month</th>
<th>geometry</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="span">1</td>
<td markdown="span">3.23</td>
<td markdown="span">-74</td>
<td markdown="span">40.7</td>
<td markdown="span">1</td>
<td markdown="span">5.8</td>
<td markdown="span">Wed</td>
<td markdown="span">0</td>
<td markdown="span">May</td>
<td markdown="span">(-73.98457 40.71474)</td>
</tr>
<tr>
<td markdown="span">2</td>
<td markdown="span">1.7</td>
<td markdown="span">-74</td>
<td markdown="span">40.7</td>
<td markdown="span">1</td>
<td markdown="span">0.75</td>
<td markdown="span">Thu</td>
<td markdown="span">12</td>
<td markdown="span">Aug</td>
<td markdown="span">(-73.99466 40.74018)</td>
</tr>
<tr>
<td markdown="span">3</td>
<td markdown="span">2.2</td>
<td markdown="span">-74</td>
<td markdown="span">40.8</td>
<td markdown="span">1</td>
<td markdown="span">0.6</td>
<td markdown="span">Thu</td>
<td markdown="span">9</td>
<td markdown="span">Feb</td>
<td markdown="span">(-73.96581 40.75857)</td>
</tr>
<tr>
<td markdown="span">4</td>
<td markdown="span">2.01</td>
<td markdown="span">-74</td>
<td markdown="span">40.8</td>
<td markdown="span">2</td>
<td markdown="span">0.9</td>
<td markdown="span">Sat</td>
<td markdown="span">20</td>
<td markdown="span">Feb</td>
<td markdown="span">(-73.9655 40.79076)</td>
</tr>
<tr>
<td markdown="span">5</td>
<td markdown="span">2.56</td>
<td markdown="span">-74</td>
<td markdown="span">40.7</td>
<td markdown="span">6</td>
<td markdown="span">2.8</td>
<td markdown="span">Sun</td>
<td markdown="span">2</td>
<td markdown="span">Oct</td>
<td markdown="span">(-73.9968 40.74702)</td>
</tr>
<tr>
<td markdown="span">6</td>
<td markdown="span">2.31</td>
<td markdown="span">-74</td>
<td markdown="span">40.8</td>
<td markdown="span">1</td>
<td markdown="span">1.83</td>
<td markdown="span">Mon</td>
<td markdown="span">1</td>
<td markdown="span">Feb</td>
<td markdown="span">(-73.9706 40.76213)</td>
</tr>
</tbody>
</table>

<br>

##### 2. Data

The data set used for this project comes from the “FOILing NYC’s Taxi Trip Data” project[^1]. Chris Whong published 20 gigabytes of NYC Taxi data which includes fares, tips, pickup and drop-off latitudes and longitudes, medallion IDs, passenger numbers, trip durations, and more (Table 1). The sheer size of the data set (more than a 160 million observations over 2013 and 2014) meant that it would be computationally too expensive trying to process it locally. Therefore, a subset of 50,000 observations was randomly sampled, with each month of each year providing a roughly equal share of data points. This hopefully preserved any temporal effects that the data might have. Relevant visualization statistics are included in Figures 1 and 2. There was no missing data and trips with zero total fares earned were excluded (as they represent extremely unlikely and illegal scenarios usually).

<br>

_Table 2. Summary statistics of non-categorical variables_

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>log(total earnings)</th>
<th>passenger count</th>
<th>trip distance</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="span">Min</td>
<td markdown="span">1.099</td>
<td markdown="span">1</td>
<td markdown="span">0</td>
</tr>
<tr>
<td markdown="span">1st Quartile</td>
<td markdown="span">2.079</td>
<td markdown="span">1</td>
<td markdown="span">1</td>
</tr>
<tr>
<td markdown="span">Median</td>
<td markdown="span">2.398</td>
<td markdown="span">1</td>
<td markdown="span">1.735</td>
</tr>
<tr>
<td markdown="span">Mean</td>
<td markdown="span">2.447</td>
<td markdown="span">1.717</td>
<td markdown="span">2.545</td>
</tr>
<tr>
<td markdown="span">3rd Quartile</td>
<td markdown="span">2.755</td>
<td markdown="span">2</td>
<td markdown="span">2.978</td>
</tr>
<tr>
<td markdown="span">Max</td>
<td markdown="span">5.08</td>
<td markdown="span">6</td>
<td markdown="span">53</td>
</tr>
</tbody>
</table>

*Figure 2 - Temporal breakdown of the dependent variable*

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/temporal_breakdown.png" data-zoomable>
    </div>
</div>

<br>

##### 3. Methods

A simple OLS approach, in which the variable of interest is the log of total earnings (sum of the fare, tip, surcharge and tax), is the baseline approach of this project. Independent variables are pickup latitude, pickup longitude, trip distance, and number of passengers. Next, a GAM is estimated over the same basic formula, but with smoothing applied to latitude, longitude and trip distance (with an assumption that the exact shape of the effect these variables have on final earnings is unknown). The use of the GAM (to be followed with kriging of residuals) comes from a very similar idea Gámez et al.[^2] had in their work on estimating housing prices in Albacete by incorporating the neighborhood effects through spatial autocorrelation estimation and subsequent kriging of GAM residuals. In Manhattan, the assumption is that downtown pickups result in higher earnings. This indicates a certain weight neighboring points will have on each other as an isotropic spatial autocorrelation process. Therefore, the residuals of the GAM are expected to be biased, and kriging will be performed to account for and fix this. Kriging itself has a random component to it, as the underlying Gaussian process is used to interpolate predictions[^3] [^4] [^5], which is what inspired the final aspect of this project, Random Forests.

$$log(Y)=\alpha * X$$ 

_Note: $$\alpha$$ is a single-column matrix of parameter values to be estimated, X is a n:1+m matrix of independent variables where n is the number of observations in data and m is the number of variables_

Much like kriging, Random Forest models (as the name implies) rely on an underlying random process and are used in estimating spatial models and making predictions[^6] [^7]. In this project, a Random Forest model is used to estimate Equation 1, by growing 500 random decision trees across the data space and averaging them out into a prediction set. For the purposes of training the three models and subsequently testing them, the data is split into two subsets: first one contains a random subset of 75% of the data for training the models, while the second contains the remaining 25% for testing the predictions and estimating root mean square errors (Figure 3). These two datasets are used for the OLS, the GAM and kriging combination (with a spherical effect variogram model of the spatial dependence), and the Random Forest.

<br>

*Figure 3 - Test vs. train data set comparison*

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/test_train.png" data-zoomable>
    </div>
</div>

<br>

##### 4. Results and Discussion

Residuals of the OLS and GAM processes were tested for residual normality, with both rejecting the null hypotheses (Table 3). After kriging of the residuals from the GAM was performed, to account for spatial dependence, initial estimates were adjusted with kriging results and after performing the same tests, residuals testing failed to reject the normality hypothesis (Table 3). Final estimates are shown in Table 4. Additionally, the variogram model for the residuals of the GAM is shown in Figure 4. Reduction of the data set down to a smaller subsample, as well as the narrow nature of Manhattan preventing higher horizontal variation in the data, is a possible explanation for the pronounced nugget effect with the cyclical trailing-off pattern in the variogram.

<br>

_Table 3. Tests for residual normality_

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th>model</th>
<th>statistic</th>
<th>p.value</th>
<th>test</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="span">OLS</td>
<td markdown="span">80898</td>
<td markdown="span">0.000</td>
<td markdown="span">Jarque Berra</td>
</tr>
<tr>
<td markdown="span">GAM</td>
<td markdown="span">7195.7</td>
<td markdown="span">0.013</td>
<td markdown="span">Jarque Berra</td>
</tr>
<tr>
<td markdown="span">GAM+Krige</td>
<td markdown="span">1889.1</td>
<td markdown="span">0.135</td>
<td markdown="span">Jarque Berra</td>
</tr>
<tr>
<td markdown="span">OLS</td>
<td markdown="span">0.88984</td>
<td markdown="span">0.000</td>
<td markdown="span">Shapiro-Wilk</td>
</tr>
<tr>
<td markdown="span">GAM</td>
<td markdown="span">0.91051</td>
<td markdown="span">0.014</td>
<td markdown="span">Shapiro-Wilk</td>
</tr>
<tr>
<td markdown="span">GAM+Krige</td>
<td markdown="span">1.0134</td>
<td markdown="span">0.091</td>
<td markdown="span">Shapiro-Wilk</td>
</tr>
</tbody>
</table>

*Figure 4 - Spherical variogram fit on GAM residuals*

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/variogram.png" data-zoomable>
    </div>
</div>

<br>

Looking at Table 4, having completed all the necessary tests, it can be noted that between two coordinate points, latitude is the relevant one in predicting total wage earned from taxi rides. By no means are these the most important factors, but the original hypothesis remains unrejected with these results. There is a strong case to be made that being uptown versus downtown is what matters more than being on the west or east side of the island. The exact effect of the relationship is quite small in the end, and not worth elaborating on more, as the goal of this project has been accomplished.

<br>

_Table 4. Model estimates_

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
<col width="10%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>OLS</th>
<th>OLS</th>
<th>OLS</th>
<th>OLS</th>
<th>GAM</th>
<th>GAM</th>
<th>GAM</th>
<th>GAM</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="span">term</td>
<td markdown="span">estimate</td>
<td markdown="span">std.error</td>
<td markdown="span">statistic</td>
<td markdown="span">p.value</td>
<td markdown="span">estimate</td>
<td markdown="span">std.error</td>
<td markdown="span">statistic</td>
<td markdown="span">p.value</td>
</tr>
<tr>
<td markdown="span">intercept</td>
<td markdown="span">-17</td>
<td markdown="span">66.4</td>
<td markdown="span">-0.255</td>
<td markdown="span">0.798</td>
<td markdown="span">2.340</td>
<td markdown="span">0.014</td>
<td markdown="span">169.866</td>
<td markdown="span">0.000</td>
</tr>
<tr>
<td markdown="span">pickup longitude</td>
<td markdown="span">1.01</td>
<td markdown="span">0.678</td>
<td markdown="span">-0.921</td>
<td markdown="span">0.357</td>
<td markdown="span">1.38</td>
<td markdown="span">1.673</td>
<td markdown="span">2.744</td>
<td markdown="span">0.134</td>
</tr>
<tr>
<td markdown="span">pickup latitude</td>
<td markdown="span">-0.669</td>
<td markdown="span">0.519</td>
<td markdown="span">-1.29</td>
<td markdown="span">0.185</td>
<td markdown="span">-0.564</td>
<td markdown="span">0.321</td>
<td markdown="span">0.086</td>
<td markdown="span">0.002</td>
</tr>
<tr>
<td markdown="span">passenger count</td>
<td markdown="span">0.010</td>
<td markdown="span">0.006</td>
<td markdown="span">1.68</td>
<td markdown="span">0.093</td>
<td markdown="span">-0.001</td>
<td markdown="span">0.004</td>
<td markdown="span">-0.193</td>
<td markdown="span">0.847</td>
</tr>
<tr>
<td markdown="span">trip distance</td>
<td markdown="span">0.148</td>
<td markdown="span">0.003</td>
<td markdown="span">51.5</td>
<td markdown="span">0.000</td>
<td markdown="span">7.664</td>
<td markdown="span">8.511</td>
<td markdown="span">933.442</td>
<td markdown="span">0.000</td>
</tr>
</tbody>
</table>

_Table 5. Predictive and explanatory power_

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th>Model</th>
<th>RMSE</th>
<th>% Variance Explained</th>
</tr>
</thead>
<tbody>
<tr>
<td markdown="span">OLS</td>
<td markdown="span">0.318</td>
<td markdown="span">71.60</td>
</tr>
<tr>
<td markdown="span">GAM+Krige</td>
<td markdown="span">0.225</td>
<td markdown="span">85.1</td>
</tr>
<tr>
<td markdown="span">RandomForest</td>
<td markdown="span">0.252</td>
<td markdown="span">81.31</td>
</tr>
</tbody>
</table>

<br>

When it comes to predictions, using the testing subset of the model data, we can see that moving from the OLS, over the GAM, to the Random Forest, there is a decreasing error in being able to predict where highest total wages will be earned (Figure 4; Table 5). It is important to note that the GAM/Krige combination explains most of the variance in the data and has best prediction rates, albeit very close to the Random Forest (see Figure 5 and Appendix for visualizations). The expectation for the project was that the Random Forest model would contain significantly better prediction rates. Kriging is a powerful (and computation heavy) tool for interpolating a random space realization into a usable set of predictions while maintaining the ability to give plausible reasoning behind starting assumptions. While it did outperform the simple machine learning technique, it should be noted that it is computationally much heavier. Were I to use the entire data set with over a 100 million observations, Random Forest prediction rate is still high enough that I would chose it over the computationally demanding kriging process. 

<br>

*Figure 5 - Maps* <br>
*From left to right: Actual data, GAM+Krige predictions, RandomForest predictions*

<div class="row-grid">
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/map_actual.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/map_krige.png" data-zoomable>
    </div>
    <div class="column-grid">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/map_randomForest.png" data-zoomable>
    </div>
</div>

##### 5. GitHub repository

For data, code, and similar projects, visit [https://github.com/antoniojurlina/spatial_analysis](https://github.com/antoniojurlina/spatial_analysis/tree/main/final%20project).

##### Appendix 1: Distributions

<br>

*GAM+Krige prediction distribution (red) vs. actual data (blue)*

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ site.baseurl }}/images/predictions_distribution.png" data-zoomable>
    </div>
</div>

<br>

##### Appendix 2: R Code

{% highlight r linenos %} 

#-------- packages --------
library(tidyverse)
library(ggthemes)
library(spdep)
library(ggmap)
library(viridis)
library(lubridate)
library(gstat)
library(sp)
library(sf)
library(lmtest)
library(tseries)
library(broom)
library(mgcv)
library(randomForest)

#-------- data and directory --------
paste0(here::here(), "/final project/data") %>% setwd()

fares <- read_csv("fares.csv")
trips <- read_csv("trips.csv")

taxi_data <- trips %>%
	left_join(fares, by = c("medallion", "hack_license", "vendor_id", "pickup_datetime")) %>%
	select(-hack_license, -vendor_id, -rate_code, -store_and_fwd_flag) %>%
	filter(total_amount > 0) %>%
	mutate(total_amount = log(total_amount)) %>%
	filter(pickup_latitude >= 40.70, pickup_latitude <= 40.83,
				 pickup_longitude >= -74.025, pickup_longitude <= -73.93) %>%
	mutate(hour = hour(pickup_datetime),
				 wday = wday(pickup_datetime, label = TRUE),
				 month = month(pickup_datetime, label = TRUE)) %>%
	select(total_amount, pickup_longitude, pickup_latitude, passenger_count, trip_distance, wday, hour, month)

sample_rows <- sample(nrow(taxi_data), 0.75 * nrow(taxi_data))

train <- taxi_data[sample_rows, ]
test <- taxi_data[-sample_rows, ]

train_sf <- SpatialPointsDataFrame(coords = train[, 2:3], data = train,
																	 proj4string = CRS("+proj=longlat +datum=WGS84 +ellps=WGS84 +towgs84=0,0,0"))
test_sf <- SpatialPointsDataFrame(coords = test[, 2:3], data = test,
																	proj4string = CRS("+proj=longlat +datum=WGS84 +ellps=WGS84 +towgs84=0,0,0"))

train_sf <- st_as_sf(train_sf)
test_sf <- st_as_sf(test_sf)

manhattan <- readRDS("manhattan.rds")
#-------- visualization and summary--------
ggmap(manhattan, darken = 0.5) +
	scale_fill_viridis(option = 'plasma') +
	geom_bin2d(data = taxi_data, aes(pickup_longitude, pickup_latitude), bins = 50, alpha = 0.6) +
	labs(x = "longitude",
			 y = "latitude",
			 fill = "count")

taxi_data %>%
	ggplot(aes(hour, total_amount, color = wday)) +
	geom_boxplot() +
	facet_wrap(~month) +
	theme_linedraw()

bind_rows(test %>%
						mutate(type = "test"),
					train %>%
						mutate(type = "train")) %>%
	ggplot(aes(type, total_amount)) +
	geom_boxplot(fill = "gold", color = "black") +
	stat_summary(fun=mean, geom="point", shape = 21, size = 3, fill = "white") +
	theme_par() +
	theme(axis.title.x = element_blank())

#-------- OLS --------
formula <- as.formula(total_amount ~ pickup_longitude + pickup_latitude + passenger_count + trip_distance + wday + hour + month)

model <- glm(formula, family = "gaussian", train)

test$pred <- predict(model, newdata = test)

test %>%
	mutate(resid = total_amount - pred) %>%
	summarize(rmse = sqrt(mean(resid^2))) -> rmse_ols

#-------- GAM --------
gam <- gam(total_amount ~ s(pickup_longitude) + s(pickup_latitude) + passenger_count + s(trip_distance) + wday + hour + month, family = "gaussian", data = train)

cbind(fitted(gam), residuals(gam)) %>%
	as_tibble() %>%
	ggplot(aes(V1, V2)) +
	geom_point()

# data with residuals
train_sf$resid <- residuals(gam)

test_sf$pred <- predict(gam, newdata = test)

test_sf %>%
	mutate(resid = total_amount - pred) %>%
	summarize(rmse = sqrt(mean(resid^2))) -> rmse_gam


#-------- Krige --------
set.seed(1234)

# variograms
taxi_variogram <- variogram(resid ~ 1,
														data = select(train_sf, -total_amount),
														cutoff = 10)
plot(taxi_variogram)

spherical_fit <- fit.variogram(taxi_variogram, vgm(0.05, "Sph", 3, 0.04))
plot(taxi_variogram, spherical_fit)

# ordinary kriging
train_sp <- as_Spatial(select(train_sf, -total_amount))
#prediction_grid <- spsample(train_sp, nrow(train_sf), type = "regular")
test_sp <- as_Spatial(select(test_sf, -pred))

spherical_krige <- krige(formula = resid ~ 1,
												 locations = train_sp,
												 model = spherical_fit,
												 newdata = test_sp,
												 nmax=15)

#-------- GAM+Krige --------
final_data <- bind_cols(test_sf, spherical_krige$var1.pred, spherical_krige$var1.var) %>%
	rename("variance" = "...12") %>%
	mutate(pred = pred + ...11) %>%
	select(- ...11)

final_data %>%
	mutate(resid = pred - total_amount) %>%
	summarize(rmse = sqrt(mean(resid^2))) -> rmse_gam_krige

#-------- randomForests --------
rf_model <- randomForest(total_amount ~ ., data = train, ntree = 500)

test$pred <-  predict(rf_model, newdata = test, type = "class")

ggplot(test, aes(pickup_longitude, pickup_latitude, color = exp(pred), alpha = pred))+
	geom_point(size = 2, show.legend = T) +
	scale_color_gradient_tableau() +
	coord_equal() +
	theme_par() +
	labs(fill = "total earnings",
			 alpha = "log(total earnings)",
			 subtitle = "Manhattan",
			 x = "longitude",
			 y = "latitude")

test %>%
	mutate(residual = pred - total_amount) %>%
	summarize(rmse = sqrt(mean(residual^2))) -> rmse_rf

#-------- plots + tests --------
jarque.bera.test(residuals(model))
jarque.bera.test(residuals(gam))
final_data %>%
	mutate(resid = pred - total_amount) %>%
	as_tibble() %>%
	select(resid) %>%
	pull() %>%
	jarque.bera.test()

shapiro.test(residuals(model))
shapiro.test(residuals(gam))
final_data %>%
	mutate(resid = pred - total_amount) %>%
	as_tibble() %>%
	select(resid) %>%
	pull() %>%
	shapiro.test()

final_data %>%
	ggplot(aes(pickup_longitude, pickup_latitude)) +
	geom_point(aes(color=exp(pred), alpha = pred), show.legend = TRUE) +
	coord_equal() +
	scale_color_gradient_tableau() +
	theme_par() +
	labs(fill = "total earnings",
			 alpha = "log(total earnings)",
			 subtitle = "Manhattan",
			 x = "longitude",
			 y = "latitude")

ggplot(final_data, aes(pickup_longitude, pickup_latitude, color = exp(total_amount), alpha = total_amount))+
	geom_point(size = 2, show.legend = T) +
	scale_color_gradient_tableau() +
	coord_equal() +
	theme_par() +
	labs(fill = "total earnings",
			 alpha = "log(total earnings)",
			 subtitle = "Manhattan",
			 x = "longitude",
			 y = "latitude")

ggplot(final_data, aes(pickup_longitude, pickup_latitude, color = exp(variance)))+
	geom_point(size = 2, show.legend = T) +
	scale_color_gradient_tableau() +
	coord_equal() +
	theme_map() +
	labs(fill = "fare") +
	theme(legend.position = "top")

ggplot(final_data) +
	geom_histogram(aes(exp(total_amount)), bins = 100, color = "royalblue", fill = "royalblue") +
	geom_histogram(aes(exp(pred)), bins = 100, alpha = 0.6, color = "red", fill = "red") +
	theme_minimal() +
	labs(y = "",
			 x = "total earnings")

{% endhighlight %}


##### Works Cited

[^1]: Whong, C. (2014, March 18). FOILing NYC’s Taxi Trip Data. https://chriswhong.com/open-data/foil_nyc_taxi/

[^2]: Gámez, M., Montero, J., & Rubio, N. (2000). Kriging methodology for regional economic analysis: Estimating the housing price in Albacete. International Advances in Economic Research, 6, 438–450. https://doi.org/10.1007/BF02294963

[^3]: Krige, D. G. (1951). A statistical approach to some mine valuation and allied problems on the Witwatersrand [Thesis]. http://wiredspace.wits.ac.za/handle/10539/17975

[^4]: Matheron, G. (1973). The Intrinsic Random Functions and Their Applications. Advances in Applied Probability, 5(3), 439–468. https://doi.org/10.2307/1425829

[^5]: Matheron, Georges. (1963). Principles of geostatistics. Economic Geology, 58(8), 1246–1266. https://doi.org/10.2113/gsecongeo.58.8.1246

[^6]: Breiman, L. (2001). Random Forests. Machine Learning, 45(1), 5–32. https://doi.org/10.1023/A:1010933404324

[^7]: Tin Kam Ho. (1998). The random subspace method for constructing decision forests. IEEE Transactions on Pattern Analysis and Machine Intelligence, 20(8), 832–844. https://doi.org/10.1109/34.709601

