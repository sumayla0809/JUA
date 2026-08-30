# Machine Learning — Exam Notes

## 1. Machine Learning

### Definition

**Machine Learning (ML)** is a branch of Artificial Intelligence (AI) that enables computers to **learn patterns from data and make predictions or decisions without being explicitly programmed for every task**.

### Basic Idea

```text
Data → Learning Algorithm → Model → Prediction/Decision
```

### How Machine Learning Works

1. **Collect Data** — Gather relevant data.
2. **Prepare Data** — Clean and organize the data.
3. **Train Model** — The algorithm learns patterns from training data.
4. **Test Model** — Evaluate the model using unseen data.
5. **Prediction** — Use the trained model to predict new values or classes.

### Important Terms

* **Data:** Information used by the ML system.
* **Feature:** Input variable used to make a prediction.
* **Label/Target:** Output that the model tries to predict.
* **Training Data:** Data used to train the model.
* **Testing Data:** Unseen data used to evaluate the model.
* **Model:** A mathematical representation learned from data.
* **Prediction:** Output produced by the trained model.

### Traditional Programming vs Machine Learning

| Traditional Programming      | Machine Learning            |
| ---------------------------- | --------------------------- |
| Rules + Data → Output        | Data + Output → Rules/Model |
| Rules are explicitly written | Rules are learned from data |
| Programmer defines logic     | Algorithm learns patterns   |

### Points to Remember

* ML is a **subset of Artificial Intelligence**.
* ML learns patterns from **data**.
* A trained model is used to make predictions on **new/unseen data**.
* Good-quality data is important for good model performance.

---

# 2. Types of Machine Learning

The major types of Machine Learning are:

1. **Supervised Learning**
2. **Unsupervised Learning**
3. **Reinforcement Learning**

## 2.1 Supervised Learning

In supervised learning, the model learns from **labeled data**, where both input and correct output are known.

```text
Input + Correct Output → Learning Algorithm → Trained Model
```

### Examples

* Predicting house prices
* Email spam detection
* Student result prediction
* Disease classification

### Main Types

* **Regression** — Predicts a continuous numerical value.
* **Classification** — Predicts a category/class.

### Examples

```text
Regression:
House size → House price

Classification:
Email → Spam / Not Spam
```

---

## 2.2 Unsupervised Learning

In unsupervised learning, the model learns from **unlabeled data** and finds hidden patterns or structures.

```text
Unlabeled Data → Algorithm → Patterns/Groups
```

### Examples

* Customer segmentation
* Grouping similar products
* Finding unusual patterns

### Common Techniques

* **Clustering**
* **Association**
* **Dimensionality Reduction**

### Example

```text
Customer Data → Clustering → Customer Groups
```

---

## 2.3 Reinforcement Learning

In reinforcement learning, an **agent learns by interacting with an environment** and receiving rewards or penalties.

```text
Agent → Action → Environment → Reward/Penalty
```

The objective is to **maximize the total reward**.

### Important Terms

* **Agent:** Learner/decision maker.
* **Environment:** World in which the agent operates.
* **Action:** Decision taken by the agent.
* **Reward:** Feedback received after an action.
* **State:** Current situation of the environment.

### Examples

* Game playing
* Robot navigation
* Autonomous systems

---

## Comparison of ML Types

| Type          | Data            | Main Goal     | Example                |
| ------------- | --------------- | ------------- | ---------------------- |
| Supervised    | Labeled         | Prediction    | House price prediction |
| Unsupervised  | Unlabeled       | Find patterns | Customer clustering    |
| Reinforcement | Feedback/Reward | Learn actions | Game playing           |

### Points to Remember

* **Supervised → Labeled data**
* **Unsupervised → Unlabeled data**
* **Reinforcement → Reward/Penalty**
* Regression predicts **continuous values**.
* Classification predicts **categories/classes**.

---

# 3. Applications of Machine Learning

Machine Learning is used in many real-world applications.

### 1. Healthcare

* Disease prediction
* Medical image analysis
* Patient risk prediction

### 2. Finance

* Fraud detection
* Credit scoring
* Stock/market prediction

### 3. E-Commerce

* Product recommendation
* Customer behavior analysis
* Personalized advertisements

### 4. Transportation

* Traffic prediction
* Route optimization
* Autonomous vehicles

### 5. Education

* Student performance prediction
* Personalized learning
* Automated evaluation

### 6. Cybersecurity

* Intrusion detection
* Malware detection
* Anomaly detection

### 7. Natural Language Processing

* Chatbots
* Language translation
* Sentiment analysis
* Spam detection

### 8. Computer Vision

* Face recognition
* Object detection
* Image classification

### Points to Remember

* ML is useful wherever **data contains patterns that can be learned**.
* Major application areas include **healthcare, finance, e-commerce, transportation, education, cybersecurity, NLP, and computer vision**.

---

# 4. Supervised Learning

### Definition

**Supervised Learning** is a type of ML in which a model learns a mapping between **input variables (X)** and a known **output/target (Y)** using labeled training data.

```text
X (Input) + Y (Target) → Learning Algorithm → Model
```

After training:

```text
New X → Model → Predicted Y
```

### Mathematical Representation

The model learns a function:

$$
Y = f(X)
$$

where:

* **X** = Input/features
* **Y** = Target/output
* **f** = Learned function/model

### Types of Supervised Learning

#### 1. Regression

Used when the output is a **continuous numerical value**.

Examples:

* Predict house price
* Predict temperature
* Predict salary

#### 2. Classification

Used when the output belongs to a **specific class/category**.

Examples:

* Spam / Not Spam
* Pass / Fail
* Disease / No Disease

### General Steps

```text
1. Collect labeled data
2. Preprocess data
3. Split data into training and testing data
4. Train the model
5. Test/evaluate the model
6. Use model for prediction
```

### Points to Remember

* Supervised learning requires **labeled data**.
* Input is represented by **X**.
* Target/output is represented by **Y**.
* Main categories are **Regression and Classification**.

---

# 5. Linear Regression

### Definition

**Linear Regression** is a supervised learning algorithm used to predict a **continuous numerical value** by finding a linear relationship between input and output.

### Example

Predicting house price based on house size.

```text
House Size → Linear Regression → Predicted Price
```

### Simple Linear Regression Equation

$$
\hat{y} = b_0 + b_1x
$$

Where:

* **ŷ** = Predicted output
* **b₀** = Intercept
* **b₁** = Slope/Coefficient
* **x** = Input feature

### Meaning of Slope

The slope **b₁** represents how much the predicted value changes when **x increases by one unit**.

### Multiple Linear Regression

When there are multiple input features:

$$
\hat{y} = b_0 + b_1x_1 + b_2x_2 + ... + b_nx_n
$$

Example:

```text
House Price = b₀ + b₁(Size) + b₂(Bedrooms) + b₃(Age)
```

### Cost Function

Linear Regression commonly uses **Mean Squared Error (MSE)**:

$$
MSE = \frac{1}{n}\sum_{i=1}^{n}(y_i-\hat{y_i})^2
$$

Where:

* **n** = Number of observations
* **yᵢ** = Actual value
* **ŷᵢ** = Predicted value

The objective is to **minimize the error/cost**.

### Least Squares Idea

Linear regression finds the line that minimizes the **sum of squared differences between actual and predicted values**.

### Advantages

* Simple and easy to understand.
* Fast to train.
* Easy to interpret.
* Useful for predicting continuous values.

### Limitations

* Assumes a linear relationship.
* Sensitive to outliers.
* Performance can be poor for complex nonlinear relationships.

### Points to Remember

* Linear Regression is used for **regression problems**.
* Output is generally **continuous**.
* Basic equation: **ŷ = b₀ + b₁x**.
* It minimizes the prediction error, commonly using **MSE**.
* The best-fit line is obtained using the **least-squares principle**.

---

# 6. Logistic Regression

### Definition

**Logistic Regression** is a supervised learning algorithm mainly used for **classification problems**.

Despite its name, Logistic Regression is primarily a **classification algorithm**, not a regression algorithm.

### Example

```text
Student Information → Logistic Regression → Pass / Fail
```

### Why Logistic Regression?

A linear equation can produce values outside the range 0 to 1. Logistic Regression uses the **Sigmoid Function** to convert the output into a probability between **0 and 1**.

### Sigmoid Function

$$
\sigma(z)=\frac{1}{1+e^{-z}}
$$

where:

$$
z=b_0+b_1x
$$

Therefore:

$$
P(y=1|x)=\frac{1}{1+e^{-(b_0+b_1x)}}
$$

### Output

The sigmoid function produces:

$$
0 \leq P \leq 1
$$

The probability is then converted into a class using a **threshold**.

Common threshold:

```text
If P ≥ 0.5 → Class 1
If P < 0.5 → Class 0
```

### Example

Suppose:

```text
Predicted probability = 0.82
Threshold = 0.5
```

Since:

```text
0.82 ≥ 0.5
```

Prediction:

```text
Class 1
```

### Binary Classification

Logistic Regression is commonly used for **binary classification**:

```text
0 → Negative class
1 → Positive class
```

Examples:

* Spam / Not Spam
* Pass / Fail
* Yes / No
* Disease / No Disease

### Cost Function

Logistic Regression commonly uses **Log Loss / Binary Cross-Entropy**:

$$
J(\theta)=-\frac{1}{n}\sum_{i=1}^{n}
[y_i\log(\hat{y_i})+(1-y_i)\log(1-\hat{y_i})]
$$

The objective is to **minimize the cost function**.

### Advantages

* Simple and efficient.
* Easy to interpret.
* Produces probabilities.
* Works well for many binary classification problems.

### Limitations

* Works best when classes can be separated reasonably well by a linear decision boundary.
* Sensitive to irrelevant features and multicollinearity.
* May not perform well on highly complex nonlinear data without feature engineering.

### Points to Remember

* Logistic Regression is mainly used for **classification**.
* It uses the **Sigmoid function**.
* Sigmoid output is between **0 and 1**.
* Output represents a **probability**.
* A threshold converts probability into a class.
* Common threshold is **0.5**.
* Common loss function is **Binary Cross-Entropy/Log Loss**.

---

# 7. Algorithms / Algorithm Code

## 7.1 Algorithm for Linear Regression

### Algorithm

```text
Step 1: Start
Step 2: Load the dataset.
Step 3: Select input features X and target variable Y.
Step 4: Split the dataset into training and testing data.
Step 5: Create a Linear Regression model.
Step 6: Train the model using training data.
Step 7: Predict output using testing data.
Step 8: Evaluate the model using an appropriate regression metric.
Step 9: Stop
```

### Python Code

```python
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score

# X = input feature(s)
# y = target/output

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# Create Linear Regression model
model = LinearRegression()

# Train the model
model.fit(X_train, y_train)

# Make predictions
y_pred = model.predict(X_test)

# Evaluate the model
mse = mean_squared_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)

print("MSE:", mse)
print("R2 Score:", r2)
```

### Important Functions/Keywords

* `train_test_split()` → Splits data into training and testing sets.
* `LinearRegression()` → Creates the linear regression model.
* `model.fit()` → Trains the model.
* `model.predict()` → Generates predictions.
* `mean_squared_error()` → Calculates MSE.
* `r2_score()` → Calculates R² score.

---

## 7.2 Algorithm for Logistic Regression

### Algorithm

```text
Step 1: Start
Step 2: Load the dataset.
Step 3: Select input features X and target variable Y.
Step 4: Split the dataset into training and testing data.
Step 5: Create a Logistic Regression model.
Step 6: Train the model using training data.
Step 7: Predict class labels using testing data.
Step 8: Evaluate the classification model.
Step 9: Stop
```

### Python Code

```python
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, confusion_matrix

# X = input feature(s)
# y = target/class label

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# Create Logistic Regression model
model = LogisticRegression()

# Train the model
model.fit(X_train, y_train)

# Predict class labels
y_pred = model.predict(X_test)

# Calculate accuracy
accuracy = accuracy_score(y_test, y_pred)

# Display confusion matrix
cm = confusion_matrix(y_test, y_pred)

print("Accuracy:", accuracy)
print("Confusion Matrix:")
print(cm)
```

### Important Functions/Keywords

* `train_test_split()` → Splits the dataset into training and testing data.
* `LogisticRegression()` → Creates the logistic regression model.
* `model.fit()` → Trains the model.
* `model.predict()` → Predicts class labels.
* `accuracy_score()` → Calculates classification accuracy.
* `confusion_matrix()` → Shows classification results using TP, TN, FP, and FN.

---

# Linear Regression vs Logistic Regression

| Feature      | Linear Regression   | Logistic Regression |
| ------------ | ------------------- | ------------------- |
| Type         | Supervised Learning | Supervised Learning |
| Main Use     | Regression          | Classification      |
| Output       | Continuous value    | Class/probability   |
| Function     | Linear equation     | Sigmoid function    |
| Example      | House price         | Spam detection      |
| Common Loss  | MSE                 | Log Loss            |
| Output Range | Any real value      | 0 to 1 probability  |

## Final Points to Remember

```text
Machine Learning → Learning from data

Types:
1. Supervised → Labeled data
2. Unsupervised → Unlabeled data
3. Reinforcement → Reward/Penalty

Supervised Learning:
1. Regression → Continuous output
2. Classification → Categorical output

Linear Regression:
ŷ = b₀ + b₁x
→ Continuous prediction
→ Uses best-fit line
→ Common cost: MSE

Logistic Regression:
σ(z) = 1 / (1 + e⁻ᶻ)
→ Classification
→ Probability between 0 and 1
→ Threshold → Class
→ Common loss: Log Loss
```
