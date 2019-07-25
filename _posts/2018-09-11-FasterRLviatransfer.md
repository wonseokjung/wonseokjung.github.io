---
title: "Faster Reinforcement Learning via Transfer"
date: 2018-09-11 12:26:28 -0400
categories: Reinforcementlearning update
use_math : true
--- 



<img src="https://www.dropbox.com/s/76z0fvih6xxnrlh/Screenshot%202018-09-07%2006.46.28.png?raw=1">







 안녕하세요. 

정원석입니다. 

오늘 강의할 내용은 Faster ReinforcementLearning via Transfer 

즉, Agent 가 빨리 학습하는 방법에 대한것 입니다. 

<img src="https://www.dropbox.com/s/epi4tp4rp5u7mhc/Screenshot%202018-09-07%2018.31.18.png?raw=1">



이 강의 내용은 9월 6일 Sk T-brain에서 주관한 Human.Machine. Experience Together 커퍼런스에서 John Shulman의 발표를 감명깊게 들어 만들게 되었습니다.



 일정부분은 제가 추가하였지만, 대부분 John Shulman이 발표한 내용을 바탕으로 만들어졌습니다. 



<img src="https://www.dropbox.com/s/7rz1ybcb2q4mywb/Screenshot%202018-09-07%2018.32.48.png?raw=1">

<img src="https://www.dropbox.com/s/98k82gvyl2pxtku/Screenshot%202018-09-07%2018.37.24.png?raw=1">



Overview를 먼저 보면 , 

1. 첫번째로 Policy Gradients방법을 소개하고요. 

1.1 이 Policy gradients가 성공한 케이스와 Policy gradients가 가진 한계에 대해서도 이야기를 하겠습니다

2. 다음으로 "Learning how to learn" 어떻게 배우는지를 배우는것의 의미를 가진 Meta Reinforcement Learning에 대해서, 

3. 그리고 최근에 생긴 Gym retro에 대해 이야기를 하도록 하겠습니다. 



<img src="https://www.dropbox.com/s/ae85rr7plrwvjci/Screenshot%202018-09-07%2018.57.49.png?raw=1">



본격적으로 설명하기 전에 먼저 강화학습의 Terminology에 대해 알아보도록 하겠습니다. 



<img src="https://www.dropbox.com/s/y1fxn9kwnrtdtv4/Screenshot%202018-09-07%2018.58.05.png?raw=1">

강화학습이란 시도와 실패를 반복하며 받는 리워드를 최대로 하는 것을 목표로 합니다. 



<img src="https://www.dropbox.com/s/i1tx6k3jy0p9yr0/Screenshot%202018-09-07%2018.58.17.png?raw=1">

그 강화학습에 뉴럴네트워크를 적용하여 강화학습 알고리즘을 represent한 것을 Deep Reinforcement Learning이라고 하고요. 



<img src="https://www.dropbox.com/s/kt2dvx57b51jmzs/Screenshot%202018-09-07%2020.07.25.png?raw=1">



<img src="https://www.dropbox.com/s/yy4pqvzcg343i8j/Screenshot%202018-09-07%2020.11.47.png?raw=1">



Meta-Learning이란 "  Learning how to Learn " 즉 어떠한 Learning을 하기위한 Task를 마스터하는 학습법을 Meta-Learning이라고 합니다. 



<img src="https://www.dropbox.com/s/z8nn4p3n2pku39c/Screenshot%202018-09-07%2020.16.02.png?raw=1">

강화학습은 Markov Decision Process를 통하여 학습을 합니다. 





<img src="https://www.dropbox.com/s/kquhvtdtcabxvy2/Screenshot%202018-09-08%2002.02.13.png?raw=1">



강화학습에서는 로봇, 프로그램과 같은 세상과 상호작용하는 Agent가 있습니다. 

<img src="https://www.dropbox.com/s/vig613wkbmgsmy6/Screenshot%202018-09-08%2002.02.32.png?raw=1">



이 Agent는 각 Time step마다 Action을 선택합니다. 





<img src="https://www.dropbox.com/s/rud5b4298nizt6a/Screenshot%202018-09-08%2002.04.31.png?raw=1">

Environment는 action을 받고 reward 와 Observation을 생성합니다. 

여기서 Observation은 $S_{t+1}$ 로 표현하였습니다. 



Observation은 Image, 소리 등 무엇이든 될 수 있습니다. 

Reward는 signal인데, 이 signal을 Agent가 최대화 하려고 시도를 합니다. 



<img src="https://www.dropbox.com/s/fme3sg9dhreyf32/Screenshot%202018-09-08%2002.22.42.png?raw=1">





Trajectory란 Obervation, action, reward의 sequence입니다. 

강화학습에서는 이 Trajectory를 보면서 최적화하는 작업을 합니다. 



<img src="https://www.dropbox.com/s/4e38mhe9mxps7ra/Screenshot%202018-09-08%2002.23.53.png?raw=1">

Return은 Agent에 의해 받은 Reward의 총합 입니다. 





# 1.Police Gradients 



<img src="https://www.dropbox.com/s/kx6lkwr0xcxhr7f/Screenshot%202018-09-08%2002.26.28.png?raw=1">



그렇다면 이제 Policy Gradients에 대해 알아보도록 하겠습니다. 





<img src="https://www.dropbox.com/s/wu9vng44pdbrrm9/Screenshot%202018-09-08%2002.28.46.png?raw=1">



먼저 Policy란 Observation에 의해 Action을 선택하는 함수 입니다. 



그리고 Policy Gradients method방법이란 

더 좋은 Policy를 찾기 위해서 Policy 자체를 최적화하는 강화학습 알고리즘 입니다. 



<img src="https://www.dropbox.com/s/vppdrshsya1ozci/Screenshot%202018-09-08%2006.48.03.png?raw=1">



Pseudocode를 보면 

첫번째 Policy 를 초기화 합니다. 이렇게 초기화하는 것은 임의로 하기로 하며, 전에 있었던 지식 ( Knowledge )를 이용하기도 합니다. 

이렇게 Initialize된 것은 보통 좋은 Policy가 아닌 경우가 많습니다. 

각 iteration마다 trajectory를 모으고 어떤 action이 좋고 나쁜지 측정합니다. 

Gradient update를 통해 좋은 action을 선택할 확률을 더 높여줍니다. 



* 이 방법이 잘되는 이유는Gradient Ascent를 통해 Reward를 더 많이 받는 쪽으로 Policy가 update되기 때문입니다.





<img src="hhttps://www.dropbox.com/s/blz5onw7ypen8xz/Screenshot%202018-09-08%2007.00.44.png?raw=1">

이 Policy gradients 방법은 사실 꽤 오래된 방법입니다. 

<img src="https://www.dropbox.com/s/pdy27dy4i9bbaox/Screenshot%202018-09-08%2007.03.09.png?raw=1">

1992년부터 시작하였고

 (John은 1992년 REINFORCE부터 시작하였다고 강의에서 말하였지만, 사실 Polciy gradients에서 사용된 방법중 하나인 Actor-critic은 1983년에 나온 "Neuronalike Adaptive Elements That can Solve Difficult Learning Control Problems"의 방법도 쓰였다고 Policy gradients논문에 소개되어 있습니다. 이것에 대해 더 자세히 알고 싶은 분은 논문은 간단하게 리뷰한 https://wonseokjung.github.io//reinforcementlearning/update/RL-PG_RE_AC/ 포스팅을 참고해주세요.)

<img src="https://www.dropbox.com/s/o9c63qsc8qcql04/Screenshot%202018-09-08%2007.22.53.png?raw=1">

이러한 Policy gradients 방법이 



최근에는 Deep learning이 적용되어 Proximal Policy Optimization (PPO)로 발전하게 되었습니다. 





<img src="https://www.dropbox.com/s/5alcz4mmwwxcpbp/Screenshot%202018-09-08%2007.28.12.png?raw=1">



Policy gradient를 사용하여 Alphago와 dota2 에서 훌륭한 결과를 얻었습니다. 

바둑에서는 프로바둑기사 이세돌을 이겼으며, Dota2 1 대 1 에서 도타 챔피언을 이겼고 2018년도에 5대 5 도타 게임에서는 탑 

레벨에 도달하였습니다.  

<img src="https://www.dropbox.com/s/9o9ggtwgi0k4cg1/Screenshot%202018-09-08%2019.42.24.png?raw=1">



지금까지 이야기했던 게임 분야 말고 Robot에 적용할 수도 있는데요. 

이 사진에서 보이는 로봇팔을 학습시키기 위해 Dota와 마찬가지로 LSTM에  PPO를 사용하였지만, Dota와 같은 게임을 학습시

키는 것보다 더 까다롭다고 합니다. 

 이렇게  강화학습을 사용하여 AlphaGo와 DOTA2를 학습시켰는데 여기서 발생하는 문제점은 무엇일까요?



<img src="https://www.dropbox.com/s/ba7xdwz0twqsjvf/Screenshot%202018-09-08%2019.56.30.png?raw=1">



첫번째로 학습시간이 너무 오래걸리는 것 입니다. 이 테이블은 AlphaGo가 사람만큼 바둑을 두기 위해 걸리는 시간을 나타낸 표 입니다. 

바둑을 학습시키기 위해서는 21Million Game이 필요한데요. (21 million = 21,000,000).  

알파고제로는 바둑을 배우기 위해서 21000000번

이 수치는 사람이 바둑을 배울때보다 훨씬 더 오래 걸리는 수치입니다.  



<img src="https://www.dropbox.com/s/8ur359y8xcspihp/Screenshot%202018-09-09%2005.48.19.png?raw=1">

Dota를 학습시키기 위해서는 현실에서 1000년정도 되는 량의 게임을 하여야 했습니다. 



1 대 1 봇을 만들기 위해 하루에 현실에서 300년 정도 되는 게임의 경험을 쌓게 했고 일주일 정도 학습시켜야 했습니다. 결국 Dota 1대1 봇은 2000년 정도의 게임 플레이를 경험하였고 5대5는 이것보다 더 오랜 경험이 필요합니다. 



사람 이런 게임을 1~2년 정도면 충분히 배울수 있는것과 달리 강화학습은 사람보다 시간이 훨씬 오래 걸린다는것이 단점입니다. 





<img src="https://www.dropbox.com/s/0eoj790ftyhzf20/Screenshot%202018-09-09%2017.54.33.png?raw=1">



인간이 이렇게 빨리 배울수 있는 이유는 어떤 Task를 새로 배울때 이전에 이미 배웠던 지식을 사용하여 배우기 때문입니다. 



그렇기 때문에 현재의 A.I system이 학습하는 것과 사람이 학습하는 방법을 비교하는 것은 공평하지 않습니다. 





<img src="https://www.dropbox.com/s/4mafm57q2oeu3zk/Screenshot%202018-09-09%2017.54.56.png?raw=1">



그렇다면, 



A.I 시스템이 Prior Knowledge ( 사전 지식 )을 사용할수 있게 하는 방법은 없을까요? 

이렇게 사전지식을 사용하여 새로운 Task를 배울수 있다면, 사람과 같이 새로운 것을 배우더라도 더 빨리 배울수 있지 않을까요? 

그래서 시도한것이 어떠한 Task를 학습하였다면 그와 연관된 task를 풀때 사전지식을 사용하여 훨씬 더 빠르게 학습할 수 있도록 하는 학습방법입니다. 



<img src="https://www.dropbox.com/s/dsy5pneuaq3gemf/Screenshot%202018-09-09%2018.19.07.png?raw=1">

이러한 학습 방법은 Meta ReinforcementLearning이라고 하며 이 Meta-Reinforcement Learning에 대해 알아보기 전에 기존 RL 패러다임에 대해서 알아보도록 하곘습니다. 







<img src="https://www.dropbox.com/s/df65tic1zh1zdh0/Screenshot%202018-09-09%2018.19.29.png?raw=1">



기존 강화학습의 패러다임은 Single task(단일 테스크) 입니다. 

싱글 테스크에서 에이전트는 일정기간 동안 환경에서 학습을 하며 쌓은 경험을 바탕으로 리워드를 최대화 시키려 한다고 하였습니다. 

이 학습을 할때 보통은 밑바닥에서부터 시작하며 학습을 합니다. 

대부분의 Deep Reinforcement learning은 이와가은 프레임워크로 학습을 하였습니다. 



그렇다면  Single task인 RL과 Meta RL의 차이점은 무엇일까요 ?







<img src="https://www.dropbox.com/s/net1oyn2h8yv3q8/Screenshot%202018-09-09%2019.32.03.png?raw=1">



미로찾기를 예로 든다면, 고전 강화학습에서는 하나의 미로가 있을 것이고 그 미로에는 시작점과 골이 있을 것입니다. 

에이전트에게 원하는 것은 "시작부터 목표점까지" 잘 가는것을 학습하는 것일 것입니다. 



<img src="https://www.dropbox.com/s/f03g9asfhg1znkm/Screenshot%202018-09-09%2019.42.44.png?raw=1">





Meta-RL 에서는 아주 많은 임의의 미로가 생성이 됩니다. 

각 에피소드마다 에이전트는 각 임의의 미로에서의 목표를 찾아야 합니다. 

에이전트는 어떤 미로에서 학습을 하게 될지 알지 모르기 때문에 임의의 미로를 경험해보며 골을 찾아야 합니다.

여기서의 목표는 여러개의 미로를 임의로 준 뒤 학습을 시켜서 새로운 미로를 경험할때 빠르게 골을 찾을수 있는 폴리시를 학습시키는 것이 목표입니다. 







사실 Meta-Reinforcement Learning이란 보통 고전 강화학습의 특별한 케이스일 뿐입니다. 



첫 타임 스텝에서 에이전트는 임의의 테스크 혹은 월드에 있게 되고, 에이전트에게 새로운 옵저베이션을 정의해줍니다. 

여기서 새로운 Obeservation이란 Original task의 observation에 reward, done, signal도 같이 주는 것 입니다. 



이 의미는  Original task가 끝날때때부터 trial을 시작하는 것입니다. 



새로운 Task의 각 Episode는 Old-task가 끝났을때부터의 K-epsisode입니다. 



여기서 결국 얘기하고 싶은 거슨 만약 기존의 강화학습 방법인 Single-task ( Learn from scratch)를 하게 하는 것을 Meta-learning으로 재 정의 할수도 있다는 것입니다. 









<img src="https://www.dropbox.com/s/dm5im9sthwt3f0a/Screenshot%202018-09-09%2020.42.55.png?raw=1">





 메타러닝을 이용한 예제로는 RL square : Fast reinforcement lerning via slow reinforcement learning 논문에서의 예제로 미로에서 새로운 미로가 와도 빠르게 배울수 있는 방법을 소개하였고요. 

<img src="https://www.dropbox.com/s/16d739bhk2rpi0v/Screenshot%202018-09-09%2021.02.49.png?raw=1">

<img src="https://www.dropbox.com/s/37qe74ezgi6384p/Screenshot%202018-09-09%2020.51.14.png?raw=1">





<img src="https://www.dropbox.com/s/n7stheja155cay8/Screenshot%202018-09-09%2021.03.04.png?raw=1">





<img src="https://www.dropbox.com/s/5ft0z3dspm6u3rt/Screenshot%202018-09-09%2021.03.21.png?raw=1">





Learning Dexterous In-Hand Manipulation에서는 Simulatior에서 임의로 샘플링된 월드에서 학습한 폴리시를 실제 로봇팔에 적용하여 빠르게 적응할수 있게한 방법을 소개하였습니다. 



<img src="https://www.dropbox.com/s/u93ocojhxnldiry/Screenshot%202018-09-09%2023.19.19.png?raw=1">

 서 환경에서는 적용하기 힘들며, 트레이닝 시간을 길게 준다면 기존 강화학습 알고리즘인 Policy Gradients 혹은 Q-learning이 더 나은 해결책을 찾는다는 것입니다. 



<img src="https://www.dropbox.com/s/conf6ypgp7n15bq/Screenshot%202018-09-09%2023.30.24.png?raw=1">



그래서 Meta RL 의 이러한 문제점들을 개선하기 위해 시도한 연구들에 대해 소개하고자 합니다. 

여기서는 무한대가 아닌 한정된 training task와 한정된 test task가 있습니다. 

Training task에서 학습된 test task에서 얼만큼 영향을 미치는지 보고싶은것 입니다. 





<img src="https://www.dropbox.com/s/rk26wnfwtvaik63/Screenshot%202018-09-09%2023.35.09.png?raw=1">

이 방법을 학습하기 위해서 Gym retro를 만들었고

이 환경을 이용한 학습 방법은 이전에 했던 비슷한 게임에서한 경험을 이용하여 

에이전트가 한번도 해보지 못한 게임을 사람처럼 빨리 마스터 하는것 입니다. 

Https://github.com/openai/retro 

https://blog.openai.com/gym-retro/



<img src="https://www.dropbox.com/s/3zsr99mp96rnoui/Screenshot%202018-09-09%2023.59.01.png?raw=1">



여러가지 강화학습 알고리즘을 사용하여 이 Sonic을 학습시킨 결과인데요. 



일반 PPO를 사용하였을때 성능이 가장 좋지 않았고, Test task를 이용하여 학습된 Parameter를 이용하여 Test task에서 사용한 PPO(Joint)의 성능이 가장 좋았습니다. 

하지만, 

사람이 15분 정도를 연습하고 소닉을 게임했을때 평균이 7000점 인 것과 비교했을때 아직까지 사람이 훨씬 더 잘한다는 것을 아록 있습니다. 



<img src="https://www.dropbox.com/s/ic0frivusupwhl0/Screenshot%202018-09-10%2000.04.11.png?raw=1">



이  Joint PPO를 이용하여 Fine tune을 한 뒤의 결과입니다. 



파란색은 Test에서의 학습 score을 나타내며, 아래 주황, 노랑, 빨강은 test set에서의 performance를 나타내는 것 입니다. 

여기서 X측은 Policy를 학습시키는 시간( 타입스텝 ) 입니다. 

그래프를 보면 50만 스텝 정도까지는 성능이 점점 좋아지다 갑자기 떨어집니다. 이것은 트레이닝 레벨에 오버피팅이 되었기 떄문입니다. 

이와 같은 문제점들을 계속 풀기 위해 노력중이며 또한 이와 관련하여 대회도 열었습니다. 



<img src="https://www.dropbox.com/s/ywchzssngpaw7vj/Screenshot%202018-09-10%2000.08.44.png?raw=1">



대회는 4월 5일부터 6월 5일까지 열렸고, 

총 923팀이 등록하였고 229팀이 해결책을 섭밋했습니다. 







<img src="https://www.dropbox.com/s/c4322sh7y5rnlh3/Screenshot%202018-09-10%2000.14.58.png?raw=1">

제가 랩장으로 있는 모두의연구소 CTRL랩 에서도 OpenAI retro에 참여하였으며 27위를 하였는데요. 

저희는 Rainbow DQN을 사용하여 소닉을 학습하였습니다. 



<img src="https://www.dropbox.com/s/mdl9u91ydwsh612/Screenshot%202018-09-10%2000.17.12.png?raw=1">



OpenAI에서는 Meta RL에서 발생하는 문제들을 개선하기 위해 다른 레벨 뿐만 아닌 다른 환경에서의 트랜스퍼러닝하려고 하고, 

그렇게 하기 위해 계획하고 있는 개선안으로는 Exploration, unsupervised learning, hierarchy를 개선하고 

긴 Horizontal한 시간에서도 적용할수 있도록 연구할 계획이라고 합니다. 





여기까지  9월 6일 Sk T-brain에서 주관한 Human.Machine. Experience Together 커퍼런스에서 John Shulman의 발표를 정리 및 일부 추가한 내용이었습니다. 



<img src="https://www.dropbox.com/s/873knk4td3t5er2/Screenshot%202018-09-10%2000.42.02.png?raw=1">



궁금한점이 있으시면 제 페이스북으로 연락주시면 답변드리도록 하겠습니다. 



감사합니다. 

























