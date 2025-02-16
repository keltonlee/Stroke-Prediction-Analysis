# Diabetes prediction analysis

Analyze [Diabetes, Hypertension and Stroke Prediction](https://www.kaggle.com/datasets/prosperchuks/health-dataset) dataset, using ML and DL approaches, to find out the relations between features and targets and the factors of diabetes.

## Dataset
The dataset(health_data.csv) used in this project is from [Kaggle](https://www.kaggle.com/datasets/prosperchuks/health-dataset), which contains 70692 data with 17 features variables and 1 target variable.

## Code
The main code(diabetes_prediction.ipynb) is in the form of a Jupyter notebook (ipynb file). It covers data preprocessing, data visualization and modeling of the dataset.

## Data Pre-processing and Visualization
(1) This dataset has no null values and all the data type are already in the form of int64. The dataset has 70692 data and includes 15 features.   

(2) Visualize the corelation between each features and targets.
<img width="708" alt="截圖 2023-01-27 下午9 11 34" src="https://user-images.githubusercontent.com/68526411/215095097-7725bfcd-dd01-4a32-849b-10a610d164e2.png"> 
 
It's a pity that most of the data are binary and that there are no really high correlation features in the dataset, GenHlth may be the one which have higher correlation. So I also look at the correlation graph between the features and the target to pick out the three most high correlation feature(Age, HighChol, GenHlth) to do further prediction.

(3) Scale the data(Standard scale and Min Max scale) to fit in the model so that every feature are in the similar scale. Later I only use Standard scale since most of the data are already binary(0 and 1).

(4) Try out PCA to reduce the dimensionality of the dataset for expecting a faster and a better performance of each models.

## Algorithms and framework
PyTorch models(Logistic Regression and Sequential Neural Network), Scikit-learn models(LogisticRegression, SVM(linear), SVM(rbf), KNN, Decision Tree Classifier, Perceptron, Random Forest Classifier) to predicting the baseline, after scaling, after doing PCA to lower the dimension, and after feature selections to analyze. I choose these models because these are the most basic and standard models, and I would like to learn about the principles and the changes through every step of the process.

## Results
The performance of models turns out not to improve much after doing some feature selection, scaling and tuning the hyper parameters(neighbor of knn, n_estimator, learning rate, epoch of deep learning model, etc.). The accuracy are mostly 0.7. This result may due to the binary value of the features(11 out of 15 are binary). Even so, there are still some interesting results I will like to share.

#### Feature Selection: 
After doing feature selection picking out 3 highest correlation feature with coef 0.3 and above using scikit-learn, I found that Decision tree and perceptron both have improve much(accuracy from 0.5 to 0.7), whereas others do not. I have tried to pick out less feature but picking out 3 performs the best. This could be possible because the more features there are, the more complex the model will be for these two simple models. Take decision tree as example, the three will have a very deep depth if there are a lot of features, this will lead to a poor performance of the model. 

#### Scaling:
After doing standard scaling to scale all of the features into Gaussian distribution with 0 mean and unit variance. I found it very interesting because Logistic Regressio has a substantial improve(accuracy from 0.5 to 0.78), even from the first epoch. This may probably due to the calculation of the logistic regression. Logistic regression or even SVM model uses the feature value and correlation to calculate the dot product to predict the final result. Therefore, feature with large value may dominate the prediction and this is really a flaw of these models which calculate the dot product. Scaling the feature may help mitigate this problem since all features are on the similar scale and all the feature may contribute equally to the performance of the model. 
<img width="541" alt="截圖 2023-01-03 上午10 53 49" src="https://user-images.githubusercontent.com/68526411/215100815-877a2fcc-0ccc-44d4-a9e3-0d83b226e646.png">

#### PCA:
Principal component analysis (PCA) is a technique that can be used to reduce the dimensionality of a dataset by transforming the features into a new set of linearly uncorrelated components. First, I would like to visualize Scree plot which represents the eigenvalues that define the magnitude of eigenvectors in order to select the number of components for PCA.  
<img width="414" alt="截圖 2023-01-28 下午7 02 21" src="https://user-images.githubusercontent.com/68526411/215263091-91589a43-d250-4009-9d67-1091e40ab775.png">
  
It turns out that the bend occurs at index 1 and index 4. As a consequence, I tried out the number of components from 1 to 4. Comparing with the baseline, linear models such as LogisticRegressions, SVC(kernel='linear') and RandomForestClassifier performs worse(accuracy from 0.7 to 0.55). I found that this result may be caused by several reasons. First, models such as DecisionTree or RandomForest, these tree-based models are highly capable of handling high-dimensional data quite well, as they are able to split the feature space into smaller regions based on the values of individual features. In such case, using PCA may not be necessary and may even hurt the performance. Second, Linear models are sensitive to correlated features, thus PCA may decorrelate these features and have the possibilites to somehow exacerbate the performance.

#### Hyperparameter Tuning:
Finally, I use RandomizedSearchCV to search for the best parameters of each model.
```python
RandomizedSearchCV(model, param_distributions=param_dist, n_iter=10, cv=5, n_jobs=-1)
```
Results:  
LogisticRegression: {'penalty': 'l2', 'C': 0.0018329807108324356}  
KNeighborsClassifier: {'weights': 'uniform', 'n_neighbors': 25}  
DecisionTreeClassifier: {'min_samples_split': 21, 'min_samples_leaf': 25, 'max_depth': 10}  
Perceptron: {'max_iter': 5000, 'alpha': 0.012742749857031334}  
RandomForestClassifier: {'n_estimators': 96, 'min_samples_split': 9, 'min_samples_leaf': 14, 'max_depth': 23, 'criterion': 'gini', 'bootstrap': True}

## What did I learned
When I was in my junior year, without any ML/AI knowledge background, I resolutely participated in the National Intelligent Manufacturing Big Data Analysis Competition. During the proccess, I'v been learing all the basic concepts of machine learning such as data preprocessing, data explore, building up models, and etc. I found it very interesting because there were a wide variety of models and each with unique machanisms. Besides, there were so many different technique that can be use to improve the performance of a single model. I really learned a lot during this time.

Surprisingly, our team reached the final round of the competition. Even though we lost in the final, the whole process was quite exciting for me. And thus, I decided to take a master course which was not run by our department -- Introduction to Artificial Intelligence. Through the course, the course contents and homeworks constantly reinforced my knowledge of ML and DL. In the final of the semester, I decided to do this project to demenstrate what I'v learned during the competition and the course. 

This is not the end, there are so much remains to be learned!
