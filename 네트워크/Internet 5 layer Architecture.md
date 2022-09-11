최신 인터넷은 OSI 모델을 엄격하게 따르진 않고, 더 간단한 인터넷 프로토콜 집합을 가깝게 따른다.

# Internet protocol stack

## 특징

- OSI 모델에서 presentation, session 계층이 없는 형태
    - 필요하다면 application 계층에 구현하면 된다.
- TCP/IP 5계층 소프트웨어 모델이라고도 불린다.

## architecture

![image](https://user-images.githubusercontent.com/55528172/189519461-edcb3a58-6a11-462e-bfab-79d836bbefd2.png)

1. 5계층 **application** : HTTP(Web browsers), FTP, SMTP, etc. & proprietary protocols
2. 4계층 **transport** : TCP, UDP, ..
3. 3계층 **network** : IP(Internet Protocol), routing protocols
4. 2계층 **data link** : Ethernet, 802.11 (WiFi), 802.15.1 (Bluetooth), 802.15.4 (Zigbee), PPP, 3G, LTE, ..
5. 1계층 **physical** : copper, fiber, RF, ..

### 5. Application layer 어플리케이션 계층

어플리케이션(이메일, 웹 브라우저 등)이 직접 상호작용하는 계층

### 4. Transport layer 전송 계층

상대 호스트에서 작동하는 어플리케이션과 `connection`을 수립한다.

신뢰성 있는 연결을 위해서는 TCP를, 빠른 연결을 위해서는 UDP를 사용한다.

어플리케이션에서 동작하는 프로세스에게 `port number`를 부여한다.

TCP/IP 네트워크에 접근하기 위해 네트워크 계층을 사용한다.

### 3. Network layer 네트워크 계층

`packet`을 생성한다.

패킷의 출처와 목적지를 확인하기 위해 `IP address`를 사용한다.

### 2. Data Link layer 데이터 연결 계층

packet을 감싸는 `frame`을 생성한다.

출처와 목적지를 확인하기 위해 `MAC address`를 사용한다.

### 1. Physical layer 물리적 계층

프레임의 bit를 인코딩, 디코딩한다.

네트워크에서 시그널을 만들고 받는 transceiver를 포함한다.

## Narrow Waist model

- Internet 5-layer model은 Narrow Waist model, Hourglass model이라고도 불린다.
- 목적: **Interconnection**

`IP`는 Internet 아키텍처의 narrow waist이다. 아래 그림을 보면 이해할 수 있다.

![image](https://user-images.githubusercontent.com/55528172/189519466-ef3dd117-10bb-4f03-bd05-73f5304780ff.png)

- 가장 위의 application 계층과 가장 아래의 physical transport 계층 사이의 상호 운용성(**interoperability**)를 위해 고안되었다.
- IP는 네트워크가 최소의 공통 기능을 제공하기 위해 만들어졌고 모든 인터넷 기기들은 IP를 가져야 한다.
- 다양한 네트워크들이 상호작용할 수 있게 해주고, 많은 어플리케이션과 기능들을 숨겨준다.
