# Animal/Pokemon Type Classifier
## Real World Application
We wanted to tackle the issue of animal cruelty in laboratories while also appealing to several age groups. Thus, we decided to investigate patterns in one characteristic of pokemon, animals from a TV show, without using data that requires them to fight one another.

## Premise
In this hypothetical scenario, we are a team of pokemon researchers in the pokemon world and we want to build a classifier to help identify elemental types for newly discovered pokemon.

<img src = 'https://github.com/MirShahiduzzaman/AnimalTypeClassifier/assets/43242843/2fb2128a-7cb0-400f-9d69-09b25f898e83' height = '150'>

## Purpose
Why is a type classifier meaningful for pokemon research?
- Pokemon data is discovered in this world
- Studying types can be challenging

## What are Types?
They the element-based nature of a pokemon. For example, a Charmander is a fire type, a Bulbasaur a grass type, a Squirtle a water type, etc.

## Challenges
Typically the most accurate way to figure out a pokemon’s type is by studying their type weaknesses to different attack moves. For example, grass type pokemons are weak to fire type moves.
- Ethical concerns
- Complaints from advocacy groups

## Preprocessing
In order to best use our classifiers, we took out columns we knew for sure to be unrelated to the type:

![image](https://github.com/MirShahiduzzaman/AnimalTypeClassifier/assets/43242843/26c72275-c8c4-42aa-a018-aa906b085d22)

8th generation pokemon had missing column data. We decided to fill it in the csv, manually with the correct values from online sources as there weren’t too many missing values.

## Data Sets (6 Total)
- DF1: Only look at type_1 of pokemon
Removes 1 column (containing secondary type of pokemon)
- DF2: Remove all pokemon that have a second type
Removes 552 pokemon
- DF3: Duplicate data
This adds 552 pokemon
But this confuses the classifier because there are 2 of the same pokemon (have same stats) but 2 diff class labels

For each of the three datasets, we then created a numerical and categorical version: 3 x 2 = 6

## Binning for Categorical
Height, Weight, HP, Attack, Defense, Special Attack, Special Defense, Speed, Total Points, Catch Rate, Base Experience, Egg Cycle
Use equal frequency for most because most were skewed
Replaced outliers with upper bound

<img src = 'https://github.com/MirShahiduzzaman/AnimalTypeClassifier/assets/43242843/661847dc-3897-4224-9254-dabb26ef7f0b' height = '220'>
<img src = 'https://github.com/MirShahiduzzaman/AnimalTypeClassifier/assets/43242843/3655ade4-c0af-4eb3-8c41-1f9817787fce'>

## Class Distribution for type 1 for the 3 datasets
![image](https://github.com/MirShahiduzzaman/AnimalTypeClassifier/assets/43242843/08183847-3a49-4c2d-9330-6e853dfcb67e)

## Assumptions
- Every pokemon has the following features:
['status', 'type_1', 'height_m', 'weight_kg', 'total_points', 'hp', 'attack', 'defense', 'sp_attack', 'sp_defense', 'speed', 'catch_rate', 'base_experience', 'growth_rate', 'egg_type_1', 'egg_cycles']
- For dataset 1 and 2: Type 1 is the primary type of the pokemon
- For dataset 3: Type 1 and 2 are equally important

## Feature Selection
- For each dataset variation, we had to select the best features to use
- First, we set up the parameters for cross validation and dataset.
    - We wanted to optimize the best K for each run of selection.
- Next, we ran the feature selection using our K parameter for Chi2 and mutual info, on each of the 6 datasets.
    - We recorded multiple rounds of feature selected performance results to find the best K.

<img src = 'https://github.com/MirShahiduzzaman/AnimalTypeClassifier/assets/43242843/025bfc99-cc2c-4242-840e-dfc06e55a174' height = '150'>

## Results of Accuracy Testing
- Dropping class labels (types) made our classifiers more accurate
    - Dropping the 8 lowest classes gave us the best results without compromising our results too much
- Accuracy values for datasets B (categorical) were higher than A (continuous) in general
- K values for our feature selection worked best at around 15-25 for most datasets
- The top 3 classifiers after initially testing:
    - BernoulliNB     - .52
    - MultinomialNB - .51
    - DStree Gini     - .47

<img src = 'https://github.com/MirShahiduzzaman/AnimalTypeClassifier/assets/43242843/4fe32916-2562-4cb5-8093-669c3f837f77' height = '150'>

## Initial Mistakes
- Looking at strongest pokemon instead of the classifying by types
- Duplicating the pokemon with 2 types instead of removing them
    - This became our dataset 3; which had the least accuracy
<img src = 'https://github.com/MirShahiduzzaman/AnimalTypeClassifier/assets/43242843/b7bfba76-87e5-4206-9edf-e6bf646c4867' height = '200'>

## Classifiers
1. Naive Bayes 
    - Gaussian
    - Bernoulli
    - Multinomial
2. Decision Tree 
    - Entropy
    - Gini
3. K-Nearest Neighbors

## Ensemble
Using bagging and random forest classifiers did not significantly increase the accuracy of our best models.
- Ran multiple rounds of ensembling, testing different sample/feature parameter sizes.
- Bernoulli NB and Multinomial NB saw a 0.01 increase in accuracy.
  - Both scored 0.52
- Decision Tree Gini saw a 0.04 increase in accuracy.
  - Gini scored 0.51

## Conclusion
The highest accuracy from the models was 52% - 4 times more accurate than randomly guessing, but still too low.

Why could this be the case?
- We are missing vital information such as appearance, color, etc.
- Pokemon stats could vary wildly depending on the evolution state or the different forms that they have (Mega Evolution, Gigantamax, etc) 
- Limited data points as a result of removing a lot for dataset 1 and 2

## Next Steps
We can train image classifiers on pokemon photos.
- It is possible that similar types of pokemon tend to have similar physical features. For example, many electric type pokemon are yellow with jagged features.

![image](https://github.com/MirShahiduzzaman/AnimalTypeClassifier/assets/43242843/bd0e351f-6775-4b52-8863-e5e3cdbd0850)

<!--
## Summary
  As a Pokemon research team, we looked into the most ethical way to classify the type of Pokemon based on its stats. We first split the dataset into 6 datasets, each with their own attributes. We then removed unhelpful data in determining the type and selected the best K value for our models. 
  
  The next step was for us to run the models. The first model was Naive Bayes, for which we used Guassian, Bernoulli, and Multinomial. We found Guassian to be least effective compared to the other classifiers in the Naive Bayes. The Bernoulli Chi-Squared’s highest accuracy was 52%. The Multinomial provided the second highest accuracy with the Chi Square feature selection yielding an accuracy of 51%. We also used a decision tree. After optimizing and trimming, we learned the most accurate tree was the gini tree, with an accuracy of 47%. As for the K Nearest Neighbor, it provided the least accurate results, mainly due to the datasets having a high number of data points. 
  
  While completing the ensemble, we were able to increase the accuracy for each, but only by 0.01 for Multinomial, 0.04 for the decision tree, and 0.02 for the decision tree using the random forest classifier. As a result, we concluded that we cannot classify pokemon solely based on its stats, but we hypothesized that it would be possible given more data, such as color and appearance.
-->
