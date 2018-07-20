

title: "On-policy Control with Approximation"
date: 2018-07-20 12:26:28 -0400
categories: Reinforcementlearning update

## use_math : true



<img src="https://www.dropbox.com/s/2osdrb8vt5g3l4m/Screenshot%202018-07-16%2021.10.10.png?raw=1">



이번에는 function approximation 을 사용하여 state value가 아닌 state-action pair을 구하는 방법을 알아보도록 하겠다. 


<img src="https://www.dropbox.com/s/6kx91r0txgt6iql/Screenshot%202018-07-16%2021.17.31.png?raw=1">

state-action pair의 approximation value는 state $S_t$, action $A_t$를 input으로 받고 function approximation의 weight에 의한 State $S_t$, Action $A_t$의  $\hat{q}$를 출력한다. 

Action value에 function approximation을 적용할때 update weight rule은 다음과 같다. 

$$w_{t+1} \doteq w_t + \alpha [U_t - \hat{q}(S_t,A_t,w_t)]\bigtriangledown \hat{q}(S_t,A_t,w_t)$$



여기서 $U_t$는 Target으로 On-policy의 example of target는 다음과 같다. 



##### Monte Carlo : $G_t$

##### Sarsa(one step) : $R_{t+1}+ \gamma \hat{q}(S_{t+1}, A_{t+1}, w_t)$

##### $w_{t+1} \doteq w_t + \alpha [U_t - \hat{q}(S_t,A_t,w_t)]\bigtriangledown \hat{q}(S_t,A_t,w_t) $



Episodic 일때 On-policy 중 하나인 Sarsa를 이용하여 $\hat{q}$ 를 Estimate하는 방법은 다음과 같다. 


<img src="https://www.dropbox.com/s/c7lhhjx7dk9n622/Screenshot%202018-07-18%2009.24.36.png?raw=1">





기존에 사용되었던 episodic task에서 time step $t$ 마다 받은 reward를 합하는 방법이 아닌, 

 continuing task에서는 reward를 평균을 내는 방법을 사용한다. 

그러므로 더이상 discount facor는 사용되지 않는다. 

 

##### 1. Continuing task 를 위한 새로운 goal : time step마다 평균 reward를 최대화 한다. 

<img src="https://www.dropbox.com/s/7821d6dpe1jy1nv/Screenshot%202018-07-19%2022.14.00.png?raw=1"> 



Continuing task에서는 policy를 따라 action을 선택하였을때의 평균 reward 를 maxize 를 하는 것으로 목표가 바뀐다. 

$\mu_\pi (s) : S \rightarrow [0,1]$ 은 $\pi$ 를 따른 steady-state distribution이며 여기서 $\pi$는 on-policy distribution 이며 다음과 같이 정의할 수 있다. 

##### $$\mu_\pi (s) \doteq lim_{t \rightarrow \infin} Pr \lbrace S_t = s \mid A_{0:t-1} \sim \pi \rbrace$$

$r(\pi)$ 는 reward rate이며 average reward 이다. 



##### 2. Average reward 일때 모든것이 새로워진다. 



Return 

$$G_t \doteq R_{t+1} - r(\pi) + R_{t+2}- r(\pi) + ...$$

매 time step $t$ 마다 받는 reward를 average reward와 비교한다. 

이로 인해 매 time step 마다 받는 reward가 average reward 보다 큰지 작은지 비교하는 것이 가능하다. 

그러므로 Bellman Equation은 다음과 같이 바꿀수 있다. (Discount factor가 들어가지 않는다.)

<img src="https://www.dropbox.com/s/tfl8m9omrvvc1wy/Screenshot%202018-07-19%2023.24.07.png?raw=1">

Continue task에서는 State를 얼마나 많이 occur했는지를 계산에 넣을때 discount factor를 더이상 사용하지 않고 state distribution term을 넣은 reward rate와 매 step마다 비교하는 방법을 사용한다. 



(사실은 이 방법은 잘 이해가 되지 않습니다. )

<img src="https://www.dropbox.com/s/q9vzuvst6mgo9f1/Screenshot%202018-07-19%2023.24.35.png?raw=1">





