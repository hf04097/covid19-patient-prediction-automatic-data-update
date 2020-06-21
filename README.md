# COVID19 New Cases Prediction for Pakistan

## Introduction
Neural Networks are considered to be one of the most efficient methods that give us predictions, patterns, trends and forecasts. With the rise in the Covid19 cases throughout the world and in Pakistan, different approaches have been adopted and forecasting networks are build. However, no method have been yet able to accurately able to predict the intricate and heterogeneous situation in the world. This project is a step to explore different forecasting techniques to predict the number of Covid19 cases in Pakistan. We tried and tested different data-driven LSTM models and regression techniques to predict the number of new COVID-19 cases.

## Data Selection

### Data source
The data used for the prediction is maintained by [Our world in Data](https://ourworldindata.org/coronavirus). The data is updated daily and includes data on confirmed cases, deaths, and testing, as well as other variables of potential interest. List of all columns of the data, it’s description and source can be found on [Codebook for the complete Our World in Data COVID-19 dataset]( https://github.com/owid/covid-19-data/blob/master/public/data/owid-covid-data-codebook.md). 

### Choosing Data Varaibles
This dataset contains data from 208 countries however; we selected 40 countries from the dataset. The top 40 countries that had the highest total cases reported were selected as of 15 June 2020. 
Data with similar columns or columns that gave double information were dropped. For example since the dataset contained the column total_cases the total_cases_per_million column was dropped as they share the same information.
According to Halkjaer and Winther, the correlations in the input data severely slow down learning therefore, we used Pearson Correlation Coefficient Matrix to determine the correlation between the input data and new_cases.
The matrix as shown in figure 1 shows the correlation between new_cases and input data. All the variables having a Pearson Correlation, P, greater than |P| > 0.1 were dropped.

## Data processing





