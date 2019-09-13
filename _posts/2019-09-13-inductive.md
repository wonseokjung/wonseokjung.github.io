---
title: "On Inductiver Biases in Deep Reinforcement Learning"
date: 2019-09-13 10:26:28 -0400
categories: Reinforcementlearning update
use_math : true
---



오늘 새롭게 시작한  RL DIVERSITY 랩에서 딥마인드의 
On Inductiver Biases in Deep Reinforcement Learning 논문을 모두연 미니모 전효정님과 함께 읽고 요약을 하였습니다.

<img src="https://www.dropbox.com/s/1isvwx1aes7prfr/Screenshot%202019-09-06%2014.34.22.png?raw=1">



https://deepmind.com/research/publications/On-Inductive-Biases-in-Deep-Reinforcement-Learning



## 요약

최근  튜닝이 잘 된  Deep Reinforcement Learning은 다양한 분야에서 성능이 매우 좋습니다. 

스타크래프트, 도타, 바둑 등에서 맹 활약을 보이고 있죠. 

Well-tune을 잘 하기 위한 Domain Knowledge, pretuned learning  parrameter와 같은 것을 이 논문에서 Inductive Biases라고 부르며, Inductive Biases에는 trade off가 존재한다고 설명합니다.

Trade off는, 

Biases가 strong 할때는 빨리, Better 하게 학습을 하지만 범용적으로(geneal) 사용하기는 어렵습니다. 

반면, Biases Fewer할때는 General하게 학습을 하는 것이 가능합니다. 

위에 언급했듯이 well-tune을 하기 위해서는 리소스가 굉장히 많이 들어갑니다. 

여기서는  pretuned leaning parameter, domain knowledge의 형태를 inductive biases라고 표현합니다. 

그리고 이 inductive biases를 넣었을때 performace가 높아지지만 generality는 줄어든다고 이야기하고 있습니다(Trade off 가 발생하는 것이죠.)

성능을 높이기 위해서는 이 inductive biases를 투입하는 것이 더 좋은데, 여기서 드는 코스트는 별로 공개되어있지 않습니다.

하이퍼파라미터를 바꿔서 실험하는 것 자체가 엄청난 인력과 기타 리소스가 들어갈 것인데 말이죠. 

이 논문에서는 두가지 방향으로 inductive biases를 실험합니다. 

1. sulpting the agent's obeject ( 리워드를 discount 하는 것과, clipping 하는 것)
2. Sculpting the agent-environment interface ( action을 반복적으로 실행)

강화학습에서 Inductive biases에 대해 궁금하신 분은 딥마인드의 이 논문을 읽어보시길 추천드립니다. 



