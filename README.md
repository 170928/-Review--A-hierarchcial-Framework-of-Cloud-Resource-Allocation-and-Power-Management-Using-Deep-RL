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
