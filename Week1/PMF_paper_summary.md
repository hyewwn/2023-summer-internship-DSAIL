# Probabilistic Matrix Factorization
기존의 Matrix Factorization 기반 모델의 주된 단점은 exact inference가 어렵다는 점이다. 해당 모델들은 hidden factor의 posterior distribution을 계산하기 위해 느리고 부정확한 근사치를 필요로 한다. Low-rank approximation은 SVD(Singular Value Decomposition)에서 찾을 수 있으나, sparse한 데이터셋으로 인해 이는 매우 어려운 non-convex optimization 문제이다. 따라서 approximation matrix $\hat{R}=U^TV$의 rank를 제한하는 대신 $U, V$의 norm을 penalizing하는 방법이 제안되었으나 이 역시 희소한 SDM(Semi-Definite Program)문제로 수백만 개의 관측치를 갖는 데이터셋에 대해 infeasible하다.

본 논문에서 제안하고 있는 PMF는 관측의 수에 따라 선형적으로 scaling 가능한 모델로, 매우 크고, 희소하고, 불균형된 데이터셋인 Netflix 데이터셋에서 매우 좋은 성능을 보여준다. 여기에 adaptive prior를 포함하여 모델 가용성을 자동적으로 조절할 수 있도록 하였으며, 유저의 preference에 대한 가정을 바탕으로 contrained PMF를 제안하였다.

*model의 성능은 RMSE(Root Mean Squared Error)를 계산하여 측정한다.*
## Basic PMF
- Conditional Distribution over the observed ratings
  
  <img width="370" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/98d40707-31c6-4a8c-a871-a64a0aaab51f">
  
데이터셋에서 점수 집합 R이 나올 확률은 각 user와 item의 latent vector를 곱한 값을 평균으로 하는 정규분포들의 곱으로 구성된다고 가정한다.
- User-factor Distribution, Item-factor Distribution
  
  <img width="391" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/550b49ae-872d-4b6d-a250-921ff3a69299">

User-factor vector와 Item-factro vector의 분포는 평균이 0이고 분산이 $\sigma^2_U, \sigma^2_V$ 인 정규분포로 구성된다고 가정한다.

이를 통해 얻을 수 있는 posterior distribution은 다음과 같다.

<img width="500" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/bee0f3ee-ddd4-4ae4-a0c3-ac44965fed92">

위 식에 log를 씌우고 Quadratic Regularization term을 추가하여 식을 정리하면 다음과 같은 최종 손실함수를 얻을 수 있다.

<img width="500" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/abbe18d6-b125-4254-a372-37254def6281">

실험 과정에서는 Prediction의 범위를 맞춰주기 위하여 logistic function을 추가하여 다음과 같은 식을 사용하였다.

<img width="389" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/520d630a-b753-4cd8-b921-f7093dc7f2db">

## Automatic Complexity Control for PMF Models
더욱 일반화된 PMF 모델을 만들기 위해서는 Capacity control이 중요하다. 일반적으로 PMF 모델에서 capacity를 조절하는 방법은 feature vector의 dimension을 조절하는 것인데 이는 데이터셋이 unbalanced한 경우 이러한 접근은 실패한다.

따라서, Regularization parameter $(\lambda_{U}, \lambda_{V})$를 사용하여 더욱 유연한 regularization을 수행할 수 있도록 한다. 

적절한 Regularization parameter 값을 결정하는 가장 간단한 방법은 parameter value 집합을 형성한 뒤 검증 데이터셋에 대해 가장 높은 성능을 보이는 값을 설정하는 것인데 이는 높은 계산량을 요한다는 치명적인 단점이 존재한다. 이에 여기에서는 모델의 학습 시간에 영향을 크게 주지 않으면서 적절한 regularization parameter 값을 찾는 새로운 방법을 제안하고자 한다.

- 하이퍼파라미터에 의해 모델의 복잡도가 조절된다. ($\sigma^{2}$, parameters of the priors ($\sigma^2_U, \sigma^2_V$))
- 하이퍼파라미터에 어떤 사전분포를 가정하고, 점 추정을 모델의 log-posterior를 최대화하도록 함으로써 수행한다. 이에 관한 식은 다음과 같다.

  <img width="700" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/aaafc2c6-7534-4c29-90e1-5b361fdfe63a">

- 사전 분포가 Gaussian이라면, 최적의 하이퍼파라미터는 user-vector와 item-vector가 고정되어 있을 때 closed form으로 나타난다. 따라서, 번갈아가며 최적화를 시켜줌으로써 하이퍼파라미터를 결정할 수 있다.

## Constrained PMF
여기에서는 infrequent user에 대해 추가적으로 user-specific feature vector에 대한 제약을 정의하고 있다.

Latent similarity constraint matrix $W$를 통해 user $i$에 대한 feature vector를 다음과 같이 정의할 수 있다. ($I$는 관측이 발생하였는지에 대한 indicator matrix이다.)

<img width="200" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/75228e8f-410f-4cba-baaa-6d71264aaf2f">

$W$ matrix의 $i$번째 컬럼은 한 유저가 평가한 특정 영화가 그 유저의 feature vector의 prior mean에 대해 얼마나 영향력을 갖는지 나타낸다. 그 결과로 비슷한 영화를 본 유저들은 비슷한 prior distribution을 갖는다.

앞에서와 동일한 방법으로 식을 정리한 결과 얻어지는 식들은 다음과 같다.

- Conditional Distribution over the observed ratings

<img width="398" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/aa3e5c15-5ef5-47a1-83b9-3b5a4e5903cb">

- Distribution of latent similarity constraint matrix

<img width="180" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/67343623-677a-45fe-865c-754b9fe83c04">

- Loss function
  
<img width="385" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/3808760a-43cf-41e5-803d-a3bef1d3af35">

실험 결과 constrained PMF의 성능이 가장 좋게 나타났으며, infrequent user에 대해서는 특히 높은 성능이 나타났다.


  


