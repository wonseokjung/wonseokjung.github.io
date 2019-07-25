---
title: "Metalearning shared Hierarchy 논문 리뷰 "
date: 2018-08-28 12:26:28 -0400
categories: Reinforcementlearning update
use_math : true
---



<img src="https://www.dropbox.com/s/ea60km7lmpqahzd/Screenshot%202018-08-26%2017.49.53.png?raw=1">





MetaLearning 방법으로 새로운 task를 더 효과적으로, 빠르게 학습할수 있는 방법을 알아보도록 하겠다. 

<img src="https://www.dropbox.com/s/loxpjz3o6qtztbo/Screenshot%202018-08-26%2017.49.23.png?raw=1">

오늘 볼 논문은 Meta Learning shared Hierarchies 란 논문으로 다음의 목차로 설명하도록 하겠다. 

<img src="https://www.dropbox.com/s/deqaw8dhoblax6u/Screenshot%202018-08-26%2017.50.32.png?raw=1">





# 1. Introduction 

<img src="https://www.dropbox.com/s/awpw7rqraxkbhg4/Screenshot%202018-08-26%2019.38.46.png?raw=1">



사람은 새로운 업무 , 테스크를 마스터하기 위해서 이전에 배웠던 지식들을 활용하여 더욱 빨리 배운다. 







반면 강화학습 알고리즘은 각 테스크들은 독립적으로 처음부터 배우는 경우가 많다.

https://www.youtube.com/watch?v=IjvbhwuCaF0



그래서 여기서 논문에서 소개된 것은, 

강화학습 에이전트가 새로운 테스크를 빨리 배우기 위한 setting이 무엇인지 연구하였다. 





<img src="https://www.dropbox.com/s/auet1yxdstg8cuf/Screenshot%202018-08-26%2018.34.05.png?raw=1">





여기서 문제가 되었던것은, 

1. 모든 테스크가 정보를 공유하여 배우도록 하고 싶다. 
2. 하지만 각의 테스크에는 각각 다른 optimal policies가 존재한다.
3. 어떠한 특정  Policy를 따라 action을 선택하면 모든 테스크의 policy는 suboptimal일 것이다. 





<img src="https://www.dropbox.com/s/84q1bztprf7lxs4/Screenshot%202018-08-26%2018.54.46.png?raw=1">



이 이슈를 해결하기 위해 sub-policies 를 공유하는 집합을 포함한 model을 만드는 방법을 제안한다. 

새로운 task를 빨리 배우도록 하기 위한 Sub-policies의 end- to-end trainning 방식을 제안하며, 이 방법은  학습된 master policy에 따라 조정된다. 

이 방법은 은 Sutton 1999, Baco 2016의 Option framwork와 관련이있다. 



<img src="https://www.dropbox.com/s/9oroike8h7tft00/Screenshot%202018-08-26%2019.32.55.png?raw=1">



Hierarchical 한 구조와 opmization algorith을 포함한 방법을 MLSH ( metalearning shared hierarchies )라고 부르도록 하겠다. 

MLSH방법을 사용하여 2D continous 환견, gridworld navigation, 3d physics 테스크에 적용해보았으며, 

3D 환경에서 같은 policy를 사용하여 humanoid이 걷고 기는것을 할수 있는 것을 확인 하였다. 



또한 4개의 다리가 달린 로봇이 sparse-reward obstacle courses의 미로를 풀기위해 움직이는 것도 확인하였다. 



<img src="https://www.dropbox.com/s/52jmx4bdc6zi44d/Screenshot%202018-08-26%2019.39.42.png?raw=1">



---



# 2. Problem Statement



<img src="https://www.dropbox.com/s/o79ebhja8q96uzd/Screenshot%202018-08-26%2019.43.09.png?raw=1">









### Notation



<img src="https://www.dropbox.com/s/ol44qwwh3jtxs4u/Screenshot%202018-08-26%2019.47.41.png?raw=1">



---

기본적인 Reinforcement Learning notation은 위와 같다. 

추가적으로, 

$$P_M$$ : distribution over MDPs $$M$$ 

$$\pi_{\theta , \phi (a \mid s)}$$ : Agent는 parameter vector $$\theta, \phi $$  를 주기적으로 update한다. 

$$\phi$$ : 모든 테스크가 share하는 parameter이다.  ( 테스크들 끼리 공유하는 파라미터의 집합)

$$\theta$$ : 각 task마다 scratch부터 배운다. ( 각 테스크 파라미터의 집합, agent가 현재 테스크 M을 배우며 업데이트 하는 파라미터) 

---





Agent는 task에서 time step $T$ 동안 상호작용을하며, 여러번의 Episodes동안 total reward $R$을 받는다. 



## $R = r_0 + r_1 + r_2 + .... + r_{T-1}$



Meta learning objective는  agent가 lifetime 동안 받은 expectre return을 optimize한다. 



## $maximize_\phi E_{M \sim P_M}, t=0... T-1 [R]$







<img src="https://www.dropbox.com/s/doxs0xo07vajwx6/Screenshot%202018-08-27%2000.04.40.png?raw=1">





 Object가 하려고 하는 것은, 새로운 MDP를 만났을때 agent가 새로운 MDP의 parameter $\theta$에 적응하여 높은 reward를 받을수 있게 하는 shared parameter vector $\phi$를 찾는 것이다.  



Per-task parameter $\theta$ 와 shared parameter $\phi$를 결합시키는 방법은 여러가지가 있지만, 여기서 제시하는 구조는 hierarchical reinforcement learning에서 motivated된 방법이다. 



Shared Parameter vector $\phi$ 는 subvector $$\phi_1, \phi_2, \phi_3 ... \phi_K$$ 의 집합으로 구성되어 있다. 

$$\phi_k$$는 sup-policy로  $$\pi_{\phi_{k}} (a \mid s)$$ 로 정의한다. 



Parameter $\theta$ 는 sub-policies와 swetches되는 seperate neural network이다. 



Parameter $\theta$ 는 stochastic polcy로서 master policy 라고 정의한다. 이 master policy의 action은 index k를 선택한다. 

Master policy는 또한 sub-policies $\phi_k$보다 늦게 action을 선택한다. ( hierarchical policy architectures , sutton, options )

Master policy는 action을 sample 할때 일정한 주기 N time stpe으로 action 을  sample 한다. 



## $t = 0 , N , 2N...$



<img src="https://www.dropbox.com/s/7zaxu2r6ta9brgk/Screenshot%202018-08-26%2021.46.57.png?raw=1">

---



 ## 3. Algorithm



Sub-policies의 집합을 반복적으로 학습시켜 agent가 T-step 동안 받을 reward를 maximize 시키는 것이 목적이다. 



Optimal sub-policies 집합은 고 성능을 내기 위해서는 충분히 fine-tuned 되어야하며, 여러가지 테스크에서도 작동하여야 한다. 

이 연구에서는 sub-policy parameter $\phi$ 가 위에 요구를 충족하는 방법을 제안한다. 



### 3.1 Policy Update in  MLSH 



 여기서는 sub-policy parameter $\phi$ 를 학습시키기 위한 MLSH(metalerning shared hierarchies)알고리즘에 대해서 설명하도록 하겠다. 



<img src="https://www.dropbox.com/s/l81wy6wzpl2tvua/Screenshot%202018-08-27%2018.26.35.png?raw=1">



MLSH의 두 가지 구성으로 나누어져있다. 

1. Warm up

2. Joint update period 









Warmup period는 master policy parameter $\theta$를 optimize하며 Joint update period는 $$\theta$$, $$\phi$$ 를 같이 Optimize 한다. 

---



 이렇게 크게 두가지로 구성된 MLSH 알고리즘의 큰 구조는 다음과 같다. 

- master-policy $$\theta$$와 sub-policies $$\phi$$를 초기화 시킨다. 

1. task $M$을 distributio  $P_M$에서 sample한다. 

2. warm up period : master-policy $$\theta$$를 optimize 시키기 위하여 warmup period를 run한다. 
3. Joint update period : master-policy $$\theta$$와 sub-policies $$\phi$$가 함께 업데이트 된다. 
4. task를 sample한 뒤 master-policy $$\theta$$를 reset하고 다시 반복한다. 

---



Warmup period 와 Joint Update period의 정확한 정의는 다음과 같다. 



###  - Warmup period 

<img src="https://www.dropbox.com/s/i4srewk8mwvbuu2/Screenshot%202018-08-27%2018.39.38.png?raw=1">



<img src="https://www.dropbox.com/s/isa9utrggh4uc2n/Screenshot%202018-08-27%2019.41.40.png?raw=1">

1. master-policy $$\theta$$를 optimize 시킨다. 

2. task를 sample한다. 

   $$\pi_{\phi, \theta}(a \mid s)$$ 를 따라 D time step 만큼의 experience를 기록한다.

3. 위에 기록한 experience는 master policy의 관점에서 보며, 그림으로 이해하면 아래와 같다. 

<img src="https://www.dropbox.com/s/eeaspm2qiqa7k0i/Screenshot%202018-08-27%2019.24.19.png?raw=1">

4. master policy 에 의해 sub-policy $$\phi$$ 중 하나가 선택된다. 
5. N timestep 뒤에 $$\theta$$를 reward가 maximize 되는 방향으로 업데이트를 한다. 
6. 위에 parameter $\theta$는 강화학습 알고리즘 (DQN, A3C, TRPO , PPO 등) 으로 업데이트 한다. 
7. 이 작업을  W 만큼 반복한다. 







### - Joint update period 

<img src="https://www.dropbox.com/s/fmf7pmp1evnyib9/Screenshot%202018-08-27%2018.40.13.png?raw=1">





<img src="https://www.dropbox.com/s/ojpoqp5pl63e4oo/Screenshot%202018-08-27%2019.42.05.png?raw=1">



1. master-policy $$\theta$$와 sub-policies $$\phi$$가 함께 업데이트 된다.

2. U iteration동안, $$\pi_{\phi, \theta}(a \mid s)$$ 를 따라 D time step 만큼의 experience 만큼 모은뒤 $$\theta$$를 업데이트 한다. 

3. 이번에는 sup-optimal 관점에서 보자. master policy는 환경의 일부중 하나이다. 

   <img src="https://www.dropbox.com/s/hji36pu07rlxzeg/Screenshot%202018-08-27%2019.25.00.png?raw=1">



4. parameter을 update할때 master policy에 의해 ativated된 policy만을 업데이트 한다. 









<img src="https://www.dropbox.com/s/i0qqxdgkfneqsqr/Screenshot%202018-08-27%2019.49.33.png?raw=1">



왼쪽의 master policy를 훈련하는 파트 : 

업데이트를 할때 master policy의 action과 total reward에 의하여 업데이트를 한다. sub-policies의 action과 reward는 환경의 transition의 일부로 treat한다. 



오른쪽 의 sub-policie 훈련하는 파트 : 

Sub-policies 를 학습시킬때는, master policy의 action을 observation의 일부로 treat 하여  updte 한다. 다른 actions들과 timesteps 는 무시한다. 















<img src="https://www.dropbox.com/s/p09nnor86odr8ma/Screenshot%202018-08-27%2022.32.57.png?raw=1">



작은 초록색 공은 Agent 이며 나머지 두 개의 공은 goal이다. agent가 공의 일정범위 안에 들어가면 reward를 1 받고 그렇지 않으면 reward을 0을 받는다. 



<img src="https://www.dropbox.com/s/d6q36gl3sqelxde/Screenshot%202018-08-27%2022.45.21.png?raw=1">







<img src="https://www.dropbox.com/s/ra6p0b2swyubgfw/Screenshot%202018-08-27%2022.50.15.png?raw=1">





<img src="https://www.dropbox.com/s/xu9oa27wb0cbj2r/Screenshot%202018-08-27%2022.49.31.png?raw=1"> 











