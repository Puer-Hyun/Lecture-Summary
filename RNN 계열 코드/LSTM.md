![](images/2023-05-04-18-35-14.png)

## 시계열 데이터를 다루는 다양한 방법들
* Data Approach
  * DTW (Dynamic Time Warping)
  * FFT
  * Time-Lag Feature 
* 요즘에는 Data Approach가 잘 안쓰인다.
* Model Approach
  * Stochastic : HMM (Hidden Markov Model)
  * Statistics : ARIMA 
  * ** Deep Learning**
    * **Vanilla RNN, LSTM, GRU, 1D-CNN, Transformer, TCN, Neural ODE, LTFS-Linear**

## Vanila RNN
![](images/2023-05-04-19-51-02.png)
![](images/2023-05-04-19-51-51.png)
![](images/2023-05-04-19-53-26.png)
![](images/2023-05-04-19-53-38.png)


### RNN 구조의 단점
* 문제의 현상 : 긴 time step에 대해서 학습이 이루어지지 않음 (보통 time-step이 10 이상이면 성능 저하)
* 문제의 원인 : Vanishing Gradient and Exploding Gradient -> BPTT 활용으로 인해 나타남 
* ![](images/2023-05-04-19-57-18.png)


### 해결방법 Long Short-Term-Memory 
* 하나의 유닛에서 계산된 정보를 길게 전파하는 Memory 구조를 제안
* 기존의 RNN Unit보다 Vanishing/Exploding Gradient를 막으며, 긴 Time Step을 학습 가능하게 함

1. 장기기억 Cell 구조 도입
   1. 장기기억 Cell인 CEC (Constant Error Carousel)을 도입하여 Vanishing Gradient를 막음
   2. Identity Mapping으로 어떠한 Matrix의 변환 없이 정보를 다음 Time Step으로 전달함 (**+연산만 존재하여 Backprop시 정보 손실 없음**)
   3. BPTT로 인한 Vanishing Gradient 현상이 없어지고, 더 깊은 Network 학습이 가능해짐. -> 더 긴 Time 학습 가능
   ![](images/2023-05-04-20-16-12.png)
2. Gate 구조의 도입
   1. Gate를 사용하여 학습 기반으로, 자동으로 Input, Output 값에 대한 제어 (Original LSTM에는 Forget Gate가 없음)
   2. ![](images/2023-05-04-21-59-17.png)
   3. **InputGate**
      1. $i_t=\sigma\left( W_i \cdot [h_{t-1},x_t]+b_i \right)$
      2. $S_t=squash\left( W_c \cdot [h_{t-1},x_t] +b_S \right)$
      3. $squash=\dfrac{4}{1+e^{-z}}-2=2\tanh\left( \dfrac{z}{2} \right)$, $\sigma=\dfrac{1}{1+e^{-s}}$
   4. Cell State에서 다음 외부 Output이 될 h_t를 결정함. 현재 정보와 바로 직전 정보로 sigmoid 사용해 비율 자동 결정함
   5. **OutputGate**
      1. $O_t=\sigma\left( W_o \cdot [h_{t-1}, x_t]+b \right)$
      2. $h_t = O_t \times squash\left( C_t \right)$
      3. ![](images/2023-05-04-22-04-23.png)

3. Training Method 개선
   1. BPTT (Back Propagation Through Time)과 RTRL (Real-Time Recurrent Learning)의 Variation을 동시에 학습에 사용함.
      1. **BPTT** : 속도가 빠르며 $O\left( n^2 \right)$, 일반적으로 많이 사용하지만 Backward 연산이 따로 필요하며, Vanishing Gradient가 잘 나타남 
      2. RTRL : 속도는 느리나 ($O\left( n^4 \right)$), BPTT보다는 좀 더 Vanishing Gradient에 강하고, Forward 연산시 미분하여 Weight를 업데이트하므로 따로 Training Phase가 필요 없음. 메모리 사용량이 적고 실시간학습에 적합 
      3. ![](images/2023-05-04-22-07-20.png)


## Methodology
* 장기기억 Cell 구조 도입
  * Influenced : ResNet에서의 Identity Mapping에서 해당 아이디어를 참조함. (Highway Network 한 번 거쳐서) ![](images/2023-05-04-22-20-25.png)
  * Gate 구조의 도입 (1/3)
    * Influenced : Highway Network(2015)의 Transform Gate (T)와 Carry Gate(C) 구조로 데이터를 얼마나 변환해서 보내줄 지 결정함
  * Gate 구조의 도입 (2/3)
    * 단점 존재 : Cell state가 1.0 Weight(Identity)만 주다 보니, Depth가 깊어질수록 오히려 Exploding Gradient 가 된다.
    * 해결책으로 저자들은 후속논문으로 Forget Gate를 추가하였다. (입력값 $h_{t-1}$과 $x_t$에 따라 $C_{t-1}$의 값의 양을 조절함)
    * Forget gate 
      * $f_t = \sigma\left( W_f \cdot[h_{t-1}, x_t] +b \right)$
  * Gate 구조의 도입 (3/3)
    * Original LSTM은 squash 함수를 $2\tanh\left( \dfrac{x}{2} \right)$와 같이 선택했는데, 왜 그런 선택을 했을까?
    * 시그모이드는 미분했을 때 최대 0.25이므로 vanishing이 더 심해짐
    * tanh는 미분 최대값이 1.0이기에 gradient vanishing에 더 안정적임
    * Original LSTM은 tanh을 미분했을 때 더 wide하여, 상대적으로 tanh보다 안정적이라 예상
    * 그러나 그냥 tanh이 아니라 $2\tanh\left( \dfrac{x}{2} \right)$ forward prop시 Cell을 더 Exploding하게 만들 것이라 예상
    * relu는 특별한 조치를 취하지 않는 한, 너무 큰 값으로 exploding됨.
![](images/2023-05-04-22-24-42.png)