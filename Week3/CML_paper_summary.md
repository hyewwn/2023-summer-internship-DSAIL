# Collaborative Metric Learning
본 논문에서는 Metric Learning과 Collaborative Filtering 간의 연관관계를 확인하고, 이를 이용해 새로운 Collaborative Filtering 방법을 제안하고 있다. Metric Learning에서는 데이터 간의 중요한 관계를 나타낼 수 있는 댜양한 distance metric을 제시한다. Distance metric이 따라야 하는 몇 가지 중요한 규칙이 있는데, 그 중 하나는 "Triangular Inequality"이다.

기존의 collaborative filtering에서 수행하는 dot product는 triangular inequality를 만족시키지 못하여 user-item 간의 관계를 통해 item-item 간의 관계를 충분히 추정하지 못하고 있다. 그러나, CML에서는 user의 선호만 반영하는 것이 아니라 user-user 간의 유사도와 item-item 간의 유사도도 반영한다. (이를 similarity propagation으로 볼 수 있다.) 이를 도식화한 그림은 다음과 같다.

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
  





