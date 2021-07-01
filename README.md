# Oscars Prediction

Best Picture Prediction for the Oscars.

<p align="center">
  <img src="https://github.com/Arcanus07/Oscars-Prediction/blob/main/Images/oscars-award.png">
</p>


## Intro

The Academy Awards, popularly known as the Oscars, are regarded as one of the most significant and prestigious awards in the entertainment industry. The awards are an international recognition of excellence in cinematic achievements, as assessed by the Academy's voting membership. 

The Academy Award for Best Picture is one of the Academy Awards presented annually by the Academy of Motion Picture Arts and Sciences (AMPAS). Best Picture is usually the final award of the night and is considered the most prestigious honor of the ceremony.

But what are the factors that could increase your chances to win the award for Best Picture? That's what we're here to find out.


## Motivation

When lockdown came into effect the movie industry was one of those that took a punch to the gut. With theaters closing and movies stopping production, there was no chance that we were going to have an award session during the pandemic. But this is where I wondered how the Academy Awards actually worked. Upon reading up on [Preferential Ballot](https://en.wikipedia.org/wiki/Preferential_voting), what I wanted to know were the features that were subconsciously contributing to a movie being selected. And that's how I went ahead and thought of making this project.


## Data

The data used in this project was obtained from varrious publicly avalaible websites which are listed below. 

The list of movies that were either nominated or won the Oscar's best picture award was scraped from [Oscars Best Picture](https://en.wikipedia.org/wiki/Academy_Award_for_Best_Picture)

Dataset of other movies was obtained from [IMDB Movies Dataset](https://www.kaggle.com/harshitshankhdhar/imdb-dataset-of-top-1000-movies-and-tv-shows)

Any missing information in the above two datasets was obtained using [OMDB API](https://www.omdbapi.com/)


## Modelling

In this project I've used the scikit-learn implementation of [Random Forest Classifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html). In this section, I shall explain more about this model.

### Decision Tree
Let us first go over decision trees as they are the building blocks of the random forest model. It’ll be much easier to understand how a decision tree works through an example which I've taken directly from [here](https://towardsdatascience.com/understanding-random-forest-58381e0602d2).

<p align="center">
  <img src="https://github.com/Arcanus07/Oscars-Prediction/blob/main/Images/decision%20tree%20example.png">
</p>

Imagine that our dataset consists of the numbers at the top of the figure to the left. We have two 1s and five 0s (1s and 0s are our classes) and we want to separate the classes using their features. The features are color (red vs. blue) and whether the observation is underlined or not.

Color seems like a pretty obvious feature to split by as all but one of the 0s are blue. So we can use the question, “Is it red?” to split our first node. You can think of a node in a tree as the point where the path splits into two — observations that meet the criteria go down the Yes branch and ones that don’t go down the No branch.

The No branch (the blues) is all 0s now so we are done there, but our Yes branch can still be split further. Now we can use the second feature and ask, “Is it underlined?” to make a second split.

The two 1s that are underlined go down the Yes subbranch and the 0 that is not underlined goes down the right subbranch and we are all done. Our decision tree was able to use the two features to split up the data perfectly.

### Random Forest Classifier
Random forest, like its name implies, consists of a large number of individual decision trees that operate as an ensemble. Each individual tree in the random forest spits out a class prediction and the class with the most votes becomes our model’s prediction.

<p align="center">
  <img src="https://github.com/Arcanus07/Oscars-Prediction/blob/main/Images/random%20forest.png">
</p>

The reason that the random forest model works so well is:

A large number of relatively uncorrelated models (trees) operating as a committee will outperform any of the individual constituent models.

## Problems With Data

What we have here is an [Imbalanced Dataset](https://www.kaggle.com/getting-started/100018). By this I mean, the amount of records of one class are much more than another. Here, the amount of movies that failed to win the Oscars is much more than those that won (obviously).

Why would this be a problem you ask? Well think about it. If 99% of your classification labels are 0s, and the remaining 1% are 1s, even if your model constantly predicts 0 for all records, you get an accuracy of 99%. 

In our situation the distribution of the labels is as follows

<p align="center">
  <img src="https://github.com/Arcanus07/Oscars-Prediction/blob/main/Images/data_distribution.png">
</p>

<p align="center">
  <img src="https://github.com/Arcanus07/Oscars-Prediction/blob/main/Images/data_distribution_graph.png">
</p>

There are two ways I have tried to deal with this issue.

### Downsampling
Here, we reduce the amount of the majority labels down to the amount of the minority labels

### Oversampling using SMOTE
Here, we increase the amount of the minority labels by creating synthetic data


## Results

I have used ROC AUC score as the metric of evaluation here.

### Downsampling
With downsampling, I obtain a roc_auc_score of 0.922
<p align="center">
  <img src="https://github.com/Arcanus07/Oscars-Prediction/blob/main/Images/downsampling_score.png">
</p>

### Oversampling
With upsampling, I obtained a roc_auc_scpre of 0.983
<p align="center">
  <img src="https://github.com/Arcanus07/Oscars-Prediction/blob/main/Images/oversampling%20score.png">
</p>
