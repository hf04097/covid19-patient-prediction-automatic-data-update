# COVID19 New Cases Prediction for Pakistan

## Introduction
Neural Networks are considered to be one of the most efficient methods that give us predictions, patterns, trends and forecasts. With the rise in the Covid19 cases throughout the world and in Pakistan, different approaches have been adopted and forecasting networks are build. However, no method have been yet able to accurately able to predict the intricate and heterogeneous situation in the world. This project is a step to explore different forecasting techniques to predict the number of Covid19 cases in Pakistan. We tried and tested different data-driven LSTM models and regression techniques to predict the number of new COVID-19 cases.

![database NN people](/images/databaseNNpatient.png)


## Data Selection

### Data source
The data used for the prediction is maintained by [Our world in Data](https://ourworldindata.org/coronavirus). The data is updated daily and includes data on confirmed cases, deaths, and testing, as well as other variables of potential interest. List of all columns of the data, it’s description and source can be found on [Codebook for the complete Our World in Data COVID-19 dataset]( https://github.com/owid/covid-19-data/blob/master/public/data/owid-covid-data-codebook.md). 

### Database design and data-pipeline

To avoid repetitive reading of files and to work smoothly when our data grows, we created a database for our Data. Data was scrapped using pandas and MySQL database was made using Amazon Web Services and has two tables, country and cases. The data-pipeline is connected to the database and only adds new data to the database i.e. data for unprocessed dates.

### Choosing Data Variables
This dataset contains data from 208 countries however; we selected 40 countries from the dataset. The top 40 countries that had the highest total cases reported were selected as of 15 June 2020. 
Data with similar columns or columns that gave double information were dropped. For example, since the dataset contained the column total_cases, the total_cases_per_million column was dropped as they share the same information.
According to Halkjaer and Winther, the correlations in the input data severely slow down learning therefore, we used Pearson Correlation Coefficient Matrix to determine the correlation between the input data and new_cases. All the variables having a Pearson Correlation, P, greater than |P| > 0.1 were dropped. The figure shows the Pearson correlation between new_cases and input data.

![Pearson Coefficient Matrix](/images/pearsonCoeffiecient.png)

## Data processing

### Making Data Categorical
The location column contains the string value representing the name of the country and this is not a valid entry for Neural Network. Hence, the location column is one hot encoded by having a column for each country and if the data row is of that country there is a 1 in that column else there is 0.

### Substitute of Dates
Since one cannot simply input a date string into your model, therefore we introduced a column of number_of_days. We found the date at which the first COVID19 case was reported in each country and number_of_days entry is set to the the number of days from the date the first case was reported.

### Dealing with Nan Values

Columns having NaN value greater than 1000 were dropped (handwashing_facilitites and extreme_poverty was dropped). To deal with NaN values of strigency_index last_stringent, the last non-null stringency_index value for each country was found. All the null stringency_index after the last_strigenent were replaced by the last_strigenent. Countries having NaN values for male_smokers and female_smokers were replaced by a similar country’s value.  


## Models, Hyperparameters, and Test and Train data

Different hyperparameters and testing and training datasets were tested on the following LSTM model architecture however, throughout Adam was used as an optimizer with varying learning rate.

![architecture](/images/model_architecture1.png)


### Data as whole world data including Pakistan
Whole world’s data including Pakistan’s was chosen and was divided with a split index of 0.70, i.e., 70% of the data was X_train and 30% was X_test.

With this selection of data an error of 4402 was reached with Linear Regression.
With the following hyperparameters the an error of  5440 was reached with LSTM.
Hyperparameters: epochs = 200, learning rate = 0.001, batch size = 128.
The  Mean Squared Error (MSE) graphs is shown below:

![mse whole](/images/xy0.7mse.png)

The prediction graph achieved after Linear Regression is shown below

![prediction Linear xy0.7](/images/xy0.7_linearRegression.png)

### Keeping test set as last 40 days of Pakistan

The data was selected in a way that last number of days (40 rows) of Pakistan were assigned as test set and train set included world data and rest days of Pakistan data.
With hyperparameters epochs = 100, learning rate = 0.001 and batch size = 64. An error of 3945 was achieved. The Mean Squared Error (MSE) graphs is shown below:

![mse last40](/images/last40_mse.png) <br/>

The prediction achieved with this method and error = 3945 is shown in the graph below 

![mse last40](/images/last40prediction.png)

### First training on World data and using trained weights on Pakistan data
First, a model was trained on world’s data but it did not included Pakistan in it. Then the trained weights from the world’s model were set of another model that was then trained using Pakistan’s model. 
With hyperparameters, epochs = 200, learning rate = 0.001 and batch size = 128 on world’s data and epochs = 100, learning rate = 0.01 and batch size = 32 on Pakistan’s data an error of 2741 was achieved.
The Mean Squared Error (MSE) graphs for model trained on world’s data and Pakistan’s data is shown below respectively.

![mse world](/images/world_model_mse.png)
![mse pak](/images/pak_model_mse.png)

The prediction achieved with this method and error = 2741 is shown in the graph below

![predicted 0](/images/world_pak_predicted.png)

With hyperparameters, epochs = 200, learning rate = 0.001 and batch size = 32 on world’s data and epochs = 50, learning rate = 0.001 and batch size = 32 on Pakistan’s data an error of 2694 was achieved.
The Mean Squared Error (MSE) graphs for model trained on world’s data and Pakistan’s data is shown below respectively.

![mse world 1](/images/model mse world 2.png)
![mse pak 1](/images/model mse pak 2.png)

The prediction achieved with this method and error = 2694 is shown in the graph below

![prediction 1](/images/world_pak_predicted2.png)

## Conclusion

Different hyperparameters and training and test set combinations were tried however the predictions achieved were inaccurate and insufficient to to fully represent the complex, evolving and heterogeneous realities of our world. We believe that the predictions are heavily conditioned by the quality and insufficient quantity of the data. The data depends on the publicly available or submitted data by different countries and the data may not represent the actual cases reported. With the change in data which is the theoretical input of our model one should expect to see changes in the predictions. 


