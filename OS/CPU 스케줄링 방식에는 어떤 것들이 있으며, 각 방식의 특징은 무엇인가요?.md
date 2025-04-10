# CPU 스케줄링 방식에는 어떤 것들이 있으며, 각 방식의 특징은 무엇인가요?

## 면접용 답변

CPU 스케줄링 방식에는 크게 비선점 방식과 선점 방식이 있습니다.

비선점형 스케줄링은 프로세스가 CPU를 할당받으면 스스로 종료하거나 대기 상태로 바뀔 때까지 CPU를 계속 사용하는 스케줄링 기법입니다.
이미 할당된 CPU를 다른 프로세스가 강제로 빼앗아 사용할 수 없습니다. 
FCFS(first-come first-served) 스케쥴링 방식과 SJF 스케쥴링 방식이 비선점형 스케줄링에 해당됩니다.

그에 반해 선점형 스케줄링은 더 높은 우선순위의 프로세스가 도착하면 현재 실행 중인 프로세스를 중단하고 CPU를 할당하는 스케줄링 기법입니다.
비교적 응답이 빠르다는 장점이 있지만, 처리 시간을 예측하기 힘들고 높은 우선순위 프로세스들이 계속 들어오는 경우 오버헤드 초래할 수 있습니다.
SRTF, 우선순위 스케줄링,  라운드 로빈 스케줄링, 다단계 큐 등이 선점형 스케쥴링 기법에 해당됩니다. 

<br><br>

## 개념 설명

### 1. CPU 스케줄링이란?

- 프로세스가 생성되어 실행될 때 필요한 시스템의 여러 자원을 해당 프로세스에게 할당하는 작업
- 대기 시간은 최소화하고 최대한 공평하게 처리하는 것을 목적으로 함


### 2. CPU 스케줄링의 목적

  - 공평성 : 모든 프로세스가 자원을 공평하게 배정받아야 하며, 특정 프로세스가 배제되어서는 안 된다. 
  - 효율성 : 시스템 자원을 놀리는 시간 없이 스케줄링해야 한다. 
  - 안정성 : 우선순위를 사용하여 중요한 프로세스가 먼저 처리되도록 해야 한다. 
  - 반응 시간 보장 : 응답이 없는 경우 사용자는 시스템이 멈춘 것으로 가정하기 때문에 시스템은 적절한 시간 안에 프로세스의 요구에 반응해야 한다. 
  - 무한 연기 방지 : 특정 프로세스의 작업이 무한히 연기되어서는 안 된다.

### 3. 비선점형 스케줄링
프로세스가 CPU를 할당받으면 스스로 종료하거나 대기 상태로 바뀔 때까지 CPU를 계속 사용
 
####  ✅ FCFS (First-Come First-Served)

가장 먼저 도착한 순서대로 실행

⛔ 단점: 긴 작업이 앞에 오면 뒤의 짧은 작업이 기다려야 함 (Convoy Effect) <br>
⭕ 장점: 구현이 단순

<br>

#### ✅ 비선점형 SJF (Shortest Job First)

실행 시간이 가장 짧은 프로세스부터 실행

⭕ 장점: 평균 대기 시간 최소 <br>
⛔ 단점: 실행 시간 예측이 어려움

<br>

#### ✅ 비선점형 우선순위 스케줄링
정해진 우선순위에 따라 실행

⛔ 단점: Starvation(기아 현상) 가능

<br>

### 4. 선점형 스케줄링

실행 중이더라도, 더 중요한 프로세스가 오면 현재 실행 중인 프로세스를 강제로 중단시킴

#### ✅ Round Robin

정해진 시간 할당량(Time Quantum)만큼 실행하고, 그 뒤엔 다음 프로세스로 전환
⭕ 장점: 응답 시간 짧음 → 인터랙티브 시스템에 적합 <br>
⛔ 단점: 너무 짧은 Time Quantum은 오버헤드 증가

<br>

#### ✅ 선점형 SJF (Shortest Remaining Time First)

남은 실행 시간이 가장 짧은 프로세스를 먼저 실행
평균 대기 시간 최소화 가능

<br>

#### ✅ 선점형 우선순위 스케줄링

더 높은 우선순위의 프로세스가 오면 선점
⛔ 단점: Starvation 가능 → Aging 기법으로 해결

> ✏️ Aging 기법 <br>
> 기다리는 시간이 길어지면 우선순위를 조금씩 높여, 언젠가는 가장 높은 우선순위가 되어 CPU를 할당 받을 수 있게 해주는 방법

<br>

#### ✅ 다단계 큐 (MLQ)

작업을 여러 큐(우선순위별)로 나누고 각각 다른 스케줄링 사용 <br>
각 큐는 자신만의 독자적인 스케쥴링을 가짐 <br>
Ex) 시스템 프로세스는 Round Robin, 사용자 프로세스는 FCFS 등

#### ✅ 다단계 피드백 큐 (MLFQ)

입출력 위주와 CPU 위주인 프로세스의 특성에 따라 큐마다 서로 다른 CPU 시간 할당량 부여 <br>
FCFS와 라운드 로빈 기법혼합 <br>
새로운 프로세스는 높은 우선순위를 가지지만 프로세스의 실행 시간이 길어질 수록 점점 낮은 우선순위 큐로 이동하며, (이 때, 우선 순위가 낮을 수록 시간할당량을 크게 줌으로써 보완 가능) 마지막 단계에서 FCFS 방식을 적용

⭕ 장점 : 유연성 뛰어남, turnaround 시간과 response time에 최적화

<br>

### 5. 성능 척도


- CPU
  - CPU utilization(CPU 사용률) 
  <br> 시스템의 동작 시간 중 CPU가 사용된 시간을 측정하는 방법으로 최대한 CPU를 바쁘게 만드는 것 
  - Throughput(처리량)
   <br> 단위 시간당 작업을 마친 프로세스의 수

- 프로세스의 입장 
  - Trun-around time(반환 시간)
    <br> 프로세스가 생성된 후 종료되어 사용하던 자원을 모두 반환하는 데까지 걸리는 시간
    <br> 프로세스의 대기 시간 + 실행시간 
  - Waiting time(대기 시간)
    <br> 프로세스가 CPU를 할당받아 실행되기 전 대기 상태일 때의 시간
  - Response time(응답 시간)
    <br> 프로세스가 대기 상태에 들어와 CPU를 최초로 얻기까지 걸리는 시간



<br><br>

## 새끼 질문 

- 스케줄링에 대해 설명해주세요.
- CPU 스케줄링의 목적은 무엇입니까?
- 우선순위 스케줄링에서 Starvation 문제를 해결하기 위한 방법은 무엇입니까?

<br><br>

----

## Reference
https://velog.io/@ohsol/운영체제-프로세스-스케줄링-종류와-기법#비선점-스케줄링
https://baebalja.tistory.com/360