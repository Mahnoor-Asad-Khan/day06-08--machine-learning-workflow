# Day 06–08 — End-to-End Machine Learning Workflow

## Learning Journey

This project was where I finally started putting the different pieces of an ML workflow together instead of learning them in isolation.

I started with the California housing dataset and worked through the process of investigating the data, cleaning it, preprocessing it, splitting it into training and testing sets, and then actually training and evaluating ML models.

The main goal wasn't just to get the lowest RMSE possible. I wanted to understand **why a model performs the way it does and what happens when we change its configuration.** Working on the model research-style. 

---

## What I Learned

### 1. Data Investigation & Preprocessing

I worked with the California housing dataset and investigated things like:

* Dataset shape and data types
* Missing values
* Duplicate samples
* Numerical vs categorical features
* Feature distributions
* Correlations between features
* Potentially interesting patterns and anomalies

I also learned how to build a preprocessing pipeline that handles:

* Missing numerical values using median imputation
* Categorical features using one-hot encoding
* Numerical feature scaling

One of the biggest things I understood here was **data leakage** and why preprocessing should be fitted only on the training data.

---

### 2. Train/Test Splitting

I learned why we separate the dataset into training and testing data.

The model should learn from the training set and only see the test set when we're evaluating how well it generalizes to unseen data.

This became especially important later when comparing different models and hyperparameters.

---

### 3. Linear Regression

I started with Linear Regression as a baseline.

```text
Mean CV RMSE: ~$68,595
```

The model performed reasonably as a baseline, but the dataset contains many nonlinear relationships that a purely linear model cannot represent well.

This gave me a useful starting point rather than immediately jumping into a more complicated model.

---

### 4. Decision Trees

Next, I tried a Decision Tree Regressor.

The unrestricted tree had:

```text
Training RMSE: $0
Test RMSE: ~$68,307
```

That was a pretty obvious sign of overfitting.

I then experimented with different `max_depth` values.

The best result was around:

```text
max_depth = 10
Mean CV RMSE: ~$61,453
```

This was interesting because increasing the depth initially improved the model, but after a certain point the test/CV performance started getting worse.

That helped me understand the relationship between model complexity and overfitting much better than simply reading about it theoretically.

---

### 5. Cross-Validation

I then used 5-fold cross-validation to get a more reliable estimate of model performance.

Instead of relying on a single train/test split, the training data was divided into different folds and the model was evaluated multiple times.

This helped me compare models more fairly and also showed me that the standard deviation of the scores tells us something about how consistent the model is across different splits.

---

### 6. Random Forest

This was probably the most interesting part of the project.

I initially understood Random Forest as basically "a bunch of Decision Trees," but experimenting with it helped me understand why the ensemble actually works.

Different trees are trained using different samples and feature subsets. Their predictions are then combined, which helps reduce variance and makes the overall model more robust than relying on one tree.

My initial Random Forest achieved:

```text
Mean CV RMSE: ~$49,281
Std: ~$373
```

I then experimented with the number of trees:

```text
10 trees  → ~$51,800
50 trees  → ~$49,568
100 trees → ~$49,281
200 trees → ~$49,186
500 trees → ~$49,107
```

The improvement became increasingly small as the number of trees increased.

The 500-tree model was also noticeably more computationally expensive for only a tiny improvement, so I decided that 200 trees was a more sensible stopping point for this project.

---

## Final Model Comparison

Using the same 5-fold cross-validation methodology:

| Model             |    Mean RMSE |       Std |
| ----------------- | -----------: | --------: |
| Linear Regression |     ~$68,595 |   ~$1,115 |
| Decision Tree     |     ~$61,453 |     ~$844 |
| **Random Forest** | **~$49,186** | **~$444** |

Random Forest was clearly the best-performing model among the three.

Compared with the Linear Regression baseline, the Random Forest reduced the mean RMSE by roughly **28%** under the same cross-validation setup.

---

## Research Thinking

This project changed how I think about ML models.

Instead of asking:

> "Which algorithm should I use?"

I started asking:

> "What is the data telling me, what assumptions is the model making, and what happens when I change those assumptions?"

For example:

* Linear Regression struggled because the relationships weren't sufficiently linear.
* An unrestricted Decision Tree overfit heavily.
* Limiting tree depth improved generalization.
* Cross-validation gave a more reliable comparison.
* Random Forest reduced the variance of individual trees.
* Increasing the number of trees helped initially but eventually produced diminishing returns.
* The most accurate model isn't automatically the best production model because computational cost and other constraints also matter.

This was probably the first project where I felt like I was **experimenting with ML rather than just implementing it.**

---

## What I'd Improve

If I revisited this project later, I'd probably spend less time manually exploring basic syntax and more time experimenting with the actual models.

I'd also investigate Random Forest further using parameters such as:

* `max_features`
* `max_depth`
* `min_samples_split`
* `min_samples_leaf`

I'd want to see whether the ~49k RMSE could be improved without introducing unnecessary computational cost.

---

## Main Takeaways

1. A good ML workflow starts with understanding the data.
2. Preprocessing must be done carefully to avoid data leakage.
3. A baseline model gives us something meaningful to compare against.
4. More complex models aren't automatically better.
5. Cross-validation makes model comparisons more reliable.
6. Hyperparameters can have a huge effect on generalization.
7. Ensemble methods can significantly improve performance.
8. More computation does not always justify a tiny improvement in performance.
9. **The goal isn't just to train a model. It's to understand why it behaves the way it does.**

---

## Tools & Libraries

* Python
* NumPy
* Pandas
* Scikit-learn
* Jupyter Notebook
* Git & GitHub

---

## Progress

**Day 06–08 of my 60-Day AI/ML Journey**

This project was a big step up from the earlier Python/NumPy exercises. I'm starting to move from learning individual tools toward actually thinking about the complete ML workflow.
