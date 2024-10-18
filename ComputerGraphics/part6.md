# Computer Graphics

# Part 6 - Interpolation and Application

#### 2024.10.18.(금)

[보간(Interpolation)](https://m.blog.naver.com/ojh6t3k/20196349385)

### 보간(Interpolation)

보간은 두 점을 연결하는 방법을 의미한다. 여기서 말하는 연결은 궤적(Trajectory)를 생성한다는 뜻이다.
보간이 필요한 이유는 정보를 압축한 것을 다시 복원하기 위함이다. 예를 들어 컴퓨터에서 선(Line)을 그린다고 가정해보자.

선은 점들의 집합이므로 선을 표현하기 위해 무수히 많은 점의 정보가 필요하다. 이를 다 저장하는 것은 메모리 낭비이다.

그래서, 특징점이라 불리는 선의 모양 복원에 꼭 필요한 점들만 취해서 저장하는데 이 과정을 Sampling 이라 부른다.

일반적으로 Sampling은 일정 시간 주기로 선의 점을 취하는 방식을 사용하는데 녹음 기술에서 많이 쓴다.
Sampling은 시간 주기보다 선의 모양 변화가 심하면 올바른 정보를 취하지 못하는 단점이 있다.

그래서, Sampling은 최소 시간 주기를 잘 설정해야 한다. Sampling으로 저장된 점들을 다시 원래대로 복원할 때 보간법이 사용된다.

앞에서 말했듯이 보간은 점과 점 사이를 연결하여 선을 만들기 위함이다.

#### 선형 보간법(Linear Interpolation)

두 점을 연결하는 가장 간단한 방법은 직선을 긋는 것이다. 두 점 사이의 최소 거리가 되며 이런 방식을 선형 보간법이라 부른다.

이 알고리즘은 고등학교 때 배웠던 직선의 방정식을 이용하면 쉽게 표현할 수 있다. (두 점을 지나는 직선의 방정식의 표현)

이 방법은 매우 간단하지만 문제가 있다.

위와 같이 원래 곡선이었던 경우 선형 보간법을 사용하면 점과 점 사이에서 오차가 발생한다.
즉, 선형 보간은 비슷하게 복원할 수 있지만 정교하지 못한 것이 문제이다.
이 복원의 정교성은 일부 시스템 제어에서는 문제가 될 수 있기에 보다 좋은 복원 방법이 필요하다.

#### 포물선 보간법(Parabolic Interpolation)

이 방법은 점과 점 사이를 포물선 방정식을 사용함으로써 보다 부드러운 곡선을 만들 수 있다.
포물선은 2차 함수이며 포물선을 그리기 위해서는 최소 3개의 점이 필요하다.
선형 보간에는 최소 2개의 점이면 되지만, 포물선 보간은 최소 3개가 필요한 것이 차이점이다.

포물선 보간은 선형 보간에 비해 복잡하지만 보다 부드러운 곡선을 만듦으로써 원래 선에 가깝게 복원할 수 있다.
그러나, 이 방법에는 단점이 있다.
3개씩 단위로 보간하다보면 급격하게 선이 꺾이는 문제가 나타난다.
그러므로, 뭔가 더 자연스럽게 복원할 수 있는 방법이 필요하다.

#### Newton, Lagrange 보간법

이것은 다항식(Polynominal)을 이용하는 방식이다. 보다 정확히 말하면 N차 다항식이다.

f(x) = a0 + ax*x + a2*x^2 + a3\*x^3 + ... + anx^n (다향식의 모습)

이것은 수치해석의 급수 표현에서 나온 개념이다.
좀 어렵지만 나름 쉽게 표현한다면 모든 수는 N개의 상관 관계로 표현 가능하다는 것이다.

여기서 상관 관계란 미분(Difference)을 뜻한다. 미분은 차이를 뜻하며 이것들의 곱은 N차 식으로 표현된다는 데에서 기인한 것이다.
여기서 N개는 그 수가 많으면 많을수록 원래 수에 가깝게 간다.

위와 같은 원리로 N개의 점이 주어졌을 때, Newton, Lagrange 보간을 사용하면 원래 선에 가까운 곡선을 얻을 수 있다.

다항식으로 보간하는 방법은 아주 많은 연산이 필요하기에 컴퓨터가 발달하기 전까지는 개념으로만 존재했다.
또한, 전체 점이 한꺼번에 주어지는 것이 아닌 순차적으로 주어지는 경우는 계산하기가 어렵다는 단점이 있다.

#### 스플라인 보간(Spline Interpolation)

스플라인 보간은 아주 자연스러운 곡선을 만들 수 있는 방법이다. 이것은 그림을 그리는 프로그램에서 곡선을 표현하기 위해 많이 사용된다.

스플라인 보간은 3차 다항식을 사용한다. 포물선 보간은 2차 다항식을 사용하는 것에 비해 더 복잡하다. 그렇지만, 더 자연스럽고 부드러운 곡선을 얻을 수 있다. 3차 다항식을 구성하기 위해서는 4개의 점이 필요하다.

보간은 일부 정보만 가지고 원래의 것을 복원하는 방법이다.
정보가 많으면 많을 수록 복원이 쉬워지는데, 이것이 바로 다항식의 원리이다.
정보가 많으면 다항식의 차수가 높아진다는 말이고 연산이 복잡하고 오래 걸리는 문제가 있다.
정밀도가 많이 필요하면 다항식의 차수를 높일 필요가 있겠지만, 스플라인의 경우 3차 정도면 충분하다.

[ML 부스팅(Boosting)](https://sonstory.tistory.com/95)

### 부스팅(Boosting)

`부스팅(Boosting)`이란 여러 개의 learning 모델을 순차적으로 구축하여 최종적으로 합치는 방법이다. 여기서 사용하는 learning 모델은 매우 단순한 모델이다. 여기서 단순한 모델이란 Model that slightly better than chance, 즉 이진 분류에서 분류 성능이 0.5를 조금 넘는 정도의 수준의 모델을 말한다. 부스팅은 모델 구축에 순서를 고려하기 때문에 각 단계에서 새로운 base learner를 학습하여 이전 단계의 base learner의 단점을 보완하며, 각 단계를 거치면서 모델이 점차 강해진다. 부스팅 모델의 종류로는 `AdaBoost`, `GBM`, `XGBoost`, `Light GBM`, `CatBoost` 등이 있다.

### Adaptive Boosting(AdaBoost)

`AdaBoost`는 각 단계에서 새로운 base learner를 학습하여 이전 단계의 base learner의 단점을 보완하는 모델이다. AdaBoost는 training error가 큰 관측치의 선택 확률(가중치)을 높이고, training error가 작은 관측치의 가중치를 낮춰 오분류한 관측치에 보다 더 집중한다. 이렇게 조정된 가중치를 기반으로 다음 단계에서 사용될 training dataset을 구성하여 다시 새로운 base learner를 학습하고 이전 단계의 base learner의 단점을 보완한다. 최종 결과물은 각 모델의 성능지표를 가중치로 하여 결합한다. AdaBoost의 알고리즘은 다음과 같다.

3개의 classifier에 기반한 AdaBoost의 예시를 보면 다음과 같다. Boosting 모델에서 많이 사용되는 base model은 간단한 구조를 가지고 있는 `Decision Tree`이다.

### Bagging vs Boosting

`Bagging`은 하나의 train sample로부터 `Bootstrap`을 통해 여러 개의 샘플을 만든 후 각 샘플에 대해 모델을 학습해 이를 가중합하는 기법이다. `Boosting`은 Parallel한 Bagging과 다르게 하나의 train sample에 대해 모델을 구축하고, 해당 모델을 통해 가중치를 새롭게 업데이트하고 다음 모델을 구축하는 Sequential한 기법이다.

### Gradient Boosting Machines(GBM)

Gradient boosting이란 Boosting with gradient decent를 의미한다. 즉, 첫 번째 단계의 모델 tree 1을 통해 Y를 예측하고, Residual을 다시 두번째 단계 모델 tree 2를 통해 예측하고, 여기서 발생한 Residual을 모델 tree 3로 예측하는 모델이다. 결국 Gradient boosted model = tree 1 + tree 2 + tree 3 가 되며, 각 단계를 거침에 따라 점점 Residual 은 작아지게 된다.

GBM은 Residual을 사용하는 모델이며, 이를 Gradient라고 표현하는 이유는 Residual이 Negative Gradient를 의미하기 때문이다.

## [Interpolation and its application in Machine Learning](https://medium.com/@akshanshmishra/interpolation-and-its-application-in-machine-learning-a0a5b5df653f)

Interpolation is a technique used in numerical to estimate the value of a function at an unknown point based on its known values at other points. It is commonly used in various areas such as computer graphics, signal processing, and machine learning. In machine learning, interpolation is used to make predictions by estimating the value of a function that maps the input data to the output.

There are several types of interpolation methods, including linear interpolation, polynomial interpolation, spline interpolation, and radial basis function interpolation. Linear interpolation is the simplest and fastest method, but it may not be suitable for non-linear data. Polynomial interpolation can provide better results, but it can be computationally expensive. Spline interpolation is a combination of linear and polynomial interpolation, which provides a smooth curve that can better fit non-linear data. Radial basis function interpolation is a type of non-parametric method that uses a set of basis functions to model the underlying function.

In machine learning, interpolation is often used in regression problems, where the goal is to estimate the relationship between the input and output data. For example, interpolation can be used to estimate the missing values in a dataset of to make predictions for new data points based on the known data. Interpolation can also be used in classification problems to estimate the probability of each class for a given input.

1. Linear Interpolation

Let's say we want to predict the temperature outside tomorrow based on the temperature today. We can use linear linear interpolation to find a line that passes through our two points (today's temperature and tomorrow's temperature) and then use that line to estimate the temperature for other days. Here's an example of linear interpolation in Python:

```Python
# Import required libraries
import numpy as np
import matplotlib.pyplot as plt

# Define the x and y values
x1 = 10
x2 = 20
y1 = 25
y2 = 30

# Calculate the slope of the line
slope = (y2 - y1) / (x2 - x1)

# Use the slope to calculate the equation of the line
x_range = np.linspace(x1, x2, num=11)

# Use the equation of the line to calculate the corresponding y values
y_range = m * x_range + b

# Plot the line
plt.plot(x_range, y_range, color="red")
plt.scatter([x1, x2], [y1, y2], color="blue")

# Show the plot
plt.show()

```

So, using linear interpolation, we can take two points (the temperature today and the temperature tomorrow) and use them to predict the temperature for other days.

2. Polynomial Interpolation

Polynomial interpolation is an important application of machine learningin Python that is both useful and amusing. You can use it to predict what will happen when you mix two ingredients together, like a science experiment! For example, let's say we have a small dataset of measurements from mixing two substances, A and B. We can use polynomail interpolation to predict the result of mixing different amounts of each together. Here's the Python code for that:

```Python
# import the necessary libraries
from sklearn.preprocessing import PolynomialFeatures
from sklearn.linear_model import LinearRegression

# Set up the data
x = [[A_amount, B_amount] for A_amount in range(10) for B_amount in range(10)]
y = [result_of_mixing(A_amount, B_amount) for A_amount in range(10) for B_amount in range(10)]

# create the polynomial interpolation model
poly = PolynomialFeatures(degree=2)
X_poly = poly.fit_transform(x)
regressor = LinearRegression()
regressor.fit(X_poly, y)

# predict the result of mixing different amounts of A and B
predicted_result = regressor.predict([[A_amount, B_amount]])
print("The predicted result of mixing {} of A and {} of B is {}".format(A_amount, B_amount, predicted_result[0]))
```

Now you can use polynomial interpolation to predict the result of your own science experiments! Who knows - maybe you'll discover something new!

3. Spline Interpolation

Have you ever been frustrated with the lack of accuracy of a particular model? It can be quite disheartening when your model is a few decimal points off the desired outcome. Fortunately, Spline Interpolation can help you get that extra bit of accuracy you need. Spline Interpolation is a method of representing the relationship between two points in a graph that is smooth and continuous. It relies on polynomial functions to accurately predict values between two points. This makes it ideal for machine learning, where accuracy is paramount. Let's take a look at an example with some Python code. Suppose we want to predict the result of a game of Tic-Tac-Toe, given the current state of the board. Using Spline Interpolation, we can fit a smooth curve between the points on the board and accurately predict the outcome of the game.

```Python
import numpy as np
from scipy.interpolate import UnivariateSpline

# Create the data points for the board
x = [0, 1, 2, 0, 1, 2, 0, 1, 2]
y = [0, 0, 0, 1, 1, 1, 2, 2, 2]

# Fit a curve to the data
spline = UnivariateSpline(x, y)

# Predict the outcome of the game
predicted_outcome = spline(3)

print(f'The predicted outcome of this game is {predicted_outcome}.')
```

4. Radial Basis Function Interpolation

Radial basis function (RBF) interpolation is a powerful tool for machine learning and is widely used in a variety of applications. It is a type of non-parametric smoothing technique that uses radial basis functions (RBFs) to approximate multivariate functions. RBF interpolation can be used to improve the accuracy of machine learning models, as it helps to accurately interpolate between data points. Example: Predicting the wind speed of a given location based on the pressure data from nearby points.

```Python
import numpy as np
from scipy.interpolate import Rbf

# Pressure data from nearby points
pressure = np.array([[967.9, 968.5, 969.3, 970.3],
                    [967.2, 968.2, 969.2, 970.2],
                    [966.1, 967.1, 968.2, 969.2],
                    [965.2, 966.2, 967.3, 968.5]])

wind_speed = np.array([[5.5, 5.7, 5.9, 6.2],
                        [5.3, 5.5, 5.7, 6.0],
                        [4.6, 5.0, 5.4, 5.8],
                        [4.2, 4.6, 5.0, 5.4]])

# Create a radial basis function interpolator
# using the pressure data as the base
interpolator = Rbf(pressure, wind_speed)

# Predict the wind speed for a given pressure
predicted_wind_speed = interpolator(967.5)

print('The predicted wind speed is: %.2f' % predicted_wind_speed)
```
