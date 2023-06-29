# Semi-Supervised Classification with Graph Convolutional Networks
본 논문에서는 graph-structured data에 대해서 convolutional network를 이용해 node classification을 효과적으로 수행하는 방법에 대해 다루고 있다. Graph 데이터의 경우, Label이 전체 데이터셋 대비 매우 적은 데이터에 대해서만 매겨져 있기 때문에 이를 전체 그래프로 smooth 시키는, 일종의 semi-supervised 방식을 사용한다.

이전의 방식은 일부 Label만을 이용해서 전체에 Regularizaation하는 과정에 인접 node는 동일한 label을 가짐을 가정한다. 이런 방식을 통해서는 정보 손실이 발생하기 때문에 여기서는 neural network model $f(X,A)$로 graph structure를 그대로 나타내며, 이를 label을 보유한 모든 node의 supervised target $L_{0}$를 이용해 학습한다. 이때 $f()$를 인접행렬로 conditioning을 함으로써 모델이 $L_{0}$ Loss를 통해 얻은 gradient 정보를 전체 Graph에 전달한다.

## Contribution of this paper
1. Introduce a simple and well-behaved layer-wise propagation rule for neural network models which operate directly on graphs.
2. Show how it can be motivated from a first-order approximation of spectral graph convolutions.
3. Demonstrate how this form of a graph-based neural network model can be used for fast and scalable semi-supervised classification of nodes in a graph.

## Fast approximate convolutions on graphs
이 논문에서 제시하고 있는 multi-layer Graph Convolutional Network는 다음과 같은 layer-wise propagation rule을 따른다.

$$
H^{l+1} = \sigma(\tilde{D}^{-1/2}\tilde{A}\tilde{D}^{-1/2}H^{(l)}W^{(l)})
$$

이때, 인접행렬 $\tilde{A} = A+I_{N}$은 인접행렬에 identity matrix를 더해줌으로써 self-loop를 추가하여 각 노드가 자기 자신의 정보를 포함하여 학습할 수 있도록 하였으며, 이에 따라 $$\tilde{D}_{ii} = \sum_j\tilde{A}_{ij}$$로 degree matrix가 변화한다.
