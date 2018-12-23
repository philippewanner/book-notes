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
- 
