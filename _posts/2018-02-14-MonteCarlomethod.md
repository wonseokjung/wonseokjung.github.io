---
title: "3.Monte Carlo method and prediction"
date: 2018-02-14 10:26:28 -0400
categories: Reinforcementlearning update
use_math : true
---

# Chapter 3. Monte Carlo Methods


이번 챕터에서는 value function을 측정하고, optimal policies를 찾아내는 방법을 배우겠습니다.
 
*Monte Carlo method는 Dynamic programing처럼 모든 정보를 알고 시작 하는 것이 아닌 actual 혹은 simulated 하게 경험을 하며 환경과 상호작용을 합니다.*

그렇기에 Monte Carlo method는 상호작용을 하며 받은 $$state,action, reward$$ 의 값들이 필요합니다.

실제로 경험을 하며 배우는 방법이 좋은 점은 environment의 prior knowledge가 없어도 실제로 경험을 하며 optimal behavior을 이루기 때문입니다. 

*Monte Carlo는 이렇게 경험을 하며 모은 sample return들을 평균하여 강화학습을 풀어갑니다.* 

*우리는 여기서 Episodic task인 Monte carlo만 다루겠습니다.* 

Monte carlo는 ***step-by-step(online)이 아닌 episode-by-episode로서 한 time step마다 value function고 policy를 변화시키는 것이 아닌 에피소드의 마지막 스테이트 terminal state 까지***가야 policy와 value function이 바뀝니다.

Monte Carlo는 Bandit문제와 비슷하게 경험을 하며 return 된 sample을 이용하여 state-action value를 평균합니다. 



##### 그렇다면 Bandit문제와 다른점은??
![title](https://user-images.githubusercontent.com/11300712/36184534-aaa0fcfe-1177-11e8-8445-e60fbb254914.jpg)

*다른점은 같은 episode안에서도 어떤 state에서 action을 함에 따라 다음의 state가 무엇이 될지에 영향을 미치는 것입니다.*

이렇게 nonstationary했을때 발생하는 문제를 해결하기 위하여 GPI를 이용하여 경험을 통해 return된 sample을 이용하여 배웁니다. 

Monte Carlo method 은 Dynamic programming처럼 GPI를 이용하여 문제를 prediction하고 improvement 하며 solution을 찾습니다. 










* * *

## 3.1 Monte Carlo Prediction

**Monte Carlo 를 이용하여 Prediction을 하기위해, 주어진 policy로 value function을 학습하는것부터 해보겠습니다.**

State value란 어떠한 state에서 시작하여 terminal state까지의 . return된 discounted가 적용된 reward의 총 합 입니다.

Monte carlomethod에서는 경험을 통하여 value를 예측한다고 하였습니다.
경험을 통하여 value를 예측한다는 의미는, **state들을 visit하며 return 된 값들을 average**하는 것입니다. 

Return 값을 더 많이 observed 한다면, 그 average가 expected value로 converge될 것 입니다. 

위의 idea를 *Monte Carlo *라고 합니다. 

예를 들어 $$v_\pi(s)$$ estimate 하고 싶다고 가정하면, $$policy \pi$$ 를 따른 $$state s$$의 value function입니다.
이 value function은 $$\pi$$를 따라 state s를 지나며 얻은 value 입니다. 


한 Episode에서 발생한 $$state s$$를 $$visit to s$$ 라고 합니다. 

물론 같은 Episode에서  $$s$$는 여러번 $$visit$$ 할수 있습니다.   

Episode에서 처음 $$visit$$한 것을 *first visit to s *라고 부르겠습니다. 

First-visit MC method는* first visit to s* 의 하여 return 된 value를 average한 expected $$v_\pi(s)$$ 입니다. 

반면에 every-visit MC method는 $$all visit to s$$ 에서 return 한 값을 average한 $$v_\pi(s)$$ 입니다.


이 first-visit MC와 every-visit MC는 비슷해보이지만 이론적으로 다른점이 있습니다. 

Every visit MC는 뒤에서 알아보기로 하고 지금은 먼저 First visit에 대해 알아보겠습니다. 










First-visit MC prediction, for estimating 

Initializer : 
	Evaluate하기 위한 policy 정의
	임의의 state value function
	모든 state에 대한 비어있는 리스트

계속 반복합니다: 
	Policy를 이용하여 episode를 생성합니다.
	Episode에서 나타나는 각 state s에서 :
		처음 발생하는 s를 따른 return값
		이 return값을 비어있는 리스트에 넣습니다.
        리스트에 들어있는 return값을 평균내어 임의의 state-value function으로 정의해줬던 곳으로 업데이트 합니다. 
        





*First-visit MC, Every-visit MC 둘다 $$visit to s$$의 횟수 n이  infinity로 다가갈때 $$v_\pi(s)$$로 converge 됩니다.*

![screenshot from 2018-02-14 13-26-22](https://user-images.githubusercontent.com/11300712/36187746-af34c512-118a-11e8-915b-1e726a8cc21a.png)


Blackjack 의 예

Blackjack은 finite MDP입니다. Blackjack 한판은 한번의 Episode와 같습니다. 

Reward는 이기면 +1, 지면 -1, 비기면 0 의 리워드는 받습니다. Discount factor은 1이며 플레이어가 할 수 있는 action은 hit 아니면 stick 입니다. 

State는 player’s의 카드에 따라 딜러의 showing카드에 따라 달라집니다. 
카드가 infinite deck 이어서 그 전의 카드 track 을 알아도 이점이 없다고 가정하겠습니다. 

만약 플레이어가 11로 쓸수있는 ace를 가지고 있다면 (bust 하지 않고) 이 ace를 usable하다고 하겠습니다. 

플레이어는 다음과 같은 세가지에 기초하여 결정을 내립니다. 

1. 현재 sum(12-21) 
2. 딜러가 보여준 한장의 카드
3. Usable카드의 보유 유무 

이것들의 조합은 200state입니다. 

Policy는 플레이어의 sum이 20-21일때 stick하고 아니면 hit 합니다. 이 state value function을 monte carlo로 찾으려면, 각 state에서 policy를 따라 받은 return값을 평균하는 작업을 여러번 해야합니다. 
여기서는 한 episode에서 똑같은 state를 visit하지 않기 때문에 first visit과 every visit의 결과값이 똑같습니다. 

여기서는 아래의 그림과 같은 결과값을 보여줍니다. 

![screenshot from 2018-02-14 13-31-04](https://user-images.githubusercontent.com/11300712/36187864-7eb333b4-118b-11e8-9a70-20b579fbfe6a.png)
Usable이 less common하기 때문에 less certain, regular 한 경향을 보입니다. 

500,000번 이상 돌렸을때 value function이 approximate가 잘 된것 같이 보입니다.

우리가 모든 정보를 갖고 있다고 하더라도 Blackjack은 DP로 풀기가 굉장히 힘듭니다. BlackJack에서는 $$P(s’,r|s,a)$$ 를 알수가 없기 때문입니다. 

반면에 Moten Carlo처럼 sample을 generate할수 있다면 가능해 집니다.


*Monte Carlo를 DP와 같이 Backup Diagram의 아이디어를 쓸수 있을까요?* 

Black up diagram의 아이디는 root node아래로 update된 모든 transitions을 
나뭇가지처럼 reward와 estimate value를 보여준 것 입니다. 

Monte Carlo 에서 $$v_\pi(s)$$를 estimate할때,  root는 state node 입니다. 
그리고 그 아래는 한 Episode 안에서의 Terminal state까지의 전체 trajectories 입니다.


Dp에서는 모든 possible transitions을 다 보여주지만 Monte Carlo 에서는 오직 한 episode에서의 sample만을 보여줍니다.  

아래 그림과 같이 시작 state s에서부터 terminal되는 state의 agent가 실제로 action을 선택하며 visit to s한 sample 입니다. 
![screenshot from 2018-02-14 13-31-21](https://user-images.githubusercontent.com/11300712/36187866-802f0696-118b-11e8-926b-c09ff1436a5d.png)


*Monte Carlo의 중요한 사실은 각 state의 estimate는 independent라는 것 입니다.  DP와는 다르게 Monte carlo는 다른 state에 의해 value가 estimate되지 않습니다.
그러므로 Monte Carlo는 Bootstrap이 아닙니다. *
