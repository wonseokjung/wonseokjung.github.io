---
title: "제2편: 강화학습의 거의 모든것"
date: 2018-4-12 12:26:28 -0400
categories: Reinforcementlearning update
use_math : true
---


### 제2편: 강화학습의 거의 모든것

Multi-armed Bandit은 아주아주 간단한 Reinforcement 의 문제중 하나이지만, 

이 챕터를 다시 읽어보며, 이 간단한 Multi-armed Bandit을 풀기위해 적용된 방법들이 현재 강화학습 이론이 바탕이 되었구나 라고 판단되었다. 

그만큼, 매우 중요하니 꼭 볼것!



#### 순서 

##### Multi-armed Bandits 

1.A k-armed Bandit Problem

2.Action-value Method

3.The 10-armed Testbed

4.Incremental Implementation

5.Tracking a Nonstationary Problem

6.Optimistic Initial values

7.Upper-Confidence-Bound Action Selection

8.Gradient Bandit Algorithms

9.Associative Search ( Contextual Bandits )





#### Multi-Armed Bandits 

Reinforcement learning는 다른 learning과 구분지을수 있는 특징으로는 ,

action을 evaluate 한다는 것이다. 

이렇게 evaluate하는 과정은 좋은 behavior을 찾기 위해 active exploration을 한다는 것이ㅏ. 

Feedback의 종류가 두가지가 있는데, 

* 첫번째는 evaluaute feedback으로 선택된 action에 따라 달라지는 feedback

* 두번째는 intructive feedback으로 선택되어진 action과 독립적인 feedback

이 챕터에서는 Reinforcement learning ( 강화학습 ) 의 evaluate feedback에 대하여 한 상황에 대해서만 행동을 하도록 학습하는 

간단한 예제를 통하여 배울 것이다. 



### 1. A k-armed Bandit Problem

K-armed Bandit 의 learning problem은 다음과 같다. 


==“Bandit problems embody in essential form a conflict evident in all human action: choosing actions which yield immediate reward vs. choosing actions (e.g. acquiring information or preparing the ground) whose benefit will come only later” - P. Whittle (1980).==



1.k-different option이나 action을 선택할 수 있다. 

2.한 번 action을 선택할 때마다 stationary probability distribution에서 numberical reward를 받는다.

3.목표는 한정된 시간동안 expected total reward를 maximize 시키는 것이다. 


k-armed Bandit의 의미는 k개의 lever가 있는 slotmachine이다.

![2155136](https://user-images.githubusercontent.com/11300712/38657749-732b5122-3e11-11e8-810f-aed3abf5a43f.png)


http://www.primarydigit.com/


각 action을 선택할때는 machine의 lever를 당기는 것과 같고, 

rewar는 jackpot을 터트려 받는 돈과 같다. 

action을 select하는 것을 반복하며 reward를 많이 주는 최고의 lever 찾아 승률을 최대화 시킨다. 

k-armed bandit problem 에서는 각 action이 선택되었을때, 그 action을  expected 또는 mean reward를 한다. 

이와 같은 것을 value of the action ( 그 행동을 한 가치)라고 한다. 

그 시간에 선택된 action을 $$A_t$$ 라고 하며, corresponding한 reward를 Reward $$R_t$$라고 한다.  

Action a가 선택되었을때의 expected reward를 $$q_*(a)$$ 라고 하며, 다음의 식으로 정의한다. 


$$q_*(a)\doteq \mathbb{E}[R_t | A_t=a]$$


**만약 각 action의 value를 알고 있다면, 가장 높은 reward를 받는 선택만 선택**하므로 bandit 문제를 해결할 수 있을 것이다. 

여기서는 action value를 모른다고 가정하며,

시간 (time ste t)에서 선택된 action을 estimate value를 $$Q_{t}(a)$$를 $$q_*(a)$$가 되기를 원한다. 

Action value를 측정할때 항상 최소한 하나의 ation은 가장 큰 value를 estimate할것이다. 
(예를 들어서 10개의 레버를 당겼을때 최소한 한개의 레버는 그중 가장 큰 reward를 줄 것이다.)

위와 같이 가장 큰 value를 선택하는 action을 greedy한 action 이라고 부르며, 

greedy 한 action을 선택할때는 exploiting한다고 한다. 

반대로, 만약 greedy 하지 않은 action중 하나를 선택한다면 explorating 한다고 한다. 

Exploitation으로 한 step에서의 expected reward를 maximize하기위해 greedy action을 선택할 수 있지만,

현재 당장 받는 reward가 적더라도, long term에서는 exploration을 함으로서 더 높은 reward를 받을 수도 있다. 


### 2. Action-value Methods

ACtion 을 선택했을때의 action value를 estimate하는 방법에 대하여 자세히 알아보겠다. 

먼저 여기서 True action value의 정의는 action이 선택되었을때의 평균 reward이다. 

이 True action value를 계산하는 방법은 실제로 받을 reward를 평균한 값이다. 

$$Q_t(a)\doteq \frac{sum of rewards when a taken prior to t}{numberotimes a taken prior to t}$$


$$= \frac{\sum_{i=1}^{t-1}R_i \cdot \mathrm{1}_{Ai=a}}{\sum_{i=1}^{t-1}\mathrm{1}{Ai=a}}{}$$



여기서 $$1_{predicate}$$ 은 predicate가 true면 1, 아니면 0이다. 

만약denominator가 0이면 $$Q_t(a)$$는 default value 정의한다. 

그리고 denominator가 무한대로 가면 $$Q_t(a)$$는 $$q_*(a)*로 converge한다. 

이런 방법을 action value를 estimate한  **sample-average**라고 하며, 이외에도 action value을 estimate하는 방법은 여러가지가 있다. 

그렇다면 action을 선택하는 방법에는 무엇이 있을까? 

1. $$$Greedy action$$$
2. $$$\varepsilon - greedy$$$


1.$$$Greedy-action$$$

$$$A_t \doteq \underset{a}{argmax}Q_t(a)$$$

greedy action은 위와같이 정의하며, $$\underset{a}{argmax}$$ 는 $$Q_t$$를 가장 maximize 시켜주는 action.

현재의 정보를 통해 바로 받는 reward를 maximze 하는 action을 선택한다. 

2.$$$\varepsilon - greedy$$$

가끔 epsilon의 확률로 greedy한 action이 아닌 다른 action을 선택하는 $$$\varepsilon - greedy$$$ 방법이 있다.

exploitation으로 한 step에서의 expected reward를 maximaze하는 action을 선택할 수 있지만, 어쩌면 long term에서는 exploration을

하여 미래에 더 높은 reward를 얻을수도 있을 것이다.

### 3. The 10-armed Testbed

Greedy와 $$$\varepsilon - greedy$$$ 방법을 실험하기 위해 비교 실험을 하였다. 

k-arm bandit ( k = 10)에서 2000개의 데이터를 random 하게 생성하였고,( normal distribution, mean 0, variance 1의 조건)


![10arm](https://user-images.githubusercontent.com/11300712/38712359-08b73d6a-3f06-11e8-9b27-12e0d2d09e14.JPG)


두번째는 greedy method 와 $$$\varepsilon - greedy$$$ method의 비교이다. ($$$\varepsilon$$$= 0.1 과 $$$\varepsilon$$$= 0.01)

10 Arm bandit에서 sample-average technique를 사용하였다.  


아래의 그래프는 experience 에 따라 expected reward가 증가하는 그래프이다. 

greedy 가  $$$\varepsilon - greedy$$$ 보다 average reward가 증가하는 량이 느리다. 

![epsilon](https://user-images.githubusercontent.com/11300712/38712617-d9693e3a-3f07-11e8-8df5-4d61cc3f120b.JPG)

step 1000에서의 average rewawrd도 greedy방법이 1 정도로 가장 낮다. 

gredy action은 suboptimal action에 갇히기 쉽게 때문에 $$$\varepsilon - greedy$$$ 에 비해  성능이 나쁘다. 

밑의 graph에서 greedy는 금방 optimal한 action을 찾고 더이상 향상되지 않는다. 

$$$\varepsilon - greedy$$$ 는 계속 optimal action  을 찾기 위해 explore하고 개선하여 더 좋은 결과를 보인다.



### 4.Incremental Implementation

지금까지 다룬 actin-value method는 관찰된 reward를 sample average하여 estimate한 것이다. 

좀 더 효율적인 방법으로 action-value를 측정할 수는 없을까?

$$$Q_n$$$은 action이 n-1만큼 선택된 후에 estimate 된 action value 이다. 

$$$Q_n$$$은 다음과 같이 정의할 수 있다. 


$$$Q_n \doteq \frac{R_1 + R_2 + .... R_{n-1}}{n-1}$$$


Obvious implementation은 모든 reward를 기록할때, estimate value가 필요한때 언제라도 사용할 수 있다. 

하지만 reward가 늘어날 때마다 memory와 computation이 늘어날 것이다.

하지만 Incremental formular을 사용하면 위의 방식이 필요없다. 


$$$Q_n \doteq \frac{R_1 + R_2 + .... R_{n-1}}{n-1}$$$


의 식을 아래와 같이 바꿀수 있다. 

![qn 1](https://user-images.githubusercontent.com/11300712/38713568-4871163c-3ec1-11e8-9d5e-249ae8871b50.JPG)

이 Implementation 방법은 $$Q_n$$과 $$n$$, $$R_n$$의 메모리만 있으면 된다. 

*Simple Bandit Algorithm* 의 Pseudocode는 아래와 같다. 


![bandital](https://user-images.githubusercontent.com/11300712/38713729-2aa102a6-3ec2-11e8-8665-acc1f16c46eb.JPG)


1.Q와 N을 0으로 초기값을 준다. 

2.Action은 $$$\varepsilon - greedy$$$ 로 선택된다. 

3.선택된 Action을 통하여 Reward를 받는다. 

4.N(A)+1로 N(A)를 업데이트 한다. 

5.Reward - Q(A) 값에 1/N(A)를 해준것에 Q(A)를 더해주어 Q(A)를 업데이트 한다.

1/n(a)를 stationary 한 경우에 사용하는 이유는, 

action이 많아지면 많아질수록 1/n 의 값이 커질 것이고, 그에따라  뒤의 [Rn-Qn] 의 값이 점점 작아질것이다. 
  
여기서 Reward - Q(A)는 estimate의 error라고 하며, 다음과 같이 표기할 수 있다. 

$$$New Estimate \leftarrow OldEstimate + StepSize[ Target - OldEstimate]$$$


[ Target - OldEstimate ] 가 estimate의 error이며, 이 error는 step을 진행하면서 Target에 가까이 갈수록 줄어든다. 

Target은 이동하여야 하는 바람직한 방향이며,*여기서* Target은 n번째의 reward이다. 

Step-size parameter는 time step 마다의 incremental method이며, action a 의 n번째 step size는 1/n 이다. 

$$$\alpha$$$또는 $$$\alpha _t(a)$$$ 라고 정의하기도 한다. 

### 5. Tracking a Nonstationary Problem 

Stationary 한 Bandit problem을 Average하는 방법을 알아보았다. 

(여기서 stationary 하다는 것은 reward의 probabilites가 시간에 따라 변하지 않는다.)

Reinforcement learning에서는 가끔 nonstationary한 상황과 마주치는데,

(예를 들어 최근의 받은 reward를 오래전에 받은 reward보다 weight를 더 많이 준다.)


이것을 하는 방법으로 유명한 방법은 ste-size parameter을 조절하는 것이다. 

예를 들어, Incremental Update rule ( n-1의 reward의 평균 Qn을 update )

또는 step마다 step-size parameter을 다르게 하는 방법도 있다.

이 방법은 다음과 같이 정의할 수 있다. 

$$Q_{n+1} \doteq Q_n + \alpha [R_n - Q_n]$$

여기서 $$$\alpha$$$ 는  $$$\alpha \in (0,1] $$$이며, 

$$$Q_{n+1}$$$ 는 weighted average의 과거 reward이다. 


위의 식을 풀면 다음과 같이 정의할수 있다. 


![asdasd](https://user-images.githubusercontent.com/11300712/38720290-aa1d6880-3f30-11e8-917e-1e6f7e0cb87c.JPG)


이것은 weighted average라고 부르는데 weight의 합이 항상 1이 되기 때문이다. 


![total1](https://user-images.githubusercontent.com/11300712/38720570-b1a43754-3f31-11e8-895d-abea0fc8ac96.JPG)


$$$\alpha(1-\alpha)^{(n-i)}Ri$$$ 에서 Reward는 n-i에 따라 weighted 되어진다.


Step 마다 step-size parameter을 다르게 하는 방법도 있다. 


$$$\alpha$$$_{n}(a)은 action a가 선택되는 














 









