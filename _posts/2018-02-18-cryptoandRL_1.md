---
title: "강화학습과 가상화폐"
date: 2018-02-15 23:26:28 -0400
categories: Crypto_RL update
use_math : true
---




# 가상화폐와 강화학습

안녕하세요.

강화학습을 공부하는 정원석이라고 합니다.

Deeplearning으로 인해 Reinforcement Learning분야가 많이 발전하였습니다.

그로 인해 전에는 가능하지 않았던 사진 혹은 비디오 데이터를 input으로 받아서 에이전트가 그 상황에 따라 최고의 action을 선택하게 가르치는 것이 가능해졌습니다.


*[Playing Atari with Deep Reinforcement Learning](https://www.cs.toronto.edu/~vmnih/docs/dqn.pdf)*


아타리와 같은 게임에서는 강화학습이 비교적 잘 되는것 같은데... 현실에서도 이 강화학습을 사용할수 있을까? 라는 생각이 들었습니다.

*[강화학습의 적용 사례](https://www.oreilly.com/ideas/practical-applications-of-reinforcement-learning-in-industry)*

그러던 중, 위의 아티클에서 Reinforcement learning이 평소에 관심있던 financial trading에 적용될수 있다는 글을 보게되어
아직 연구단계인것 같지만 의미있는 연구라고 생각되어 관련공부를 시작하였습니다.

여기서 의미있는 연구란: 
금융공학으로 미국랭킹 1위인 대학교에 들어가기 위해 열심히 공부했는데, 갑자기 하기 싫어져서 다른 전공을 하게된.. ( 이상한 전공을 스스로 만듬 ) 
지금 생각하니 너무 아까워서 이제라도 공부해보자라는 의미;;

https://www.quantnet.com/mfe-programs-rankings/



그러므로! 

개인적으로 만족할만큼의 강화된 trading bot을 만들때까지 공부한 자료들을 포스팅할 예정입니다. 

첫번째로, 

이 포스트에서는 강화학습이 financial 혹은 cryptocurrency(가상화폐, 암호화폐)에서 적용될수 있다는 것을 설명하도록 하겠습니다.
(이 포스트는 Denny Britz의 글을 바탕으로 작성하였습니다.)



하지만 궁금한것은 
주식시장에서 금융모델을 만들기 위하여 머신러닝을 사용하는것으로 알고 있는데,
왜 머신러닝 기술을 계속 사용하지 않고 강화학습을 적용하려고 하는 걸까요?


*강화학습 관점에서 문제를 보기전에, 기존에 적용되고 있는 supervised learning에서 어떠한 트레이드 전략으로 이익을 얻는지 알아보도록 하겠습니다.*

Price prediction을 하여 트레이딩을 하는 알고리즘을 만든다면, 

1. 미래에 가격이 높아질 것이라고 판단 : 지금 사고 market이 예측값으로 오르면 매도한다.

2. 미래에 가격이 낮아질 것이라고 판단 : short( 현재 가지고 있지 않은 자산을 빌려옴 ) 을 하고 마켓이 예상범위로 들어왔을때 매수한다. 


하지만 이 과정에는 몇가지 문제점이 있습니다. 

*1.한번 주문할때 매수가격이 일정하지 않습니다. *

주문할때 order books에서 그때 가능한 양의 가상화폐만을 매수할수 있기 때문입니다. 그리고 또한 매수할때 그에따른 수수료도 지불해야 합니다. 

*2.Time scale의 문제.*
predict를 해야하는 시간( 초, 분, 시간, 일 , 달) 의 확실하지 않기에 더욱 예측하기가 힘듭니다.

쉽게 이해하기 위해 위의 문제점들을 예로 들어본다면,


현재 비트코인의 가격이 10,000이고 5분뒤에 10,050으로 상승할 것이라고 Predict하였다고 가정할때, 우리는 과연 5분뒤에 50을 벌수 있을까요??  

비트코인 1개의 가격이 현재 10,000이고 5분뒤에 10,050으로 상승할 것이라고 예측했습니다.  그래서 저는 5분 뒤에 50의 이익을 얻기 위하여 비트코인 1개를 매수하려고 합니다. 

하지만 order book에 그 가격에 파는 비트코인이 충분하지가 않습니다. 

저는 그래서 0.5BTC는 $10,000에 나머지 반은 $10,010에 매수하였습니다. 
그렇데 되면 저는 비트코인 한개 가격은 10,050에 매술하였고 수수료 0.03%(GDAX 기준)을 지불하였습니다. 

우리가 Predict한대로 비트코인의 가격이 잘 올라 $10,050이 되었고, 이제 비트코인을 매도하려고 합니다. 

Sell order을 걸어놓았지만, market이 너무 빠르게 움직여서 바로 $10,040으로 떨어졌고 매수할때와 마찬가지로 1BTC를 한번에 사지 못하였습니다. 

그래서 어쩔수 없이 0.5BTC는 $10,045에 나머지 0.5BTC는 10,040에 매도하였습니다. 결국 비트코인 1개를 $10,042.5에 매도하였고 매도 수수료 0.03%까지 지불하였습니다.

**그래서 결국 얼마를 벌었을까요???**

-10005,30-30+100042.5 = -$22.5 

$50 을 벌수 있다고 predict하였는데, 실제로는 $22.5를 손해보았습니다. 

결국 price predict이외에도 장기의 price movement와 아래의 세가지도 함께 predict해야 합니다. 

1. 자산의 유동성
2. 네트워크의 지연
3. 수수료 
 
하지만 supervised learning은 이 세가지 문제를 계산에 넣을수 없습니다. 

그리고 위의 예제에서 또다른 supervised learning의 문제점은!

비트코인의 가격이 5분뒤에 오를것이라고 예측했고, 실제로 우리가 예상한 것만큼 올랐다는 가정이었습니다. 

하지만,

만약에 오르지 않고 떨어졌다면? 잠깐 올랐다가 바로 떨어졌다면.. 또 predict할때 65%로 오르고 35%로 떨어질것이다라는 예측을 하게 되면 어떻게 해야할까요? 

위의 문제점을 해결하기 위해 market을 predict할때 price prediction model뿐만이 아닌 rule-based-policy도 필요하다는 것입니다. (Price를 input으로 받았을때 무엇을 할것인지 결정하는것 : 주문해라, 가만히 있어라 주문을 취소해라 등등) 

많은 사람들이 직관들 따라서 하는 이 policy를 어떻게 만들까요? 또 이 policy를 최적화 시키는 방법은 어떤 방법이 있을까요 ?? 

다행히도 그 문제를 해결하는 방법은 여러가지가 있지만, 아주 효과가 좋지는 않습니다. 전형적인 trading strategy의 workflow는 다음과 같습니다. 

1. Data Analysis : Data를 분석합니다. ( 차트를 보거나 통계 ) trading strategy를 찾기위해 아이디어를 내는 과정입니다.
2. Supervised Model Training : Strategy를 찾기위해 (가능하다면) supervised learning을 사용하여 학습시킵니다.
3. Policy Development : rule-based policy를 설립합니다.
4. Strategy Backtesting: 시뮬레이터를 이용하여 모델을 실험합니다.
5. Parametere Optimization : Parameter을 최적화 합니다. 
6. Simulation & Paper Trading : strategy를 실제 상황에 적용하기 전에 real-time 시장에서 시뮬레이트 해봅니다. 
7. Live Trading: 실제 상황에서 trading을 합니다. 

하지만 위의 방법은 몇가지 문제점이 있습니다. 

1. Iteration cycles이 느립니다. 1-3번은 직관에 의한것이며 4,5번에 실험하며 최적화 시킬때까지 이 model이 잘되는지 안되는지 모릅니다. 만약 잘못되면 1번부터 다시 돌아가야 하는 상황도 발생합니다. 
2. Simulation이 너무 늦게 옵니다. step 4까지 우리가 아까 추가로 고려해야 한다고 했던 자산의 유동성, 네트워크의 지연, 수수료 등을 학습시키지 못합니다. 
3. Policies가 사람이 할수 있는것으로 한정되어있습니다. 


위와 같이 기존 supervised learning으로 trading을 했을때 발생할수 있는 여러가지 문제점들에 대해 알아보았습니다. 

어떻게 강화학습은 이러한 문제점들을 해결할수 있을까요? 

다음포스팅에서 강화학습의 기초부터 강화학습을 가상화폐 price preditioc에 어떻게 적용하는지 알아보도록 하겠습니다.











# References
* Deep Reinforcement Learning Framework for the Financial Portfolio Management Problem
htps://arxiv.org/abs/1706.10059

* Reinforcement Learning for Automated Financial Trading: Basics and Applications
https://link.springer.com/chap…/10.1007/978-3-319-04129-2_20

* Introduction to Learning to Trade with Reinforcement Learning
http://www.wildml.com/2018/02/introduction-to-learning-to-trade-with-reinforcement-learning/
