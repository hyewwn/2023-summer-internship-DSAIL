# metapath2vec: Scalable Representation Learning for Heterogeneous Networks
기존의 연구들은 word2vec-based network representation learning을 수행함으로써 node embedding을 생성하였다. 이에 대표적인 프레임워크로는 DeepWalk, LINE, node2vec 등이 있다. 그러나 이러한 연구들은 homogeneous network에 초점을 맞추고 있다. 그러나 많은 network가 heterogeneous한 특성을 갖기 때문에 이를 위한 새로운 프레임워크가 필요하다. 따라서 본 논문에서는 metapath2vec, metapath2vec++ 두 모델을 소개하고 있다. 주된 contribution으로는 1) meta-path 기반 random walks를 heterogeneous network에 적용하였으며 2) skip-gram 모델을 확장하여 가까운 node간의 위치와 의미를 모델링하고자 하였고 3) heterogeneous negative-sampling을 이용하여 metapath2vec++로 모델을 확장하였다. 

## Homegeneous vs. Heterogeneous
Homogeneous network와 Heterogenous network는 node를 연결하는 edge가 하나의 타입만 있는지, 여러 타입이 있는지에 따라 나누어진다. Heterogeneous Network는 다음과 같이 정의될 수 있다.

<img width="500" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/cea7bf55-205f-4c79-b908-14d38ebb0805">

예를 들어, academic network에서 각 node는 authors(A), papers(P), venues(V), organizations(O)의 타입을 가질 수 있으며, 이는 곧 각 node를 잇는 edge가 co-author(A-A), publish(A-P, P-V), affilation(O-A)의 타입을 가짐을 의미한다.

이에 따라서 node-neighborhood를 정의하는 방식이 달라져야 하며, network의 구조와 각 node, edge의 의미를 유지하며 어떻게 embedding할 것인가 고민하여야 한다.

## Homogeneous Network Embedding
Random walk + skip-gram을 사용하는 모델(DeepWalk, node2vec)의 동작은 다음 목적식에 따라서 수행된다.

<img width="242" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/b3d2ce44-a3db-418a-8168-dea33071067d">

## Heterogeneous Network Embedding: metapath2vec
### Heterogeneous Skip-Gram
Heterogeneous network: $G=(V,E,T)$ with $|T_V|$

여기에서 $N_t(v)$는 $t^{th}$ type of nodes를 가진 $v$의 neighborhood를 의미하며 $p(c_t|v;\theta)$는 일반적으로 softmax function으로 정의된다.

<img width="237" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/e1cb139c-3ee2-4276-8083-8852b447ef1d">

그러나 효율적인 optimization을 수행하기 위해 negative sampling을 수행하는데 metapath2vec 모델에서는 negative sampling을 수행할 때 type을 고려하지 않고 homogeneously draw한다. 이에 따라 목적식은 다음과 같이 업데이트된다.

$$
log \sigma(X_{c_t} \cdot X_v)+\sum^M_{m=1}E_{u^m~P(u)}\[log \sigma(-X_{u^m} \cdot X_v)]
$$

여기에서 $P(u)$는 pre-defined distribution이다.
### Meta-Path-Based Random Walks
Meta-path scheme $P$는 다음과 같이 나타낼 수 있다. 

<img width="390" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/830de4ba-c6e0-4229-a0d8-a5b7f16ce83f">

위에서 사용한 academic network에서의 path는 아래와 같다.

<img width="450" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/68886ed4-ad84-44e2-8075-681a208cf3f5">

이에 따라 step $i$에서의 transition probability는 다음과 같이 정의된다.

<img width="452" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/6e32fafe-4a8e-4123-bb05-73059346fb8c">

즉, 존재하는 edge 중 다음 path에 해당하는 특정 node type인 경우에만 다음 step으로 고려된다. Meta-path는 일반적으로 symmetric하게 만들어진다. Meta-path를 통해 skip-gram 모델에 각 node를 semantic relationship을 고려하며 넣어줄 수 있다.

## metapath2vec++
Metapath2vec 모델에서는 negative sampling을 할 때 모든 type의 노드를 모두 포함한다. Metapath2vec++ 모델에서는 heterogeneous negative sampling을 수행할 때 node type을 고려하여 특정 node type에 해당하는 node 중에서 negative sampling을 수행한다. 이때 목적식은 다음과 같이 나타낼 수 있다.

<img width="364" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/34013898-6b65-4c65-b04d-453d00d771d8">

이에 따른 Gradient는 다음과 같이 나타난다.

<img width="256" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/8506f293-4952-4318-a151-cef5a3be47a3">

## Results
각 모델을 비교한 결과는 다음과 같다. metapath2vec++이 가장 높은 성능을 보였으며, metapath2vec이 그 뒤를 잇는다.

<img width="600" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/2cb15f6e-63e7-401b-be22-e9ae77c0163b">


