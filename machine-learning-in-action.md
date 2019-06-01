# Machine Learning in Action
Author: Peter Harrington

## Chapter 1: Machine learning basics
- A **training set** is the set of training examples used to train the machine learning 
algorithms. Each training example has some features and one target variable.
- The **target variable** is what we'll be trying to predict with the machine learning 
algorithm.
- The machine learns by finding some relationship between the features and the target
variable.
- Features (or attributes) are the individual measurements that, when combined with
other features, make up a training example. This is usually columns in a training or
test set.
- **Test set** ...
- In **classification**, the job is to predict what class an instance of data should
fall into.
- **Regression** is the prediction of a numeric value, e.g. best-fit line drawn through
some data points to generalize the data points.
- Classification and regression are examples of **supervised learning**. This set of
problems is known as supervised because we're telling the algorithm what to predict.
- In **unsupervised learning**, there's no label or target value given for the data.
- A task where we group similar items together is known as **clustering**.
- **Density estimation**'s goal is to find statistical values that describe that data.
- Important question before choosing an algorithm: **what are you trying to get out of 
the data**? Do you want a probability that it might rain tomorrow, or to find groups of
voters with similar interests? If you're trying to predict or forecast a target value,
then you need to look into *supervised learning*. If not, then *unsupervised learning*.
If you've chosen supervised learning, what's your target value? Is it a discrete value
like Yes/No, 1/2/3, A/B,C, or Red/Yellow/ Black? If so, then look into classification.
If the target value can take on a number of values, say any value from 0.00 to 100.00,
or -999 to 999, or +infinity to -infinity, then look into regression. Etc.
- Things to know about your data: 
    - Are the features nominal or continuous?
    - Are there missing values in the features?
    - Are there outliers in the data?
- There's no single answer to what the best algorithm is or what will give you the
best results: you're going to have to try different algorithms and see how they 
perform.