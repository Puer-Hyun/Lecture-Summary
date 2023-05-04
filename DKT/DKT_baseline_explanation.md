* get_linear_schedule_with_warmup 은 Transformer 쓸 때 중욯다. warmup을 잘 찾아가면서 써라.
* process_batch라는 함수는 없다.
* 한 명이 똑같은 문제를 또 풀었으면 그 때는 어떻게 해야할까? 
* LGCN은 논문을 꼭 읽어보세요.
    * 학습 되는게 처음 embedding밖에 없어요. parameter를 조절할 게 별로 없어요.
    * Num_layers 는 2~4만 쓰세요?
    * embeding_dim 은 클 수록 쓸 수도 있지만..? 너무 크면 안되겠지? 데이터 개수가 딸릴 수도 있다. (DKT는 아주 많지도 아주 적지도 않은 것)
    * $\alpha$같은 건 논문을 읽어보면, 상수로 쓰는게 낫다고 써있다. $\dfrac{1}{K+1}$을 쓰는 게 좋더라?
* 쓸 수 있는 함수 (link_pred_loss 함수 등이 있다.)

    
