강의 : [반효경 교수님의 KOCW 강의](http://www.kocw.net/home/cview.do?cid=3646706b4347ef09)

- 강의 들어가기 앞서 가져야 할 태도
  OS 사용자 관점이 아니라 OS 개발자 관점에서 수강

# 운영체제란?

컴퓨터 하드웨어 바로 위에 설치되어 사용자 및 다른 모든 소프트웨어와 하드웨어를 연결하는 소프트웨어 계층

## 운영체제의 목표?

컴퓨터 시스템의 자원을 효율적으로 관리 (자원 관리자)
=> 자원? CPU, memory, 하드디스크 등 입출력장치들

### 커널(협의의 운영체제)

운영체제의 핵심 부분으로 메모리에 상주하는 부분

### 운영 체제의 분류

1. 동시 작업 가능 여부

- 단일 작업
- 다중 작업

2. 사용자의 수

- 단일 사용자 ex) MS Windows
- 다중 사용자 ex) UNIX

3. 처리 방식

- 일괄 처리(batch processing)
- 시분할(time sharing): 여러 작업을 수행할 때 컴퓨터 처리 능력을 일정한 시간 단위로 분할하여 사용 (현재 컴퓨터)
- 실시간(realtime OS): 정해진 시간 안에 어떠한 일이 반드시 종료됨이 보장되어야 하는 OS  
   ex) 원자로/공장 제어, 미사일 제어, 반도체 장비

### 운영 체제의 구조

누구한테 CPU를 줄까? => _CPU 스케줄링_  
한정된 메모리를 어떻게 쪼개어 쓰지? => _메모리 관리_  
디스크에 파일을 어떻게 보관하지? => _파일 관리_  
각기 다른 입출력장치와 컴퓨터 간에 어떻게 정보를 주고 받게 하지? => _입출력 관리_  
_프로세스 관리_

### 컴퓨터 시스템 구조

![컴퓨터 시스템 구조](https://user-images.githubusercontent.com/76620786/153706741-e8c76d48-c99b-40a4-94e6-7b6d25a02a01.png)  
timer는 특정프로그램이 cpu를 독점하는 것을 막기 위함, time sharing을 구현하기 위해 이용, 현재 시간을 계산하기 위함  
Mode bit의 역할 : 사용자 프로그램은 나쁜 짓을 할 수 있기 때문에 다른 프로그램이나 운영체제에 피해가 가지 않도록 보호하는 장치  
Mode bit

- 0 (커널 모드) : OS 코드 수행
- 1 (사용자 모드) : 사용자 프로그램 수행  
   보안을 해칠 수 있는 중요한 명령어는 모니터 모드에서만 수행가능한 **특권명령**으로 규정
  Interrupt나 Exception 발생시 하드웨어가 mode bit을 0으로 바꿈  
   사용자 프로그램에게 CPU를 넘기기 전에 mode bit을 1로 세팅

Direct Memory Access (DMA) controller : IO장치가 interrupt를 많이 하게 되면 CPU가 효율적으로 일을 못하니까 local buffer에 있는 작업이 끝나면 그 내용을 직접 메모리에 복사해서 CPU에 interrupt를 한번만 걸게 해서 CPU를 효율적으로 일하게 해줌  
Device Controller : IO장치의 일종의 작은 CPU -> hardware

### 인터럽트 (Interrupt)

1. 하드웨어 인터럽트: 하드웨어가 발생시킨 인터럽트
2. 소프트웨어 인터럽트 (Trap)

- Exception : 프로그램이 오류를 범한 경우
- System call : 프로그램이 커널 함수를 호출하는 경우

### 입출력의 수행

모든 입출력 명령은 **특권 명령**
사용자 프로그램은 어떻게 I/O에 접근하는가?

- System call => 사용자 프로그램은 운영체제에게 I/O요청
- 올바른 I/O요청인지 확인 후 I/O 수행
- I/O 완료 시 제어권을 시스템콜 다음 명령으로 옮김

### 동기식 입출력과 비동기식 입출력

![Synchronous and Asynchronous I O](https://user-images.githubusercontent.com/76620786/153748135-13ab6ac9-1da0-49f8-8131-0e9d52525bf2.png)  
동기식 입출력

1. 구현방법 1  
   I/O가 끝날 때까지 CPU를 낭비시킴
   매시점 하나의 I/O만 일어날 수 있음

2. 구현방법 2
   I/O가 완료될 때까지 해당 프로그램에게서 CPU를 빼앗음
   다른 프로그램에게 CPU를 줌

비동기식 입출력  
 I/O가 시작된 후 입출력 작업이 끝나기를 기다리지 않고 제어가 사용자 프로그램에 즉시 넘어감

### 프로그램의 실행 (memory load)

File system -> virtual memory -> address translation -> physical memory

virtual memory

- code => 커널 코드
  - 시스템 콜, 인터럽트 처리 코드
  - 자원 관리를 위한 코드
  - 편리한 서비스 제공을 위한 코드
- data
- stack => 커널 스택

## 프로세스

프로세스의 문맥  
 CPU 수행 상태를 나타내는 하드웨어 문맥  
 프로세스의 주소 공간(code, data, stack)  
 프로세스 관련 커널 자료 구조

### 프로세스의 상태

- Running : CPU를 잡고 instruction을 수행중인 상태
- Ready : CPU를 기다리는 상태 (메모리 등 다른 조건을 모두 만족하고)
- Blocked(wait, sleep) :
  - CPU를 주어도 당장 instruction을 수행할 수 없는 상태
  - 디스크에서 file을 읽어와야 하는 경우
- Suspended (stopped)
  - 외부적인 이유로 프로세스의 수행이 정지된 상태

> Blocked는 자신이 요청한 event가 만족되면 Ready  
> Suspended는 외부에서 재개해 주어야 Active

### 문맥 교환 (Context Switch)

CPU를 한 프로세스에서 다른 프로세스로 넘겨주는 과정  
시스템 콜이나 인터럽트 발생시 반드시 context switch가 일어나는 것은 아님

### 스케줄러 (Scheduler)

1. 장기 스케줄러 (job scheduler)  
   시작 프로세스 중 어떤 것들을 ready queue로 보낼지 결정  
   프로세스에 memory를 주는 문제
   time sharing system에는 보통 장기 스케줄러가 없음 (무조건 ready)

2. 단기 스케줄러 (CPU scheduler)  
   어떤 프로세스를 다음번에 running 시킬지 결정  
   프로세스에 CPU를 주는 문제

3. 중기 스케줄러 (Swapper)  
   여유 공간 마련을 위해 프로세스를 통째로 메모리에서 디스크로 쫓아냄  
   프로세스에게서 memory를 뺏는 문제

### 스레드 (Thread)

프로세스 하나에 CPU 수행단위만 여러개 두고 있는 것  
스레드가 동료 스레드와 공유하는 부분

- code section
- data section
- OS resources

스레드가 별도로 가지고 있는 구성

- program counter
- register set
- stack space

> 스레드의 장점

    - 다중 스레드로 구성된 태스크 구조에서는 하나의 서버 스레드가 blocked 상태인 동안에도 동일한 태스크 내의 다른 스레드가 실행되어 빠른 처리를 할 수 있다.
    - 동일한 일을 수행하는 다중 스레드가 협력하여 높은 처리율과 성능 향상을 얻을 수 있다.
    - 병렬성을 높일 수 있다. (CPU가 여러개 달린 경우)

### 프로세스 생성

부모 프로세스가 자식 프로세스 생성  
프로세스의 트리(계층 구조) 형성

주소 공간

- 자식은 부모의 공간을 복사함
- 자식은 그 공간에 새로운 프로그램을 올림

프로세스 소멸하려면 자식 프로세스부터 죽임

##### Copy-on-write(COW)

[COW란?](https://talkingaboutme.tistory.com/entry/Study-Copy-On-Write-COW)

### 프로세스와 관련있는 시스템 콜

fork() create a child(copy)  
exit() frees all the resources, notify parent  
exec() overlay new image  
wait() sleep until child is done

### 프로세스 간 협력 메커니즘(IPC)

메시지를 전달하는 방법

- message passing: 커널을 통해 메시지 전달
  - message system : 프로세스 사이에 공유 변수를 일체 사용하지 않고 통신하는 시스템
  - direct communication : 통신하려는 프로세스의 이름을 명시적으로 표시
  - indirect communication : mailbox (or port)를 통해 메시지를 간접 전달

주소 공간을 공유하는 방법

- shared memory : 서로 다른 프로세스 간에도 일부 주소 공간을 공유하게 하는 shared memory 메커니즘이 있음

> thread : thread는 사실상 하나의 프로세스이므로 프로세스 간 협력으로 보기는 어렵지만 동일한 process를 구성하는 thread들 간에는 주소 공간을 공유하므로 협력이 가능

## CPU Scheduling

CPU 스케줄링의 필요성  
여러 종류의 job(=process)이 섞여 있기 때문 (특히 I/O burst)

### CPU Scheduler & Dispatcher

_CPU Scheduler_(\*독립적인 하드웨어가 아니라 운영체제 안에서 CPU 스케줄링을 하는 코드를 뜻함)  
 Ready 상태의 프로세스 중에서 이번에 CPU를 줄 프로세스를 고른다.

_Dispatcher_  
 CPU의 제어권을 CPU scheduler에 의해 선택된 프로세스에게 넘긴다. (문맥 교환)

### CPU Scheduling 성능 척도

System 입장에서의 성능 척도

- CPU utilization(이용률) : CPU가 놀지 않고 일한 시간 비율
- Throughput(처리량) : 주어진 시간동안 몇개의 작업을 처리했는가

고객 입장에서의 성능 척도

- Turnaround time(소요시간) => CPU를 쓰러 와서 다 쓰고 나갈때까지 걸린 시간
- Waiting time(대기 시간) => CPU를 쓰고자 줄서서 기다린 시간
- Response time(응답 시간) => 처음 CPU를 얻고자 기다린 시간

**중국집 비유**
중국집 사장입장에서  
전체 시간중에서 주방장이 놀지 않고 일한 시간 = utilization  
단위 시간당 손님이 얼마나 왔는가 = throughput

고객 입장에서  
밥을 다먹고 나가는 시간 = turnaround time  
요리가 나오기 전 기다리는 총 시간 = waiting time  
첫번째 요리를 기다리는 시간 = response time

### SJF (Shortest-Job-First)

CPU burst time이 가장 짧은 프로세스를 제일 먼저 스케쥴

- Nonpreemptive
  => 일단 CPU를 잡으면 이번 CPU burst가 완료될 때까지 CPU를 선점당하지 않음
- Preemptive
  => 현재 수행중인 프로세스와 남은 burst time보다 더 짧은 CPU burst time을 가지는 새로운 프로세스가 도착하면 CPU를 빼앗김

### Priority Scheduling

우선순위가 높은 프로세스에게 CPU 할당

부작용 : 우선순위가 낮은 프로세스가 기다리는 시간이 길어서 CPU를 얻지 못할 수 있음.

해결책 : Aging => 오래 기다리게 되면 우선순위를 높여줌

### Round Robin

각 프로세스는 동일한 크기와 할당 시간을 가짐  
할당 시간이 지나면 프로세스는 선점당하고 ready queue의 제일 뒤에 가서 다시 줄을 선다.

장점: response time이 빨라짐

특징: 할당시간이 길면 FCFS, 짧으면 context switch가 잦아짐.

### Multilevel Queue

Ready queue를 여러 개로 분할  
-foreground  
-background

큐에 대한 스케줄링이 필요  
-fixed priority scheduling을 하면 우선순위가 높은 프로세스가 처리되고 다음 background에게 CPU를 줌  
-time slice를 하면 각 큐에 CPU time을 적절한 비율로 할당 ex) 80& to foreground in RR, 20% to background in FCFS

### Multilevel Feedback Queue

프로세스가 다른 큐를 이동 가능 (Multilevel queue는 우선순위가 이미 정해져있음)

scheduler를 정의하는 파라미터들  
-queue의 수  
-각 큐의 scheduling algorithm  
-Process를 상위 큐로 보내는 기준  
-Process를 하위 큐로 보내는 기준  
-프로세스가 CPU 서비스를 받으려 할 때 들어갈 큐를 결정하는 기준

예를 들면, 3개의 큐가 있다.  
-Q0 : 8 milliseconds를 부여하는 큐  
-Q1 : 16 milliseconds를 부여하는 큐  
-Q2 : FCFS

스케줄링

1. new job이 Q0로 들어감
2. CPU를 잡아서 할당 시간 8 milliseconds동안 수행함
3. 8 milliseconds동안 다 끝내지 못했으면 Q1으로 내려감
4. Q1에 줄서서 기다렸다가 CPU를 잡아서 16 milliseconds 동안 수행됨
5. 16 milliseconds에 끝내지 못한 경우 Q2로 쫓겨남

위로 갈수록 더 할당시간을 짧게 줌

### 다양한 스케줄링

- Multiple-Process Scheduling(CPU가 여러 개인 경우)
- Real-Time Scheduling
- Thread Scheduling

## Process Synchronization

문제

1. *공유 데이터*의 동시 접근은 데이터의 불일치 문제를 발생시킬 수 있다.
2. 일관성 유지를 위해서는 협력 프로세스 간의 실행 순서를 정해주는 메커니즘 필요

### Race condtion

여러 프로세스들이 동시에 공유 데이터를 접근하는 상황  
데이터의 최종 연산 결과는 마지막에 그 데이터를 다룬 프로세스에 따라 달라짐

문제 발생과 해결방안

1.  kernel 수행 중 인터럽트 발생시 => 양쪽 다 커널 코드이므로 kernel address space를 공유(순서를 정해주면 됨)
2.  Process가 system call을 하여 kernel mode로 수행 중인데 context switch가 일어나는 경우 => 커널 모드에서 수행 중일 때는 CPU를 preempt하지 않게 하고 커널 모드에서 사용자 모드로 돌아갈 때 preempt
3.  Multiprocessor에서 shared memory 내의 kernel data =>
    1. 한번에 하나의 CPU만이 거널에 들어갈 수 있게 하는 방법
    2. 커널 내부에 있는 각 공유 데이터에 접근할 때마다 그 데이터에 대한 lock / unlock을 하는 방법

### Critical Section

n 개의 프로세스가 공유 데이터를 동시에 사용하기를 원하는 경우 각 프로세스의 code segment에는 *공유 데이터를 접근하는 코드*인 critical section이 존재

### Deadlock

둘 이상의 프로세스가 서로 상대방에 의해 충족될 수 있는 event를 무한히 기다리는 현상

### Bounded-Buffer Problem (producer-consumer problem)

버퍼의 크기가 유한한 환경에서 여러 개의 생산자 프로세스들과 여러 개의 소비자 프로세스들이 있다.  
버퍼의 공간이 가득 차면 생산자는 더 이상 버퍼에 접근할 수 없고, 버퍼가 비어 있으면 소비자는 버퍼에 접근할 수 없다.

생산자(데이터 생성) 입장에서

1. Empty buffer가 있나요? (없으면 기다림)
2. 공유 데이터에 lock을 건다
3. Empty buffer에 데이터 입력 및 buffer 조작
4. Lock을 푼다.
5. Full buffer 하나 증가

소비자(데이터 소비) 입장에서는 full buffer에서 데이터 꺼내고 조작한다. 그 외 과정은 생산자와 같다.

shared data : buffer 자체 및 buffer 조작 변수(empty/full buffer의 시작 위치)

### Readers-Writers Problem

- 한 process가 DB에 write 중일 때 다른 process가 접근하면 안됨
- read는 동시에 여럿이 해도 됨
  **Solution**
- Writer가 DB에 접근 허가를 아직 얻지 못한 상태에서는 모든 대기중인 Reader들을 다 DB에 접근하게 해준다.
- Writer는 대기 중인 Reader가 하나도 없을 때 DB접근이 허용된다.
- 일단 Writer가 DB에 접근 중이면 Reader들은 접근이 금지된다.
- Writer가 DB에서 빠져나가야만 Reader의 접근이 허용된다.

* starvation이 일어날 수 있음. (ex. reader가 끝도 없이 올 때 writer는 무한 대기)

shared data : DB자체, readcount(\*현재 DB에 접근 중인 Reader의 수)

### Dining-Philosophers Problem

식사하는 철학자 문제는 5명의 철학자가 식사를 하고 있는 상황을 가정한 문제

> 프로세스 동기화 관련 아주 잘 작성되어 있는 블로그 : https://comdolidol-i.tistory.com/171?category=844795

## Deadlock(교착상태)

일련의 프로세스들이 서로가 가진 **resource**을 기다리며 block된 상태

**resource** : 하드웨어, 소프트웨어 등을 포함하는 개념 (I/O device, CPU cycle, semaphore 등)

Deadlock 발생의 4가지 필수 조건

- Mutual Exclusion: 매 순간 하나의 프로세스만이 자원을 사용할 수 있음
- No preemption: 프로세스는 자원을 스스로 내어놓을 뿐 강제로 빼앗기지 않음
- Hold and wait: 자원을 가진 프로세스가 다른 자원을 기다릴 때 보유 자원을 놓지 않고 계속 가지고 있음
- Circular wait: 자원을 기다리는 프로세스간에 사이클이 형성되어야 함

Deadlock의 처리방법

- Deadlock Prevention: 자원 할당 시 Deadlock의 4가지 필요 조건 중 어느 하나가 만족되지 않도록 하는 것
- Deadlock Avoidance: 자원 요청에 대한 부가적인 정보를 이용해서 deadlock의 가능성이 없는 경우에만 자원을 할당
- Deadlock Detection and recovery: Deadlock 발생은 허용하되 그에 대한 detection 루틴을 두어 deadlock 발견시 recover
- Deadlock Ignorance: Deadlock을 시스템이 책임지지 않음, UNIX를 포함한 대부분의 OS가 채택

### Deadlock Prevention

- No preemption

  - process가 어떤 자원을 기다려야 하는 경우 이미 보유한 자원이 선점됨
  - 모든 필요한 자원을 얻을 수 있을 때 그 프로세스는 다시 시작된다
  - State를 쉽게 save하고 restore할 수 있는 자원에서 주로 사용(CPU, memory)

- Hold and wait

  - 프로세스가 자원을 요청할 때 다른 어떤 자원도 가지고 있지 않아야 한다
  - 방법 1. 프로세스 시작 시 모든 필요한 자원을 할당받게 하는 방법
  - 방법 2. 자원이 필요할 경우 보유 자원을 모두 놓고 다시 요청

- Circular Wait

  - 모든 자원 유형에 할당 순서를 정하여 정해진 순서대로만 자원 할당
  - ex. 순서가 3인 자원 R0를 보유 중인 프로세스가 순서가 1인 자원 R1을 할당받기 위해서는 우선 R0를 release해야 한다

이 방법의 문제점: 생기지도 않을 deadlock을 대비해 제약을 걸기 때문에 Utilization 저하, throughput 감소, starvation 문제

### Deadlock Avoidance

자원 요청에 대한 부가정보를 이용해서 자원 할당이 deadlock으로부터 안전한지를 동적으로 조사해서 안전한 경우에만 할당

2가지 알고리즘을 통해 Deadlock Avoidance 구현

1. Banker's Algorithm
2. Resource Allocation Graph Algorithm

### Deadlock Detection and recovery

Detection

- Resource type 당 single instance인 경우 => 자원할당 그래프에서의 cycle이 곧 deadlock을 의미
- Resource type 당 multi instance인 경우 => Banker's Algorithm과 유사한 방법 사용

Recovery

- Process termination (프로세스를 종료)
- Resource Premption (deadlock에 연루된 프로세스들을 하나씩 자원을 뺏음)

### Deadlock Ignorance

Deadlock이 매우 드물게 발생하므로 deadlock에 대한 조치 자체가 더 큰 overhead일 수 있음  
만약, 시스템에 deadlock이 발생한 경우 시스템이 비정상적으로 작동하는 것을 사람이 느낀 후 직접 process를 죽이는 등의 방법으로 대처

## Memory Management

- Logcial address : 프로세스마다 독립적으로 가지는 주소 공간, CPU가 보는 주소
- Physical address : 메모리에 실제 올라가는 위치

### 주소 바인딩 (Address Binding)

Compile time binding

- 물리적 메모리 주소가 컴파일 시 알려짐
- 시작 위치 변경시 재컴파일
- 컴파일러는 절대 코드 생성

Load time binding

- Loader의 책임하에 물리적 메모리 주소 부여
- 컴파일러가 재배치가능코드를 생성한 경우 가능

Execution time binding(=run time binding)

- 수행이 시작된 이후에도 프로세스의 메모리 상 위치를 옮길 수 있음
- CPU가 주소를 참조할 때마다 binding을 점검

### Memory-Management Unit(MMU)

logical address를 physical address로 매핑해 주는 Hardware device

MMU scheme

- 사용자 프로세스가 CPU에서 수행되며 생성해내는 모든 주소값에 대해 base register(=relocation register)의 값을 더한다.

user program

- logical address만을 다룬다, 실제 physical address를 볼 수 없으며 알 필요가 없다.

### Dynamic Loading

프로세스 전체를 메모리에 미리 다 올리는 것이 아니라 해당 루틴이 불려질 때 메모리에 load하는 것.  
운영체제의 특별한 지원 없이 프로그램 자체에서 구현 가능

### Overlays

메모리에 프로세스의 부분 중 실제 필요한 정보만을 올림  
운영체제의 지원없이 사용자에 의해 구현  
프로세스 크기가 메모리보다 클 때 유용

### Swapping

---

## 용어 정리

> Multiprocessor : 하나의 컴퓨터에 CPU(processor)가 여러 개 붙어 있음을 의미
> Device driver : OS코드 중 각 장치별 처리 루틴 -> software  
> 인터럽트 벡터 : 해당 인터럽트의 처리 루틴 주소를 가지고 있음  
> 인터럽트 처리 루틴 : 해당 인터럽트를 처리하는 커널 함수  
> Caching: copying information into faster storage system
> preemptive : 강제로 빼앗음
> nonpreemptive : 강제로 뺴앗지 않고 자진 반납
> Starvation : indefinite blocking. 프로세스가 suspend된 이후에 해당하는 큐에서 빠져나갈 수 없는 현상.
