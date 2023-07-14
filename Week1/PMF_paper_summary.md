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

User-factor vector와 Item-factro vector의 분포는 평균이 0이고 분산이 $\sigma^{2}_{U}, \sigma^{2}_{V}$인 정규분포로 구성된다고 가정한다.

이를 통해 얻을 수 있는 posterior distribution은 다음과 같다.

$p(U,V \mid R,\sigma^{2},\sigma^{2}_{V},\sigma^{2}_{U}) \propto p(R \mid U,V,\sigma^{2})*p(U \mid \sigma^{2}_{U})*p(V \mid \sigma^{2}_{V})$



  


