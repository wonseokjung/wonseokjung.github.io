
# coding: utf-8

# # 안녕하세요. 정원석 입니다. 
# 
# ## 지금부터 "밑바닥부터 만들어보는 핸드메이드 인공지능 실습"을 하겠습니다. 
# 
# 

# ![6_28 001](https://user-images.githubusercontent.com/11300712/41980009-4aa7f43a-79da-11e8-91ec-8c485c6e7ef8.jpeg)
# 

# 먼저 OpenAI의 gym을 설명할게요. 
# 
# 참고 링크 : 
# OpenAI : 
# https://gym.openai.com/
# 
# OpenAi Environment : 
# 
# https://gym.openai.com/envs/#classic_control
# 
# OpenAi gym 사용법
# 
# https://wonseokjung.github.io//reinforcementlearning/update/openai-gym/
# 
# 

# # 강화학습과 친해지기 위한 OpenAI의 Gym사용법
# 
# 강화학습을 공부해보신 분들은 Gym이라는 용어를 많이 들어보셨을 것이다. 
# 
# 체육관의 의미를 가진 Gym은 무엇이며, 강화학습에 어떻게 사용될까?
# 
# 
# 
# 먼저 Gym은 OpenAI라는 회사에서 만들었다.
# 
# Gym을 만든 OpenAI는 비영리 인공지능 연구소이며, 안전한 인공지능을 만드는 것이 목표라고 한다. 
# 
# 
# OpenAI는 Gym과 Baselines라는 라이브러리를 제공한다. 
# 
# Gym은 Reinforcement Learning Algorithms을 개발하고 비교하기 위한 툴킷 이고,
# 
# algorithm을 실험해 볼 수 있도록 여러가지 Environments을 제공한다. 	
# 
# 
# [Gym 환경모음 링크](http://gym.openai.com)
# 
# [Gym  webistie 링크](http://GYM.OPENAI.COM)
# 
# Baselines는 강화학습 알고리즘 모음이다. 
# 
# OpenAI는 강화학습을 실험해볼 수 있도록, gym과 Baselines같은 강화학습 환경과 알고리즘을 제공한다. 
# 
# 
# 
# [Baselines 깃허브 링크](https://github.com/openai/baselines)
# 
# 
# 
# 

# ### 1. gym 설치하기
# `pip3 install gym`
# 
# gym을 설치하기 위해 python 3.5이상 버전에서 pip3 명령어로 gym을 설치한다. 
# 
# 또는
# 
# git clone을 하여 설치한다. 
# 
# ```
# git clone https://github.com/openai/gym
# cd gym
# pip3 install -e .
# ```
# 
# 
# 

# 
# ### 2. Environments
# 
# Algorithm을 환경에 적용해 보기전에, 
# 
# Cartpole이란 게임을 gym라이브러리를 통해 불러와서 action을 선택해보도록 하겠다. 
# 
# a. 함수 설명
# 
# `gym.make()` : 강화학습 환경을 불러온다.  
# 
# `env.reset()` : 환경을 초기화 한다. 
# 
# `env.render()` : 화면을 출력한다.
# 
# `env.action_space.sample()` : 임의의 action을 선택 
# 
# `env.step` : 선택한 action을 환경으로 보낸다.
# 
# ```
# import gym
# 
# env=gym.make('CartPole-V0')
# 
# env.reset()
# 
# for i in range(1000):
# 	env.render()
#     	env.step(env.action_space.sample())
# 
# ```
# 
# 위와같은 python코드로, 
# 
# gym을 통하여 카트폴 환경을 부르고, action을 선택하며 화면에 표시를 할수 있다.
# 
# 다른 환경을 불러오기를 원하면 `gym.make('CartPole-V0')` 대신에 [Gym 환경모음 링크](http://gym.openai.com) 에서 원하시는 환경을 선택하면 된다.
# 
# - - -
# 

# 
# ### 3. Observations
# 
# 위의 예는 env.action_space.sample()을 통하여 임의의 action을 선택하였다.
# 
# 이렇게 임의의 action을 선택하지 않고 더 좋은 action을 선택하려면, 
# 
# 우리가 선택한 action이 환경에 어떻게 작용하는지 알면 좋을것이다. 
# 
# 이 기능한 하는 함수는 위에서 본 `step`함수이다. 
# 
# 이 선택한 action을 `step`함수로 보내면, 다음의 4가지 value를 return한다. 
# 
# * observation : 픽셀 데이터과 같은 관찰값
# 
# * reward : 그 action을 하므로서 환경에서 받는 reward값
# 
# * done : 에피소드가 terminal 되면 True( 목표를 달성했거나, 에이전트가 목숨을 잃었을때)
# 
# * info : 환경의 정보들( 점수 등등 ) 
# 
# 
# Agent는 각 time step마다 action을 선택하며 Environment과 상호작용을 한다. 
# 
# 이때 Environment는 Agent로부터 action을 받고 reward와 observation을 return 한다.
# 

# ## 카트폴 환경 불러서 random action 선택하여 환경과 상호작용

# In[1]:


import gym
env=gym.make('CartPole-v0')

for i in range(20):
    observation=env.reset()
    for t in range(100):
        env.render()
        
        action= env.action_space.sample()
        observation, reward, done, info = env.step(action)
        
        if done:
            
            break

 
 


# In[2]:


env.close()


# # 환경을 바꿔볼까요? 

# https://gym.openai.com/envs/#classic_control
# 
# 
# 위의 링크에서 원하는 환경을 선택하세요 :) 
# 
# 저는 MountainCarContinuous-v0라는 환경을 사용해볼게요. 
# 
# 
# # 만약 Atari 환경을 원하시면 
# 
# ## MAC
# 
# $ pip3 install Pillow
# 
# $ pip3 install numpy
# 
# $ pip3 install torch=='0.3.1'
# 
# 
# $ pip3 install matplotlib
# 
# $ pip3 install h5py
# 
# $ pip3 install gym
# 
# $ pip3 install gym[atari]
# 
# $ pip3 install -U scikit-learn
# 
# ## Window
# 
# ![screen shot 2018-06-26 at 21 17 55](https://user-images.githubusercontent.com/11300712/41952789-9c54b948-7986-11e8-8ac0-8e130331e4c1.png)
# 

# In[3]:


import gym

env=gym.make("MountainCarContinuous-v0")


# In[4]:


env.reset()


# In[5]:


env.render()


# In[6]:


env.step(env.action_space.sample())


# In[7]:


for i in range(100):
    env.render()
    action= env.action_space.sample()
    
    observation, reward, done, info = env.step(action)
    
    if i==100:
        break
        
        
            
            


# In[8]:


env.close()


# #  DQN을 사용하여 학습

# 
# ![screen shot 2018-06-27 at 07 10 01](https://user-images.githubusercontent.com/11300712/41979814-cbc4ecea-79d9-11e8-8f1b-7e197575cd07.png)
# 

# In[9]:


import math, random

import gym
import numpy as np

import torch
import torch.nn as nn
import torch.optim as optim
import torch.autograd as autograd 
import torch.nn.functional as F


# In[10]:


from IPython.display import clear_output
import matplotlib.pyplot as plt
get_ipython().run_line_magic('matplotlib', 'inline')


# <h3>Use Cuda</h3>

# In[11]:


USE_CUDA = torch.cuda.is_available()
Variable = lambda *args, **kwargs: autograd.Variable(*args, **kwargs).cuda() if USE_CUDA else autograd.Variable(*args, **kwargs)


# <h2>Replay Buffer</h2>

# In[12]:


from collections import deque

class ReplayBuffer(object):
    def __init__(self, capacity):
        self.buffer = deque(maxlen=capacity)
    
    def push(self, state, action, reward, next_state, done):
        state      = np.expand_dims(state, 0)
        next_state = np.expand_dims(next_state, 0)
            
        self.buffer.append((state, action, reward, next_state, done))
    
    def sample(self, batch_size):
        state, action, reward, next_state, done = zip(*random.sample(self.buffer, batch_size))
        return np.concatenate(state), action, reward, np.concatenate(next_state), done
    
    def __len__(self):
        return len(self.buffer)


# <h2>Cart Pole Environment</h2>

# In[13]:


import gym
env=gym.make('CartPole-v0')


# <h2>Epsilon greedy exploration</h2>

# In[14]:


epsilon_start = 1.0
epsilon_final = 0.01
epsilon_decay = 500

epsilon_by_frame = lambda frame_idx: epsilon_final + (epsilon_start - epsilon_final) * math.exp(-1. * frame_idx / epsilon_decay)


# In[15]:


plt.plot([epsilon_by_frame(i) for i in range(10000)])


# <h2>Deep Q Network</h2>

# In[16]:


class DQN(nn.Module):
    def __init__(self, num_inputs, num_actions):
        super(DQN, self).__init__()
        
        self.layers = nn.Sequential(
            nn.Linear(env.observation_space.shape[0], 128),
            nn.ReLU(),
            nn.Linear(128, 128),
            nn.ReLU(),
            nn.Linear(128, env.action_space.n)
        )
        
    def forward(self, x):
        return self.layers(x)
    
    def act(self, state, epsilon):
        if random.random() > epsilon:
            state   = Variable(torch.FloatTensor(state).unsqueeze(0), volatile=True)
            q_value = self.forward(state)
            action  = q_value.max(1)[1].data[0]
        else:
            action = random.randrange(env.action_space.n)
        return action


# In[17]:


model = DQN(env.observation_space.shape[0], env.action_space.n)

if USE_CUDA:
    model = model.cuda()
    
optimizer = optim.Adam(model.parameters())

replay_buffer = ReplayBuffer(1000)


# <h2>Computing Temporal Difference Loss</h2>

# In[18]:


def compute_td_loss(batch_size):
    state, action, reward, next_state, done = replay_buffer.sample(batch_size)

    state      = Variable(torch.FloatTensor(np.float32(state)))
    next_state = Variable(torch.FloatTensor(np.float32(next_state)), volatile=True)
    action     = Variable(torch.LongTensor(action))
    reward     = Variable(torch.FloatTensor(reward))
    done       = Variable(torch.FloatTensor(done))

    q_values      = model(state)
    next_q_values = model(next_state)

    q_value          = q_values.gather(1, action.unsqueeze(1)).squeeze(1)
    next_q_value     = next_q_values.max(1)[0]
    expected_q_value = reward + gamma * next_q_value * (1 - done)
    
    loss = (q_value - Variable(expected_q_value.data)).pow(2).mean()
        
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()
    
    return loss


# In[19]:


def plot(frame_idx, rewards, losses):
    clear_output(True)
    plt.figure(figsize=(20,5))
    plt.subplot(131)
    plt.title('frame %s. reward: %s' % (frame_idx, np.mean(rewards[-10:])))
    plt.plot(rewards)
    plt.subplot(132)
    plt.title('loss')
    plt.plot(losses)
    plt.show()


# <h2>Training</h2>

# In[20]:


num_frames = 10000
batch_size = 32
gamma      = 0.99

losses = []
all_rewards = []
episode_reward = 0

state = env.reset()
for frame_idx in range(1, num_frames + 1):
    env.render()
    epsilon = epsilon_by_frame(frame_idx)
    action = model.act(state, epsilon)
    
    next_state, reward, done, _ = env.step(action)
    replay_buffer.push(state, action, reward, next_state, done)
    
    state = next_state
    episode_reward += reward
    
    if done:
        state = env.reset()
        all_rewards.append(episode_reward)
        episode_reward = 0
        
    if len(replay_buffer) > batch_size:
        loss = compute_td_loss(batch_size)
        losses.append(loss.data[0])
        
    if frame_idx % 200 == 0:
        plot(frame_idx, all_rewards, losses)


# ### References
# 
# https://github.com/wonseokjung
# 
# https://wonseokjung.github.io/
# 
# https://github.com/higgsfield/RL-Adventure
# 

# <p><hr></p>
