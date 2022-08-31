---
share: true
category: ml-notes
tags: []
---

# Chapter 1: Introduction
## Machine Learning
> **Definition 1.1.1:** *Machine Learning*: trains computers to accurately recognize patterns in data for purposes of data analysis, prediction, and/or action selection by autonomous agents

The definition requires elaborations on certain terms:
- Data: We must both consider the ethics behind the data we use, and how organized it may be
	- **Quiz Revision**: Clustering is referenced in this section as an unsupervised method to group like instances
- Patterns: Computers can beat humans at recognizing patterns, but we must be the ones to organize the data.
- Predictions from Data: We can train algorithms on a portion of the data, and test it on the remaining data. They can learn from data over time as well
- Accuracy: That's the rub, we have to be accurate to some baseline. If our data says 99%, then we want to beat a baseline of 99%.
- Actions: Autonomous agents, or things like smart thermostats or assistants, take actions based on what they have learned, the end goal of a lot of ML

## Machine Learning Scenarios
Machine learning can be divided into into different classes
### Informative v. Active
***Informative*** algorithms are most algorithms looked at in this book. they input data observations and output a model of the data that can be used to predict outcomes for a new data data fed into the model. ***Active*** agents are taught by reinforcement learning to identify optimal actions given the current environment based on learned passed experience. They might keep learning with sensors or inputs

### Supervised v. Unsupervised Learning
The informative algorithms are of these two types. ***Supervised Learning***  refers to scenarios where each data instance has a label. This label is used to train the algorithm so the labels can be predicted in the future. however ***Unsupervised Learning*** refers to scenarios where data does not have labels and the goal is just learning.

### Regression v. Classification
In ***Regression*** the target is a real-numbered ***Quantitative*** value, like trying to predict the market value of a home given its square footage. ***Classification*** means the target is ***Qualitive*** a class like predicting if a borrower is a good credit risk *or*  not.

## Machine Learning v. Traditional Algo
Instead of processing input data with an algorithm, we train an algorithm that outputs a model to handle the problem. Some reasons for this are:
1. There are complex problems that humans may be able to solve, but we couldn't just program a computer to solve them. Instead we let the computer process all the fine details
2. The problem's scale could be massive! Just letting a computer do the job saves man hours

## Terminology
There are a lot of terms for the same thing in machine learning.

| GPA | Hours | SAT  | Class     |
| --- | ----- | ---- | --------- |
| 3.2 | 15    | 1450 | Junior    |
| 3.8 | 21    | 1420 | Sophomore |
| 2.5 | 9     | 1367 | Freshman  | 

Given the data above, we have rows and columns with different names:
- ***Rows*** are also known as ***instances***, ***examples***, and ***observations***.
- ***Columns*** are known as ***attributes*** or ***features*** and ***predictors***.

The data is also two types of values. ***Quantitative*** values are numerical while ***Qualitative*** values only take on a set of finite values. Those features are called ***Factors*** or ***Categorical Data***.

If we want to learn about one of the attributes or features, it is our ***Target*** or ***Response*** while the other data we use to predict that fact could be our ***Features*** or ***Predictors***.

## Notation
- $x_i$ subscript $i$ indexes observations in a data set; $i$ ranges from $1$ to $N$, the number of observations (rows) in the data set.
- $x_{i,j}$ subscript $j$ indexes predictors in a data set; $j$ ranges from $1$ to $P$, the number of predictors (columns) in the data set. Here we are referencing predictor $j$ from observation $i$.
- In matrix notation, lower case letters like $x$ represent vectors, and upper case bold face letters like $X$ represent matrices.


| file.inlinks                                                                                                                                                                                                                                                                        |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <ul><li>[[School/machine-learning/ml-study-guide.md\\|ml-study-guide]]</li><li>[[School/machine-learning/video-notes/ml01-the-craft-of-machine-learning.md\\|ml01-the-craft-of-machine-learning]]</li><li>[[School/machine-learning/quiz-notes/ml-quiz-1.md\\|ml-quiz-1]]</li></ul> |
