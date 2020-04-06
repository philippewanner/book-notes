# 1. What is machine learning?

## 1.1 Understand how machines learn
- When we talk about human learning, we distinguish between memorization, and true intelligence.
- When children play in groups, they observe how others respond to their actions. Their future social behaviors are informed by this experience.
- They assess each new situation based on the features it has in common with past situations. Their learning is more than gathering knowledge. They're building what might be called *insight*.
- The ability to generalize, to apply knowledge gained through training to new unseen examples, is a key characteristic of both human and machine learning.
- What's the difference between AI and Machine Learning? ML is one form of AI, and AI is far broader subject encompassing such areas as robotics, language processing, and computer vision systems. *A computer program is said to learn, if its performance of a certain task, as measured by a computable score, improves with experience.*
- The ability to generalize: learning from examples (experience) and the capacity to apply what they've learned to new, unseen cases.
- It can take on problems that can't be managed manually because of the huge amount of data that must be processed. ML can sometimes find relationships so subtle that no amount of manual scrutiny would ever discover them. And when many such 'weak' relationships are combined, they become strong predictors.

Problem | Description | Example use cases
--- | --- | ---
Classification | Determine the discrete class to which each individual belongs, based on input data | Spam filtering, sentiment analysis, fraud detection
Regression | Predict the real-valued output for each individual, based on input data | Stock-market prediction, demand forcasting, price estimation, ad bid optimization, risk management
Recommendation | Predict which alternatives a user would prefer | Product recommendation, job recruiting, Netflix Prize, online dating, content recommendation
Imputation | Infer the values of missing input data | Incomplete patient medical records, missing customer data, census data

## 1.2 Using data to make decisions
### 1.2.1 Traditional approach
- People bring all sorts of conscious and unconscious biases to the decision-making process.
- Manually finding effective filters becomes harder and harder - if not impossible - as the filtering system grows in complexity
- The business rules become so complicated and opaque that debugging them and ripping out old, irrelevant rules becomes virtually impossible.
- The construction of your rules has no statistical rigor. You're pretty sure that better 'rules' can be found by better exploration of the data, but can't know for sure.
- As the pattern of loan repayment change over time - perhaps due to changes in the population of applicants - the system doesn't adapt to those changes. To stay up to date, the system needs to be constantly  adjusted.

==> the sytem doesn't automatically learn from data

### 1.2.2 Machine learning approach
- Completely automated nature of the process
- Optimal decisions *directly from the data* without having to arbitrary hard-code decision rules.
- The data provides the foundation for deriving insights about the problem at hand.
- Uses historical *training data* to predict the best course of action.
- Training data consists of the input data, along with the known outcome.
- Input data consists of a set of *features* - numerical or categorical metrics that capture the relevant aspects of each application, such as credit score, gender, occupation.
- Historical data (labeled training data) trains the machine-learning model.
- As new unlabeled data come in, predictions of the probability are generated instantaneously from the application data.
- ML modeling determines how the input data can be used to best predict the outcome. By finding and using patterns in the training set, ML produces a model that produces a prediction of the outcome based on the unlabeled data.
- Most traditional statistical business models fall into the category *parametric models*. This models use simple, fixed equations to express the relationship between the outcome and the inputs. Data is then used to learn the best values of the unknown terms in the equation. Approaches such as linear regression, logistic regression, and autoregressive models fit under this category. (See chapter 3)
- With the log odds, the optimal values of each coefficient of the equation are learned from thousands of training data examples (historical data).
- Parametric models work well when you have a prior understanding of the relationship between your inputs and the response you're trying to depict. Linear algorithms can be easier to explain and reason about, and they can be faster to compute and scale to larger datasets.
- In the real world, you're often presented with problems for which such transformations aren't possible to guess. You need more flexible models that can automatically discover complex trends and structure in data without being told what the pattern look like: these are *nonparametric* machine-learning algorithms. Examples of nonparametric algorithms: k-nearest neighbors, kernel smoothing, support vector machines, decision trees, and ensemble methods.

#### Log odds
The odds ratio is one way of expressing probability. You’ve undoubtedly heard someone say that a (favorite) team’s chance of winning is 3 to 1. Odds are the probability of success (for example, winning) divided by the probability of failure (losing).

Mathematically, this can be expressed as follows:
*Odds(A) = P(A) / P(~A) = The probability of A divided by the probability of not A*

So 3-to-1 odds is equivalent to 0.75 / 0.25 = 3 and log(3) = 0.47712...

If A were a fair coin toss, the odds of heads would be 0.5 / 0.5 = 1. Log(1) = 0. It turns out that the log(Odds) can take on any real-valued number. A log odds value near *-infinity* denotes a highly unlikely event. A value near *infinity* indicates near certainty, and log(1) = 0 indicates an even random change. Using log-odds instead of regular prob- abilities is a mathematical trick that makes certain computations easier, because unlike probabilities, they’re not limited to values between 0 and 1.

### 1.2.3 Five advantages to machine learning
- *accurate*: ML use data to discover the optimal decision-making engine for your problem. As you collect more data, the accuracy can increase automatically.
- *automated*: as answers are validated or discarded, the ML model can learn new patterns automatically. This allows users to embed ML directly into an automated workflow.
- *fast*: ML can generate answers in a matter of milliseconds as new data streams in, allowing systems to react in real time.
- *customazable*: Many data-driven problems can be addressed with machine learning. ML models are custom built from your own data, and can be configured to optimize whatever metric drives your buisness.
- *scalable*: ML easily scales to handle increased data rates. Some ML algorithms can scale to handle large amounts of data on many machines in the cloud.

### 1.2.4 Challenges
- Acquiring data in a usable form. Data scientists spend 80% of their time on data preparation. Extracting useful data from the residue can be tedious and messy work.
- Formulating the problem so that maching learning can be applied, and will yield a result that's actionable and measurable.


## 1.3 Following the ML workflow: from data to deployment
The main workflow introduces how to integrate machine-learning models into your applications or data pipelines.
*ML workflow* has five main components:
- data preparation
- model building
- evalutation
- optimization
- predictions on new data
Most real-world machine-learning applications require revisiting each step multiple times in an iterative process.
From historical input data you can build a model using an ML algorithm. You then need to evaluate the performance of the model, and optimize accuracy and scalability to fit your requirements. With the final model, you can make predicitons on new data.

<img src="https://github.com/philippewanner/book-notes/blob/master/real-world-machine-learning-media/figure-1.7.png" width="250">
      
### 1.3.1 Data collection and preparation
- Collecting and preparing data for machine-learning systems usually entails getting the data into tabular format.

### 1.3.2 Learning a model from data
- The first part of building a successful machine-learning system is to ask a question that can be answered by the data.
- With a Person table (name, age, income, marital status), you could build an ML model to predict whether a person is married or single. The *Marital status* variable is the *target* or *label* and the remaining variables are the *features*. The job of the ML algorithm is to find how the set of input features can successfully predict the target. For people whose marital status is unknown, you can use the model to predict marital status based on the input variables.

<img src="https://github.com/philippewanner/book-notes/blob/master/real-world-machine-learning-media/figure-1.9.png" width="250">

- ML algorithm performs the mapping from input features to output data.
- Some algorithms are relative immune to uninformative features (ex: name feature to predict martial status), see Chap. 3.
- Valuable information can sometimes be extracted from seemingly uninformative features.
- Every machine-learning system is about building models and using those to make predictions.

Initial structure of ML workflow program:
```python
data = load_data("data/people.csv")
model = build_model(data, target="Marital status")
new_data = load_data("data/new_people.csv")
predictions = model.predict(new_data)
```

### 1.3.3 Evaluating model performance
- To validate the performance of the model, you take out some of the data and pretend you don't know the target variable.
- You then build a model on the remaining data and use the held-out data (testing data) to make predictions.

### 1.3.4 Optimizing model performance
Use the results of your model evaluation to go back and make the model better. You can achieve better model accuracy in 3 ways:
1. Tuning the model parameters
2. Selecting a subset of features
3. Preprocessing the data


## 1.4 Boosting model performance with advanced techniques

### 1.4.1 Data preprocessing and feature engineering
- Extract additional value from the data that might improve your model performance
- Specific knowledge goes into deciding the data to collect, and this valuable domain knowledge can also be used to extract value
from the collected data, in effect *adding to the features* of the model before model building. We call this process *feature engineering*.
Important examples of features engineering:
- Dates and times
- Location
- Digital media

### 1.4.2 Improving models continually with online methods
- Most traditional ML models are static or only rarely rebuilt.
- In many cases, you'll have data and predictions flowing back into the system, and you want the model to improve with time and adapt to changes in the data. Several ML algorithms support this type of *online learning*.

## 1.5 Summary
- Machine-learning algorithms are distinguished from rule-based systems in that they create their own models based on data. Supervised ML systems generalize by learning from the features of examples with known results.
- Machine learning is often more accurate, automated, fast, customizable, and scalable than manually constructed rule-based systems.
- Machine-learning challenges include identifying and formulating problems to which ML can be applied, acquiring and transforming data to make it usable, find- ing the right algorithms for the problem, feature engineering, and overfitting.
- The basic machine-learning workflow consists of data preparation, model build- ing, model evaluation, optimization, and predictions on new data.
- Online learning models continually relearn by using the results of their predic- tions to update themselves.

## 1.6 Terms from this chapter
Word | Definition
| --- | --- |
|instance or example|A single object, observation, transaction, or record.|
|target or label|The numerical or categorical (label) attribute of interest. This is the variable to be predicted for each new instance.|
|features|The input attributes that are used to predict the target. These also may be numerical or categorical.|
|model|A mathematical object describing the relationship between the features and the target.|
|training data| The set of instances with a known target to be used to fit an ML model.|
|recall|Using a model to predict a target or label.|
|supervised machine learning|Machine learning in which, given examples for which the output value is known, the training process infers a function that relates input values to the output.|
|unsupervised machine learning|Machine-learning techniques that don’t rely on labeled examples, but rather try to find hidden structure in unlabeled data.|
|ML workflow|The stages in the ML process: data preparation, model building, evaluation, optimization, and prediction.|
|online machine learning|A form of machine learning in which predictions are made, and the model is updated, for each new example.|
