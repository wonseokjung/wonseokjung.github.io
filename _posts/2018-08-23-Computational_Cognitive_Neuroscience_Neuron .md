---
title: "뇌과학으로 보는 강화학습 - Neuron"
date: 2018-08-03 22:26:28 -0400
categories: NeuroScience
use_math : true
---



# Computational Cognitive Neuroscience - Neuron 

### 첫번째 Neuron 편 





<img src="https://www.dropbox.com/s/lvjrfnyarzcm6rv/Screenshot%202018-08-19%2020.35.47.png?raw=1">



---



뇌가 여러가지 일들을 할수 있는 이유는 수 많은 neuron들이 각각 연결되어 있기 때문이다. 
 뇌는 Lego 세트와 같은데, Lego 조각 하나는 단순하지만 이 조각을 모아서 수많이 많은것들을 재 조립하여 만들수 있기 때문이다. 



<img src="https://www.dropbox.com/s/tfzhw3zrw8sk264/Screenshot%202018-08-19%2021.23.38.png?raw=1">





Neuron 들은 다른 Neuron으로부터 여러가지 신호를 Input으로 받으며 그 신호들 중 의미가 있는 특정한 신호를 찾는다. 



예를 들어 "Smoke detector"과 같다.  Smoke detector은 Smoke를 측정하고 그 Smoke가 일정이상 

( threshold )를 넘으면 알람이 울리는 작용이다. 

Smoke dectecor과 같이 Neuron은 theshold가 넘은것을 감지했을때 alarm signal을 다른 neuron에게 보낸다. 

이 Alarm을 action potential 또는 Spike라고 부르며 이것들은 neuron들 끼리 communication 하기 위한 기본 단위이다. 



여기서는 어떠한 방법으로 neuron이 input signals을 다른 nueron들로부터 수신하고 그 수신한 값들을 통합하여 threshold값과 비교를 하는지, 그리고 그 결과값을 다른 neuron들과 어떻게 communication 하는지 이해하는것이 목표이다. 





### 1.1 Basic Biology of a Neuron as Detector



<img src="https://www.dropbox.com/s/28xsg6q7atd5zg7/Screenshot%202018-08-19%2022.38.48.png?raw=1">



위의 그림에서 Synapse는 sending neurons ( 시그널을 보내는 뉴런 ) 과 Receiving neurons( Signal을 받는 neurons) 들을 연결하는 points이다. 



대부분의 Synapses는 dendrites 위에 있다. 이 Dendrites는 neuron의  input signals를 통합하는 역할을 한다.



이렇게 통합된 Neuron들의 signal들이 cell body를 통과하며 최종적으로 통합된다. 



그렇게 통합된 신호들이 Axon을 통해 다른 neuron으로 전달된다. 



그 밖에 input signal에 의한 biological properties가 있다.

neuron이 받은 input signal의 주요 source는 세가지로 다음과 같다. 



#### 1. Excitatory inputs : 

다른 뉴론에서의 보통, 일반적인 타입의 인풋 ( 모든 input에서 대략 85% 정도 ) , nueron을 receive 하는 것을 exciting 하는 효과가 있는 것들이다. (threshold를 끝내게 만들고 알람을 울리게 하는 것 ) 

AMPA라고 불리는 synaptic channel에 의해 운반되어지며, 이 AMPA는 neurotransmitter glutamate에 의해 개방된다. 



#### 2. Inhibitory inputs : 

Excitatory inputs와 반대되는 input의 15%. 이 input은 neuron을 fire하지 않게 만든다! 



inhibiroty interneurons 에서 inhibitory inputs에 의해 만들어 지며 이 input은 GABA synapti channel에 의해 운반된다. 



### 3. Leak inputs : 

이 Leak inputs은 항상 활동을 하는 input signal은 아니지만 inhibiroty inputs와 비슷한 기능을 serve한다. 

excitation에 대응하며 전체 균형을 유지한다. 

생물학적으로 leark channels은 potassiu, channels( K )이다. 

---

inhibitory inputs와 excitatory inputs는 cortex의 다른 neurons에서 온다. neuron 끼리 communication을 할때 neuron은 inhibiroty input 또는 excitatory input을 다른 neuron에게 보내는 것이 가능하다. 

두 종류의 input을 한번에 보내는 것은 가능하지 않다. 









<img src="https://www.dropbox.com/s/uyrvjvekqmfpsn2/Screenshot%202018-08-20%2000.31.37.png?raw=1">







### 1.2 Net synaptic efficacy or weight

 

Synaptic connection에 따라 보내는 neuorn의 signal이 receiving neuron에게 주는 "signal의 강도" 이다.



또한 Synaptic weight는 computational cognitive neuroscience에서 가장 중요한 컨셉중 하나이다. 

생물학적으로는, neurotransmitter을 release하기 위해 neuron의 action 을 보내는  순수한 능력이다. 

그리고 neurotransmitter은 postsynaptic side에 있는  channel을 여는 역할을 수행한다. 

Computation 의 관점으로, weight는 detecting된 neuron을 측정한다. Weight value가 높은 경우는 어떠한 input neuron에 neuron의 반응이 굉장히 강한 것이며, 반대로 weight value가 낮을경우 그 input neuron이 많이 중요하지 않다는 의미이다. 



##### "당신이 아는 모든것, 당신의 뇌 안에 있는 모든 소중한 기억들은 모두 synaptic weights의 패턴으로 encoded 되어있다."





<img src="https://www.dropbox.com/s/eq9k7l58360ps0e/Screenshot%202018-08-20%2004.56.41.png?raw=1">





### 1.3 Dynamics of Intergration : Excitation vs Inhibition and Leak

통합하는 과정에서 세개의 다른 타입의 인풋 신호 ( (excitation, inhibition, leak) )은 neural computation에서 가장 중요하다.

이 section에는 위의 과정을 개념적, 직관적으로 이해하는 section이며 또한 이것들이 electrical properties of neurons와 어떻게 relate되어있는지 알아보도록 하겠다. 

그후에 이 process가 어떻게 수학적 equation으로 translate되어 computer에서 simulated 되어지는지 보도록 하겠다. 

위에서 보았던 Inhibition과 Excitation 또는 leaky inputs들이 intergration되는 과정은 

##### tug-of-war이라고 불린다

Tug-of-war은 excitation과 inhibition의 싸움이라고 볼수 있다.

https://grey.colorado.edu/CompCogNeuro/index.php/File:fig_vm_as_tug_of_war.png



만약 Excitation이 inhibition보다 강력하다면, neuron의 electrical potential이 증가한다. 이렇게 증가해서 어떠한 threshold를 넘는다면 output action potential을 발사한다. 

반대로 inhibition이 더 강력하다면, neuron의 electrical potential은 감소하며 fire하는 threshold의 반대방향으로 이동한다. 

---

#####  더 자세히 알아보기 전에 먼저 neuroscience에서 사용하는 terminology를 보도록 하자 





<img src="https://www.dropbox.com/s/v47szwp1p6virw8/Screenshot%202018-08-22%2004.40.50.png?raw=1">



### $g_i$ 

Inhibitory conductance : inhibitory input의 총 강도 



####  $E_i$

inhibitory driving potential



### $\theta$

action potential threshold : electircal potential at which the neuron will fire an action potential output to signal other neuron. 



### $V_m$

membrance potential 



### $E_e$

Excitatory driving potential 



### $g_e$

Excitatory conductance 





---

<img src="https://www.dropbox.com/s/y8hlx5tvr8awiyz/Screenshot%202018-08-22%2004.45.25.png?raw=1">

















Note 1 : 뇌는 plastic하다. 

<img src="https://www.dropbox.com/s/pdh8mvmigio7u79/Screenshot%202018-08-19%2020.28.00.png?raw=1">







#### 1.4 Mathmatical Formulations 



neuron 이 어떠한 방법으로 excitation 과  inhibition을 intergrate하는지 이해하여보자. 



이 과정을 수학 equation을 computer로 시뮬레이션하여 알수가 있다. 



이 mathmatical formulation을 
1.computing inputs 

2.computing outputs 

로 나누어 알아보도록 하겠다. 





#### 1.4.1 Computing Inputs







##### Neural Integration

##### 

-  excitatory current 

  $I_e=g_e (E_e - V_m)$ 






https://sharpbrains.com/blog/2008/02/26/brain-plasticity-how-learning-changes-your-brain





#### References

 

1. O'Reilly, R. C., Munakata, Y., Frank, M. J., Hazy, T. E., and Contributors (2012). Computational Cognitive 

   Neuroscience. Wiki Book, 1st Edition. URL: http://ccnbook.colorado.edu 

2. https://sharpbrains.com/blog/2008/02/26/brain-plasticity-how-learning-changes-your-brain
