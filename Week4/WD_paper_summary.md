# Wide & Deep Learning for Recommender Systems
Recommender system은 input query가 유저 집합과 맥락 정보이고, output이 아이템의 랭킹 리스트인 search ranking system으로 볼 수 있다. 따라서, recommender system에서도 search system과 같이 memorization과 generalization 성능을 모두 달성할 필요가 있다. 이때 memorization이란 이전에 유저가 했던 action과 직접적이고 국소적으로 관련이 있는 item을 추천하는 성능을 말하며, generalization은 이전에 유저가 사용하지 않았던 아이템 등 추천 아이텐들의 다양성을 확보하는 성능을 말한다.

일반적으로 산업 환경에서 사용되는 massive-scale online recommendation & ranking systems에서는 logistic regression과 같은 generalized linear model이 널리 사용된다. 이는 이들이 간단하고, 크기 변화가 쉬우며, 해석이 가능하기 때문이다. 이런 linear model들은 종종 one-hot encoding에 의해 만들어지는 이진 희소 feature를 통해 학습된다. 이때 memorization은 희소 feature간의 cross-product transformation을 통해 효과적으로 달성될 수 있으며, generalization은 feature engineering을 통해 달성될 수 있다. 그러나 이런 방법은 training set에 존재하지 않는 query-item feature pair까지 일반화할 수 없으며, feature engineering을 직접 수행해야 한다는 단점이 존재한다. 

따라서, 본 논문에서 제시하고 있는 Wide & Deep Learning은 Embedding-based model을 통해 feature engineering을 피하고, sparse 하고 high-rank에 해당하는 아이템들을 linear model을 통해 나타날 수 있도록 하여 generalized 되어있으면서도 유저의 특수한 선호를 반영할 수 있게 하였다.

## Wide Component
- general linear model: $y=w^Tx+b$
- $x$: vector of $d$ features
  Feature set은 원래 input feature와 cross-product transformation을 통해 변환된 feature가 모두 concat되어 구성된다. 이를 통해 binary feature 간에 interaction을 반영하고 추가적으로 nonlinearity를 반영한다.
- cross-product transformation
  <img width="269" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/a0992bea-5978-4d5d-aefd-b57de36b1665">

## Deep Component
- feed forward neural network
- 범주형 변수의 경우 embedding vector로 변환하여 사용 (초기 랜덤하게 -> 학습)
- hidden layer
  <img width="236" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/7519ca7b-8d4e-4624-8892-6f2bcfdc6391">

## Joint Training of Wide & Deep Model
Wide component와 deep component는 각각의 output의 log odds를 가중합한 뒤 이를 하나의 logistic loss function에 넣어짐으로써 joint training이 가능하다. Joint training에서는 ensemble과 달리 wide와 deep component의 모든 파라미터를 동시에 최적화한다. 또한, joint training에서는 wide는 deep의 단점을 보충해주는 역할로만 사용되기 때문에 적은 수의 cross-product feature transformation을 수행하여도 된다.

Joint training은 gradient를 이용한 역전파 과정을 통해 학습되며, wide와 deep part 모두 mini-batch stochastic optimization을 이용한다. 실험에서 wide part에 대해서는 Follow-the-regularized-leader(FTRL) 알고리즘을, deep part에 대해서는 AdaGrad를 사용하였다.

최종 combined model의 prediction은 다음 식처럼 이루어진다.

<img width="430" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/c2622fa7-34ac-451a-a637-71c6957fa8b8">

전체 과정을 그림으로 나타내면 다음과 같다.

<img width="554" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/a1e45fbd-ec2a-465c-9dc0-be3336d3a59e">

<img width="819" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/f60772a8-a143-41f9-91e8-b569ff7c6e81">

