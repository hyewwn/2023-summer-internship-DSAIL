# Translating Embddings for Modeling Multi-relational Data
본 논문에서는 multi-relational data를 embedding하는 방법에 대해 다루고 있다. Multi-relational data란 directed graph로, 여러 종류의 relation을 갖는 data를 의미한다. Multi-relational data는 (head, label, tail)의 형태로 entitiy와 relation을 표현한다. 이를 Embedding 한 결과를 이용하여 link prediction을 수행할 수 있고, 이를 통해 새로운 fact가 입력되었을 때 graph를 자동적으로 업데이트할 수 있도록 하고자 한다.

Collaborative filtering에서 성공적으로 user/item clustering 또는 matrix factorization 방법을 통해 single relational entity 간의 connectivity pattern을 나타낸 것에 이어서 대부분의 multi-relational data에 대한 방법들은 latent-attribute를 학습하는 방식으로 제안되었다. 앞서 제안되었던 stochastic blockmodel, tensor factorization, collective matrix factorization 등의 방법은 모델의 expressivity와 universality를 높이는 방식에 집중하였다. 그러나 이와 같은 방식은 높은 복잡도와 계산량을 기지며, regularization이 어려워 overfitting이 되거나 non-convex optimization problem이 되어 underfitting이 되는 문제를 갖는다. 그에 비해 본 논문에서 제시하고 있는 TransE는 더욱 간단한 구조를 가지고 있으나 더욱 높은 성능을 보여주었다.

## Translation-based model (TransE)
TransE는 (head, label, tail)의 형태로 이루어진 각 entity와 relation을 하나의 low-dimensional embedding으로 학습시키기 때문에 parameter의 수가 크게 감소한다. TransE에서는 hierarchical relationship으로 나타내기 때문에 relation을 가지고 있는 각 entity는 $h+l \approx t$로 embedding 되어야 한다. 이는 곧 실제 정답 relation에 대해서는 $h+l$과 가장 가까운 entity가 $t$가 되어야 함을 의미하며, 정답이 아닌 $t'$은 $h+l$과 멀어져야 함을 의미한다.

Similarity를 기준으로 energy-based approach를 이용하여 d(h+l,t)를 dissimilarity measure로 정의하면 다음과 같은 손실함수를 정의할 수 있다.

<img width="473" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/b198f265-8813-43f3-a276-09fbbf6573de">

이때 $\[x]_+$은 $x$의 양의 부분을 의미하고, negative sampling을 위한 $S'$은 다음과 같이 정의된다.

<img width="404" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/f61944dc-b041-4a11-ade8-e184ea594152">

Optimization은 Stochastic gradient descent를 이용해 수행될 수 있다.

학습 알고리즘은 다음과 같다.

<img width="600" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/320bb0ae-1993-414d-9887-078031136ebb">


TransE 모델은 규모를 키우기 쉽다는 장점을 가지고 있어 Freebase data에 대해 거대한 스케일의 chunk를 이용할 수 있었다. 그러나 1-N relation을 나타낼 수 없다는 단점이 존재한다.




