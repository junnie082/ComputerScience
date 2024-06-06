# Lecture 09: Linear Model Selection and Regularization

#### 2024.06.05.(수)

### Linear Model Selection and Regularization

- Recall the linear model

Y = beta0 + beta1X1 + ... + betapXp + epsilon

- In the lectures that follow, we consider some approaches for extending the linear model framework

  - In the lectures covering Chapter 7 of the text, we generalize the linear model in order to accommodate nonlinear, but still additive, relationships

- In the lectures covering Chapter 8 we consider even more general nonlinear methods.

### In Praise of Linear models

- Despite its simplicity, the linear model has distinct advantages in terms of its interpretability and often shows good predictive performance

- Hence we discuss in this lecture some ways in which the simpler linear model can be improved, by replacing ordinary least squares fitting with some alternative fitting procedures.

### Why Consider Alternatives to Least Squares?

- Prediction Accuracy

  - Especially when p > n, to control the variance

- Model Interpretability
  - By removing irrelevant features that is, by setting the corresponding coefficient estimates to zero, we can obtain a model that is more easily interpreted
  - We will present some approaches for automatically performing feature selection

### Three Classes of Methods

- Subset Selection

* We identify a subset of the p predictors that we believe to be related to the response
* We then fit a model using least squares on the reduced set of variables.

- Shrinkage

* We fit a model involving all p predictors, but the estimated coefficients are shrunken towards zero relative to the least squares estimates
* This shrinkage (also known as regularization) has the effect of reducing variance and can also perform variable selection

- Dimension Reduction

* We project the p predictors into a M-dimensional subspace, where M < p
* This is achieved by computing M different linear combinations, or projections, of the variables
* Then these M projections are used as predictors to fit a linear regression model by least squares

### Subset Selection

- Best subset and stepwise model selection procedures

- Best subset selection

```
1. Let M0 denote the null model, which contains no predictors. This model simply predicts the sample mean for each observation.

2. For k=1,2,...,p:
  (a) Fit all (p) models that contain exactly k predictors.
              (k)
  (b) Pick the best among these (p) models, and call it Mk.
                                (k)
      Here best is defined as having the smallest RSS, or equivalently largest R^2.
   3. Select a single best model from among M0, ..., Mp using cross-validated prediction error, Cp(AIC), BIC, or adjusted R^2.
```

### Example: Credit Data Set

- For each possible model containing a subset of the ten predictors in the Credit data set, the RSS and R^2 are displayed

- The red frontier tracks the best model for a given number of predictors, according to RSS and R^2

- Through the data set contains only ten predictors, the x-axis ranges from 1 to 11, since one of the variables is categorical and takes on three values, leading to creation of two dummy variables

### Extensions to Other Models

- Although we have presented best subset selection here for least squares regression, the same ideas apply to other types of models, such as logistic regression

- The deviance, negative two times the maximized log-likelihood, plays the role of RSS for a broader class of models.

### Stepwise Selection

- For computational reasons, best subset selection cannot be applied with very large p.

- Best subset selection may also suffer from statistical problems when p is large

  - Large the search space, the higher the chance of finding models that look good on the training data, even though they might not have any predictive power on future data.

- Thus an enormous search space can lead to overfitting and high variance of the coefficient estimates.

- For both of these reasons, stepwise methods, which explore a far more restricted set of models, are attractive alternatives to best subset selection.

### Forward Stepwise Selection

- Forward stepwise selection begins with a model containing no predictors, and then adds predictors to the model, one-at-a-time, untill all of the predictors are in the model

- In particular, at each step the variable that gives the greatest additional improvement to the fit is added to the model

- Forward Stepwise Selection

```
1. Let M0 denote the null model, which contains no predictors.
2. For k = 0, ..., p - 1:
  2.1 Consider all p - k models that augment the predictors in Mk with one additional predictor.
  2.2 Choose the best among these p - k models, and call it Mk+1. Here best is defined as having smallest RSS or highest R^2.
3. Select a single best model from among M0, ..., Mp using cross-validated prediction error, Cp(AIC), BIC, or adjusted R^2.
```

### More on Forward Stepwise Selection

- Computational advantage over best subset selection is clear

  - Only 1 + sum(from k = 0 to k = p-1) (p-k) = 1 + p(p+1)/2 models

- It is not guaranteed to find the best possible model out of all 2^p models containing subsets of the p predictors

* Why not?

### Forward Stepwise Selection: Credit Data Example

- The first four selected models for best subset selection and forward stepwise selection on the Credit data set

- The first three models are identical but the fourth models differ

| # Variables | Best subset | Forward stepwise |
| One | rating | rating |
| Two | rating, income | rating, income |
| Three | rating, income, student | rating, income, student |
| Four | cards, income | rating, income |
| | student, limit | student, limit |

### Backward Stepwise Selection

- Like forward stepwise selection, backward stepwise selection provides an efficient alternative to best subset selection

- However, unlike forward stepwise selection, it begins with the full least squares model containing all p predictors, and then iteratively removes the least useful predictor, one-at-a-time

- Backward Stepwise Selection

```
1. Let Mp denote the full model, which contains all p predictors.
2. for k = p, p-1, ..., 1:
   2.1 Consider all k models that contain all but one of the predictors in Mk, for a total of k-1 predictors.
   2.2 Choose the best among these k models, and call it Mk-1.
       Here best is defined as having smallest RSS or highest R^2.
3. Select a single best model from among M0, ..., Mp using cross-validated prediction error, Cp (AIC), BIC, or adjusted R^2.
```

### More on Backward Stepwise Selection

- Like forward stepwise selection, the backward selection approach searches through only 1 + p(p+1)/2 models, and so can be applied in settings where p is too large to apply best subset selection

- Like forward stepwise selection, backward stepwise selection is not guaranteed to yield the best model containing a subset of the p predictors

- Backward selection requires that the number of samples n is larger than the number of variables p (so that the full model can be fit)
  - In contrast, forward stepwise can be used even when n < p, and so is the only viable subset method when p is very large

### Choosing the Optimal Model

- The model containing all of the predictors will always have the smallest RSS and the largest R^2, since these quantities are related to the training error

- We wish to choose a model with low test error, not a model with low training error. Recall that training error is usually a poor estimate of test error

- Therefore, RSS and R^2 are not suitable for selecting the best model among a collection of models with different numbers of predictors

### Estimating Test Error: Two Approaches

- We can indirectly estimate test error by making an adjustment to the training error to account for the bias due to overfiting.

- We can directly estimate the test error, using either a validation set approach or a cross-validatioon approach, as discussed in previous lectures

- We illustrate both approaches next

### Cp, AIC, BIC, and Adjusted R^2

- These techniques adjust the training error for the model size, and can be used to select among a set of models with different numbers of variables

- The next figure displays Cp, BIC, and adjusted R^2 for the best model of each size produced by best subset selection on the Credit data set

### Now for Some Details

- Mallow's Cp:

`Cp = 1/n(RSS + 2do^2)`

- d is the total # of parameters used and o^2 is an estimate of the variance of the error epsilon associated with each response measurement

* AIC (Akaike information criterion) criterion is defined for a large class of models fit by maximum likelihood:

`AIC = -2 log L + 2d`

- L is the maximized value of the likelihood function for the estimated model

* In the case of the linear model with Gaussian errors, maximum likelihood and lease squares are the same thing, and Cp and AIC are equivalent

-2 log L = RSS / o^2

### Details on BIC

- Like Cp, the BIC will tend to take on a small value for a model with a low test error, and so generally we select the model that has the lowest BIC value.

- Notice that BIC replaces the 2do^2 used by Cp with a log(n)do^2 term, where n is the number of observations

`BIC = 1/n(RSS + log(n)do^2)`

- Since log n > 2 for any n >7, the BIC statistic generally places a heavier penalty on models with many variables, and hence results in the selection of smaller models than Cp

### Adjusted R^2

- For a least squares model with d variables, the adjusted R^2 statistic is calculated as

`Adjusted R^2 = 1 - (RSS/(n-d-1)) / (TSS/(n-2))`

- TSS is the total sum of squares

* Unlike Cp, AIC, and BIC, for which a small value indicates a model with a low test error, a large value of adjusted R^2 indicates a model with a small test error

* Maximizing the adjusted R^2 is equivalent to minimizing RSS/(n-d-1)

* While RSS always decreases as the number of varialbes in the model increases, RSS/(n-d-1) may increase or decrease, due to the presence of d in the denominator

* Unlike the R^2 statistic, the adjusted R^2 statistic pays a price for the inclusion of unnecessary variables in the model
