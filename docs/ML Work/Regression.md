---
title: "Regression"
author: "Zachary Canoot & Gray Simpson"
category: "ML Work"
share: true
---
# Regression

  Using the Hungary Dataset [Weather in Szeged 2006-2016](https://www.kaggle.com/datasets/budincsevity/szeged-weather)
Found on Kaggle. 

  Our goal is to see if we can see how other weather factors, such as Wind Speed and Humidity, relate to the difference between Apparent Temperature and actual Temperature. Though we identify apparent temperature as a very good predictor of the difference, we do not use this in this assignment as we are interested in exploring more the other factors that influence the disparity. 

  Linear Regression is one of many supervised models of Machine Learning that functions by finding a trend in given data, using one or more input parameters to find a line of best fit, though it is not always a straight line. As shown by the name, linear regression models assume that the relationship between relevant attributes is linear. The model will predict coefficients for the effect of each predictor. It has low variance due to its linear nature, but with such an assumption, it will also be very high bias.


### Data Exploration
  First, we read the data in, then divide our data up into training and testing. We have to add a column for the data we are interested in learning about, however, it is simply the difference between two other columns. 
```r
df <- read.csv("weatherHistory.csv")
#Here we'll add the data that we are interested in: difference in Apparent Temp and Temp.
df$Temperature.Diff <- df$Temperature..C. - df$Apparent.Temperature..C.
#We'll also convert some data to factors for ease.
df$Precip.Type <- as.factor(df$Precip.Type)
df$Summary <- as.factor(df$Summary)
str(df)
#Now we'll divide into train and test.
set.seed(8)
trainindex <- sample(1:nrow(df),nrow(df)*.8,replace=FALSE)
train <- df[trainindex,]
test <- df[-trainindex,]
```



Next, we want to explore our training data. 
```r
names(df)
dim(df)
head(df)
colMeans(df[4:11])

#Noticing the mean of 0 of df$Loud.Cover, lets check its sum in specific.
sum(df$Loud.Cover)

#Let's see if we have any NAs more generally, now.
colSums(is.na(df))
#Okay, so we don't have any NAs.

#Now, lets see how R would summarize this data. 
summary(df)
#We would also like to look at this particular aspect to see how the different values pan out.
summary(df$Summary)

```
  One thing we notice is that there is an attribute labeled 'Loud Cover' that all values are 0 in. Therefore, this will be an aspect that we will ignore.
  
  However, in the summary, we can notice that there is a minimum value of 0 on Pressure, which has an average and max similar to each other. We can assume a 0 is a NA value here. 
  
  The other values that are 0 we can't make assumptions on validity.
  
  If Wind Speed is 0, so will Wind Bearing, we can realize from looking through the data in passing. 
  Since there is no place in earth without wind, we can also state that these values aren't accurate. After we look a the data some more, we'll decide how we want to clean them up.
  
  We cannot come to a clear resolution on other attributes.


  We'll pull up some graphs to get a better idea of what we have to do, now. Yellow dots are null precipitation days, green is rain, and blue is snow.
```r
cor(df[4:7])
boxplot(df$Temperature.Diff,main="Apparent Temperature")
boxplot(df$Humidity,main="Humidity")
boxplot(df$Wind.Speed..km.h.,main="Wind Speed")
#pairs(df[4:7],main="Temperature, Humidity, and Wind Correlations")
plot(df$Temperature.Diff,df$Wind.Speed..km.h.,pch=21,bg=c("yellow","green","blue")[as.integer(df$Precip.Type)])
plot(df$Temperature.Diff,df$Humidity,pch=21,bg=c("yellow","green","blue")[as.integer(df$Precip.Type)])
plot(df$Temperature.Diff, df$Temperature..C.,pch=21,bg=c("yellow","green","blue")[as.integer(df$Precip.Type)])

```
  We can notice that there are some outliers in humidity at the 0 line we'll want to sort out, as well.
  
  We also notice a sort of "knife" shape in the data when being compared, at around 10 degrees Celsius.
  By and large, we have such a large amount of data, it's difficult to notice quick correlations, aside from wind speed and temperature when it is snowing/below freezing.
  
  That's why we have Machine Learning, we suppose, even if it implies that linear regression may not be the best fit for this data set.
  
  While we would like to predict the results without the base temperature, we can see that is is very clearly related and helpful.



  Now, we'll clean up the data according to what we found. We'll clean up only what is referenced, but we will delete what we are uncertain about, since we have such a large amount of data.
```r
df[,6:7][df[,6:7]==0] <- NA
df[,13:13][df[,13:13]==0] <- NA
df <- na.omit(df)
summary(df)
```


  Now we'll do some more graphs.
```r
cor(df[4:7])
boxplot(df$Temperature.Diff,main="Apparent Temperature")
boxplot(df$Humidity,main="Humidity")
boxplot(df$Wind.Speed..km.h.,main="Wind Speed")
#pairs(df[4:7],main="Temperature, Humidity, and Wind Correlations")
plot(df$Temperature.Diff,df$Wind.Speed..km.h.,pch=21,bg=c("yellow","green","blue")[as.integer(df$Precip.Type)])
plot(df$Temperature.Diff,df$Humidity,pch=21,bg=c("yellow","green","blue")[as.integer(df$Precip.Type)])
plot(df$Temperature.Diff, df$Temperature..C.,pch=21,bg=c("yellow","green","blue")[as.integer(df$Precip.Type)])

```

  Before we move on to linear regression, we have one more question before we investigate disparities in the data. 
  
  Since this project has multiple contributors, perhaps there are even more hidden NA's. 
  
  Let's see what the data looks like if we remove data where there is no difference. 
```r
diffOnly <- df
diffOnly[,3:4][diffOnly[,3:3]==diffOnly[,4:4]] <- NA
diffOnly <- na.omit(diffOnly)
plot(diffOnly$Temperature.Diff,diffOnly$Wind.Speed..km.h.,pch=21,bg=c("yellow","green","blue")[as.integer(df$Precip.Type)])
plot(diffOnly$Temperature.Diff,diffOnly$Humidity,pch=21,bg=c("yellow","green","blue")[as.integer(df$Precip.Type)])
#These graphs show the overall descriptor of the weather. There are 27 options.
plot(diffOnly$Temperature.Diff,diffOnly$Wind.Speed..km.h.,pch=21,bg=c("white","aquamarine","blue","brown","green","yellow","red","cyan","darkgray","darkgreen","magenta","orange","dodgerblue","forestgreen","gold","sienna","thistle","violet","springgreen","slateblue","wheat","tomato","yellowgreen","tan","lightblue","hotpink","darkred")[as.integer(df$Summary)])
plot(diffOnly$Temperature.Diff,diffOnly$Humidity,pch=21,bg=c("white","aquamarine","blue","brown","green","yellow","red","cyan","darkgray","darkgreen","magenta","orange","dodgerblue","forestgreen","gold","sienna","thistle","violet","springgreen","slateblue","wheat","tomato","yellowgreen","tan","lightblue","hotpink","darkred")[as.integer(df$Summary)])
plot(df$Temperature.Diff, df$Temperature..C.,pch=21,bg=c("white","aquamarine","blue","brown","green","yellow","red","cyan","darkgray","darkgreen","magenta","orange","dodgerblue","forestgreen","gold","sienna","thistle","violet","springgreen","slateblue","wheat","tomato","yellowgreen","tan","lightblue","hotpink","darkred")[as.integer(df$Summary)])

```
  This does not seem to have effected data trends very much.
  
  Looking at the data based on Summary does not help us much, but we can notice that the cloud of data that does not seem to have much trend is in the violet(18) and slateblue (20), or rather Mostly Cloudy and Partly Cloudy. There isn't much helpful we can do with that information at this time, however.

  Since that seemed to cause no changes, but may have helped clean it up a small amount, let's write it to df. We'll also clean up the train and test data again.
```r
df <- diffOnly
trainindex <- sample(1:nrow(df),nrow(df)*.8,replace=FALSE)
train <- df[trainindex,]
test <- df[-trainindex,]
```


  Now, we'll move on to the regression.


### Linear Regression: Simple
  Let's start with a linear regression model with one predictor, wind speed, and summarize it. 
```r
simplelinreg <- lm(Temperature.Diff~Wind.Speed..km.h.,data=train)
summary(simplelinreg)
```
  So, it's better than nothing it seems. The R^2 isn't great, but we can see that there's enough of a correlation to count. We could get a better reading by using the actual temperature, since those are very closely related, but one goal of this is learning to understand how the change in temperature works based on other factors. 

  Lets plot the residual errors, and evaluate.
```r
par(mfrow=c(2,2))
plot(simplelinreg)
```
  We can see that the trends are fairly close to the given lines. They are in no way perfect, but they seem to get the gist. The most concerning piece seems to be Residuals vs Leverage. The given line implies that we do have outlier (y-axis) leverage (x-axis) values that may influence our trend line. It may be something such as an issue during a case of severe weather, or a broken device used in data collection at that time.
  
  As well, this is only the data from our simple regression. We will be able to see how other models compare at a later time.



### Linear Regression: Multiple
  Let's up the complexity, now. We'll build a multiple linear regression model, and see if we can improve the accuracy.
```r
multlinreg <- lm(Temperature.Diff~Humidity+Wind.Speed..km.h.+Precip.Type,data=train)
summary(multlinreg)
par(mfrow=c(2,2))
plot(multlinreg)
```
  We understand from our data exploration that Humidity, Wind Speed, and Precipitation Type all relate to the data in different ways. We can find different trends depending on what we're looking at, so we can ask the model to reference all of that data when its processing now. When the precipitation type was rain, it didn't add much to figuring things out, but knowing that it was in the snow range was very helpful.
  
  It's doing better than our simple model, getting the R^2 up much more and a lower RSE. The Residuals vs Leverage chart looks like it has encountered some issues, however the two entirely separate sections does match up with some inconsistent trends that we noticed when we were graphing the attributes we planned on working with. The Residuals vs Fitted and Scale-Location graphs look comparatively stellar. Normal Q-Q is about the same.


### Linear Regression: Combinations
  Now let's go a step even farther. We'll use a combination of predictors, interaction effects, and polynomial regression to see if we can get even more accurate. 
```r
combolinreg <- lm(Temperature.Diff~poly(Humidity*Wind.Speed..km.h.)+Precip.Type+Summary,data=train)
summary(combolinreg)
par(mfrow=c(2,2))
plot(combolinreg)
```
  Here, we added Summary as well as an interaction effect with precipitation. We made this decision based on the cloud of Partly Cloudy values that didn't seem to follow other data, and we can see that some specific Summary values were quite helpful in the result, and some were not.
  
  Overall, though, R^2 is up a bit more, and RSE is down. It's not a huge change, but it does help. Humidity and Wind Speed seemed to have some similar trends and attributes when we graphed them, and the type of weather is related to the type of precipitation, which is why we had those certain attributes marked as an interaction effect.
  
  The residuals are now very different from the other two models' results. It seems like the values are much more as intended, horizontal where they should be to indicate a good fit, though Q-Q seems to be the same. The outlying x and y observations also seem to be different than the ones the other models denoted. 


### Evaluation 
  With each new type, using more aspects of different Machine Learning model results, we were able to increase the model's ability to find lines within the data, which should help us when we predict our results. In general, the more ways we let the data interact, the better the resulting model seemed to be, so long as we did not do it blindly. Depending on how related certain attributes are, they need to be treated differently, since some attributes are a result of other attributes that may be included in the data. 
  
  So, in the end, the combination of interaction effects and multiple regression provided the best trends. Simple regression did not seem to fit the data well at all in comparison to the other models. The combination data may have only been a little about +.02 better on R^2 than multiple regression, however it is a significant enough change to be useful in data prediction. 



### Predictions
  Using the three models, we will predict and evaluate using the metric correlation and MSE. 
```r
simplepred <- predict(simplelinreg,newdata=test)
simplecor <- cor(simplepred,test$Temperature.Diff)
simplemse <- mean((simplepred-test$Temperature.Diff)^2)
simplermse <- sqrt(simplemse)
multpred <- predict(multlinreg,newdata=test)
multcor <- cor(multpred,test$Temperature.Diff)
multmse <- mean((multpred-test$Temperature.Diff)^2)
multrmse <- sqrt(multmse)
combopred <- predict(combolinreg,newdata=test)
combocor <- cor(combopred,test$Temperature.Diff)
combomse <- mean((combopred-test$Temperature.Diff)^2)
combormse <- sqrt(combomse)
#Output results
print("-------Simple Model-------")
print(paste("Correlation: ", simplecor))
print(paste("MSE: ", simplemse))
print(paste("RMSE: ", simplermse))
print("-------Multiple Model-------")
print(paste("Correlation: ", multcor))
print(paste("MSE: ", multmse))
print(paste("RMSE: ", multrmse))
print("-------Combo Model-------")
print(paste("Correlation: ", combocor))
print(paste("MSE: ", combomse))
print(paste("RMSE: ", combormse))


anova(simplelinreg,multlinreg,combolinreg)
```
  Judging by these values, we can verify our evaluation that the combination linear regression model was the best model for our data. The low MSE (mean squared error) in comparison to the simple and multiple models means that the mistakes made were smaller than others. The RMSE (root MSE) says that we were off, on average, .939 degrees Celsius. While this isn't entirely accurate, the range of the value was around -5 degrees to +10 degrees, and if someone was predicting the weather the average person would likely be tolerant of a one degree difference. In that sense, the multiple regression would also be considered accurate enough to be helpful. The simple model is not terrible either, however the low correlation and high MSE do support the fact that there is much more room for improvement.
  
  The difference in temperature is not extremely related to the wind speed, as we attempted in that first model. While it is a factor, the apparent temperature is a multifaceted issue better represented by numerous other effects, such as Humidity, precipitation, etc. 
  
  Our results were also very good considering we were purposely avoiding using one aspect of given data, and that there were disparities showing what may have been differences due to how different contributors to the data set initially reading data in different ways.
  
  In summary, we can extract a surprising amount of data about the disparity in temperature based on wind, humidity, precipitation, and even descriptors of the sky. The more we combine usage of different attributes, acknowledging how they interact and work together, the better a result we can get.




