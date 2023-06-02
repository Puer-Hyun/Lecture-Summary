# 시작하기 전에
![](images/2023-06-01-17-05-54.png)
![](images/2023-06-01-17-06-28.png)
* Self-supervised signal을 이용하여, Item Embedding을 잘 Initialize 해주는 과정

# 코드 및 데이터 흐름
![](images/2023-06-01-17-07-21.png)
![](images/2023-06-01-17-08-09.png)
![](images/2023-06-01-17-08-17.png)
![](images/2023-06-01-17-09-22.png)
![](images/2023-06-01-17-09-52.png)
![](images/2023-06-01-17-11-07.png)
![](images/2023-06-01-17-12-18.png)
* 유저를 다시 매핑하면 귀찮으니까.
![](images/2023-06-01-17-12-49.png)
![](images/2023-06-01-17-13-57.png)
* self.test_neg_items는 사용되지 않는다. 
* Negative Sample 구성 : 시청하지 않은 영화는 Negative한 Feedback으로 간주 
* ![](images/2023-06-01-17-15-21.png)
* ![](images/2023-06-01-17-16-23.png)

## Trainers.py
![](images/2023-06-01-17-17-58.png)
* Hidden_Size는 무엇인가?
* Movie (e.g., 131번)의 표현력을 더욱 풍부하게 만들고 싶다. ![](images/2023-06-01-17-19-18.png)
![](images/2023-06-01-17-20-48.png)
![](images/2023-06-01-17-21-09.png)
![](images/2023-06-01-17-22-08.png)
![](images/2023-06-01-17-22-29.png)
* 파란색이 정답, 빨간색이 부정, 초록색이 예상한 것
* ![](images/2023-06-01-17-23-14.png)
* ![](images/2023-06-01-17-24-38.png)
* ![](images/2023-06-01-17-25-47.png)
## 평가를 위한 예측
![](images/2023-06-01-17-26-09.png)
![](images/2023-06-01-17-26-16.png)
![](images/2023-06-01-17-27-03.png)
![](images/2023-06-01-17-28-14.png)
![](images/2023-06-01-17-29-37.png)
# 코어 모델

# Self-Supervised Learning with MIM
<span style="color:red">**MIM이 중요한 개념 중 하나!**</span>
* MIM : Mutual Information Maximization : 상호의존정보 최대화 
![](images/2023-06-01-17-38-45.png)
![](images/2023-06-01-17-39-20.png)
![](images/2023-06-01-17-40-07.png)

### AAP 
* Associated Attribute Prediction


### MIP
* Masked Item Prediction
![](images/2023-06-01-17-43-18.png)

### MAP
* Masked Attribute Prediction
![](images/2023-06-01-17-45-10.png)

### SP 
* Segmented Prediction
![](images/2023-06-01-17-47-25.png)

### joint_loss
* sp_weight에만 early stopping이 걸려있는데, 다양한 실험을 해볼 수 있을 것이다.

# 대회를 위한 팁
* 과연 SOTA가 최선일까?
* Static과 Sequential
  * 우리 베이스라인은 Sequential, 복잡한 모델입니다.
  * 가장 최근 중 하나이다.
  * 그런데 사실, 기존 기수의 조교 분께서 실험을 해본 결과에 따르면 Traditional한 모델과 SOTA 모델이 크게 차이가 나지 않았었다.
  * 반드시 SOTA 모델을 쓸 필요는 없다.

## 생각해볼 점
* 아이템 embedding 최적화에 집중?
* Sequentiial 예측에 최적화된 모델이 중간에 비어 있는 영화를 잘 맞출 수 있을까?
* Static 예측에 최적화된 모델이 다음 영화를 잘 예측할 수 있을까?
  * 다음이라는 것도 Static하게 볼 수도 있으니 가능할 수도!
* 그렇다면 앙상블? Voting 방식?
* 그냥 반반 나눠서 합치면 되지 않을까?
  * Static 모델에서 나온 결과 반
  * Sequentioal 모델에서 나온 결과 반
* SOTA가 과연 좋을까? 


## 드리고 싶은 말씀
* S3 코드 구현이 복잡하게 되어 있는 부분이 많아서 코드를 바로 이해하려 하지 말고, 논문을 정독하신 후에 코드를 보셔야 이해가 빠릅니다. (끼워맞추기 식으로 구현된 부분들은 코드만 봐서 이해하기가 어렵다.)
* 따라서 Baseline 코드 분석에 너무 많은 시간을 할애하기 보단, 실험 계획을 빠르게 세워서 점진적으로 모델 성능을 업데이트 시켜 나가는 것을 권장드립니다.
* 한달이라는 시간동안 고전적인 모델부터 최신 모델까지 다양하게 실험을 해보면서 많은 고민을 모든 팀원분들이 함께 하셨으면 좋겠습니다!