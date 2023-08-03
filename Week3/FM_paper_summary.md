# Factorization Machines
Factorization Machines는 SVM과 factorization model의 장점을 결합하여 고안한 모델이다. SVM은 machine learning에서 굉장히 유용하게 사용되는 모델임에도 불구하고 collaborative filtering과 같은 상황에서는 잘 사용되지 못한다. 이는 데이터가 상당히 sparse하기 때문이다. 또한, 기존의 factorization model의 경우 일반적인 real-valued feature vector 등 예측용 데이터에 적용할 수 없다는 단점이 있다. 이에 이 논문에서 제시하고 있는 Factorization Machine (FM)은 SVM과 같은 general predictor이지만 매우 높은 sparsity 상황에서도 적용가능한 모델이다.

FMs는 collaborative filtering에서의 biased MF, SVD++, PITF, FPMC 등을 포함한다.
