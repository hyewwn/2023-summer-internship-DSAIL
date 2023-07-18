# Translating Embddings for Modeling Multi-relational Data
본 논문에서는 multi-relational data를 embedding하는 방법에 대해 다루고 있다. Multi-relational data란 directed graph로, 여러 종류의 relation을 갖는 data를 의미한다. Multi-relational data는 (head, label, tail)의 형태로 entitiy와 relation을 표현한다. 이를 Embedding 한 결과를 이용하여 link prediction을 수행할 수 있고, 이를 통해 새로운 fact가 입력되었을 때 graph를 자동적으로 업데이트할 수 있도록 하고자 한다.

Collaborative filtering에서 성공적으로 user/item clustering 또는 matrix factorization 방법을 통해 single relational entity 간의 connectivity pattern을 나타낸 것에 이어서 대부분의 multi-relational data에 대한 방법들은 latent-attribute를 학습하는 방식으로 제안되었다. 앞서 제안되었던 stochastic blockmodel, tensor factorization, collective matrix factorization 등의 방법은 모델의 expressivity와 universality를 높이는 방식에 집중하였다. 그러나 이와 같은 방식은 높은 복잡도와 계산량을 기지며, regularization이 어려워 overfitting이 되거나 non-convex optimization problem이 되어 underfitting이 되는 문제를 갖는다. 그에 비해 본 논문에서 제시하고 있는 TransE는 더욱 간단한 구조를 가지고 있으나 더욱 높은 성능을 보여주었다.

## Translation-based model (TransE)
