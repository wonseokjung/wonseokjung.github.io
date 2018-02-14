---
title: "Monte Carlo Estimation and Monte Carlo control"
date: 2018-02-14 14:16:28 -0400
categories: Reinforcementlearning update
use_math:true
---




# Chapter3.2Monte Carlo Estimation of Action Value



*만약 모델이 있으면 state value하나로도 충분하지만 만약 model이 없다면, state value를 측정하는 것보다 action value를 측정하는 것이 더 유용합니다.* 


state $$s$$ 에서 다음의 state $$s$$ 로 갔을때 value function이 가장 큰 action $$a$$을 선택합니다. 

그렇기때문에, Monte Carlo의 목적은 $$q_*$$ 를 estimate 하자! 입니다. 

이 action value policy evaluation 문제는 $$q_*(s,a)$$ 를 측정하기 위한 evaluation 입니다. (state $$s$$에서 action $$a$$를 선택하였을때의 return값을 측정) 

*visit to state s* 한 state에서 action $$a$$를 선택한것을 state-action value가 episode에서 visited 했다고 합니다. 


state value와 같이 expected state-action value또한 각 state를 visited 한 횟수가 $$\infty$$ 에 approach 할때 converge합니다 .

*하지만 이 방법의 문제점은 모든 state-action pair가 visit 되지 않을수도 있다는 것입니다.* 

각 state에서 여러가지 action을 선택하여야 하는데 그렇지 못하기에 *maintaining exploration* 문제가 발생합니다. 

Monte Carlo에서는 DP와 다르게 MDP에 대한 정보가 없으므로 state value보다 state action pair을 사용하여야 한다고 했습니다. 

*하지만 action을 선택하는게 deterministic하다면 각 state에서 action은 한가지밖에 선택할수없고, 모든 state를 전부 visit하기가 어려울 수도 있다는 추정입니다.*

그래서 이를 방지하기 위해 exploration 을 하는데  모든  state action pair이 start state로 될 확률을 nonzero로 줘서 시작 state action pair을 exploration 합니다. 











# Chapter 3.3 Monte Carlo Control

*Optimal policies를 approximate하기 위해서 Monte Carlo simulation 을 어떻게 consider할까요 ? *

Monte Carlo의 classical policy iteration에서는 arbitrary policy $$\pi_0$$으로 시작하여 optimal policy와 optimal action value function으로 마칩니다. 

$$\pi_0\overset{E}{\rightarrow}q_{\pi_0}\overset{I}{\rightarrow}\pi_1\overset{E}{\rightarrow}q_{\pi_1}\overset{I}{\rightarrow}... \pi*\overset{E}{\rightarrow}q_{\pi_*}$$



$$E$$는 *policy evaluation* 이고  I는 *policy improvement* 입니다. 많은 Episodes를 경험하면서 approximate된 action-value function은 점점 *true function*으로 접근합니다. 그리고 episodes는 exploring state합니다. 그래서 Monte carlo $$\pi_k$$ 를 따라서$$q_{\pi_k}$$ $$q_{\pi_k}$$를 계산합니다 .




Policy improvement는 현재의 value function 을 policy greedy한것입니다.
아래의 식과 같이 Action value function을 최대화 로 만들어주는 action $$\pi(s)$$ 로 사용합니다. 

$$\pi(s) \doteq \underset{a}{argmax}q(s,a)$$

이렇게 Monte carlo는 환경의 dynamic 정보없이 sample episodes로 optimal policies를 찾습니다. 
지금까지 Monte Carlo로 Converge를 좀 더 쉽게하는 방법을 두가지 알아보았습니다 .

Exploring start하는 방법
Policy evaluation을 $$\infty$$하게 하는법



지금은 2번인 policy evaluation의 에가피소드가 $$\infty$$으로 다가갈때의 경우를 먼저 살펴보겠습니다.  



DP와 Monte Carlo cases에서 문제를 풀기 위한 방법이 두가지가 있습니다. 

첫번째는, 각 policy evaluation 에서 $$q_{pi_k} approximate하는 것을 고정하는것 입니다.

Estimate할때 error의 magnitude와 probability를 측정하고 추정하는데요,
이때 policy evaluation 에서 충분할 step을 taken하게 되면 bound가 충분히 줄어듭니다. 

이 방법 어느정도의 approximation까지 convergence가 올바르게 될 것이라고 게런티 합니다.

하지만 이 방법의 문제는 이렇게 하기위해 Episode가 너무 많이 필요로 하는 것입니다.

그래서 두번째 방법은 policy evaluation할때 episode가 $$\infty$$하지 않아도 되는 방법입니다.

이 방법은 police evaluation을 완전히 하지 않고 바로 policy improvement를 합니다. 

아이디어는 GPI에서 value iteration과 같습니다. 

*각 Policy improvement step마다, 한번의 policy evaluation이 수행 됩니다. 
(그렇기에 policy evaluation이 converge될 필요가 없습니다.)*

Monte Carlo Policy Evaluation의 evaluation을 에피소드가 끝날때마다 improvement 합니다.

각 Episode마다 observed된 return을 policy evaluation으로 사용됩니다. 

그리고 policy가 episode에서 visited된 state로 update합니다. 


아래는 Monte Carlo Exploring start 수도코드 입니다. 

![screenshot from 2018-02-14 15-17-49](https://user-images.githubusercontent.com/11300712/36190240-775d3394-119a-11e8-9cec-bd7cf7407d98.png)


Monte Carlo ES ( Exploring Starts )


Initializer : 
	임의의 값으로 Q(s,a)를 초기화합니다.
	마찬가지로 임의의 값으로 ㅠ(s)를 설정하고요. 
	비어있는 list인 Returns(s,a)를 생성합니다, 

계속 반복합니다: 
모든 state S0과 A0이 선택될 확률은 non zero의 값으로 선택합니다. 
Episode에서 S0,A0에서 시작되어 ㅠ(s)를 따른 값을 생성합니다. 
각 Episode에서 s,a에 대해 :
	

처음 발생한 s,a에 따라 return된 값을 G에 넣습니다. 
G를 returns(s,a)에 append합니다. 
Append된 returns(s,a)를  average한 뒤  Q(s,a)의 값으로 업데이트 합니다. 
Episode의 각 state마다 Q(s,a)를 최대화 시키는 action을 ㅠ(s)로 업데이트 합니다. 


