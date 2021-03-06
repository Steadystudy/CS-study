강의: KOCW 컴퓨터 네트워크 (이석복 교수님)

## 통신 서비스

목표: end system(hosts) 간에 데이터 통신

### TCP

- 신뢰성있고, 순서대로 서버에 전달해준다.
- flow control: receiver 능력에 맞춰 데이터 속도를 전달한다.
- congestion control: 네트워크 혼잡 시 sender가 전송 속도를 늦춘다.

### UDP

- receiver 능력과 상관없이 sender가 reliable하지 않은 데이터를 마음대로 보낸다.
- 신뢰성이 별로 필요 없을 때 사용할 수 있다.

### Protocol

프로토콜은 컴퓨터 내부에서, 또는 컴퓨터 사이에서 데이터의 교환 방식을 정의하는 규칙 체계입니다. 기기 간 통신은 교환되는 데이터의 형식에 대해 상호 합의를 요구합니다. 이런 형식을 정의하는 규칙의 집합을 프로토콜이라고 합니다

### Network Core

- Circuit Switching: 하나의 회선을 할당받아 데이터를 주고받기 때문에 중간에 다른 사람이 끼어들 수 없다. (전화와 같은 실시간 통신에 사용)

- Packet Switching: 패킷을 받으면 들어오는 데로 보내준다.
  왜 컴퓨터는 packet switching을 사용하는가?  
  유저의 제약이 없다. 인터넷 사용하는 특성을 생각해 보면 유저가 동시에 사용할 확률이 낮기 때문에 사람이 몰리지만 않으면 많은 사람에게 전달할 수 있다.

  문제점 : nodal delay

  1. processing delay (패킷을 라우터에서 받았을 때 목적지에 따라서 분류하는데 걸리는 딜레이)
  2. queuing delay (그 라우트 링크에 다른 패킷들이 기다리고 있을 떄 생기는 딜레이)
  3. transmission delay (패킷은 쪼개질수 있는 단위가 아니어서 마지막 비트가 도착할 때까지 기다려야 해서 생기는 딜레이)
  4. propagation delay (마지막비트가 그 다음 라우터에 도달할 때까지 걸리는 딜레이)
