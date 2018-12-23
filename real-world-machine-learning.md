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

#### Log odds
The odds ratio is one way of expressing probability. You’ve undoubtedly heard someone say that a (favorite) team’s chance of winning is 3 to 1. Odds are the probability of success (for example, winning) divided by the probability of failure (losing).

Mathematically, this can be expressed as follows:
*Odds(A) = P(A) / P(~A) = The probability of A divided by the probability of not A*

So 3-to-1 odds is equivalent to 0.75 / 0.25 = 3 and log(3) = 0.47712...

If A were a fair coin toss, the odds of heads would be 0.5 / 0.5 = 1. Log(1) = 0. It turns out that the log(Odds) can take on any real-valued number. A log odds value near *-infinity* denotes a highly unlikely event. A value near *infinity* indicates near certainty, and log(1) = 0 indicates an even random change. Using log-odds instead of regular prob- abilities is a mathematical trick that makes certain computations easier, because unlike probabilities, they’re not limited to values between 0 and 1.

