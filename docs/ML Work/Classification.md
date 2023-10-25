---
title: "Classification"
author: "Zachary Canoot & Gray Simpson"
share: true
category: "ML Work"
---
# Classification
This time we will be using linear models in order to classify observations. Linear models like logistic regression and Naive Bayes work by finding the probability of a target variable given a predictor variable. This means we are predicting a class as opposed to a continuous value like in linear regression. These models are great for data with outliers and are easy to implement and interpret. Linear regression isn't very flexible however, and Naive Bayes makes a naive assumption that predictors are independent. 

## What is Our Data?
The weather data we used in our quantitative didn't have a suitable categorical target field, so we are switching to [income census data](https://www.kaggle.com/datasets/rdcmdev/adult-income-dataset). The data has a great binary classification in the form of an IncomeClass attribute that only states whether a given person's income is below or above 50k. We have plenty of categories for each person, and continuous measurements like age and work hours.

The census itself is from the year 1994, and spans various socieo-economic groups. We both trying to predict this income classification based on all of the data, as well as just get an understanding of some key predictors in the data.

With IncomeClass as our target, lets analyze the data!

### Reading the Data
The data is stored as two files, with rows just delimited by commas, so we read them in to one whole data frame, and label the headers manual using our source as a reference. It's worth noting that this data was extracted with the intention of creating a classification model, so the two files are meant to be training and test data, but we are going to re-distribute the data later.
```r
income_train <- read.table("adult.data", sep=",", header=FALSE)
income_test <- read.table("adult.test", sep=",", header=FALSE)
income <- rbind(income_test, income_train)
colnames(income) <- c("Age", "WorkClass", "Weight", "Education", "YearsEdu", "Marital-Status", "Job", "Relationship", "Race", "Sex", "CapitalGain", "CapitalLoss", "HoursWorked", "NativeCountry", "IncomeClass")
#Just to check to make sure it read properly
str(income)
```
Now we want to turn the qualitative data into factors.

Find all attributes of income that are non-numeric
  - sapply() returns a logical object of every attribute run through the given function
  - which() returns all of the true indices of a logical object
  - income[,<object>] extracts the attributes (See help(Extract))
  - We then lapply, with as.factor forcing them to be factors in a list
  
Then just factor them.
```r
# Note here that while sapply returns a vector, lapply returns a list
income[, sapply(income, is.character)] <- lapply(income[, sapply(income, is.character)], as.factor)
# Checking our work
str(income)
```

Now the data is a bit cleaner we can start to look at it!
```r
summary(income)
```
Now that we can really see our factor's options, I see a couple skewed data points:
  - Twice as many men as women! Hope those numbers are better in 2022!
  - A large percent of the data is for natives to the US, which is kind of expected
  - Weight: Now, this represent what census takers thought a particular row represented the whole of the dataset. I must admit at the time I don't know how to account for statistical weight, but considering our model only needs to match training data, not other data from 1994, we are safe to ignore it.

The data looks very clean! Except for a bit of an anomaly with how the Target column, IncomeClass is stored. Some levels have a "." at the end, which we would like to remove. So lets go ahead and condense that, remove the Weight attribute, and create our training and test data.

```r 
# Simply just reassign the levels
levels(income$IncomeClass) <- c("<=50k", "<=50k", ">50k", ">50k")
levels(income$IncomeClass)
# Then remove the attribute weight using it's index
income <- income[, -3]
income
```

Then we are good to start exploring!

## Training Data Exploration
### Spliting Training Data
We are splitting training data on a 80/20 split
```r
set.seed(42069)
trainindex <- sample(1:nrow(income),nrow(income)*.8,replace=FALSE)
train <- income[trainindex,]
test <- income[-trainindex,]
# Cleaning up earlier data
rm("income", "income_test", "income_train")
```

### Textual Measurements
And what does that training data look like!

We would want to use different metrics, like mean, or count our factors:
```r
mean(train$Age)
nlevels(train$WorkClass)
```

But we can just do that in `summary()`.

```r
summary(train)
```
The summary above is good for making sure there is no errors in the data, and of course skews we can deal with. For this data, there sure are a lot of men native to America, but that as said earlier is expected. Looking a bit more:

```r
sum(is.na(train))
head(train)
tail(train)
```
We get an example of whats at the end and start of the data set, and make sure there are no NA's. The census people really keep their data clean.

For one more look lets see some correlation data. Curious how much capital loss went up with age? We can see below... well not much honestly.
```r
cor(train$Age, train$CapitalLoss)
```

#### Text Analysis Conclusion
We fear the skew of my data towards 1 type of person (Married Men about to hit their 40's) will make the model's we produce perform well for our dataset, but fail to get any real world accuracy. Obviously if this model was actually destined to predict in the real world if people's income was above or below a certain level (in the 1990's), well if we had all this data we would probably already know their income. So the model is a pointless but fun experiment...

Regardless it is worth noting that a transformation of the data before running logistic regression or naive bayes could produce better results, but it is beyond the scope of this experiment.

While it is probably a realistic distribution of income class (3 people with less than 50k for every person over 50k), the data may just guess that everyone doesn't make that much money due to the skew. This actually is a lot more important then skewed predictors, as our eventual precision/recall could be quite bad. For now, simply observing this is good enough, but this should be onsidered for the final analysis. (And perhaps in our comparision between Bayes and logistic regression).

### Visual Analysis
We want to see how our target, IncomeClass relates to our numerical data:
```r
plot(x = train$IncomeClass, y=train$Age, ylab="", xlab="Income", main="Age")
plot(x = train$IncomeClass, y=train$CapitalLoss, ylab="", xlab="Income", main="Capital Loss")
plot(x = train$IncomeClass, y=train$CapitalGain, ylab="", xlab="Income", main="Capital Gain")
plot(x = train$IncomeClass, y=train$HoursWorked, ylab="", xlab="Income", main="Hours Worked")
```

Numerical trends are just easier to spot, especially the effect of age on IncomeClass. You can definitely see in the ease graphs, particular age and hours worked, that there are *some* grounds to predict this income classification based on the predictor data.

For another view:
```r
cdplot(train$Age, train$IncomeClass)
breaks <- (0:10)*10
plot(train$IncomeClass ~ findInterval(train$HoursWorked, breaks))
plot(train$Sex, train$IncomeClass)
```

Above we can see a couple trends relating to Income Class:
  - Women don't make as much as men
  - It seems the more hours worked, the higher your chances of making it over 50k
  - Right around 50 years old is when people were the most likely to make >50k

#### Visual Analysis Conclusion
There are so many different factors in this data, that we think assuming the factors are independent could harm the 
eventual accuracy of our linear models. While we can graph individual factors relation to the target, there are complicated relationships between the predictor data. We may be able to guess that more education would lead to a higher income, but an in-depth analysis of how gender or native country may hamper access to education isnt represented by just the relationship from gender to income. To the final product, it just *looks* like you can bet women make less money, even if that may be due to a compaction of other factors.

Just a couple trends are seen above, and they still tell us that there is some merit to this data being alble to predict relations between our predictors and our target. Now it is time to see if all of those predictors together have a good chance of classifying them into the >50k or <=50k levels.

## Classification Regression
### Logistic Regression
```r
glm1 <- glm(IncomeClass~., data=train, family=binomial)
summary(glm1)
```
Ah! Well we sure do get to get to view the impact of every level (dummy variable) on the output model. Before analyzing the coefficients predicted by the model, I want to examine which attributes were better for the model as compared to others.

#### Explanation
The data produced by the model is the coefficients of each predictor. The coefficient represents the effect the value of the predictor has on our target. If we have a positive coefficient like age, as age goes up we can expect the probability of our target (IncomeClass) to go up. The final model then considers each of these coefficients in it's prediction. Different parts of the data are:
- Deviance Residuals:
- The Null Deviance:
- Residual Deviance:
- Degrees of Freedom:
- AIC:
- Fisher Scoring Iterations:
- Standard Error:
- Z Value:
- P Value:

#### Looking at P-Values
A coefficient estimate's p-values can tell us which features are valuable predictors. However, because the data is mostly qualitative, each level of each factor has a different impact on the data. 

WorkClass seems like it is a good predictor *overall*, but if a given person's WorkClass is Never-worked, well the p-value is huge! Now, obviously if you have never worked your income isn't going to be very high, and the model estimates a high negative correlation. Yet the P-Value is super high!

This could be due to a number of factors:
- The sample size of people who have never worked in this data is much smaller than the total population.
- Our target factor is skewed, so this predictor can't differ too much from the null hypothesis 
- People who have never worked have varying life experiences, so the final accuracy of their coefficients isn't going to be able to fit the data

As humans we can see the this coefficient should be significant, so perhaps this isn't the best dataset for logistic regression. The summary of the model basically is this: 

> While factors that you would expect to negatively impact income class do have large negative coefficents their p-values are very large because the overall target is very skewed (probably) towards what they are predicting (low income).

#### Probability Warning
Another issue with the data is the warning:

> `Warning: glm.fit: fitted probabilities numerically 0 or 1 occurred`

This error occurs when our model fits the data so well it is most likely too perfect. This means there is *somewhat likely* an error in our data. We can check it by looking at a couple predictions:

```r
head(predict(glm1, train, type="response"), 30) # Looking at some probabilities
```

Looking at just 30 fitted probabilities we see that not every single probability is 1 or 0, but another warning:

> Warning: prediction from a rank-deficent fit may be misleading

This means our number of linearly independent columns does not equal the number of parameters. Funny enough, the actual model throws out what it believes are perfectly colinear variables, causing this warning. The solution would then be to remove the colinear attributes, which will be done in just a moment.

#### Initial Impressions

Dismissing those issues, good predictors are:
- Age
- Work Class
- Education (Specifically higher education)
- Job
- Marriage Status
- Sex
- Hours Worked

This model makes me wonder what would happen if we selected a sample from this dataset that is less skewed, but I'm unsure what this would do to the accuracy of this model in the real world.

#### Improving the Model
We wanted to see if removing predictors would help the overall accuracy, especially given that our predictors are somewhat dependent on each other. A brief search revealed that the anova function can show how adding each predictor effects the model.

```r
anova(glm1, test="Chisq")
```

Looking below, we can tell that each addition to the model is statistically relevant

#### Conclusions
There are issues with the data, mostly a high bias and a skewed target variable, but our current model still could give good predictions given a similar data set. If you took another sample of census data just after this one it could probably predict income class a bit

### Naive Bayes Model
```r
library(e1071)
nb1 <- naiveBayes(train$IncomeClass~., data=train)
nb1
```

Naive Bayes produces a model that first finds the prior probability (A-priori, or the probability of having <=50k or >50k with no considerations of other data) and then finds the probability of the income given each condition independently. For example the table for Sex states that the probability that someone is female given that you make less than 50k is ~40%, while if a person makes more than 50k the chance they are a woman is ~15%.

We also see the results for quantified predictors. For a continuous predictor like age, the mean age for people <=50k is 36.85352 while people >50k are older at a mean of 44.32006 years old.

The model may just be finding the independent probabilities of the target event given each predictor but using all of the probabilities at once can provide a pretty good guess. Good enough to predict our training data!

#### Issues in the Data
It's worth noting once again that our predictors may not be completely independent but our model here assumes they are. That is why we call it naive! With such a large amount of data, probability can overcome the shortcomings of this assumption and we could get reasonably accurate predictions

### Predictions
```r
p1 <- predict(glm1, newdata=test, type="response")
pred1 <- ifelse(p1>0.5, ">50k", "<=50k")
head(pred1)
head(test$IncomeClass)
cm1 <- caret::confusionMatrix(as.factor(pred1), reference=test$IncomeClass)
cm1
```



```r
p2 <- predict(nb1, newdata=test, type="class")
head(p2)
head(test$IncomeClass)
cm2 <- caret::confusionMatrix(as.factor(p2), test$IncomeClass)
cm2
```

```r
cm1$byClass
```


#### Initial conclusion
The initial conclusion to be drawn from our predictions is that our accuracy for both our models is okay, and our logistic regression model did better than our Naive Bayes. This could probably be due to Naive Bayes often doing better with small data sets while logistic regression works better with large datasets. On the other hand the logistic regression model might have still been overwhelmed by the amount of factors, and the accuracy was only ~84%.

The confusion matrix tells us True Positive, False Positive, True Negative, and False Negative results from applying the model to the test data. We can use the ratios between these numbers to evaluate useful metrics like accuracy or sensitivity.

#### The Confusion Matrix
```
          Reference
Prediction <=50k >50k
     <=50k  6898 1225
     >50k    498 1148
```
Just for an example we are looking at the naive bayes confusion matrix.
- 6898: The number of True Positives
- 1148: The number of True Negatives
- 498:  The number of False Negatives
- 1225: The number of False Positives

We can use these to calculate other metrics

#### Accuracy
```
Logistic R.: ~85%
Naive Bayes: ~82%
```

The diagonals, or our true results, divided by all of our predictions is our accuracy, or the percentage we were correct. As you can see, our logistic regression model was accurate more of the time. Most likely because it thrived more with the large amount of data.

#### Sensitivity & Specificity
```
Logistic R.: 0.9287 and 0.5874
Naive Bayes: 0.9327 and 0.4838
```
Naive Bayes had a higher sensitivity, which is the number of true positives out of true positives + false negatives (the number of positives in the data). If we were trying to perhaps locate all people with "low" income but didn't care about our accuracy with people above 50k, the stat shows naive bayes could be useful.

Specificity is the measure of true negatives in the negative class. We can tell then that we were much better at identifying our people with <=50k income than people with >50k income. However, logistic regression was still better than Naive Bayes in this stat.

Well you ignore part of the data and perhaps get to ignore issues ini your model (like ignoring a bunch of false negatives), these are great for getting what matters out of data.

#### Kappa
```
Logistic R.: 0.5519 
Naive Bayes: 0.4648 
```

Woah! These aren't the best numbers, but considering this is a measure of accuracy that corrects for prediction by chance, I'm surprised the number is so high. The data set was skewed, it seemed a large margin of the success of our models was due to random chance. According to a reference on kappa scores though, these numbers are in "moderate agreement" with what is expected.

Kappa is great for regarding datasets where the random chance of getting a prediction high is right. Of course, there isn't a consensus on what the number means on a scale, but its still generally useful.

#### ROC Curves and AUC

```r
library(ROCR)
head(p1)
head(test$IncomeClass)
pr <- prediction(p1, test$IncomeClass)
prf <- performance(pr, measure = "tpr", x.measure = "fpr")
plot(prf)
# Compute AUC
auc <- performance(pr, measure = "auc")
auc <- auc@y.values[[1]]
auc
```

```r
library(ROCR)
p2raw <- predict(nb1, newdata=test, type="raw")[,2]
pr2 <- prediction(p2raw,as.numeric(test$IncomeClass))
prf2 <- performance(pr2, measure = "tpr", x.measure = "fpr")
plot(prf2)

# Compute AUC
auc <- performance(pr2, measure = "auc")
auc <- auc@y.values[[1]]
auc
```

#### Matthew's Correlation Coefficient (MCC):
```r
# Logistic Regression
mltools::mcc(as.factor(pred1), test$IncomeClass)
```

```r
# Naive Bayes
mltools::mcc(p2, test$IncomeClass)
```

.4771213 is smack dab in between a perfect model (1) and a model that is perfectly average (0). Pretty good!

### Strengths and Weaknesses
Logistic Regression basically is attempting to draw a line between classes. It ends up being quite computationally inexpensive, easy to understand, and does its job well if classes are easy to separate. But because of it's simplicity as a line, it just isn't complex enough to capture complex non-linear decision boundaries. Naive Bayes is also simple, but with the added bonus that it works well with high dimensions (complex data sets) *if* they aren't too big. It's simple however because it assumes variables are independent, and ends up lacking with larger data sets.

### Summary of Metrics
Accuracy being the ratio of correct predictions to incorrect predictions, it is broadly useful. But often we are searching for subsections of accuracy. Sensitivity is good for detecting the amount we get one (the positive) class and ignores the other. Specificity on the other hand is the ratio of correct negative classes. This means we can use these metrics to see how ell our data is at guessing what matters in the at. If we want to see general accuracy, but account for the chance of getting the prediction randomly correct, Kappa is great for checking that.

Now ROC... well it graphs the true positive rate and the false positive rate (sensitivity and specificity). Unfortunately we tried til the deadline to get this to work for Naive Bayes but we swear we understand what it means! The name, Receiver Operator Characteristic curve comes from signal detection theory so it doesn't help much to remind what it means. However, basically it graphs the trade off of a model between sensitivity and specificity. The Area under the curve then represents how much the model is capable of distinguishing between classes.

The MCC is a metric that basically gives a good value if you get a good reliable rate in all 4 values of the confusion matrix. The values are considered in proportional the size of the positive and negative values. Rather then combining the sensitivity and specificity of a metric into a single metric (like with an F1-Score), MCC considers the size of of negative samples. MCC's account for class distribution makes it great at providing an accuracy rating for the whole model rated from -1 to 1.


## Conclusion
We have 1 large takeaway from this data, linear data has limitations, and none of that is helped by having a skewed data set. In the future we would like to select a data set that has less of skewed target, or at least try to sample this data at a better ratio again. It was fun to look at though!
