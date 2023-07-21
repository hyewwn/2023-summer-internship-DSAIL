# Collaborative Metric Learning
본 논문에서는 Metric Learning과 Collaborative Filtering 간의 연관관계를 확인하고, 이를 이용해 새로운 Collaborative Filtering 방법을 제안하고 있다. Metric Learning에서는 데이터 간의 중요한 관계를 나타낼 수 있는 댜양한 distance metric을 제시한다. Distance metric이 따라야 하는 몇 가지 중요한 규칙이 있는데, 그 중 하나는 "Triangle Inequality"이다.

기존의 collaborative filtering에서 수행하는 dot product는 triangle inequality를 만족시키지 못하여 user-item 간의 관계를 통해 item-item 간의 관계를 충분히 추정하지 못하고 있다. 그러나, CML에서는 user의 선호만 반영하는 것이 아니라 user-user 간의 유사도와 item-item 간의 유사도도 반영한다. (이를 similarity propagation으로 볼 수 있다.) 이를 도식화한 그림은 다음과 같다.

<img width="500" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/3366fbfc-4d61-4d2b-be82-d7db24ffcf52">


## Metric Learning
$\chi=\{x_1,x_2,...,x_n}$: collection of data

<img width="374" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/b89cd683-4c1c-4b43-abf9-b0bda35c9d92">

<img width="401" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/299505f8-9de6-499e-b74f-d63a0ec38922">

### Mahalanobis distance metic: most original metric learning approach
<img width="297" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/284b10b4-fa68-4baa-a6b3-c76495137224">

위 metric을 생성하기 위해서는 일반적으로 다음과 같은 convex optimization problem을 풀어준다.

<img width="320" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/23648170-859e-4441-98b7-412760b6ee60">

그러나 이러한 global optimization은 항상 feasible하지 않을 수 있다. 따라서 여기에서는 이를 동일 label을 가진 객체로 축소한 형태인 k-Nearest Neighborhood에서 사용하는 Metric Learning을 차용하였다.

## Metric Learning for kNN
Input $x$에 대하여 가장 가까운 데이터를 target neighbors라 하며, 이들은 $x$와 동일한 label을 가질 수 있도록 해야 한다. $x$와 다른 label을 가진 데이터는 imposter라 한다. Imposter의 수를 최소화도록 하는 model에는 Large margin nearest neighbor (LMNN)이 있다.
- Large Margin Nearest Neighbor (LMNN)
  Pull loss와 Push loss가 존재한다. Target Neighbor에 대해서는 가까이 당겨야하며, imposter에 대해서는 밀어내야 한다. 이를 위한 식은 다음과 같다.
  
  <img width="181" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/e03b2b75-6bef-4cb9-ad66-c00c61f6cdaf">

  <img width="426" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/fc9f2c2c-b778-4778-b29a-99e80a366d8c">

  이는 semi-definite programming으로 풀 수 있다.

## Collaborative Metric Learning
CML에서는 implicit feedback을 user-item pairs $S$로 보고 해당 관계들을 encoding한다.이때 triangle inequality로 인해 CML은 동일한 아이템을 선호하는 유저들을 clustering하고, 동일한 유저가 선호하는 아이템들을 clustering한다. 이에 따라 어떤 유저에 대해 가장 가까운 neighbor item은 해당 유저가 과거에 선호했던 아이템이며, 유사한 선호를 갖는 유저들이 선호했던 아이템이 될 것이다. 이러한 관계는 user-item pair에만 한정되지 않고, 직접 얻어낼 수 없는 정보인 user-user, item-item pair로도 전파된다.

### Model Formulaton
- Euclidean distance
  <img width="145" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/d0a65526-f4f7-4e34-b9e0-2d73f34599f5">
- Loss function
  <img width="374" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/caed4b3b-7e6f-425d-b15a-9570ad01e5db">

  $m$: safety margin

  $w_{ij}$: ranking loss weight

  위 손실함수를 통한 동작 과정은 아래 그림과 같다.

  이 손실함수는 LMNN과 비슷하나, 몇 가지 차이점이 존재한다. 1)각 User에 대해 target neighbors는 해당 user가 선호하는 모든 item이 되며, item에 대해서는 target neighbor가 존재하지 않는다. 또한, 2) $L_{pull}$이 존재하지 않는데 이는 어떤 아이템이 여러 유저에 의해 선호될 수 있기 때문이다. 마지막으로, 3)Top-K 추천 성능을 높이기 위해 weighted ranking loss를 적용하였다.
<img width="600" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/e45d225f-90ed-459b-9fce-45bef9abe4db">

### Approximated Ranking Weight
Weighted Approximate-Rank Pairwise (WARP) loss로 불리는 rank-based weighting scheme을 적용하고자 한다. 이는 낮은 rank에 위치한 positive item을 penalize하는 것으로 다음과 같은 식을 사용하여야 한다.

<img width="186" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/a90d572a-a2ac-44d5-901a-f1c1e067bf03">

그러나 rank를 계산하는 과정에서 Gradient descent를 이용해야 하므로 높은 계산량을 요구한다. 따라서 이를 negative sampling 사용하여 $rank_d(i,j)$를 $\lfloor J/N \rfloor$로 근사한다. 본 논문에서는 이를 병렬처리가 가능하도록 하고, $\lfloor (J*M)/N \rfloor$으로 근사하였다.

### Integrating Item Features
Item에 대한 추가적인 정보가 있을 때 이를 반영할 수 있도록 하고자 한다. 이를 위해 $x_j$를 통해 item $j$에 대한 m-dimensional raw feature vector로 정의하고 이를 어떤 함수 $f$를 통해 projection한다. 함수 $f$는 Multi-layer perceptron (MLP)를 통해 학습할 수 있다. 손실 함수는 다음과 같다.

<img width="270" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/b932dbe2-2d20-4818-97b0-e11d5b734d17">

### Regularization
정규화를 위해서 item vector와 user vector를 각각 unit sphere로 bound시키고, 여기에 추가적으로 Covariance를 고려한다. Covariance는 redundancy를 나타내는 measure로 볼 수 있는데 이를 통해 시스템이 주어진 space를 더욱 효율적으로 사용할 수 있도록 할 수 있다.

각각을 위한 식은 다음과 같다.

<img width="220" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/f482385c-532a-4f3d-8331-bc5886b7de29">

<img width="240" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/5b9c4be8-f60b-472b-aed1-eee940f36563">

### Complete objective function
<img width="260" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/3b0d1dde-47fa-42a7-801a-cd8573f74742">





