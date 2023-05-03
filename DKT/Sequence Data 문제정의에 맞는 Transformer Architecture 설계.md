# Variations of Transformers 
## 다양한 Transformer Models
* Transformer는 다양한 Sequence 데이터에 있어서 강점을 보이지만 모델의 구조가 아주 많은 데이터와 연산량을 요구합니다. 이때문에 종종 Transformer를 상황에 맞게 변형하여 사용해야 하는 경우가 생기는데 (inductive  bias), 예를 들면
  * 보다 작은 구조를 사용해야 하는 경우
  * 대회 플랫폼에서 Inference Time이나 메모리에 제약이 걸리는 경우
  * 적용하려는 task에 맞게 변형을 하는 경우 등

![](images/2023-05-03-11-52-42.png)