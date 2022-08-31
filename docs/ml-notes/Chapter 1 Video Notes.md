---
title: Chapter 1 Video Notes
category: ml-notes
share: true
tags: []
---

# 1: Introduction
> [!seealso]- Links
> [Youtube Video](https://www.youtube.com/watch?v=j0fqVLyZHf0&list=PLfe6IcA_dEWkcHFfBA6XSXW31H8t4XSbB)
> [Github](https://github.com/kjmazidi/Machine_Learning_2nd_edition)
> [TIOBE Reference](https://www.tiobe.com/tiobe-index/)
> 
## Uses for Machine Learning
Some key areas are:
- Computer Vision
- Natural Language Processesing (NLP) like spam detection
- Customer Segmentation for market research
- Data Analysis for things like credit fraud detection

## What is it?
Machine Learning is the intersection between A.I., Statistics, Probability, and CS. Mazidi's definition is:
> Machine Learning trains computers to accurately recognize patters in data for data anylysis, prediction, and action selection by autonomous agents.

Note that we must note some meanings:
- What accuracy is good enough
- How do we ethically use data
- Computers are better at recognizing *some* patterns

### Uses
**Data Analysis**:  Sometimes we just want to understand data, like with clustering algorithms
**Prediction**: Sometimes we want to predict outcomes on new data
Action
**Action Selection by Autonomous Agents**: The field of reenforcemnet learning trains autonomous agents often with deep learning and huge amounts of data

### Subcategories
![[../assets/img/Pasted image 20220817163910.png]]
We can divide it into two large categories: *Active Learning* on the right, and *Informative Learning*  on the left. The main field is the left, where how:
- *Supervised Learning* is deciding $y$ based on $x$.
- *Unsupervised Learning* is learning without a specified target

We further divide Supervised Learning into:
- *Regression*: Where the target is a real number like a selling price
- *Classification*: Where the target is membership in a set of classes like a binary calssifacation of credit risk

### Do We Need It?
![[../assets/img/Pasted image 20220817164343.png]]

We first usually come up with an algorithm that produces an output given an input. It is very deterministic. This usually works for normal applicaitons, like payroll. It just automates it. However, we can't solve problems with a bunch of rules. Like facial recognition. Instead we give a computer input, and give it an algorithm that produces a model for reading faces.

Machine learning's advantage is going through data quicker than any human could. And we have many algorithms that produce models of patterns they can reconize

## Terminology
ML grew out of math and science, so we see a lot of duplicate terminology. Like a table representing data:
![[../assets/img/Pasted image 20220817164732.png]]
- A **row** might be an *example*, *instance*, or *observation*
- A **column** might be an *attribute*, or *feature*
	- In *supervised learning*, we select a *target* column, while the rest are *predictors*.
- Data can be *quantitative*  and *numeric*, or *qualitative*, *categorical*, and *factors*.
	- The GPA, Hours, SAT Score above are numeric
	- Class membership is encoded given some quantitative term

## Challenges for Society
- What is the legal precedence of bad machine learning models
- Are algorithms fair? They can learn prejudice like in judicial sentencing
- There are security issues
- There are privacy issues, as privacy issues vary by country

And AI Hype:
![[../assets/img/Pasted image 20220817165329.png]]

## Notes on Learning M.L.
- Be systematic
	- Have a plan for learning
	- Keep a notebook
- Find Mentors!
	- Like in your organization
	- Online
- Do Reproducible Research
	- Take notes on your work
	- Document, Document, Document

Here is an example workflow:
![[../assets/img/Pasted image 20220817165538.png]]
Note that the green boxes are for management, the yellow boxes are for data wrangling, and the blue boxes are for machine learning. All of these steps take various teams with various fields of research

### Our Workflow
- We explore the data,
- Perform machine learning on the data
- Evaluate the results
This is with the goal of learningg algorithms

## R and Python
In the TIOBE Index (May 2020), Python was 3rd, while R was 10th. It was simply a measure of popularity. We shall learn the purely statistical language R first, then move on to python.

## More
All of this content is further explored in [[ml-chapter-1]] and tested on [[ml-quiz-1]]