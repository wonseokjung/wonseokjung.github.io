---
title: "2편 : Open AI Retro 소닉 대회 : 대회 목표, 대회 참여, 에이전트 제출"
date: 2018-04-11 22:26:28 -0400
categories: OpenAIretro update
use_math : true
---


# 제2편  Open AI Retro 소닉 대회 : 대회 목표, 대회 참여, 에이전트 제출

1편 : OpenAIRetro Contest 참여 및 소닉 설치방법

https://wonseokjung.github.io//openairetro/update/Retro-1/

강화학습과 친해지기 위한 OpenAI의 Gym사용법

https://wonseokjung.github.io//reinforcementlearning/update/openai-gym/


## 1.대회 목표

Sonic The Hedgehog™ series of games for SEGA Genesis 을 사용하여, 

Transfer-learning contest를 개최합니다. 

Transfer-learning을 이용하여 소닉게임을 플레이 하는 최고의 소닉게임을 만들어보세요. 




### 2.대회에 참여하기 위한 필요 요소

1. git repo 설치하기

To install the repo for the contest, which includes environment wrappers to set things up in the official way, run this:

```
git clone --recursive https://github.com/openai/retro-contest.git
sudo pip3 install -e "retro-contest/support[docker]"
```

Sonic-train.csv를 이용하여 python에서 새로운 contest 환경을 만들수 있습니다. 

sonic-train와 랜덤한 행동을 하는 에이전트의 파일은 아래의 링크에서 다운받을 수 있습니다. 


sonic-train.csv 다운로드 링크 :

https://contest.openai.com/static/sonic-train.csv

random agent 링크: 

https://contest.openai.com/static/random-agent-contest.py

```
from retro_contest.local import make

def main():
    env = make(game='SonicTheHedgehog-Genesis', state='LabyrinthZone.Act1')
    obs = env.reset()
    while True:
        obs, rew, done, info = env.step(env.action_space.sample())
        env.render()
        if done:
            obs = env.reset()


if __name__ == '__main__':
    main()
```



Note that it imports from retro_contest.local instead of retro. When you run this script you should see Sonic jumping randomly around a level. At this point, you can submit a simple agent to the evaluation server.

Also, be sure to check out our Discord server to chat with us or other contestants.


### 3.에이전트 Submit하기

리더보드에 에이전트를 오리기 위해서는 오픈에이아이의 서버에서 evaluate하여야 합니다. 

아래는 submit을 하기위한 에이전트의 예 입니다. 

프레임워크 없이 submission을 하기 위해서 Doncker을 이용합니다. 

만약 Mac을 사용하고 있다면 [맥을 위한 Docker](https://store.docker.com/editions/community/docker-ce-desktop-mac)을 이용하고, 윈도우 사용자라면 [윈도우를 위한 Dokcer](https://store.docker.com/editions/community/docker-ce-desktop-windows)
를 이용하세요. 

만약 저와같이 우분투를 사용한다면, [docker 설치 가이드를 따라가세요. ](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

Docker가 설치되면, 다음의 링크에서 register을 하세요.

https://contest.openai.com/register

등록을 하신 뒤 프로필을 열어서 보시면, Dokcer의 URL, 유저이름, 패스워드가 있습니다. 

프로필 링크 : 

https://contest.openai.com/user

Docker command line tool로 login을 하실수 있습니다. 

```
export DOCKER_REGISTRY=<docker registry url>
docker login $DOCKER_REGISTRY \
    --username <docker registry username> \
    --password <docker registry password>
```
export DOCKER_REGISTRY=retrocontestrtziplgvadskhfwu.azurecr.io

* registry와 username, password 를 넣을때 < > 는 제거해주셔야하며, 우분투 기준으로 dock login에 sudo로 해줘야 permission denied가 되지 않습니다.


다음의 코드는 random action을 하는 간단한 agent입니다. 

하지만 이 agent느 항상 오른쪽으로 가는 키가 눌러져 있습니다. 

링크 주소 : 

https://contest.openai.com/static/simple-agent.py
```
import gym_remote.exceptions as gre
import gym_remote.client as grc


def main():
    print('connecting to remote environment')
    env = grc.RemoteEnv('tmp/sock')
    print('starting episode')
    env.reset()
    while True:
        action = env.action_space.sample()
        action[7] = 1
        ob, reward, done, _ = env.step(action)
        if done:
            print('episode complete')
            env.reset()


if __name__ == '__main__':
    try:
        main()
    except gre.GymRemoteError as e:
        print('exception', e)
```



* 에이전트 이미지를 build할 Dockerfile 

링크 주소 : https://contest.openai.com/static/simple-agent.docker

```
FROM openai/retro-agent
ADD simple-agent.py .
CMD ["python", "-u", "/root/compo/simple-agent.py"]
```

만약 이 파일들을 받고 에이전트를 build 하려면 다음을 하세요

```
mkdir simple-agent
cd simple-agent
curl -O https://contest.openai.com/static/simple-agent.py
curl -O https://contest.openai.com/static/simple-agent.docker
docker build -f simple-agent.docker -t $DOCKER_REGISTRY/simple-agent:v1 .
```

Docker이미지를 업로드 합니다. 

```
docker push $DOCKER_REGISTRY/simple-agent:v1
```

그 다음 [Submit하기 ](https://contest.openai.com/user/job/submit)로 들어가서 submit을 합니다. 

처음 submission은 리더보드에 올라가기까지 몇시간이 소요됩니다. 



레트로 콘세스트 깃헙주소

https://github.com/openai/retro-contest

















* Reference

https://contest.openai.com/

https://contest.openai.com/leaderboard

https://blog.openai.com/retro-contest/