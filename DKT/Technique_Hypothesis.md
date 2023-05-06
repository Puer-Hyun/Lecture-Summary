1. 시퀀스를 뒤집어본다.
2. Mixup Training
![](images/2023-05-06-23-20-48.png)
3. Label Smoothing 
![](images/2023-05-06-23-21-35.png)
4. max-seq 값을 조절해가며 실험한다.
5. test,target Datalodaer를 만드는 과정에서 최대한 비슷한 sequence끼리 하나의 배치에 들어갈 수 있도록 조정할 것.