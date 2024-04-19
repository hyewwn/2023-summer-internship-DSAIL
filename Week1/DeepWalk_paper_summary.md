# DeepWalk: Online Learning of Social Representations
본 논문에서는 graph 구조 속 각 노드의 연결을 반영하여 각 노드를 continuous한 낮은 차원의 벡터공간에 나타내는 노드 임베딩 방법론으로써 DeepWalk를 제안하고 있다. DeepWalk는 NLP 분야에서 제안된 단어 임베딩 방법론인  Word2Vec의 Skip-gram을 이용하는데, 이때 문장은 Random walk sequence로 대응되고 단어는 각 노드로 대응된다.
임베딩 결과의 효과성을 확인하기 위하여 Blog-Catalog, Flicker, YouTube 네트워크를 각각 임베딩한 후 이를 이용해 multi-label network classification을 수행하였다. 이는 기존의 challenging baselines보다 최대 10% 가량 높은 성능을 보여줬으며, 60% 가량 적은 training data를 사용한 경우에도 높은 성능 향상이 나타났다.
DeepWalk는 확장 가능한 모델로, 병렬처리가 가능하여 실제 다양한 문제에 적용 가능하다.

## Contribution of this paper
1. Introduce deep learning method to build robust representations that are suitable for statistical modeling.
2. DeepWalk's representations can outperform its competitors.
3. Demonstrate the scalability of our algorithm by building representations of web-scale graphs, using a parallel implementation.

## Learning social representation
Authors seek learning social representations with the following characteristics.
- **Adaptability**: 실제 social network는 계속해서 진화하기 때문에 새로운 social relation이 발생했을 때 전체 프로세스를 다시 수행하지 않고도 이를 반영할 수 있어야 한다.
- **Community aware**: Latent dimension 간의 거리는 social similarity를 나타낼 수 있어야 한다.
- **Low dimensional**: 라벨이 있는 데이터가 희소할 때 저차원의 모델은 더 잘 일반화되며, 빠르게 수렴하고, 추론 가능하다.
- **Continuous**: Continuous representation은 smooth decision boundaries를 가지고 있어 더욱 robust한 분류가 가능하다.

## Random Walks
Random Walk rooted at vertex $v_{i}$: $W_{v_{i}}$

Random Walk는 similarity를 측정하기 위한 용도로 사용되어왔다. 또한, *Stream* of short random walk를 이용하면 Graph에서 local한 정보를 추출해낼 수 있다. 이를 이용함으로써 얻을 수 있는 주요 효과로는, 쉬운 parallelize와 graph의 작은 변화에 대한 쉬운 accomodation이 있다.

## Connection: Power laws
만약 graph의 degree distribution이 power-law를 따른다면, short random walk에서 각 노드의 관측 빈도 역시 power-law를 따를 것이다. 이때, NLP에서 각 단어의 발생 빈도 역시 power-law를 따르기에 이런 두 유사성을 바탕으로 NLP 분야에서의 techniques를 차용하여 사용할 수 있을 것이라 예상할 수 있다.

## Language Modeling
DeepWalk는 NLP 분야에서 제안된 단어 임베딩 방법론인  Word2Vec의 Skip-gram을 이용하는데, 이때 문장은 Random walk sequence로 대응되고 단어는 각 노드로 대응된다. Mapping function은 다음과 같이 표현한다.

$$
\Phi: v \in V \to R^{|V|\times d}
$$

이를 통해 optimization 문제를 정의한 결과는 다음과 같다. 이를 통해 각 노드의 vector representation을 얻을 수 있다.

$$
minimize_{\Phi} -log Pr({v_{i-w}, ..., v_{i-1}, v_{i+1},...,v_{i+w}}|\Phi(v_{i}))
$$

이때 partition function을 계산하는 것이 높은 computational cost를 요하기 때문에 이를 binary tree에 할당한 후 계산함으로써 계산복잡도를 낮출 수 있다. 이를 위해 기존 skip-gram 알고리즘에서 사용하였던 Hierarchical Softmax를 추가적으로 이용하였다.
