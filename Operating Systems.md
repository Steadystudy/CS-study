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

### 프로그램의 실행 (memory load)

File system -> virtual memory -> address translation -> physical memory

virtual memory

- code => 커널 코드
  - 시스템 콜, 인터럽트 처리 코드
  - 자원 관리를 위한 코드
  - 편리한 서비스 제공을 위한 코드
- data
- stack => 커널 스택

---

## 용어 정리

Multiprocessor : 하나의 컴퓨터에 CPU(processor)가 여러 개 붙어 있음을 의미
Device driver : OS코드 중 각 장치별 처리 루틴 -> software  
인터럽트 벡터 : 해당 인터럽트의 처리 루틴 주소를 가지고 있음  
인터럽트 처리 루틴 : 해당 인터럽트를 처리하는 커널 함수  
Caching: copying information into faster storage system
