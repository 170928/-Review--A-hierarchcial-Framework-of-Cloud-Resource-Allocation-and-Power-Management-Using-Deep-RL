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


