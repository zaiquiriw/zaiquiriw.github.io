---
share: true
title: ML Work
---

# Machine Learning Work
Back in 2022 I took an introduction to Machine Learning with the wonderful [Karen Mazidi](https://www.linkedin.com/in/mazidiaiconsulting/) who gave us a large overview of both data science basics, and basic machine learning. The class was project based, with a focus on providing documentation of the process.

## Learning in R
We covered using R for a variety of algorithms:
- Linear Regression
- Logistic Regression
- Naive Bayes
- kNN
- k-means Clustering
- Decision Trees and Random Forests
- Support Vector Machines

As well as best practices for picking good attributes for analysis, cleaning up data in CSVs, and visualizing the results

## Learning in Python
We then pivoted to implementing these solutions in the Python package ecosystem, using:
- NumPy
- Pandas
- Scikit-Learn
- Seaborn

To implement all the algorithms we just learned. Python allowed us to branch into Neural Networks using *Keras* for the implementation of zero-shot classification

>[!note]
>We even touched on Hidden Markov Models and Bayesian nets, but not much on their implementation

## Continued Work
This class carried into my work in NLP, which happened to coincide with the emergence of Open-AI's ChatGPT. [[nlp overview|Check it out]] for work on more advanced neural nets

## The Projects
> [!seealso] While I will leave my original code open source on [github](https://github.com/zaiquiriw/ml-portfolio/tree/main) the following brief summaries will link to pdfs summarizing the projects.

- [[data exploration|Low Level Basics]]: I worked a little bit in C++ to implement basic statistics calculations, just to make sure I had the swing of things.
- ***Linear Models***: Assuming that problem, its input and output, are linearly related, there are multiple ways to create a predictive supervised model:
	- [[linear regression.pdf|Linear Regression]]
	- [[linear classification.pdf|Linear Classification]]: Naive Bayes and Logistic Regression
	- [[from scratch.pdf|Building From Scratch]]
- ***Similarities***: If instead of predicting a target value, we just wanted to understand the data, we have many different methods of breaking down complex (and high dimensional) data. This works hand in hand with dimensionality reduction. I have more on the subject [[similarities main.pdf|here]]. kNN and K-means algorithms were both good tools to improve the performance of our previous models.
	- [[similarities regression.pdf|Using similarties to improve regression]]
	- [[similarities classification.pdf|Using similarities to improve classification]]
	- [[Clustering.pdf|Clustering Spotify genres]]
	- [[Dimensionality.pdf|Failed dimensionality reduction of Spotify]]
- ***Support Vector Machines***: SVM's divide data in such a way that optimizes the margin between data. While we can perhaps visualize data being split by a line for classification, this method can not only be used for high dimensional classification but regression as well. I have a much better description [[SVM and ensemble.pdf|here]].
	- [[ClassificationSVM.pdf|Classifciaton with SVMs]]
	- [[RegressionSVM.pdf|Regression with SVMs]]
- ***Neural Networks***: While I explore neural networks in full in my future [[nlp overview|Natural Language Processing]] class, we still got some experience with how neural networks work with keras and tensorflow. Using RNN, CNN, and even finetuning on Google's MobileNet V2, I was able to create a pretty good [[keras_image_recognition.pdf|rice identification model]].


Out of anything I would recommend reading my [[keras_image_recognition.pdf|Keras Image Classification]] paper for a good picture of the progress made in this class. Needless to say this was maybe the second most impactful class of my degree, right before [[nlp overview|NLP]].