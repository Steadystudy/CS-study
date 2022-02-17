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

---

## 용어 정리

> Multiprocessor : 하나의 컴퓨터에 CPU(processor)가 여러 개 붙어 있음을 의미
> Device driver : OS코드 중 각 장치별 처리 루틴 -> software  
> 인터럽트 벡터 : 해당 인터럽트의 처리 루틴 주소를 가지고 있음  
> 인터럽트 처리 루틴 : 해당 인터럽트를 처리하는 커널 함수  
> Caching: copying information into faster storage system
