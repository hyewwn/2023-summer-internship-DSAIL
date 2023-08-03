# Factorization Machines
Factorization Machines는 SVM과 factorization model의 장점을 결합하여 고안한 모델이다. SVM은 machine learning에서 굉장히 유용하게 사용되는 모델임에도 불구하고 collaborative filtering과 같은 상황에서는 잘 사용되지 못한다. 이는 데이터가 상당히 sparse하기 때문이다. 또한, 기존의 factorization model의 경우 일반적인 real-valued feature vector 등 예측용 데이터에 적용할 수 없다는 단점이 있다. 이에 이 논문에서 제시하고 있는 Factorization Machine (FM)은 SVM과 같은 general predictor이지만 매우 높은 sparsity 상황에서도 적용가능한 모델이다.

FMs는 collaborative filtering에서의 biased MF, SVD++, PITF, FPMC 등을 포함한다.

## Example for sparse real valued feature vectors x
<img width="544" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/56d5f150-d80c-49a5-88f5-15a242c073cb">

## Factorization Machine Model
### Model Equation
Degree $d=2$인 FM은 다음과 같이 정의된다. 

<img width="402" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/faba79c7-e489-496d-a13a-7cf21b3899b7">

이때 모델 파라미터는 다음과 같고, <x,y>는 두 벡터의 dot product를 의미한다.

<img width="257" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/29e5fd90-ea9a-4897-b863-33630bc2046a">

<img width="186" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/c642bfc6-4063-4f13-99be-f84846fe90e8">

2-way FM은 모든 pairwise interaction을 capture한다. 

### Expressiveness
모든 positive definite matrix $W$는 $k$가 충분히 크다면 항상 $W=V \cdot V^t$로 분해가 가능한 것으로 알려져 있다. 이는 충분히 큰 $k$가 주어진다면 FM이 모든 interaction matrix $W$를 표현할 수 있음을 의미한다. 그러나 sparsity가 큰 상황이기에 $k$가 크다면 복잡한 interaction을 충분히 추정할 만큼의 데이터가 존재하지 않는다. 따라서 FM에서는 $k$를 제한한다.

### Parameter Estimation Under Sparsity
Sparse 한 상황 때문에 변수들 간의 interaction을 직접적으로, 그리고 독립적으로 추정하기에는 충분한 데이터가 존재하지 않는다. 따라서 FM에서는 interaction parameters의 independence를 깨뜨려 준다.

예를 들어, 위 Example에서 user A가 영화 ST를 보지 않았으므로 두 요소는 interaction이 없는 것으로 나타날 것이다. $(w_{A,ST})=0$ 그러나, factorized interaction parameters < $V_A,V_{ST}$ >를 통해서는 interaction을 추정해낼 수 있다.

먼저, user B와 user C가 모두 영화 SW에 대해 비슷하게 평가하였다면 두 user는 유사한 factor vector $V_B, V_C$를 가져야 한다. 즉, < $V_B,V_{SW}$ >와 < $V_C,V_{SW}$ >는 유사해야 한다. 다음으로 user A와 user B가 영화 SW와 T에 대해 다르게 평가하였다면 두 user는 다른 factor vector $V_A, V_B$를 가져야 한다. 그리고 user B가 SW와 ST에 대해 비슷하게 평가하였다면 $V_{SW}, V_{ST}$도 유사하여야 한다. 이는 곧 < $V_A,V_ST$ >가 < $V_A,V_{SW}$ >와 유사할 것으로 예상할 수 있다.

### Computation
위 (1) 식은 계산 복잡도를 $O(kn^2)$으로 나타낼 수 있다. 그러나 이를 reformulating 함으로써 linear runtime으로 감소시킬 수 있다. 결과적으로 $O(kn)$으로 감소되며 그 과정은 아래와 같다.

<img width="360" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/58b0575f-e5f8-404e-bc4b-e0729cdf1027">

## Factorization Machines as Predictors
FM은 general predictor로 원하는 task에 맞는 loss function을 사용한다.

<img width="400" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/289c8e5a-008a-4ea1-8c21-31fbd6eabd02">

## Learning Factorization Machines
FM은 linear time 내에 계산 가능한 closed model이므로 gradient descent method를 통해 최적화 가능하다. FM의 gradient는 다음과 같다.

<img width="400" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/d7bbc8d0-2163-4831-b924-0739f2b4de41">

## d-way Factorization Machine
<img width="420" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/48a8dd9e-266a-4ee0-8e75-bb6b05c613f8">


