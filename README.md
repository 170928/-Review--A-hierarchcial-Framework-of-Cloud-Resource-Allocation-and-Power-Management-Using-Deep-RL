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
![image](https://user-images.githubusercontent.com/40893452/45281496-f9377480-b512-11e8-8c52-ff3883d4b410.png)  
본 논문에서는 클라우드 자원 할당 및 전원 관리 프레임 워크와 관련하여  
(1) D 개의 type을 가진 자원  
(2) M 개의 물리적 서버가있는 서버 클러스터를 고려합니다.  

서버는 power management를 위해 active mode or sleep mode에있을 수 있습니다.  
M을 물리적 서버 집합으로, D를 리소스 집합으로 나타냅니다.  

제안 된 계층 구조 프레임 워크의 전역 계층에서 제어하는 작업 중개자는  
도착시 클러스터의 서버 중 하나에 처리 리소스를 요청하는 작업을 전달합니다.  

"Job broker"는 "global tier"에 의해서 조정됩니다.  
Job broker는 resource를 할당 받기를 원하는 job에게 resource를 할당합니다.  

각각의 server는 모든 할당된 job들을 queue에 입력합니다.   
그리고 First-come-first-serve 방법에 따라서 resource를 할당합니다.   
만약 server가 불충분한  resource를 가지고 있다면, 충분한 resource가 생길 때 까지 대기합니다.    
그와 동시에, "local tier"가 power management를 수행하고 각각의 server를 On/Off 수행합니다.  
이렇게 global tier와 local tier의 job-scheduling과 power management는 전체적인 server cluster들의 performance와 power consumption에 영향을 미칩니다.  


![image](https://user-images.githubusercontent.com/40893452/45282096-ede54880-b514-11e8-92dc-b4e7a81becb6.png)    

"job latency"는 job의 arrival time과 completion time 의 차로 정의됩니다.  
위의 예시에서 latency로 보여주고 있습니다.  
Job 3 의 t(6) - t(3) 의 latency는 job duration 보다 긴 latency를 가지고 있습니다.  
그러므로, job broker는 scheduling 과정에서 하나의 server를 overload하게 만들면 안됩니다.  

Job이 "sleep mode"의 server에게 할당되면, T(on) time을 거친 후에 server가 active mode로 전환됩니다.  
유사하게, T(off) time을 거친 후에 server는 다시 "sleep mode"로 전환될 수 있습니다.  

![image](https://user-images.githubusercontent.com/40893452/45285320-e37b7c80-b51d-11e8-9eb8-b78b7add6f6e.png)  
위는 time t에서의 active mode인 server의 power consumption에 대한 수식입니다.  
x(t)는 time t에서의 server의 CPU 사용률을 나타냅니다.  
일반적으로, sleep mode에서 active mode로 전환하는데 사용되는 power consumption은 P(0%) 보다 큽니다.  

![image](https://user-images.githubusercontent.com/40893452/45285616-892eeb80-b51e-11e8-831a-789b86a0f2d2.png)  
(a)는 ad-hoc mode에서의 예시입니다.  
Job 1은 t(1)에 들어오며, Job 2는 t(3)에 들어옵니다.  
이때, Job1이 끝나는 시간 t(2)이후에 sleep mode로 전환을 위해서 T(off) time이 강제적으로 수행 된 이후,
t(3)에서 Job 2가 도착했음에도 sleep mode전환 이후 다시 T(on) time을 기다려 active mode전환 이후 Job 2가 할당됩니다.

반면, (b)는 dynamic power management 기법이 활용되어,  
Job 2 가 도착한 t(3)에 따라서 sleep mode 전환을 하지 않고 바로 Job 2를  할당하는 것으로  
최종 실행 완료시간이 훨씬 짧아지며, 이를 통해서 mode 전환 power consumption 등을 아껴 효율적으로 이루어집니다.  

이러한 작동은 power management를 수행하는 "local tier"에서 일어납니다.  
local tie의 효과적인 DPM 기술은 "global tier"의 VM 할당 결과를 기반으로  합니다.  
그러므로, 온라인 적응 방식으로 가장 바람직한 "time out" 값을 적절하게 결정하는 것이 필요합니다.  

> (b)에서 time out 값 이내에 Job 2 가 t(3)에서 도착하였기 때문에, sleep mode 전환이 이루어지지 않았습니다.  

workload predictor는 power management가 미래에 올 workload를 예측할 수 있게 해줍니다.  
대기열 (queue) 에있는 보류중인 작업의 수와 같은 "current information"들을 사용해서이루어진 예측 결과는 power management에 전달되고 시스템의 관측 영역에서 해당 조치 및 학습을 선택하기위한 전원 관리자의 상태로 제공됩니다.  

power management는 서버의 전력 소비 및 작업 대기 시간을 동시에 줄이는 데 도움이되는 가장 적절한 작업 (시간 초과 값)을 유도해야하며 model-free RL 기술은 적응 형 전원 관리 알고리즘이 됩니다.  

## [OVERVIEW OF DEEP REINFORCEMENT LEARNING]

![image](https://user-images.githubusercontent.com/40893452/45286169-0d35a300-b520-11e8-84b2-a60ad1f2ab3e.png)  

> DQN 에 대한 일반적인 내용을 설명하는 section이므로 github의 DQN 글을 참조해주세요.

## [THE GLOBAL TIER OF THE HIERARCHICAL FRAMEWORK – DRL-BASED CLOUD RESOURCE ALLOCATION]

"global tier"는 DQN 을 활용해서 high-dimension의 state space를 이용하여 VM resource allocation을 수행합니다.  
그와 동시에 action space를 줄기이 위해서, continuous time domain & event-driven dicision 을 사용합니다.  
action space는 단순히 target server의 index가 됩니다.  
그러므로, continuous-time Q-learning for SMDP 알고리즘이 이 논문에서 제시하는 프레임워크의 기반 알고리즘이 됩니다.  

"global tier"의 resource allocation은 "Job broker"가 수행하며 Job broker는 DRL algrotihm을 기반으로 작업을 수행합니다.  

### [State space]

![image](https://user-images.githubusercontent.com/40893452/45287111-78807480-b522-11e8-8ffe-be5bd2c63076.png)  

>(1) D 개의 type을 가진 자원  
>(2) M 개의 물리적 서버가있는 서버 클러스터를 고려합니다.  

### [Action space]
Action space는 index of server for VM (job) allocation.  
Action space 는 다음과 같이 정의됩니다.   
![image](https://user-images.githubusercontent.com/40893452/45287305-e4fb7380-b522-11e8-9910-d86769817dc3.png)  
(the same size as the total number of servers) 

### [Reward]
Profit (Revenue)는 모든 Job들을 처리하는데서 얻은 수익 과 소모된 총 에너지 비용 & reliability penalty의 차이입니다.  
Job을 처리하는 것에 의해서 얻게되는 income은 "job latency"와 함께 감소합니다.  
> waiting time in queue and processing time in the server   

"power consumption, job latency, reliability issue"를 모두 동시에 고려하기 위해서, reward function r(t)는 다음과 같이 정의됩니다.  
![image](https://user-images.githubusercontent.com/40893452/45287805-24768f80-b524-11e8-88a9-6d35749136e1.png)  

사용된 세가지 값들은 다음과 같습니다.  
(1) 총 전력 소비  
(2) 시스템의 VM 수  
(3) 안정성 목표 함수 값  
그리고 위의 세가지 값들에 "negative weight" 가 곱해진 형태입니다.  

"global tier"의 DRL agent는 위의 보상 함수를 기반으로 총 전력 소비, VM 대기 시간 및 안정성 메트릭의 선형 조합을 최적화합니다.  

DQN의 학습과정에서 state의 크기로 인해 발생하는 training time의 문제를 해소하기 위해서,  
autoencoder를 활용해서 lower-dimensional high level representation of server group state g(t_j)_k 
를 만들어냅니다.  

### [Sub-Qk]
> 자세한 내용은 사진으로 대체합니다.  
![image](https://user-images.githubusercontent.com/40893452/45288186-04939b80-b525-11e8-9c7a-83e324b00d72.png)  

### [Weight Sharing]
With weight sharing, any training samples can be used to train the Sub-Qk’s and autoencoders,  
compared to the case without weight sharing, where only the training samples allocated to a server in Gk  
can be used to train Sub-Qk.  
가중치 공유를 통해서 G(k) 에서의 experience of RL agent를 다른 G(k)의 RL agent 학습에도 사용할 수 있다는 소리입니다.  

![image](https://user-images.githubusercontent.com/40893452/45288677-35280500-b526-11e8-8245-25141d85d007.png)  

## [THE LOCAL TIER OF THE HIERARCHICAL FRAMEWORK – RL-BASED POWER MANAGEMENT FOR SERVER]
이전 section 에서는 global tier의 VM allocation RL agent에 대해서 다루었습니다.  
이 section 에서는 local tier의 power management RL agent에 대해서 다룹니다.  

![image](https://user-images.githubusercontent.com/40893452/45288832-9d76e680-b526-11e8-8c7b-c0fa40fe29e8.png)  

### [LSTM based Worklad Predictor]
local server를 power management를 위해서 ON/OFF 하며, average job latency를 줄이는 것이 agent의 목표가 됩니다.  
local tier는 "workload predictor"가 존재하며 이는 LSTM을 기반으로 구성되어 있습니다.  

workload predictor가 예측해야하는 workload는 VM resource allocation을 수행하는 "global tier"의 행동 결과에 따라서 정해집니다.  
> local server가 갖게되는 workload는 global tier의 Job Broker가 allocation하는 방법에 따라 영향을 받기 떄문입니다.  

### [Distributed Dynamic Power Management for Local Servers]
