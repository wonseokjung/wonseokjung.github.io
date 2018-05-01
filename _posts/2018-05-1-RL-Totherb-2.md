---
title: "제2편: To the Rainbow- Noisy Networks for Exploration"
date: 2018-5-1 12:26:28 -0400
categories: Reinforcementlearning update
use_math : true
---


## To the Rainbow 제 2편 : Noisy Networks for Exploration


Weight에 Parametric noise를 추가한 Deep Reinforcement 방법인 Noisy Net을 소개한다.



![5_1_noisynetworks 001](https://user-images.githubusercontent.com/11300712/39461984-444c74ec-4cc3-11e8-9375-414e7b133543.jpeg)


![5_1_noisynetworks 002](https://user-images.githubusercontent.com/11300712/39461985-447c9eec-4cc3-11e8-8f45-5fcfb94a8021.jpeg)

#### 1. 소개


![5_1_noisynetworks 003](https://user-images.githubusercontent.com/11300712/39461986-44d7a5da-4cc3-11e8-8482-fbcd29a28f78.jpeg)

RL에서 exploration을 하는 방법으로 agent가 action을 선택할때, policy를 기반으로 한 $$\epsilon-greedy$$ (sutton & Barto, 1998) 혹은 entropy regularisation (Williams, 1992) 을 사용하였다. 


![5_1_noisynetworks 004](https://user-images.githubusercontent.com/11300712/39461987-4507d854-4cc3-11e8-8cfa-a533eec15820.jpeg)

RL에서 위와같은 방법은 작은 state-action이나 linear function에 제한이 되어있고 좀더 복잡한 neural network같은 function approximator에서는 적용하기가 쉽지 않다. 


![5_1_noisynetworks 005](https://user-images.githubusercontent.com/11300712/39461988-4572c5f6-4cc3-11e8-82e9-4f2826ef1526.jpeg)

또한 좀 더 구조화된 방법의 exploration은 환경의 reward signal을 motivation term으로 증가시켜 

exploration을 할때 reward를 bonus로 주는 방법을 사용하였다. 



![5_1_noisynetworks 006](https://user-images.githubusercontent.com/11300712/39461990-45bb8eda-4cc3-11e8-8aa2-9188f4af5658.jpeg)


Deepmind에서 2018년 2월 15일  Noisy Networks for exploration이란 논문으로 새로운 exploration 방법인 noisy net을 propose하였고. 

이 noisy net은 exploration을 하기위해 network weight의 pertubation을 배운다. 



 Key insight는 weight vector 하나를 바꿈으로서 복잡한 state-action에서 성공적으로 영향을 끼친다는 것이다. 


![5_1_noisynetworks 007](https://user-images.githubusercontent.com/11300712/39461992-4609e97c-4cc3-11e8-8d86-af4f6cd747d2.jpeg)



Pertubations은 noisy distribution에서 sample되어진다. 

이 알고리즘에서의 functional form은 neural network이며,  valuefunction을 randomized한다. 

![5_1_noisynetworks 008](https://user-images.githubusercontent.com/11300712/39461993-468bc8f2-4cc3-11e8-9bff-7bced2155ea7.jpeg)

또한 이렇게 randomized된 value function은 exploration 효과를 보인다고 입증 한다. 

![5_1_noisynetworks 009](https://user-images.githubusercontent.com/11300712/39461994-46dd4736-4cc3-11e8-86ed-33b395bf0c29.jpeg)



이 방법은 valuefunction을 측정하는 방법만이 아닌 A3C와 같이 policy gradient에서도 적용이 가능하다. 




Noisy network 가 적용된 DQN, Dueling, A3c를 실험해본 결과

noisy network version의 DQN, Dueling에서는 54개의 Atari에서 기존의 DQN, Dueling에 비해 아주 좋은 결과를 보였으며, 

A3C에서는 약간의 개선을 보였다. 


### 2. 배경지식


1.MDP

2.DeepRL(Q-learning)

3.Dueling




#### 2.1 MDP

MDP는 model stochastic, discrete-time과 finite action space control problem이다. 

MDP는 tuple로 이루어져 있으며 
$$ M = (\chi ,A,R,P,\gamma) $$ 

여기서

$$\chi$$: state space

$$A$$: action space

$$R$$: reward function

$$\gamma$$ : discount factor을 뜻한다. 

$$P(y|x,a)$$ : state $ㅈ$\chi$$에서 action a를 했을때의 y로 transition할 probability

$$\pi(.|x)$$ : stochastic policy $$\pi$$ 은 action에 따라 각 state로 mapping되어있다. 

$$\pi(a|x)$$ : state $$\chi$$에서 action a 를 선택할수 있는 probability 

와 같이 정의하며, 

이 policy $$\pi$$ 로 인해 결정된 action-value function $$Q_{\pi}$$는 아래와 같이 정의한다. 

$$Q_{\pi}(x,a)$$ $$=\mathbb{E}_{\pi}[\sum{t=0}^{+\infty }\gamma^{t}R(x_t,a_t) ]$$


$$\mathbb{E}_{\pi}$$ :x0, a0 에서 시작하여 policy 를 따른  trajectories (x0, a0,x1,a1 ... )에 의한 expectation


$$Q_{\pi}(x,a)$$는 x에서 a를 선택하며 policy를 따른 discounted된 reward의 총 합이다. 


higher return을 받지 않을때 policy가 optimal 되었다고 하며, optimal 된 value function은 아래와 같이 정의한다. 

$$Q_{*} = \underset{a}{argmax} Q_{\pi}(\chi,a)$$

마찬가지로, 

Policy를 따른 value fucntion $$V_{\pi}$$ 은 다음과 같이 정의한다. 

$$V_{\pi}(x)=\mathbb{E}_{a~\pi(.|x)}Q_{\pi}(\chi,a)$$ 


#### 2.2 Deep Reinforcement Learning

Deep Reinforcement Learning 은 RL methods에서 deep neural network를 function approximator로 

사용한다. 

Deep Q-Networks, Dueling architecture, Asynchronous Advantage Actor-Critic, Trust Region Policy Optimisation, Deep Deterministic Policy Gradient, Distributional RL은 모두 Deep reinforcement leaning 알고리즘의 예이다. 

위의 알고리즘의 기초는 Loss function $$L(\theta)$$ 를 Minimisation 하는 것이다. 
($$\theta$$는 network의 parameter의 의미한다.)

DQN (자세한 설명을 보고 싶다면 :https://wonseokjung.github.io//rl_paper/update/RL-PP-DQN/)

DQN은 neural network를action-value function의 optimal policy $$Q_{*}($$\chi $$,a)$$를 위한 approximator로 사용한다. 

DQN은  action-value function $$Q(x,a)$$의 neural network parameter $$\theta$$를 Minimising하여  optimal action-value function을 estimate한다.


![screen shot 2018-04-30 at 03 12 23](https://user-images.githubusercontent.com/11300712/39422902-523254ae-4c24-11e8-8cda-b5a9e0a7c9e5.png)



* D : Replaybuffer에서의 $$e = x,a,r = R(x,a)y~ P(.|x,a))$$ transition 을 따른

distribution에서 drawn한 것이다. 

* $$\theta^{-}$$ : 고정되어있는 target network  $$\theta$$ 가 $$\theta^{-}$$ 로 일정하게 업데이트 된다. 

* $$\epsilon -greedy$$는 acion-value function Q 를 greedily 선택하거나, $$\epsilon$$의

확률만큼 random 하게 action 을 선택한다. 


#### 2.3 Dueling

Dueling DQN은 DQN archtecture의 연장선이다. DQN과 main difference는 DQN 에서의 Q-network과 반대의 구조를 사용한다는 것이다. 

Dueling network는 action-value function을 estimate할때 두개의 parallel sub-networks를 사용한다. 

value와 advantage subnetwork는 convolutional layer을 사용한다. 

$$\theta_{conv}$$, $$\theta_{V}$$, $$\theta_{A}$$ convolutional endoer f, value network V, advantage network A 의 Parameter이라고 한다면, 

$$\theta = {\theta_{conv}, \theta_{V}, \theta_{A}}$$ 의 concatenation이다. 

이 두개의 networks의 output은 다음과 같이 정의한다. 


![screen shot 2018-04-30 at 03 20 40](https://user-images.githubusercontent.com/11300712/39423370-716a11fc-4c26-11e8-9316-b7c89bf25546.png)

또한, Dueling algorithm의 update 룰은 다음과 같다. 


![screen shot 2018-04-30 at 03 20 46](https://user-images.githubusercontent.com/11300712/39423369-70cdf722-4c26-11e8-9d4b-28dd2c16dc41.png)


distribution D 와 target network parameter set $$\theta^{-}$$은 DQN과 비슷하다. 




### 3.NoisyNets for Reinforcement Learning


![5_1_noisynetworks 010](https://user-images.githubusercontent.com/11300712/39462144-52d768f4-4cc4-11e8-98b4-09390e94316e.jpeg)



NoisyNets은 parametric function of noise에 의해 pertubed된 neural netwok의 weight와 bias이다. 

이 parameter들은 gradient descent에 의해 adapted된다. 

$$y=f_{\theta}(x)$$를 input을 x 로 받고 output을 y로 return하는 
noisy parameter $$\theta$$에 의한 neural netwrok parameter라고 하자. 

이 Noisy parameters $$\theta$$ 는

$$\theta \overset{def}{=} \mu + \sum \odot \epsilon$$

로 정의할수 있고. 

$$\zeta \overset{def}{=} (\mu, \sum)$$는  learnable parameters 의 set of vector이다. 

*  learable parameter : training 중 network에 의하여 학습된 parameter

$$\epsilon$$ : vector of zero-mean noise with fixed statistics 

$$\odot$$ : element-wise multiplication 


p를 input으로 q를 output으로 하는 Linear layer of neural network가 다음과 같다고 해보자, 

![5_1_oisynetworks 011noisynetworks 011](https://user-images.githubusercontent.com/11300712/39461996-47574748-4cc3-11e8-997d-acaeea55f99a.jpeg)

$$y=wx+b$$

$$x\in R^p$$ : layer input

$$w\in R^{r*p}$$ : weight matrix 

$$b \in R^q $$ : bias 

위와같다고 할때 , corresponding noisy linear layer은 다음과 같이 정의할 수 있다. 

$$y \overset{def}{=}(\mu^{w} + \sigma^{w} \odot \epsilon^{w}) x +\mu^{b} + \sigma^{b}\odot \epsilon^(b)$$


여기서 w와 b가 $$\mu^{w} + \sigma^{w} \odot \epsilon^{w}$$ 와   $$\mu^{b} + \sigma^{b}\odot \epsilon^(b)$$로 바뀌었다. 

여기서 $$\mu^{w}$$, $$\mu^{b}$$, $$\sigma^{w}$$, $$\sigma^{b}$$는 Learnable이며, 


$$\epsilon^{w}$$, $$\epsilon^{b}$$는 noise random variables이다. 



### 4.Deep Reinforcement learning with NoisyNets




![5_1_noisynetworks 013](https://user-images.githubusercontent.com/11300712/39461998-47d8cb2e-4cc3-11e8-91ca-7f299cb965ab.jpeg)

Noise는 RL에서 exploration 효과를 보인다. 



RL에서 Noisy network agent는 매 step마다 새로운 parameter을 sample 한다.

Agent가 action을 선택할때 이 noise가 적용된 parameter에 의하여 action을 선택한다. 


Deep Q-Networks( DQN )과 Dueling에서 NoisyNets가 적용될때, 더이상 $$\epsilon -greedy$$를 사용하지 않는다. 

DQN과 Dueling에서의 Fully connected layer의 valuenetwork는 매 step마다 Noisy network distribution에서 뽑은 Noisy network이다. 

![5_1_noisynetworks 012](https://user-images.githubusercontent.com/11300712/39461997-478ee9dc-4cc3-11e8-86f8-092af767b48e.jpeg)

이 noisy network가 적용된 DQN, Dueling은 각 step마다 action을 선택하기 전에 resampling한다. 

이러한 Noisy network가 적용된 DQN과 Dueling를 NoisyNet-DQN,  NoisyNet_dueling라고 한다. 



먼저 NoisyNet-DQN의 Loss function은 는 다음과 같이 정의할수 있다. 




![5_1_noisynetworks 015](https://user-images.githubusercontent.com/11300712/39462000-48911922-4cc3-11e8-9270-6367948d4b70.jpeg)




$$Q(x,a,\epsilon ; \zeta)$$ : noise variables의 distribution $$\epsilon$$이 적용된 noisy value function. 

$$Q(x,a,\epsilon^{'},; \zeta^{-})$$:  noise variables의 distribution $$\epsilon^{'}$$이 적용된 noisy target value function


![5_1_noisynetworks 016](https://user-images.githubusercontent.com/11300712/39462001-48e3b70e-4cc3-11e8-9089-b8e5a35f9dad.jpeg)

비슷하게 NoisyNet-Dueling는 다음과 같이 정의한다. 


![screen shot 2018-04-30 at 20 33 00](https://user-images.githubusercontent.com/11300712/39460135-b7722c68-4cb5-11e8-80d9-0dbac4a004f3.png)

### 5. 결과

Noisy network agents를 57개의 아타리게임에서 적용해보았고, baseline을 통하여  비교해보았다. 

![screen shot 2018-04-30 at 20 35 03](https://user-images.githubusercontent.com/11300712/39460744-d1ee6ad4-4cba-11e8-99b8-4ed807865ff4.png)

DQN, Dueling, A3C 에서 noisyNet의 적용 유무로 Score 비교를 하였다. 

![screen shot 2018-04-30 at 20 35 29](https://user-images.githubusercontent.com/11300712/39460746-d3a1473e-4cba-11e8-867f-aa6dd8113033.png)


NoisyNet을 적용한 결과, 기존의 DQN, Dueling, A3C에 비교해 성능이 좋다. 

![screen shot 2018-04-30 at 20 35 17](https://user-images.githubusercontent.com/11300712/39460747-d3cef724-4cba-11e8-8f5d-c865271c8f28.png)




* * *

### References 


* Noisy Networks for Exploration

https://arxiv.org/abs/1706.10295









