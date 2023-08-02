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


