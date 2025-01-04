# 선형회귀에서의 MSE와 편미분

기본적으로, 단순 선형회귀(Simple Linear Regression)는
$ \hat{y} = \beta_0 + \beta_1 x $
형태를 가정합니다. 여기서 학습(훈련) 과정에서는 평균제곱오차(MSE) 를 최소화합니다.

## 1. 평균제곱오차 (MSE)

$$
\text{MSE}(\beta_0, \beta_1)
= \frac{1}{N} \sum_{i=1}^{N}
\bigl(
y_i - (\beta_0 + \beta_1 x_i)
\bigr)^2
$$

- $N$: 전체 데이터 개수
- $\beta_0$: 절편(intercept)
- $\beta_1$: 기울기(slope)
- $(x_i, y_i)$: $i$번째 데이터 포인트

## 2. $\beta_0$ (절편)에 대한 편미분

(1) 미분 과정

MSE를 $\beta_0$에 대해 미분하려면, 각 항
$\bigl(y_i - (\beta_0 + \beta_1 x_i)\bigr)^2$
에 체인 룰(Chain Rule)을 적용합니다.

$$
\frac{\partial ,\text{MSE}}{\partial \beta_0}
= \frac{1}{N} \sum_{i=1}^{N}
\frac{\partial}{\partial \beta_0}
\bigl[
y_i - (\beta_0 + \beta_1 x_i)
\bigr]^2
$$

(2) 결과

아래처럼 전개하면, -2와 ( $y_i - (\beta_0 + \beta_1 x_i)$ )가 곱해집니다.

$$
= \frac{1}{N} \sum_{i=1}^{N}
\Bigl(
-2 ,\bigl[, y_i - (\beta_0 + \beta_1 x_i) \bigr]
\Bigr)
= -\frac{2}{N} \sum_{i=1}^{N}
\Bigl[
y_i - (\beta_0 + \beta_1 x_i)
\Bigr].
$$

## 3. $\beta_1$ (기울기)에 대한 편미분

(1) 미분 과정

이번에는 $\beta_1$이 $x_i$와 곱해져 있으므로, 아래처럼 $-2x_i$가 추가로 곱해집니다.

$$
\frac{\partial ,\text{MSE}}{\partial \beta_1}
= \frac{1}{N} \sum_{i=1}^{N}
\frac{\partial}{\partial \beta_1}
\bigl[
y_i - (\beta_0 + \beta_1 x_i)
\bigr]^2
$$

(2) 결과

$$
= \frac{1}{N} \sum_{i=1}^{N}
\Bigl(
-2 , x_i ,\bigl[, y_i - (\beta_0 + \beta_1 x_i) \bigr]
\Bigr)
= -\frac{2}{N} \sum_{i=1}^{N}
x_i \Bigl[
y_i - (\beta_0 + \beta_1 x_i)
\Bigr].
$$

## 4. 경사하강(Gradient Descent) 업데이트

오차를 줄이는 방향(편미분의 음수 방향)으로 $\beta_0, \beta_1$를 갱신해 나갑니다.
학습률(learning rate)을 $\alpha$라 하면,

$$
\beta_0 \leftarrow \beta_0
	•	\alpha \cdot \frac{\partial ,\text{MSE}}{\partial \beta_0},
\quad
\beta_1 \leftarrow \beta_1
	•	\alpha \cdot \frac{\partial ,\text{MSE}}{\partial \beta_1}.
$$

이를 여러 번 반복(예: 수백~수천 회)하면서 오차가 점차 줄어드는 쪽으로 파라미터가 최적화됩니다.
