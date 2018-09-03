# Intelligent-Resource-Management-in-Blockchain-Based-Cloud-Datacenters
> Chenhan Xu, Nanjing University of Posts and Telecommunications Kun Wang, Nanjing University of Posts and Telecommunications, Shanghai Jiao Tong University Mingyi Guo, Shanghai Jiao Tong University


[간단 번역]
## Abstract
요즘에는 점점 더 많은 기업들이 비즈니스를 자체 서버에서 클라우드로 마이그레이션하고 있습니다. 계산 요구가 증가함에 따라 데이터 센터는 매일 엄청난 에너지를 소비하므로 에너지 효율 문제가 큰 관심을 받고 있습니다.  

본 논문에서는 예측 불가능한 용량의 녹색 에너지를 고려한 클라우드 데이터 센터에서의 에너지 인식 자원 관리 문제를 조사한다.  
블록 체인 기반의 분산 형 자원 관리 프레임 워크를 제안함으로써 요청 스케줄러가 소비하는 에너지를 절약합니다.  
또한 reinforcement learning method을 smart contract에 적용시켜 energy cost를 최소화할 수 있는 방법을 제안한다.  
Reinforcement learning method은 the historical knowledge으로부터 정보를 얻고 작동하기 때문에,  request arrival 및  energy supply에 의존하지 않습니다.  
Google 클러스터 추적 및 실제 전력 가격에 대한 실험 결과에 따르면, 제안되는 접근 방식은 다른 벤치 마크 알고리즘에 비해 데이터 센터의 비용을 크게 절감 할 수 있음을 보여줍니다.  

## Introduction
클라우드 컴퓨팅은 점점 더 우리 삶의 측면에 스며 들고 있습니다. 웹 메일, 검색 및 온라인 교육과 같은 기존 웹 서비스 외에도 IoT (Internet of Things)에있는 수십억 개의 장치는 데이터를 클라우드에 업로드합니다.  
데이터 양은 매우 빠르게 증가하고 있습니다.  
회사, 조직, 개인 개발자 및 심지어 개인까지도 서비스로서의 플랫폼 및 서비스로서의 인프라의 실현과 함께 클라우드 컴퓨팅을 사용할 수 있습니다.  

Google, Amazon 및 Microsoft와 같은 대기업은 회사 비즈니스를 지원할뿐만 아니라 일종의 구매 가능한 서비스로 데이터 센터 (DC)를 배포합니다.  
그러나 엄청난 양의 Data Center를 사용하면 엄청난 양의 에너지가 소모되어 에너지 위기가 심각 해지고 경제적 부담이 커집니다.  
미 에너지 국 (US Department of Energy) 국립 연구소의보고 1에 따르면 미국의 Data Center는 미국의 총 전기 소비량의 약 1.8 %를 소비하여 700 억 kWh를 달성했습니다.  

연구원들은 Data Center의 에너지 비용을 줄이기 위해 server 차원 과 Data Center 차원의 두 가지를 집중적으로 연구합니다.  
서버 차원에서, Gu 등은 DVFS (dynamic voltage frequencyscaling) 기반의 운영 비용 최소화 방법을 제안했다.  
이 연구는 minimization problem을 혼합 정수 비선형 프로그래밍 (MINLP)으로 간주하고 iterative searching algorithm으로 MINLP 문제를 해결했습니다.  

DC 수준에서 Zhang은 네트워크 처리량을 줄이기 위해 Exchanged Cube-Connected Cycles (Exchanged Cube-Connected Cycles)라고 불리는 DC 용 네트워크 아키텍처를 제안했습니다. 최근에 Chen 등은 DC의 비용을 종합적으로 평가할 수있는 틀을 개발했다. 녹색 에너지를 고려할 때,이 프레임 워크는 에너지 및 유지 보수 비용 공동 최적화를 뒷받침합니다. 그러나 DC의 에너지 비용을 줄이는 방법은 여전히 열려있는 문제입니다.  

본 논문에서는 클라우드 DC에서 지능형 에너지 절약 자원 관리 문제를 연구합니다.  
특히 우리는 가상 머신 (VM)을 실행하기 위해 많은 양의 계산 리소스가 필요한 일련의 요청을 고려합니다.  
이러한 요청 및 해당 VM은 풍력, 태양 및 조수에 의해 생성 된 변동하는 녹색 에너지와 전력망 모두에 연결된 여러 가지 클라우드 DC로 수용됩니다.  
본질적으로 클라우드 컴퓨팅 서비스 제공 업체는 더 많은 녹색 에너지를 사용할 수있는 DC에서 VM을 실행하는 것을 선호합니다.  
Google은 친환경 에너지 공급 업체 근처에서 DC를 구축하고 그리드 전력 이외의 저렴한 에너지를 저렴한 가격에 구입합니다.  
그러나 마이그레이션 프로세스에서 네트워크 정체가 발생하면 요청 및 VM을 해당 DC로 마이그레이션하는 것이 비용이 많이 든다.  

우리의 목표는 미래의 그린 에너지 생성에 대한 추가 지식없이 그리드 전력과 그린 에너지 모두를 사용하여 총 에너지 비용을 최소화하는 것입니다.  
이 문제를 해결하기 위해 먼저 트랜잭션의 모든 활동을 기록하는 분산 데이터 구조의 일종 인 블록 체인 (blockchain)을 기반으로 한 자원 관리 프레임 워크를 제안합니다.  
블록 체인을 사용하여 다른 프레임 워크와 달리, 제안하는 알고리즘은 (1) 스케줄러를 필요로하지 않고 (2) 추가 에너지 비용을 초래하지 않으며, (3) DC의 견고성을 감소시키지 않습니다.  
그런 다음 smart contract에서 reinforcement learning  (RL) 기반 요청 마이그레이션 방법에 대해 설명합니다.  
smart contract은 우리의 블록 체인 기반 프레임 워크에 저장되고 실행되며 제안 된 프레임 워크의 트랜잭션에 의해 트리거되는 스크립트입니다.  
DC와 상호 작용하여 생성 된 history data로부터 학습하는 RL 방법은 사전 지식이 필요하지 않습니다.  

이를 위해 본 논문의 주요 내용은 다음과 같이 요약된다.

1. DC의 에너지 비용 최소화 문제에서 요구 스케줄링 비용을 고려한다. 우리의 목표는 요청 스케줄링과 DC 간의 마이그레이션 요청으로 인한 총 에너지 소비 비용을 최소화하는 것입니다. 
2. 우리는 대부분의 전통적인 모델에 존재하는 스케줄러에 의해 에너지 비용을 절감하기 위해 블록 체인 기반 분산 자원 관리 프레임 워크를 제안합니다. 하나의 DC의 오작동은 지속적인 리소스 관리에 영향을 미치지 않으므로 프레임 워크에 우수한 견고성을 제공합니다. 
3. DC 간의 요청 이동의 총 비용을 최소화하기 위해 프레임 워크에서 스마트 계약에 의한 RL 기반 요청 마이그레이션 방법을 구현한다. RL이 에너지 비용 최소화 문제를 해결하기 위해 스마트 계약에 포함되기위한 첫 번째 시도입니다. 
4. 우리는 재생 가능 전력, 계통 가격 및 작업 부하의 실제 데이터 추적을 사용하여 제안 된 알고리즘의 성능을 평가하기 위해 시뮬레이션을 수행합니다. 수치 결과는 우리의 제안이 다른 벤치 마크 접근법보다 우수한 성능을 보임을 보여줍니다.

## System Model and Problem Formulation
![image](https://user-images.githubusercontent.com/40893452/44958202-b6c0d700-af17-11e8-80f6-a1017a95cb30.png)  
그림 1에서 볼 수 있듯이 클라우드 DC, 사용자 및 요청, 녹색 에너지 및 그리드를 포함하는 모델을 고려합니다.  
서로 다른 영역에 분산 된 DC는 집합으로 표시됩니다.  

일반적인 아키텍처와 달리 4 명의 사용자가 다른 DC에 요청을 제출할 수 있습니다.  
제안 된 블록 체인 기반 프레임 워크에서 이러한 요청은 DC 자체에서 예약 할 수 있으므로 클라우드 DC의 스케줄러에 대한 종속성이 제거됩니다.  

특히 우리는 사용자 요청 집합 R을 고려합니다.  
각 R은 VM을 실행하기 위해 더 많거나 적은 계산 리소스를 요구합니다.  
제안 된 프레임 워크는 분산되어 있기 때문에 각 DC에 요청을 제출할 수있는 권한이 있습니다. 
그림 1에서는 파선 경로를 사용하여 제출할 수있는 요청을 보여줍니다.  
또한, 우리의 모델은 T로 표현되는 이산 시계열을 고려한다.  
요청 r(k)의 데이터 처리 속도는 시간 tj에 걸쳐 변동하는 d로 표시됩니다.  
![image](https://user-images.githubusercontent.com/40893452/44964375-14cada00-af6b-11e8-9d8c-b84eed4b7cbd.png)  
많은 양의 데이터가 도착하면, 증가 된 계산 자원 사용은 더 많은 에너지를 소비하게됩니다.  
![image](https://user-images.githubusercontent.com/40893452/44964414-55c2ee80-af6b-11e8-852d-938bba0813b6.png)  
앞에서 언급 한 표기법에 기초하여, 시간 슬롯에서 DC ci의 에너지 소비  
![image](https://user-images.githubusercontent.com/40893452/44964426-725f2680-af6b-11e8-8ee9-b0dff5f4d86d.png)  

DC c(i) 에서 작동하는 VM에 의한 에너지 소비는 다음과 같다.  
![image](https://user-images.githubusercontent.com/40893452/44964480-c10cc080-af6b-11e8-9ebd-8be61ff0835a.png)   
이때, energy consumption은 DC c(i)의 hardware에 의존한다.  

타임 슬롯 t(j-1), t(j) 에 DC c(i)가 수용 한 요청.  
![image](https://user-images.githubusercontent.com/40893452/44964976-3c6f7180-af6e-11e8-86cc-b4d508044901.png)  

DC c(i)에 의해서 생산되는 green energy generation at time t(j-1), t(j) 는 g로 표현된다.  
![image](https://user-images.githubusercontent.com/40893452/44965003-5c9f3080-af6e-11e8-80df-6aa983a7d10a.png)  

DC는 Google과 같은 우선 순위가 높은 녹색 에너지를 사용하고 불충분 한 부분은 기존 그리드에 의해 전원이 공급된다고 가정합니다.  

녹색 에너지 및 전력망 가격은 다음과 같이 표기됩니다.  
![image](https://user-images.githubusercontent.com/40893452/44965047-97a16400-af6e-11e8-9acc-9a52b798c8ea.png)  

관리자는 요청과 해당 VM을 DC간에 마이그레이션하고 게이트웨이에서 라우팅 규칙을 업데이트하여 데이터 흐름을 리디렉션해야합니다.  
이때, 마이그레이션에 의한 cost는 다음과 같습니다.   
![image](https://user-images.githubusercontent.com/40893452/44965097-d3d4c480-af6e-11e8-901b-11762323d8e9.png)  
이때, c(i) 에서 c'(i)로 마이그레이션하는 cost (at time t(j-1), t(j))는 다음과 같습니다.  
![image](https://user-images.githubusercontent.com/40893452/44965111-e6e79480-af6e-11e8-9712-d0cbf29887ad.png)   

마지막으로, 에너지 소비 및 요청 이전에 대한 전체 시간 슬롯에 걸친 총 비용은 다음과 같이 계산할 수 있습니다.  
![image](https://user-images.githubusercontent.com/40893452/44965147-11d1e880-af6f-11e8-82ae-e18814d14283.png)  

또한, 시간 t(j)에서 DC들 간의 요청들의 파노라마 뷰를 다음과 같이 정의한다.  
![image](https://user-images.githubusercontent.com/40893452/44965171-2a420300-af6f-11e8-9d97-c5e0eccad9f6.png)  

또한 제안 된 프레임 워크는 클라우드 DC 간의 사용자 요청을 관리하여 총 에너지 비용을 낮추는 것을 목표로합니다.  
따라서 V를 계획하고 마이그레이션을 실행하여 총 비용 E를 최소화 할 것입니다.  
이 두 작업은 미래의 (1) 들어오는 그린 에너지 gij 와 (2) 요청 이행 비용에 대한 지식없이 수행됩니다.  

## Case Study: RL-Based Energy Cost Minimization 
RL은 이익을 극대화하기 위해 여러 상황에서 무엇을해야하는지 배우는 접근법입니다. 
RL의 핵심 요소는 상태, 조치, 보상 및 에이전트입니다.  
에이전트의 학습 과정에는 일련의 행동과 그에 상응하는 보상이 포함됩니다.  
각 상태에서 에이전트는 가치 함수 (즉, 가치 함수는 보상을위한 함수 매핑 상태 및 조치 임)에 따라 다양한 가능한 액션의 예상 수익을 평가합니다.  

그런 다음 에이전트는 특정 정책에 따라 수행 할 작업을 선택하고 그 이후 상태가 변경됩니다.  

이전 상태 및 취해진 조치와 관련된 보상은 가치 함수를 업데이트하는 데 사용됩니다.  

일반적으로 수익이라고도하는 수익은 특정 주에서 취해진 조치의 이점을 측정하는 누적 된 보상입니다.  
RL은 사전 지식이 필요 없기 때문에 복잡한 DC의 에너지 비용을 줄이기위한 이상적인 솔루션입니다.  

우리의 제안에서, 아이디어는 이전 마이그레이션 의사 결정의 에너지 비용에 따라 DC간에 요청 및 VM을 마이그레이션하는 것입니다.

우리는 들어오는 모든 요청에의해 촉발되는 현명한 계약으로 아이디어를 구현합니다.  
즉, 요청 제출 또는 리소스 할당이 발생하면 스마트 계약에 의해 구현 된 비용 최소화 알고리즘이 트리거됩니다.  
요청에 의해 유도 된 각 학습 반복에서 DC는 먼저 동작을 선택합니다 (요청과 해당 VM을 DC로 마이그레이션).  
그런 다음 이주가 수행됩니다.  
마지막으로 학습을 위해 새로운 주 및 보상 (DC의 부하 및 에너지 비용)을 얻습니다.  

특히 작업에는 DC 간의 가능한 모든 요청 및 VM 마이그레이션이 포함되며 상태는 DC가 달성 할 수있는 부하 상황입니다.

매 time slot의 시작에, next mining DC로 선정된 data center는 parnoramic view of request를 얻습니다.  
이 정보를 기반으로, migration 이후의 VMr과 request의 위치를 결정합니다.  
![image](https://user-images.githubusercontent.com/40893452/44966253-17323180-af75-11e8-91ca-4c5edb38590a.png)  
![image](https://user-images.githubusercontent.com/40893452/44965475-d1736a00-af70-11e8-891f-37e1ac6022c2.png)  

이때, DC들은 request의 panoramic view V(j)와 V(j+1)을 비교하면서 migration a(j)를 수행합니다.  
이에대한 보상 r(j)는 equation 4 인 E 로 결정됩니다.  (cost)  

migration 을 다음과 같이 Markov Decision Process로써 모델링 하였습니다.  
![image](https://user-images.githubusercontent.com/40893452/44966350-a0e1ff00-af75-11e8-931f-3a6d9a17b198.png)  

이 MDP 모델을 기반으로, value function Q(s, a)를 다음과 같이 정의합니다.  
![image](https://user-images.githubusercontent.com/40893452/44966385-d1299d80-af75-11e8-9901-182850b11d0a.png)  

알고리즘 2에서 RL 기반 마이그레이션 방법을 제시합니다. 
π(φ)로 표현되는 정책을 사용합니다.
임의의 행동을 확률 φ로 선택하거나 정책 π를 사용하여 행동을 선택합니다.
> e-greedy 를 의미하며, epsilon 대신 φ로 표기한 것으로 보입니다.

정책 (policy) π는 state s 에서 가장 낮은 값을 가진 action을 선택합니다.  
이는 reward 로써 사용되는 Equation 4가 cost 이기 때문에 최소한의 cost를 가지는 action을 선택하는 것이 올바른 선택이기 때문입니다.  
![image](https://user-images.githubusercontent.com/40893452/44966486-6c227780-af76-11e8-9182-5eef81522de2.png)   
모든 DC는 requests와 VMs migration에 참여할 수 있기 때문에, smart constract는 Algorithm 2를 따라 작동합니다.  
> 논문에 Algorithm2가 첨부되어있지 않네요 ... 


















