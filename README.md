# Malaysia_age_structure_prediction_ARIMA_ILR_SLR_transformation

This project is about predict age structure of Malaysia. 

Ex: Kid(33%), Youth(33%), Elder(34%)

## Why predict age structure of Malaysia is important? 

Malaysia has ageing population. WHO predict 1/6 person will be more than 60 by 2030. Based on standard of United Nations, 7% elder is considered ageing population and Malaysia reach 7.12% in 2020.

Cause : Live healthier and longer live expectancy, more educated population with birth control.

Impact : Shortage of workers, goverment higher expense on healthcare, different population savings and consumption patterns

## Literature review

### Two papers are my main references:
1) "Predicting population age structures of China, India, and Vietnam by 2030 based on compositional data" by Wei Y, Wang Z, Wang H, Li Y, Jiang Z (2019)
2) "The isometric logratio transformation in compositional data analysis: a practical evaluation." by Greenacre, Michael & Grunsky, Eric. (2018)

### Main findings in my literature review:

#### Previous studies of prediction of population proportion by age 
- Isometric log-ratio transformation (ILR) outperform Linear combined component (LCC), and Dimension Reduction Approach through Hyperspherical Transformation (DRHT), base for compositional  data. The compositional data after prediction will have broken compositional data structure. After make the data compositional again, the prediction will be affected.
- ARIMA and ETS outperform VAR and NNETTS. Note: small data set

#### Previous studies on time series forecasting model : ARIMA and ETS
- ARIMA win in more cases

####Previous studies on the comparison of Isometric log ratio transformation (ILR) and summed log ratio transformation (SLR)
- SLR might be a better way to transform data as it preserve relationship of ratio better.

![image](https://user-images.githubusercontent.com/124423169/216757173-f6227841-0970-4ade-82dd-137fff7f427c.png)

#### Previous studies on the comparison of multivariate and univariate time series model
- ARIMA is univariate model. Performance of univariate about the same multivariate based on the papers I can access. Some multivariate time series model supporter said multivariate is better than univariate if the correlation between variable is high. However, in paper "Comparative Study between Univariate and Multivariate Linear Stationary Time Series Models" (2016) show that even the correlation between variables are high, univariate still better. 

#### Previous studies on ARIMA hybrid model
- Many models have hybrid with ARIMA and get a good result. They all use the same approaches, which is predict the residual produce by ARIMA. 

## Below are main things I done based on the findings.

1) Compare performance of optimization method for ARIMA.
Normally parameters of ARIMA is decided using AutoARIMA and ACF, PACF graph. I compared AutoARIMA with grid search method.

p, d, q chosen for ARIMA

![image](https://user-images.githubusercontent.com/124423169/216759176-9913575a-f79b-407c-9b93-50f82157ff31.png)


2) Create function for SLR. (transform and inverse transform)
ILR has existing function in scikit-bio. SLR do not have any in python. Function built base on the formula of SLR and different kind of comparison in log ratio tried.


SLR formula

![image](https://user-images.githubusercontent.com/124423169/216758160-cdb9a1bb-f211-4338-b7b0-4be365c9dc81.png)


SLR equation 

![image](https://user-images.githubusercontent.com/124423169/216758199-9cf2b3ad-a460-4f2d-a2fe-c9866364cdc7.png)


SLR2 equation

![image](https://user-images.githubusercontent.com/124423169/216758234-123ccdca-5279-4432-9698-017c112ec63e.png)


SLR inverse function equation (E, Y, K are variables of proportion. A, B, C are constant which is the transformed data value)

![image](https://user-images.githubusercontent.com/124423169/216758289-bef5c39b-493a-4bfa-a44c-ebd075c71b1e.png)
![image](https://user-images.githubusercontent.com/124423169/216758300-16adef54-6019-45e9-b211-60c07fcd8e5f.png)


SLR2 inverse function equation (E, Y, K are variables of proportion. A, B, C are constant which is the transformed data value)

![image](https://user-images.githubusercontent.com/124423169/216758330-207c11c8-a38d-43a7-8457-efa5b4ccbc09.png)


In both inverse function, E is assumed to be 1 to ease the calculation.


## Main outcomes

Performance of AutoARIMA(left) and grid search(right) with different kind of transformation and base data.
![image](https://user-images.githubusercontent.com/124423169/216758515-411d0286-b5d0-4987-ad6f-c4e35909a685.png)

Observation:

-Grid Search outperform Auto ARIMA in optimization. 

-Original data general perform worse than transformed data.

-Notice Auto ARIMA of ILR and SLR has performance almost the same in testing but ILR better in cross validation indicate a possible overfit of using ILR.

-In grid search, ILR data perform better than SLR data while SLR2 data perform slightly better than ILR data. But in cross validation, ILR is better than SLR2, indicate overfit.



![image](https://user-images.githubusercontent.com/124423169/216759342-27f98e86-18dd-4e50-a48d-7140ceb3f0eb.png)

In 2041, elder will increase reach 15.0045%, youth is predicted to reach 65.6714% of population, while kid will decrease and reach 19.3241%. 

## Some conclusion
- For short univariate linear time series data without seasonality and cycle, ARIMA is a good model for prediction. 

- In this case, instead of inference order p, d, q using ACF, PACF plot or use Auto ARIMA, grid search can be considered.

- For compositional data, various kind of transformation should be considered for better performance. 

## Recommendation
- SLR still other ways to compare ratio can be tried out. 

- Instead of use ILR function that existed, self made ILR function can be more customizable

- Order can also be determined when observing ACF and PACF plot. 

- For hybrid of model, there might be more models can be tried to predict residual left by ARIMA. 


## The flows

The flow of file

<img src="https://user-images.githubusercontent.com/124423169/216753836-38657a64-57fa-4389-bc54-678b89745581.png" width="30%" height="30%">


The whole flow of how it works

<img src="https://user-images.githubusercontent.com/124423169/216754299-61c71ce6-e0de-4667-b1fa-2a24fbf85203.png" width="30%" height="30%">


All the data used

<img src="https://user-images.githubusercontent.com/124423169/216754811-c5048ef9-ff3a-4fbf-92ff-8b99ec95dc74.png" width="30%" height="30%">


Flow of ARIMA

<img src="https://user-images.githubusercontent.com/124423169/216754689-7370629c-6c32-4b66-a4a9-c2e7e2da94c1.png" width="30%" height="30%">


Flow of XGBoost with ARIMA

<img src="https://user-images.githubusercontent.com/124423169/216754769-65f43340-8205-472c-ab07-c4b0a1e9a654.png" width="30%" height="30%">
