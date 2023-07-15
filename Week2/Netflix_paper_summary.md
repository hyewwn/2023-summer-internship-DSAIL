# Factorization Meets the Neighborhood: a Multifaceted Collaborative Filtering Model
본 논문에서는 추천시스템의 주된 두 접근법인 Neighborhood model과 Collaborative Filtering model 모두에 대하여 몇 가지 새로운 innovation을 제안하고, 두 접근법을 결합한 모델과, explicit feedback, implicit feedback을 모두 반영하는 모델을 통해 높은 성능을 달성하였다. 이에 추가적으로 새로운 평가 척도를 제안하고 있다.

Neighborhood model들은 매우 localized한 relationship을 찾는데 가장 효과적이나, 이는 몇몇 중요한 relationship에만 집중하도록 하여 유저의 많은 rating을 무시하곤 한다. 이에 반해 Latent factor model들은 대부분의, 또는 모든 아이템들에 동시에 연관이 되는 전반적인 구조를 추정하는데 효과적이다. 그러나 이 모델들은 일부 아이템들 간에 존재하는 높은 연관성을 발견하는 것에는 취약하다. 따라서, 두 모델을 합쳐 두 접근법의 장점을 극대화할 수 있을 것이라 예상할 수 있다.

## Baseline estimates
- 전반적으로 높은/낮은 평점을 주는 유저, 전반적으로 높은/낮은 평점을 갖는 아이템을 반영한다.
- $\mu$: overall average rating

  <img width="150" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/c273c74f-d6c9-4ea3-8a51-31a7c591fa4c">

- loss function
  
  <img width="391" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/5b9a05f2-6208-4543-bddd-c3f2fa228bba">

## Neighborhood models
본 논문에서는 추천의 설명력을 위해 item-oriented 방법에 집중한다. 이를 위해서는 item 간의 유사도를 반영하는 것이 중요한데 일반적으로 Pearson correlation coefficient($\rho_{ij}$)를 이용하여 유사도를 측정해왔다.

여기에 많은 rating을 매긴 유저에게 높은 신뢰성을 부여하기 위해 다음과 같은 shrunk correlation coefficient를 사용하기도 하였다. ($n_{ij}$는 item $i$와 $j$에 모두 rating을 한 유저의 수를 의미한다.)

<img width="165" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/a38aafe1-20df-477e-a78f-92a2c5f03d13">

<img width="289" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/4bd31bb8-04da-46f5-b0fb-10c39cf590b2">

그러나 위 식을 이용하는 경우 1)모든 이웃 정보가 아닌 소수의 정보만을 활용한다는 점 2)interpolation weight의 합이 1이 되어 실제 관계가 없는 item간에도 이웃으로 의존하게 만든다는 점 3)user마다 rating한 item이 달라 가중치 계산이 번거로운 점이 문제점으로 제기되었다. 따라서 본 논문에서는 이를 극복하기 위해 새로운 interpolation weight를 제시하고자 한다.

### Model1)
<img width="280" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/37b0e98c-43cb-451d-9ab1-4e4ff383e466">

- not user specific parameter $w_{ij}$
- sum over all item

### Model2)
<img width="380" alt="image" src="https://github.com/hyewwn/2023-summer-internship-DSAIL/assets/74613565/88d7bfcd-dab5-4930-9230-74922074016b">

- 


# Matrix Factorization Techniques for Recommender Systems
