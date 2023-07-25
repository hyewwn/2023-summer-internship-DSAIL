# Semi-Supervised Classification with Graph Convolutional Networks
본 논문에서는 graph-structured data에 대해서 convolutional network를 이용해 node classification을 효과적으로 수행하는 방법에 대해 다루고 있다. Graph 데이터의 경우, Label이 전체 데이터셋 대비 매우 적은 데이터에 대해서만 매겨져 있기 때문에 이를 전체 그래프로 smooth 시키는, 일종의 semi-supervised 방식을 사용한다.

이전의 방식은 일부 Label만을 이용해서 전체에 Regularization하는 과정에 인접 node는 동일한 label을 가짐을 가정한다. 이런 방식을 통해서는 정보 손실이 발생하기 때문에 여기서는 neural network model $f(X,A)$로 graph structure를 그대로 나타내며, 이를 label을 보유한 모든 node의 supervised target $L_{0}$를 이용해 학습한다. 이때 $f()$를 인접행렬로 conditioning을 함으로써 모델이 $L_{0}$ Loss를 통해 얻은 gradient 정보를 전체 Graph에 전달한다.

## Contribution of This Paper
1. Introduce a simple and well-behaved layer-wise propagation rule for neural network models which operate directly on graphs.
2. Show how it can be motivated from a first-order approximation of spectral graph convolutions.
3. Demonstrate how this form of a graph-based neural network model can be used for fast and scalable semi-supervised classification of nodes in a graph.

## Fast Approximate Convolutions on Graphs
이 논문에서 제시하고 있는 multi-layer Graph Convolutional Network는 다음과 같은 layer-wise propagation rule을 따른다.

$$
H^{l+1} = \sigma(\tilde{D}^{-1/2}\tilde{A}\tilde{D}^{-1/2}H^{(l)}W^{(l)})
$$

이때, 인접행렬 $\tilde{A} = A+I_{N}$은 인접행렬에 identity matrix를 더해줌으로써 self-loop를 추가하여 각 노드가 자기 자신의 정보를 포함하여 학습할 수 있도록 하였으며, 이에 따라 degree matrix도 $\tilde{D}$로 변화한다. 위 식을 통해 $\tilde{A}$에 의해 연결성이 높을수록 높은 값으로 업데이트 되지만, gradient가 폭발하거나 소실되지 않도록 $\tilde{D}$로 정규화를 수행함을 알 수 있다. $H^{0}=X$이다.

Spectral Convolution을 수행하는 과정에서 고유값 분해를 수행하는 것이 높은 computational cost를 유발하기 때문에 위 수식은 approximation을 이용하여 유도되었다. 본 논문에서는 해당 유도 과정을 자세히 설명하고 있다.

## Semi-Supervised Node Classification
위 propagation 방법을 이용해 semi-supevised node classification을 소개하고 있다. Two-layer GCN을 통해 위 task를 수행하는 과정은 다음과 같다.
1. Calculate $\hat{A}=\tilde{D}^{-1/2}\tilde{A}\tilde{D}^{-1/2}$ in a pre-processing step.
2. Forward model then takes the simple form below.

$$
Z = f(X,A) = softmax(\hat{A}ReLU(\hat{A}XW^{(0)})W^{(1)})
$$

일반적인 딥러닝 방법론과 동일하게 분류 문제이기에 softmax 함수를 사용하고 있으며 Label에 대한 Loss function을 이용한다. 논문에서는 batch gradient descent를 사용하고 있어 out of memory 문제가 발생하기도 하였다. 따라서 이후 mini-batch stochastic gradient descent를 이용해야 함을 제시하고 있다.

## Experiment Result
### Dataset info
<img width="500" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/98c23d46-9f4f-4116-8db9-85e1903b6624">

### Summary of results
<img width="500" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/f2adf291-6946-4cbe-a106-a833230cde67">

### Evaluation of propagation model
<img width="500" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/6afdb348-3222-44ad-b899-2f27e1612f27">

