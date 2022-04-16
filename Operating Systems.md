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

- Logcial address : 프로세스마다 독립적으로 가지는 주소 공간, _CPU가 보는 주소_
- Physical address : 메모리에 실제 올라가는 위치

### 주소 바인딩 (Address Binding)

> ![memory management](https://user-images.githubusercontent.com/76620786/156090903-7cb2dccb-dc69-4a29-a5bc-4ab60eece90f.png)

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

![MMU](https://user-images.githubusercontent.com/76620786/156094021-327870f7-0ad6-4b71-89ad-dcdddaf56084.PNG)

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

프로세스를 일시적으로 메모리에서 backing store로 쫓아내는 것

Backing store(=swap area) 디스크 => 많은 사용자의 프로세스 이미지를 담을 만큼 충분히 빠르고 큰 저장 공간

### Dynamic Linking

Linking을 실행 시간까지 미루는 기법

Static linking

- 라이브러리가 프로그램의 실행 파일 코드에 포함됨
- 실행 파일의 크기가 커짐
- 동일한 라이브러리를 각각의 프로세스가 메모리에 올리므로 메모리 낭비

Dynamic linking

- 라이브러리가 실행시 연결됨
- 라이브러리 호출 부분에 라이브러리 루틴의 위치를 찾기 위한 stub이라는 작은 코드를 둠
- 라이브러리가 이미 메모리에 있으면 그 루틴의 주소로 가고 없으면 디스크에서 읽어옴
- 운영체제의 도움이 필요

### Allocation of Physical Memory

메모리는 일반적으로 두 영역으로 나뉘어 사용

- OS 상주 영역 => interrupt vector과 함께 낮은 주소 영역 사용
- 사용자 프로세스 영역 => 높은 주소 영역 사용

할당 방법

1. Contiguous allocation : 각각의 프로세스가 메모리의 연속적인 공간에 적재되도록 하는 것
2. Noncontiguous allocation : 하나의 프로세스가 메모리의 여러 영역에 분산되어 올라갈 수 있음, _요즘 쓰는 할당_

### Contiguous allocation

1. 고정분할 방식

- 물리적 메모리를 몇 개의 영구적 분할로 나눔
- 분할의 크기가 모두 동일한 방식과 서로 다른 방식이 존재
- 융통성이 없음 => 동시에 메모리에 load되는 프로그램의 수가 고정됨, 최대 수행 가능 프로그램 크기 제한

2. 가변분할 방식

- 분할의 크기, 개수가 동적으로 변함
- 프로그램의 크기를 고려해서 할당

Hole

- 가용 메모리 공간
- 다양한 크기의 hole들이 메모리 여러 곳에 흩어져 있음
- 프로세스가 도착하면 수용가능한 hole을 할당
- 운영체제는 a) 할당 공간 b) 가용 공간(hole)로 정보를 유지

Dynamic Storage-Allocation Problem : 가변 분할 방식에서 size n인 요청을 만족하는 가장 적절한 hole을 찾는 문제

### Noncontiguous allocation

1. Paging
2. Segmentation
3. Paged Segmentation

### Paging

Process의 virtual memory를 동일한 사이즈의 page 단위로 나눔  
Virtual memory의 내용이 page 단위로 *noncontiguous*하게 저장됨  
일부는 backing storage에, 일부는 physical memory에 저장

> page Table

- main memory에 상주
- Page-table base register(PTBR)가 page table을 가리킴
- Page-table length register(PTLR)가 테이블 크기를 보관
- 모든 메모리 접근 연산에는 2번의 memory access필요
- page talbe 접근 1번, 실제 data/instruction 접근 1번
- 속도 향상을 위해 TLB라 불리는 고속의 lookup hardware cache 사용
  ![paging with TLB](https://user-images.githubusercontent.com/76620786/156271845-c82b901d-8631-496f-a71a-92e8f35d4e04.png)

### Two-Level Page Table

속도는 줄어들지 않지만 pagetable을 위한 공간이 줄어듬  
페이지 테이블 자체를 페이지화 시켜서 하드 디스크에 저장해두고 있다가 필요시 메모리로 로드하는 방식  
왜 쓰는가? 사용되지 않는 주소 공간에 대한 outer page table의 엔트리 값은 NULL

### Multilevel Paging and Performance

Address space가 더 커지면 다단계 페이지 테이블 필요
logical address의 physical adress 변환에 더 많은 메모리 접근 필요
TLB를 통해 메모리 접근 시간을 줄일 수 있음

### Inverted Page Table

page table이 매우 큰 이유

- 모든 process 별로 그 logical address에 대응하는 모든 page에 대해 page table entry가 존재
- 대응하는 page가 메모리에 있든 아니든 간에 page table 에는 entry로 존재

Inverted page table

- page frame 하나당 page table에 하나의 entry를 둔 것
- 각 page table entry는 각각의 물리적 메모리의 page frame이 담고 있는 내용 표시
- 단점 : 테이블 전체를 탐색해야함 => 조치 : associative register 사용 (expensive)

### Shared page

read-only로 하여 프로세스 간에 하나의 code만 메모리에 올림  
shared code는 모든 프로세스의 logical address space에서 동일한 위치에 있어야함

### Segmentation

프로그램은 의미 단위인 여러 개의 segment로 구성

![Segmentation](https://user-images.githubusercontent.com/76620786/156862224-f236b3eb-dd06-4b48-9a1f-6fa7f90da030.PNG)

logical address는 두가지로 구성 (<segment-number, offset>)  
Segment-table base register(STBR) : 물리적 메모리에서의 segment table의 위치  
Segment-table length register(STLR) : 프로그램이 사용하는 sement의 수

Protection

- 각 세그먼트 별로 protection bit가 있음

Sharing

- segment는 의미 단위이기 때문에 공유와 보안에 있어 paging보다 훨씬 효과적이다

Allocation

- segment의 길이가 동일하지 않으므로 가변분할 방식에서와 동일한 문제점들이 발생

### Segmentation with Paging

pure segmentation과의 차이점 : segment-table entry가 segment의 base address를 가지고 있는 것이 아니라 segment를 구성하는 page table의 base address를 가지고 있음  
![seg with paging](https://user-images.githubusercontent.com/76620786/156864425-600c1cb0-fed0-48d2-a319-81bbabfd6c14.PNG)

### Memory management 정리

CPU가 논리적 주소를 주면 물리적 메모리 주소로 변환을 시켜 메모리 주소를 참조하는 것
주소변환에 있어서 운영체제의 역할 ? 없음. 다 하드웨어가 해줘야 함. MMU를 통해
운영체제가 끼어들어야할 때? I/O장치에 접근할 때

## Virtual Memory

### Demand Paging

실제로 필요할 때 page를 메모리에 올리는 것

- I/O 양의 감소
- Memory 사용량 감소
- 빠른 응답 시간
- 더 많은 사용자 수용

Valid/ Invalid bit의 사용

- invalid의 의미 : 사용되지 않는 주소 영역인 경우, 페이지가 물리적 메모리에 없는 경우
- 처음에는 모든 page entry가 invalid로 초기화
- address translation 시에 invalid bit이 set되어 있으면 => "_page fault_"

### Page Fault

invalid page를 접근하면 MMU가 trap을 발생시킴 (page fault trap)  
Kernel mode로 들어가서 page fault handler가 invoke됨  
page fault 처리 순서

1. Invalid reference ? => abort process
2. Get an empty page frame (없으면 replace)
3. 해당 페이지를 disk에서 memory로 읽어온다.
   1. Disk I/O가 끝나기까지 이 프로세스는 CPU를 preempt 당함 (block)
   2. Disk read가 끝나면 page tables entry 기록, valid/invalid bit = "valid"
   3. ready queue에 process를 insert -> dispatch later
4. 이 프로세스가 CPU를 잡고 다시 running
5. 아까 중단되었던 instruction을 재개

### Various Caching Environment

Caching 기법 : 한정된 빠른 공간(=캐쉬)에 요청된 데이터를 저장해 두었다가 후속 요청시 캐쉬로부터 직접 서비스하는 방식  
캐쉬 운영의 시간 제약

- 교체 알고리즘에서 삭제할 항목을 결정하는 일에 지나치게 많은 시간이 걸리는 경우 실제 시스템에서 사용할 수 없음
- Paging system인 경우 : page fault인 경우에만 OS가 관여, 페이지가 이미 메모리에 존재하는 경우 참조시각 등의 정보를 OS가 알 수 없음 => LRU, LFU 사용불가 그래서 _Clock Algorithm_ 사용

### Page Frame의 Allocation

메모리 참조 명령어 수행시 명령어, 데이터 등 여러 페이지 동시 참조 => 명령어 수행을 위해 최소한 할당되어야 하는 frame의 수가 있음  
Loop를 구성하는 page들은 한꺼번에 allocate 되는 것이 유리함 => 최소한의 allocation이 없으면 매 loop마다 page fault

### Thrashing

- 프로세스의 원활한 수행에 필요한 최소한의 page frame의 수를 할당 받지 못한 경우 발생
- Page fault rate 매우 높아짐 => CPU utilization 낮아짐 => OS는 MPD를 높여야 한다고 판단 => 또 다른 프로세스가 시스템에 추가됨 => 프로세스 당 할당된 frame의 수가 더욱 감소 => 프로세스는 page의 swap in/out으로 매우 바쁨 => 대부분의 시간에 CPU는 한가함 => low throught put

### Page Size의 결정

Page size를 감소시키면

- 페이지 수 증가
- 페이지 테이블 크기 증가
- internal fragmentation 감소
- disk transfer의 효율성 감소
- 필요한 정보만 메모리에 올라와 메모리 이용이 효율적

## File and File System

File : "A named collection of related information"  
File attribute (metadata)

- 파일 자체의 내용이 아니라 파일을 관리하기 위한 각종 정보들

* 파일 이름, 유형. 저장된 위치, 파일 사이즈 등

File system

- 운영체제에서 파일을 관리하는 부분
- 파일 및 파일의 메타데이터, 디렉토리 정보 등을 관리
- 파일의 저장 방법 결정
- 파일 보호 등

### Directory and Logical Disk

Directory

- 파일의 메타데이터 중 일부를 보관하고 있는 일종의 특별한 파일
- 그 디렉토리에 속한 파일 이름 및 파일 attribute들

Partition (=Logical Disk)

- 하나의 (물리적) 디스크 안에 여러 파티션을 두는 게 일반적
- 여러 개의 물리적인 디스크를 하나의 파티션으로 구성하기도 함
- (물리적) 디스크를 파티션으로 구성한 뒤 각각의 파티션에 file system을 깔거나 swapping 등 다른 용도로 사용

### open()

![open](https://user-images.githubusercontent.com/76620786/159261607-b2de7d86-0d54-4df4-b4dd-6cc1e986ae96.png)

### File Protection

Access Control 방법

1. Access control Matrix
2. _Grouping_

- 전체 user를 owner, group, public의 세 그룹으로 구분
- 각 파일에 대해 세 그룹의 접근 권환을 3비트 씩으로 표시  
  ![grouping 접근 권한](https://user-images.githubusercontent.com/76620786/159262363-5ec07a6e-b00d-45de-b194-b4828d412f6d.png)
- ex. UNIX

3. Password

### Access Methods

1. 순차 접근

- 카세트 테이프를 사용하는 방식처럼 접근 (a에서 c로 가려면 b를 거쳐야함)
- 읽거나 쓰면 offset은 자동적으로 증가

2. 직접 접근

- LP 레코드 판과 같이 접근하도록 함
- 파일을 구성하는 레코드를 임의의 순서로 접근할 수 있음

### Contiguous Allocation

- 장점

  - Fast I/O
    - 한 번의 seek 로 많은 바이트 transfer
    - Realtime file용으로, 또는 이미 run 중이던 process의 swapping 용
  - Direct access 가능

- 단점
  - File grow가 어려움
    - file 생성시 얼마나 큰 hole을 배당할 것인가?
  - external Fragmentation

![스크린샷 2022-04-16 오후 1 46 14](https://user-images.githubusercontent.com/76620786/163661862-1aa563cf-efe6-454e-b62b-1b643cc438c2.png)

### Linked Allocation

하드디스크임에도 불구하고 Direct access 불가능 왜?  
directory에는 처음과 끝이 나와있는데 중간위치를 알려면 모든 데이터를 다 접근해야지만 알 수 있다. 그래서 순차접근만 가능

- 장점

  - external Fragmentation 발생 안함

- 단점

  - No random access
  - Reliability 문제
    - 한 sector가 고장나 pointer가 유실되면 많은 부분을 잃음
  - Pointer를 위한 공간이 block의 일부가 되어 공간 효율성을 떨어뜨림
    - 512 bytes/sector, 4bytes/pointer => pointer가 4바이트를 잡아 먹음

- 변형
  - _File-allocation table_ (FAT) 파일 시스템
    - 포인터를 별도의 위치에 보관하여 reliability와 공간효율성 문제 해결

![스크린샷 2022-04-16 오후 1 45 56](https://user-images.githubusercontent.com/76620786/163661867-d508830d-7f6c-413a-8a09-e661564e1e6d.png)

### Indexed Allocation

위치 정보를 하나의 인덱스에 내용으로 적어놓음

- 장점

  - Direct Access 가능
  - External fragmentation이 발생하지 않음

- 단점
  - small file의 경우 공간 낭비 (실제로 많은 file들이 small)
  - Too Large file의 경우 하나의 block으로 index를 저장하기에 부족
    - 해결 방안
      - linked scheme (양이 많으면 마지막에 또 다른 인덱스를 연결해줌)
      - multi-level index

![스크린샷 2022-04-16 오후 1 46 22](https://user-images.githubusercontent.com/76620786/163661847-f267522a-ca42-41e4-80fa-d3b2bd762f0d.png)

### UNIX 파일 시스템의 구조

![스크린샷 2022-04-16 오후 1 45 29](https://user-images.githubusercontent.com/76620786/163661883-954ebbea-8f01-4353-ab7e-11414e8a3702.png)

### FAT File System

중요한 정보를 담고 있어서 두 카피 이상 들고있다.

![스크린샷 2022-04-16 오후 7 28 09](https://user-images.githubusercontent.com/76620786/163671519-a50c94e4-657b-4a20-ae37-1f8878aa9360.png)

\*현재 사용되고 있는 File System은 어떨까?

### Free-Space Management

- Linked-list
  - 모든 free block들을 링크로 연결
  - 연속적인 가용공간을 찾는 것은 쉽지가 않다
  - 공간의 낭비가 없다
- Grouping
  - linked list 방법의 변형
  - 첫번째 free block이 n개의 pointer를 가짐
    - n-1 pointer는 free data block을 가리킴
    - 마지막 pointer가 가리키는 block은 또 다시 n pointer를 가짐
- Counting
  - 프로그램들이 종종 여러 개의 연속적인 block을 할당하고 반납한다는 성질에 착안
  - 빈 블럭의 위치를 가리키고 몇 개가 비어있는지 알려줌

### Directory Implementation

- Linear list
  - <file name, file의 metadat>의 list
  - 디렉토리 내에 파일이 있는지 찾기 위해서 linear search가 필요
  - 구현이 간단하지만 시간이 많이 필요
- Hash Table
  - Hash table은 file name을 이 파일의 linear list의 위치로 바꾸어줌
  - search time을 없앰
  - Collision 발생 가능
- File의 metadata의 보관 위치
  - 디렉토리 내에 직접 보관
  - 디렉토리에는 포인터를 두고 다른 곳에 보관 (ex. inode, FAT)

### VFS, NFS

Virtual File System

- 서로 다른 다양한 file system에 대해 동일한 시스템 콜 인터페이스를 통해 접근할 수 있게 OS의 layer

Network File System

- 분산 시스템에서는 네트워크를 통해 파일이 공유될 수 있음
- NFS는 분산 환경에서의 대표적인 파일 공유 방법

![스크린샷 2022-04-16 오후 7 47 04](https://user-images.githubusercontent.com/76620786/163672049-704114fd-d23b-43cb-a835-30bb9c620139.png)

### Page cache and Buffer Cache

- Page Cache

  - Virtual memory의 pagin system에서 사용하는 page frame을 caching의 관점에서 설명하는 용어
  - 캐쉬 히트가 나면 즉 이미 메모리에 존재하는 데이터에 대해서 하드 웨어적인 주소변환만 하기 때문에 정확한 접근 시간이나 이런 정보를 알 수 없어서 클락 알고리즘 사용

- Buffer Cache

  - 파일 시스템을 통한 I/O 연산은 메모리의 특정 영역인 buffer cache 사용
  - 모든 프로세스가 공용으로 사용
  - Replacement algorithm 필요
  - 파일을 접근할 때 시스템 콜을 해야하기 때문에 파일에 대한 요청이 언제 일어나는지 알 수 있음. 캐쉬 히트가 나든 미스가 나든 알 수 있음.

- Unified Buffer Cache
  - 최근의 OS에서는 기존의 buffer cache가 page cache에 통합된
  - 버퍼 캐쉬도 페이지 단위로 관리를 한다.고 볼 수 있음
  - ![스크린샷 2022-04-16 오후 7 56 59](https://user-images.githubusercontent.com/76620786/163672355-0381f752-4d6f-4396-98c0-78414e230014.png)

* 파일에 접근하는 루트는?  
  그 파일을 오픈한 다음 read system or write system을 통해 한다.
  그러나 Memory-Mapped I/O는 다르다.
  파일의 일부를 virtual memory 영역에 mapping을 시켜놓고 쓰는 것.  
  매핑 시킨 영역에 대한 메모리 접근 연산은 파일의 입출력을 수행하게 한다.

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
> TLB(Translation Lookaside Buffers) : Address Translation 과정에서 VPN을 PFN으로 변환하려면 Page Table에 접근하여야 한다. 이 과정을 더 빠르게 하기 위해 자주 쓰이는 Translation들을 MMU에 저장하여 사용하는데 이렇게 저장한 Cache를 뜻함.  
> LRU(Least Recently Used Algorithm), LFU(Least Frequently Used Algorithm)
