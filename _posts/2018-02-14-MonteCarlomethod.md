---

title: "3.Monte Carlo method"
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

예를 들어 $$v_\pi(s)$$ estimate 하고 싶다고 가정하면, policy ㅠ를 따른 state s의 value function입니다. 
이 value function은 ㅠ를 따라 state s를 지나며 얻은 value 입니다. 



한 Episode에서 발생한 state s를 visit to s 라고 합니다. 

물론 같은 Episode에서  s는 여러번 visit 할수 있습니다.   

Episode에서 처음 visit한 것을 first visit to s 라고 부르겠습니다. 

First-visit MC method는 first visit to s 의 하여 return 된 value를 average한 expected vㅠ(s) 입니다. 

반면에 every-visit MC method는 all visit to s 에서 return 한 값을 average한 vㅠ(s) 입니다.


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



First-visit MC, Every-visit MC 둘다 visit to s의 횟수 n이  infinity로 다가갈때 vㅠ(s)로 됩니다.

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




Usable이 less common하기 떄문에 less certain, regular 한 경향을 보입니다. 

500,000번 이상 돌렸을때 value function이 approximate가 잘 된것 같이 보입니다.

우리가 모든 정보를 갖고 있다고 하더라도 Blackjack은 DP로 풀기가 굉장히 힘듭니다. BlackJack에서는 P(s’,r|s,a)를 알수가 없기 때문입니다. 

반면에 Moten Carlo처럼 sample generate할수 있다면 가능해 집니다.


Monte Carlo를 Backup Diagram  의 아이디어를 쓸수 있을까?? 

Black up diagram의 아이디는 root node아래로 update된 모든 transitions을 
나뭇가지처럼 reward와 estimate value를 보여준 것 입니다. 

Monte Carlo 에서 Vㅠ를 estimate할때,  root는 state node 입니다. 
그리고 그 아래는 한 Episode 안에서의 Terminal state까지의 전체 trajectories 입니다. 

Dp에서는 모든 possible transitions을 다 보여주지만 Monte Carlo 에서는 오직 한 episode에서의 sample만을 보여줍니다.  아래 그림과 같이 시작 스테이트에서부터 terminal되는 스테이트까지의 agent가 실제로 action을 선택하며 visit to s한 sample. 




Monte Carlo의 중요한 사실은 각 state의 estimate는 independent라는 것 입니다.  DP와는 다르게 Monte carlo는 다른 state에 의해 value가 estimate되지 않습니다. 그러므로 Monte Carlo는 Bootstrap이 아닙니다. 















3.2 Monte Carlo Estimation 



모델이 있으면 state value하나로도 충분하지만 만약 model이 없다면, state value를 측정하는 것보다 action value를 측정하는 것이 더 유용합니다. 

State에서 다음의 state로 갔을때 value function이 가장 큰 action을 선택합니다. 

그렇기때문에, Monte Carlo의 목적은 q* 를 estimate 하자! 입니다. 

이 action value policy evaluation 문제는 q*(s,a)를 측정하기 위한 evaluation 입니다. (state s에서 action a를 선택하였을때의 return값을 측정) 

Visit to state s 한 state에서 action a를 선택한것을 state-action value가 episode에서 visited 했다고 합니다. 

Expected state-action value또한 각 state를 visited 한 횟수가 infinity에 approach 할때 converge합니다 .

하지만 이 방법의 문제점은 모든 state-action pair가 visit 되지 않을수도 있다는 것입니다. 

각 state에서 여러가지 action을 선택하여야 하는데 그렇지 못하기에 maintaining exploration 문제가 발생합니다. 

Monte Carlo에서는 DP와 다르게 MDP에 대한 정보가 없으므로 state value보다 state action pair을 사용하여야 한다고 했습니다. 

하지만 action을 선택하는게 deterministic하다면 각 state에서 action은 한가지밖에 선택할수없고,
모든 state를 전부 visit하기가 어려울 수도 있다는 추정입니다.

그래서 이를 방지하기 위해 exploration 을 하는데 action을 exploration 을 하는 것과 달리 모든 
state action pair이 start state로 될 확률을 nonzero로 줘서 시작 state action pair을 exploration 합니다. 







3.3 Monte Carlo Estimation 

Optimal policies를 approximate하기 위해서 Monte Carlo simulation 을 어떻게 consider할까요 ? 

Monte Carlo의 classical policy iteration에서는 arbitrary policy ㅠ0으로 시작하여 optimal policy와 optimal action value function으로 마칩니다. 



E는 complete policy evaluation 이고  I는 policy improvement 입니다. 많은 Episodes를 경험하면서 approximate된 action-value function은 점점 true function으로 접근합니다. 그리고 episodes는 exploring state합니다. 그래서 Monte carlo ㅠk를 따라서 qㅠk를 계산합니다 .

  

Policy improvement는 현재의 value function 을 policy greedy한것입니다.

이 경우에는 action-value function이 있습니다. 그러므로 greedy policy를 선택하기 위해서 model은 필요하지 않습니다. 


Action value function을 최대화 로 만들어주는 action  ㅠ(s)로 사용합니다. 



ㅠk+1 은 ㅠk보다 uniformly better 하거나 ㅠk와 같습니다. 


이렇게 Monte carlo는 환경의 dynamic 정보없이 sample episodes로 optimal policies를 찾습니다. 지금까지 Monte Carlo로 Converge를 좀 더 쉽게하는 방법을 두가지 알아보았습니다 .


Exploring start하는 방법
Policy evaluation을 infinite하게 하는법



지금은 2번인 policy evaluation의 에피소드가 무한으로 다가갈때의 경우를 먼저 살펴보겠습니다.  

사실 DP방법에서도 iterative policy evaluation을 할때도 몬테카를로와 같이 문제가 발생합니다. 

DP와 Monte Carlo cases에서 문제를 풀기 위한 방법이 두가지가 있습니다. 

첫번째는, 각 policy evaluation 에서 qㅠk approximate하는 것을 고정하는것 입니다.

Estimate할때 error의 magnitude와 probability를 측정하고 추정하는데요,
 이때 policy evaluation 에서 충분할 step을 taken하게 되면 bound가 충분히 줄어듭니다. 

이 방법 어느정도의 approximation까지 convergence가 올바르게 될 것이라고 게런티 합니다.

하지만 이 방법의 문제는 이렇게 하기위해 Episode가 너무 많이 필요로 하는 것입니다.

그래서 두번째 방법은 policy evaluation할때 episode가 infinite하지 않아도 되는 방법입니다. 
이 방법은 police evaluation을 완전히 하지 않고 policy improvement를 합니다. 

아이디어는 GPI에서 value iteration과 같습니다. 
각 Policy improvement step마다, 한번의 policy evaluation이 perform 됩니다. (그렇기에 policy evaluation이 converge될 필요가 없습니다.)

Monte Carlo Policy Evaluation의 evaluation을 improvement an episodes by episode를 기반으로 합니다. 

각 Episode마다 observed된 return을 policy evaluation으로 사용됩니다. 그리고 policy가 episode에서 visited된 state로 update합니다. 


아래는 Monte Carlo Exploring start 수도코드 입니다. 



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










3.4 Monte Carlo Control without Exploring Starts 


예상하지 못하는 Exploring start를 어떻게 방지할까요 ? 

가장 general한 방법으로는 agent가 모든 action을 전부 선택하는것 입니다. 그렇게 하기 위해서 두가지 방법인 On-policy와 Off- policy방법이 있습니다. 

Onpolicy는 의사결정을 하기위해 Policy를 Evaluate하거나 improve하는것을 말합니다. 

반면에 off policy는 policy different를 evalue하거나 improve합니다. 

먼저 on - policy 부터 보도록 하겠습니다. 

On-policy에서 policy는 soft합니다. Soft한다는 것의 의미는 모든 state s, action a 에서 ㅠ(a|s) > 0 이고 점점 deterministic한 optimal policy로 다가갑니다. 

여기서 On-policy 방법으로 epsilon-greedy를 사용합니다. 

Epsilon greedy는 대부분 action value가 가장 큰 것을 사용하지만 epsilon의 확률로 가장 큰 action이 아닌 다른 action 을 선택합니다.

On-policy Monte Carlo Control의 전체적인 Idea또한 GPI 입니다. Monte Carlo CS에서는 first-visit을 이용하여 현재 policy에대한 action-value를 측정합니다. 

여기서의 On-policy의 방법으로 epsilon-greedy method를 사용한 것입니다. 

epsilon-greedy 에 대해 생성된 ㅠ를 따른 qㅠ는 ㅠ보다 같거나 더 좋을것을 Gurantee합니다. 


On policy first visit MC control ( e-soft policies ) 알고리즘

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
	
	

모든 action은 epsilon greedy로 선택됩니다. 




















3.5 Off-policy prediction via Importance Sampling

모든 Learning control methods 는 딜레마가 있습니다. 

다음의 optimal behavior을 조건으로 action value를 배우는데 모든 action으로 explore하기 위해서는 optimal하지 않은 action도 또한 필요합니다. 

어떻게 Exploratory policy를 하는 도중에 Optimal policy를 배울수 있을까??

On policy와는 달리 Policy 두개를 만들어 보겠습니다!

Policy하나는 Optimal policy가 되는 Policy를 배우고, 

다른 Policy는 Exploratory를 하며 Behavior을 생성합니다. 

배우는 Policy를 Target Policy, 생성하는 policy를 Behavior Policy라고 합니다. 

이러한 전체 Process를 off-policy learning 이라고 합니다. 


On-policy와 Off-policy를 비교하면, On-policy가 좀더 Simple합니다. 

Off-policy는 variance가 많고, Converge가 느리게 됩니다. 하지만 보통 off-policy 방법이 더 강력합니다. 

Off-policy를 좀 더 많은 분야에서 사용할수 있으며, Behavior policy와 target policy가 같을때 
Off-policys는 on-policy를 포함한다고 할수 있습니다. 

여기서는 prediction problem을 푸는 목적으로, target policy와 behavior policy가 고정된 off-policy를 공부하도록 하겠습니다. 	

이 말은 예를들어 우리가 vㅠ와 qㅠ를 estimate하려고 하지만 우리는 b라는 policy를 따를것이고 여기서 b는  ㅠ와 다른 policy입니다. b는 behavior policy이고 ㅠ는 target policy입니다. 그리고 이 policies는 주어졌으며 변하지 않습니다. 

 b를 따른 episode를 사용하여 ㅠ의 value를 estimate하기 위해서는,
 ㅠ를 따른 action도 항상 같이 estimate하기 위해서는

 b를 따를때 ㅠ를 따른 action도 같이 선택이 되어야 합니다. 

(최소한 아주 가끔씩이더라도) 

그래서 ㅠ(a|s) >0 은 b(a|s)>0을 의미합니다. 

이것을 assumption of convergence라고 합니다.  Converage b은 ㅠ와 동일하지 않은  states에서 는 stochastic이어야 하고 Target policy ㅠ는 그 반대로 deterministic할 것입니다. 

이러한 case가 특정 control problems에서 발생합니다. 

Control에서, target policy는 보통 현재 action-value function을 estimate한것의 deterministic greedy policy입니다. 

이 policy는 behavior policy가 계속 stochastic하며 exploratory하는 도중에  deterministic optimal이 됩니다. 예를 들면 epsilon-greedy policy입니다. 

하지만 여기 prediction problem에는 ㅠ가 변하지 않고 주어졌다고 전제하겠습니다. 

거의 모든 off-policy는 importance sampling을 활용합니다. 

이 importance sampling은 주어진 다른 sample 의 distribution 아래의 에서 expected value를 estimating 하는 general technique 입니다. 

Target policy와 behavior policy에서 발생한 trajectory의 확률을 비교해 weight를 하여 배운 off-policy에 importance sampling을 apply 합니다. 

이 방법을 importance-sampling ratio 라고 합니다. 


Given starting state St에서 under 어떠한 policy ㅠ를 따라 probability of the subsequent state-action trajectory (At,St+1,At+1,....ST) 은 다음과같습니다. 













여기서 p는 state transition probability을 의미합니다. 그러므로 target policy와 behavior policy를 따른  relative probability 의 trajectories는 다음과 같습니다. 

 


Trajectories probability가 MDP에 따라 달라도 ( 보통 Unknown 입니다. ) numerator과 denominator에 둘다 나오기 때문에 cancel됩니다. 

그래서 importance sampling ratio는 MDP에 의하여 변하지 않습니다.

여기서 tme step은 episode가 증가함에 따른다고 하겠습니다. 만약에 첫 Episode가 100에 끝나면 다음 Episode는 101부터 시작합니다. 

여기서 state s 가 visited한 모든 time step의 집합을 T(s)라고 denote합니다. 

이것이 every-visit method입니다. 

First-visit methods 는 한 episode에서 first visit to s의 time step 만을 포함합니다.

T(t)는 time을 따라 처음 termination 된 것을 denoted 합니다.  

Gt는 t부터 T(t)까지의 return을 denote합니다. 

 state s 에 관련된 return 입니다. . 

그리고 는 corresponding importance ratios입니다. 

Vㅠ(s)를 estimate하기 위해서 ratio와 average로  return을 scale 합니다.
Importance sampling이 위와 같이 간단한 average로 된다면, 
ordinary importance  sampling. 

Weighted 된 average로 importance 하는 방법도 있는데, 이것을 weighted importance sampling 이라고 합니다. 

식은 다음과 같습니다. 



 v
이 두 importance sampling을 이해하려면 , single return을 observe해야 합니다. 

Weighted-average estimate에서 가 single return에 cancel됩니다.

그래서 그 estimate는 return independent of ratio와 같아지게 됩니다.























3.6 Incremental Implementation

몬테카를로 prediction에서는implemented Incrementally이 사용될수 있습니다. 

Off policy monte carlo method에서 ordinary importance sampling를 쓰는것과

 weighted importance sampling를 쓰는 것으로 구분해야 합니다.

Ordinary importance sampling은 return이 importance sampling p:T(t)-1에 의해 scaled 되고, average 됩니다.

Weighted importance sample에서는,  sequence of return 이 G1,G2,G3… Gn-1 이라고 하고, 항상 같은  state에서 출발합니다. 

Random weight Wi(eg. Wi=pr:T(t)-1)에 corresponding한다고 가정해보겠습니다. 



그리고 계속 additional return Gn으로 up to date를 유지합니다. 

추가적으로 Vn을 유지하기 위해서 Cumulative Sum의 Weight를 유지해줘야 합니다. (First returns의)

Vn의 update rule은 다음과 같습니다. 

 


그리고


C0=0 입니다. 아래는 MonteCarlo Policy Evaluation의 episode by episode incremental algorithm 입니다. 

이 알고리즘은 off-policy case이며 weighted importance sampling을 사용하였습니다. 





First-visit MC prediction, for estimating 

Input : 임의의 Target policy ㅠ 

Initializer 모든 state와 모든 action에 대해서:
 	임의의 Q(s,a)
	C(s,a) <- 0
	계속 반복합니다: 
		b<- Converage ㅠ를 이용한 any policy
		b를 이용하여 generate 합니다.
S0,A0,R0 … St - 1 ,at-1, RT,ST
G<- 0으로 
W <- 1로 
t=T-1,T-2,.... 0까지 : 
	G<- discount factor * G + Rt+1
	C(St,At) <- C(St,At) + W 
	Q(St,At) <- Q(St,At) + W/C(St,At) * [G-Q(St,At)]
	W<- W* (At|St) / b(At|bt)
	만약 W=0이면 loop를 벗어납니다. 













































3.7 Off-policy prediction via Importance Sampling

On-policy method는 policy value를 estimate하는 것이고, off-policy는 두 function이 나누어져 있는 것입니다. 

Behavior policy는 Behavior을 생성하는 policy이고, 
Policy와 상관없는 evalue하고 improvement하는 것을 Target policy라고 합니다. 

이렇게 policy를 나눈것의 장점은 target policy를 deterministic(greedy)하게 사용하는 도중에 behavior policy를 가능한 action의 sample을 계속 할 수있기 때문입니다. 

Off-policy MonteCarlo control method는 다음 두 section의 technic 중 하나를 사용합니다. 

Behavior policy를 따르면서 Target policy의 improving을 배웁니다. 이 technique는 behavior policy가 nonzero probability를 가지고 있다는 것입니다. 이 nonzero probability는 target policy에 의해 선택되어진 모든 action을 선택할 확률이 nonzero이다 라는 뜻입니다. 

모든 action을 explore 하려면 policy가 soft해야 합니다. (모든 state에서 모든 action을 선택하는 확률이 nonzero의 확률이다) 545
다음 Box에 off-policy Monte Carlo control method가 있습니다. 



GPI를 이용하며 weighted importance sampling을 ㅠ*와 q*에 적용했습니다. 

Target policy ㅠ~~ㅠ*는 greedy policy가 Q에 최대한 것입니다 . 이것으로 qㅠ를 측정합니다.

Behavior policy b는 어떤것이던 될수 있지만 ㅠ가 optimal policy가 되려면 infinite number의 return이 각 state s 와 action a 에서 얻어져야 합니다. 

B는 epsilon-soft 에서부터 선택되어야 합니다. Policyㅠ는 optimal로 converge 합니다. 이것은 모든 encountered state에서 해야하며 action이 다른 soft policy b에서부터 선택되어도 마찬가지여야 합니다. 가장 potential한 문제는 tail of episode만 배운다는것 입니다. Episode에 남은 action들이 greedy 할때 말이죠 

만약 nongreedy한 action이 common하면, 학습이 느려집니다. 이런 학습이 느려지는 것을 개선하기 위한 방법으로 다음에 배울 Temporal-difference learning을 사용합니다. 

































5.10 Summary 

몬테카를로 방법은 value function과 optimal function을 sample episodes에서부터 경험으로 배우는 것입니다. 

이 방법은 DP 방법보다 좋은점이 몇가지 있는데요. 

첫번째는 optimal behavior을 환경과의 상호작용에서 바로 배울수 있습니다. 환경의 dynamics 정보 없이 말이죠. 

두번째, 시뮬레이션이나 sample model로 사용될수 있습니다.

세번째, 작은 state에 대해 사용하기가 편하고 효과적입니다. 

네번째는, bootstrap을 하지 않기 때문에 value estimate를 successor states에서 가져오지 않습니다. 그러므로 violation of the Markov property에 피해를 적게 입습니다. 

Monte Carlo control 방법은 GPI generalized policy iteration을 이용합니다. 

GPI는 policy evaluation과 policy improvement의 상호작용 입니다. Monte Carlo methods는 이것도 다른 policy evaluation process를 합니다. 
Model의 각 state에서의 value를 사용하지않고, state에서 시작하여 받은 많은 return들을 average합니다. 

왜냐하면 이 state의 value는 expected return이고 이 average는 value의 good approximation이 될수있기 때문입니다. 

Monte Carlo methods는 policy evaluation과 policy improvement을 각 step마다 intermix하며 episode-by-episode basis합니다. 또한 incrementally implemented 도 합니다. 

Monte Carlo control method에서 Sufficient exploration을 유지하는 것이 issue입니다. 현재 estimate한것에서 최고를 선택하는 것만으로는 충분하지 않습니다 왜냐하면 다른 좋은 action이 혹시 있더라도 그것을 배우지 못할것이기 때문입니다. 

One approach는 episode를 시작할때 state-action pair을 random하게 선택하는것 입니다. 

On-policy 에서는 agent는 항상 exploring 하면서 best policy를 배우려고 합니다. 

Off-policy도 explore하지만 policy followed와 다를수 있는 deterministic한 optimal policy를 배웁니다. 






[^]: 
