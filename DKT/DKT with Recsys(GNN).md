![](images/2023-05-08-02-48-33.png)
![](images/2023-05-08-02-48-56.png)
* Session은 Sequence보다 작은 단위라고 생각하면 된다.
  * 우리의 데이터에서는 '시험지 단위'가 Session이라고 생각할 수 있다.

# Representation learning in Graph
![](images/2023-05-08-02-50-42.png)
* GNN을 input, output 관점에서 보면 다음과 같다.
* ![](images/2023-05-08-02-51-19.png)
  * 노드의 hidden vector가 $d$차원이었을 때, $d_l$차원으로 바꾸는 것.
* ![](images/2023-05-08-02-52-16.png)

## Tasks on Graphs
![](images/2023-05-08-02-52-56.png)

## Brief history of graph neural nets
![](images/2023-05-08-02-53-36.png)
![](images/2023-05-08-02-53-48.png)

## Matrix Representation of Graph
![](images/2023-05-08-02-55-32.png)
![](images/2023-05-08-02-55-53.png)

## GNN Terminology - Node-feature matrix
![](images/2023-05-08-02-56-30.png)

* Node Feature matrix
  * 각각의 노드들을 vector로 나타내서 연결시킨 것들
  * ![](images/2023-05-08-02-57-45.png)
* Adjacency Matrix
  * 인접행렬
* Degree matrix
  * 얼마나 연결되어있느냐 
* Laplacian matrix
  * ![](images/2023-05-08-02-58-09.png)
  * Spectral -> Spatial로 넘어갈 때 필요한 행렬 

# GNN Architecture
![](images/2023-05-08-02-58-42.png)


# Graph Convolutional Networks (GCN)
![](images/2023-05-08-03-02-00.png)
![](images/2023-05-08-03-02-10.png)
![](images/2023-05-08-03-02-59.png)
![](images/2023-05-08-03-04-09.png)
![](images/2023-05-08-03-05-36.png)
![](images/2023-05-08-03-06-36.png)
* GCN의 Weight는 전체 노드 정보를 고려한 차원 변환 파라미터
  * 그래프 구조적 정보가 반영된 파라미터 -> Spectral
  * 새로운 노드가 오면 전체 그래프와의 연결성을 고려 -> Transductive
  * 새로운 노드가 들어오면 5번 노드가 생긴 거잖아요. 4\*4로 학습했는데 5\*5를 예상할 수는 없는 거잖아요. GCN을 가지고 예측을 하려면 테스트 데이터가 훈련에 포함되어야만 한다. 
  * 보지 않은 데이터에 대해선 예측할 수 없잖아요. 
* ![](images/2023-05-08-03-08-42.png)


# Graph Attention Networks (GAT)
* GAT는 Adjacent Matrix를 사용하지 않는다. (노드별 Weight가 반영된다.)
* ![](images/2023-05-08-03-09-43.png)
* ![](images/2023-05-08-03-09-55.png)
* ![](images/2023-05-08-03-11-11.png)
* ![](images/2023-05-08-03-11-30.png)
* ![](images/2023-05-08-08-51-35.png)
  * 스코어를 구하는 과정에서 dot product를 할 수도 있고
  * concat을 하고 MLP에 태울 수도 있다.
* ![](images/2023-05-08-08-53-10.png)

## Receptive Field 
![](images/2023-05-08-08-54-41.png)
![](images/2023-05-08-08-55-03.png)
![](images/2023-05-08-08-55-37.png)
![](images/2023-05-08-08-55-42.png)
![](images/2023-05-08-08-56-53.png)
* Layer가 5 이상으로 가면 Smoothing이 되어버려서 문제가 생기는데 이것을 해결하기 위해
  * DropOut을 쓸 수도 있다.
    * Node Dropout
    * Edge Dropout
    * Layer-wise Edge Dropout (Random Walk)
    * PairNorm
* ![](images/2023-05-08-09-11-44.png)
* ![](images/2023-05-08-09-11-56.png)
* ![](images/2023-05-08-09-14-59.png)


# Why GNN-RS ?
* 딥러닝 기반 추천 시스템 -> 그래프 기반 추천 시스템?
* Cold-start에 대한 성능 향상이 많이 올라갔다.
* diversity 관점에서도 딥러닝 모델보다 다양하게 그래프가 추천을 하게 되었다.
* ![](images/2023-05-08-09-25-20.png)
* ![](images/2023-05-08-09-55-25.png)
* ![](images/2023-05-08-09-57-09.png)

## Graph Types
![](images/2023-05-08-09-57-23.png)
![](images/2023-05-08-10-18-09.png)


# NGCF 
* 기존의 방법들은 user와 item의 embedding이 잘 되지 않았다.


# NGCF ablation study -> LightGCN
* NGCF-f : feature transformation 제거
* NGCF-n : non-linear activation function 제거
* NGCF-nf 등등 
![](images/2023-05-08-10-24-20.png)
![](images/2023-05-08-10-24-36.png)
![](images/2023-05-08-10-27-26.png)


# To-Do : UltraGCN
![](images/2023-05-08-10-29-59.png)


# Graph 모델링 : Uniqueness (중복 레코드 문제)
* 중복이 있을 경우 Link의 유무를 판단하기 어려움. 
* 예시) User가 Assessment를 여러 번 풀었을 경우 중복이 생김
* 예시 코드에서는 중복을 제거하고, 마지막 레코드만 고려