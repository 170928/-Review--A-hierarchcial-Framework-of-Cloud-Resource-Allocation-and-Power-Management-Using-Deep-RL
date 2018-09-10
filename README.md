# -Review--A-hierarchcial-Framework-of-Cloud-Resource-Allocation-and-Power-Management-Using-Deep-RL
Ning Liu∗, Zhe Li∗, Jielong Xu∗, Zhiyuan Xu, Sheng Lin, Qinru Qiu, Jian Tang, Yanzhi Wang

- Short Review -  
> ICDCS 2017 에 실린 논문으로 간단하게 리뷰 한 내용입니다. 

## [Abstract]
강화 학습 (RL)과 같은 자동 의사 결정 접근법은 클라우드 컴퓨팅 시스템에서 리소스 할당 문제를 부분적 (partially) 으로 적용되었습니다.  
그러나 클라우드 리소스 할당 프레임 워크는 state와 action space 에서 높은 차원을 나타냅니다.  
이는 전통적인 강화 학습 (RL) 방법의 적용을 어렵게 만듭니다.   
또한, 높은 전력 소모는 클라우드 컴퓨팅 시스템의 설계 및 제어에서 중요한 문제 중 하나입니다.  
이는 시스템의 안정성을 저하시키고 냉각과 같은 유지 비용을 증가시킵니다.  
효과적인 동적 전력 관리 (DPM) 정책은 성능 저하를 유지하면서 전력 소비를 최소화해야합니다.  
따라서 공동 가상 컴퓨터 (VM) 자원 할당 및 전원 관리 프레임 워크는 전체 클라우드 컴퓨팅 시스템에 중요합니다.  
이 논문에서 제시하는 프레임 워크는 해결하고자 하는 "Cloud resource allocation framework"가 작동하는 state 와 action space에서 high dimension의 문제를 다룰 수 있습니다.  

본 논문에서는 클라우드 컴퓨팅 시스템에서 자원 배분 및 전력 관리 문제를 해결하기위한 새로운 계층 적 프레임 워크를 제안합니다.  
제안 된 계층 적 프레임 워크는 서버에 대한 (1) VM 리소스 할당을위한 "글로벌 티어"와 로컬 서버의 분산 된 (2) 전원 관리를위한 "로컬 티어"로 구성됩니다.

복잡한 제어 문제를 다룰 수있는 DRL (deep reinforcement learning) 기술을 통해서 high dimension의 state / action space 문제를 다루고,  
이를 통해서 글로벌 계층 문제를 해결합니다.  
또한, high dimension의 state / action space 을 처리하고 학습 과정에서 알고리즘의 수렴 속도를 가속화하기 위해 자동 엔코더 및 새로운 가중치 공유 구조가 채택되었습니다.  

반면, 분산 서버 전력 관리의 로컬 티어는 "LSTM 기반 작업 부하 예측기"와  "model-free" RL 기반 전력 관리자를 분산 방식으로 운영합니다.  
실제 Google 클러스터 추적을 사용한 실험 결과는 제안 된 계층 적 프레임 워크가 심각한 대기 시간 저하를 만들지 않으면서도 기본 전력 소모량과 에너지 사용량을 크게 절약 할 수 있음을 보여줍니다.  

## [INTRODUCTION]
클라우드 컴퓨팅은 오늘날의 컴퓨터 업계에서 가장 보편적 인 컴퓨팅 패러다임으로 떠오르고 있습니다.  
가상화 기술을 사용한 클라우드 컴퓨팅은 주문형 방식 (on-demand manner) 으로 가상 컴퓨터 (VM)를 할당하여 데이터 센터 또는 서버 클러스터의 전산 자원 (CPU, 메모리, 디스크, 통신 대역폭 등)을 공유 할 수 있습니다.  
데이터 센터에서 VM을 효과적으로 채택하는 것은 대규모 클라우드 컴퓨팅 패러다임의 중 하나입니다.  
이러한 기능을 지원하려면 효율적이고 강력한 "VM 할당 및 관리 체계"가 중요합니다.  

워크로드의 시간적 차이 ( time-variance of workloads )로 인해 online adaptive manner로 클라우드 리소스 할당 및 관리를 수행하는 것이 바람직합니다.  
강화 학습 (RL)과 같은 자동 의사 결정 접근법은 클라우드 컴퓨팅 시스템의 자원 할당 문제를 (partially) 해결하기 위해 적용되었습니다.  
model-free RL 기반 방법의 핵심 속priori modeling of state transition, workload, and power/performance
of the underlying system을 요구하지 않기 때문에 클라우드 컴퓨팅 시스템에 적합합니다.  
RL 기반 에이전트는 시스템이 실행될 때 최적의 자원 할당 결정을 학습하고 시스템 운영을 온라인 방식으로 제어합니다.

그러나 클라우드 컴퓨팅 시스템의 전체 자원 할당 프레임 워크는 상태 및 작업 공간에서 높은 차원을 나타냅니다.

예를 들어, 상태 공간에서의 상태는 특성 및 현재 자원의 데카르트 곱일 수있다
현재 서버의 작업 부하 수준 (할당 할 VM의 수와 특성)뿐만 아니라 각 서버의 사용률 수준 (서버 수백 대)

action space에서의 동작은 VM (즉, 물리적 머신)을 서버에 할당하고 VM 실행을 위해 서버에 리소스를 할당하는 것입니다.  

state and action space 의 높은 차원 (high dimension)은 전통적인 RL 기법이 전체 클라우드 컴퓨팅 시스템에 유용하지 못하다는 점에서 전통적인 방식의 학습 수렴 속도가 비 실용적으로 길어지게 되고 Deep learning based reinforcement learning이 필요해지는 이유입니다.  

결과적으로, 이전 연구에서는 단지 single physical server의 power 를 조절하거나, homogenous VM들을 제어하는 연구만이 이루어졌습니다.  

Power consumption은 클라우드 컴퓨팅의 설계 및 제어에있어 중요한 요소 입니다.  
높은 Power consumption는 시스템 안정성을 떨어 뜨리고 고성능 시스템의 냉각 비용을 증가시킵니다.  
"idle" or "uunderutilized" 상태의 component들을 선택적으로 "shut-off"하거나 "slow-down" 하는 것과같은 Dynamic power management (DPM)은 power consumtion을 줄이는 효과적인 방법임이 입증되었습니다.  

DPM (dynamic power management) 정책은 주로 "server level"에 적용되므로 VM 리소스 할당 결과에 따라 달라집니다.  
따라서 전체 데이터 센터 또는 서버 클러스터를 대상으로하는 VM 자원 할당 및 전원 관리 프레임 워크는 전체 클라우드 컴퓨팅 시스템에 중요합니다.  

> DQN 에 대한 간략한 설명이 써있는 부분은 생략하였습니다. github 의 다른 글을 참조해주세요.

DRL ( 이 논문에서는 DQN을 다음과 같이 표기합니다 ) 프레임 워크는 상대적으로 low dimension을 필요로합니다.  
각 결정 에포크에서 DRL 에이전트는 현재 상태에서 가능한 모든 동작을 열거하고 DNN을 사용하여 추론을 수행하여 최적의 Q (s, a) 값 추정을 유도해야하기 때문입니다.  
그러므로, 클라우드 리소스 할당 및 전원 관리의 작업 공간이 크게 감소해야합니다.

위의 목적을 달성하기 위해 본 논문에서는 클라우드 컴퓨팅 시스템에서 자원 배분 및 전력 관리 문제를 해결하기위한 새로운 계층 적 프레임 워크를 제안합니다.  
제안 된 계층 적 프레임 워크는 다음과 같은 구성을 갖습니다.  
(1) a global tier for VM resource allocation to the servers 
(2) a local tier for power management of local servers.   

제안 된 계층 적 프레임 워크는 향상된 확장 성을 갖습니다.  
그리고, DQN을 위해서 reduced 된 action / state dimension 외에도 server의 local power management를 온라인 및 분산 방식으로 수행 할 수있게하여 병렬 처리를 더욱 향상시킵니다.  

a global tier for VM resource allocation to the servers는 high dimension의 action / state space를 가지므로 DRL이 글로벌 티어 문제를 해결하기 위해 채택됩니다.  
action space를 줄이기 위해 이 논문에서는 (1) continuous time" (2) event-driven decision framework를 채택합니다.  

이때, action process에서의 action은 "target server" 를 나타내도록 합니다.  
그리고, high dimension의 state space를 줄이기 위해서, "autoencoder"와 "weight sharing" 구조를 제안합니다.  

"local tier"의 power management system은 다음과 같이 구성됩니다.  
(1) workload 예측기  
(2) power manager 

workload predictor는 앞으로 생길 workload를 예측합니다.  
이를 위해서 LSTM 을 사용합니다.  

Predicted workload 와 curren workload 정보를 ㅎ뢀용하여, power manager는 model-free RL 알고리즘을 사용합니다.  
power manager는 RL 알고리즘을 통해서 job latency와 power consumption을 줄이기 위해 server를 ON / OFF 합니다.  

## [BACKGROUND OF THE AGENT-ENVIRONMENT INTERACTION SYSTEM AND CONTINUOUS-TIME Q-LEARNING]
### Agent-Environment Interaction System
일반적인 agnet-environment model이 적용됩니다.  
agent, environment 그리고 finite state space와 action 그리고 reward function으로 구성된 환경으로 이루어져 있습니다.  
"decision maker"는 agent로 불립니다.  
이 agent는 environmnet와 상호작용하면서 action을 취하고 그에따라 reward를 받으며 가지고 있는 정책 (policy)를 평가하고 업데이트 해나갑니다.  

### Continous Time Q-Learning for Semi-Markov Decision Process (SMDP)
![image](https://user-images.githubusercontent.com/40893452/45281107-bde87600-b511-11e8-8bef-f8456905b517.png)  
agent는 다음과 같은 Q(s,a) 의 action-value function을 통해서 state s 에서 action a를 선택해 나아 갑니다.  
이 과정에서 cumulated reward를 최대화 하는 policy를 학습하는 것이 agent의 목적입니다.  
이 논문에서는 continuous-time system을 사용합니다.  
Q-Learning for SMDP는 "online adaptive RL which operates in continous time domain" 알고리즘입니다.  
이 알고리즘은 discrete-time domain에서의 Q-learning이 주기적으로 action-value function을 업데이트하는 것에서 오는 overhead를 줄일 수 있습니다.  
Q-learning with SMDP 알고리즘은 다음과 같은 방법으로 Q value를 업데이트 합니다.  
![image](https://user-images.githubusercontent.com/40893452/45281320-6dbde380-b512-11e8-877f-f95864a000eb.png)  
![image](https://user-images.githubusercontent.com/40893452/45281459-d6a55b80-b512-11e8-926f-ec5899274f08.png)  

## [SYSTEM MODEL AND PROBLEM STATEMENT]


