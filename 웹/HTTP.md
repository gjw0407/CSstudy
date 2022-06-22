# HTTP

> **HyperText Transfer Protocol
HTML 문서와 같은 리소스들을 가져올 수 있도록 해주는 프로토콜**
텍스트 기반의 통신 규약으로 **인터넷에서 데이터를 주고받을 수 있는 프로토콜**
이렇게 규약을 정해두었기 때문에 모든 프로그램이 이 규약에 맞춰 개발해서 서로 정보를 교환할 수 있게 되었다.
> 

![Untitled](https://user-images.githubusercontent.com/55426397/174922080-c554e0f4-189c-487b-832d-85a1e0737334.png)

## HTTP 동작

클라이언트 즉, 사용자가 브라우저를 통해서 어떠한 서비스를 url을 통하거나 다른 것을 통해서 요청(request)을 하면 서버에서는 해당 요청사항에 맞는 결과를 찾아서 사용자에게 응답(response)하는 형태로 동작한다.

- 요청 : client -> server
- 응답 : server -> client

HTML 문서만이 HTTP 통신을 위한 유일한 정보 문서는 아니다. Plain text로 부터 JSON 데이터 및 XML과 같은 형태의 정보도 주고 받을 수 있으며, 보통은 클라이언트가 어떤 정보를 HTML 형태로 받고 싶은지, JSON 형태로 받고 싶은지 명시해주는 경우가 많다.

## HTTP 특징

- HTTP 메시지는 HTTP 서버와 HTTP 클라이언트에 의해 해석이 된다.
- TCP/ IP를 이용하는 응용 프로토콜이다.(컴퓨터와 컴퓨터간에 데이터를 전송 할 수 있도록 하는 장치로 인터넷이라는 거대한 통신망을 통해 원하는 정보(데이터)를 주고 받는 기능을 이용하는 응용 프로토콜)
- HTTP는 연결 상태를 유지하지 않는 비연결성 프로토콜이다.(이러한 단점을 해결하기 위해 Cookie와 Session이 등장하였다.)
- HTTP는 연결을 유지하지 않는 프로토콜이기 때문에 요청/응답 방식으로 동작한다.

![https://velog.velcdn.com/post-images%2Fsurim014%2Fe0aa5520-2d59-11ea-86da-fb3b00230640%2Fimage.png](https://velog.velcdn.com/post-images%2Fsurim014%2Fe0aa5520-2d59-11ea-86da-fb3b00230640%2Fimage.png)

## 예시로 알아보는 HTTP

- **서버** : 어떠한 자료에 대한 접근을 관리하는 네트워크 상의 시스템 **(요청에 대한 응답을 보내준다.)**
- **클라이언트** : 그 자료에 접근할 수 있는 프로그램 Ex) 웹 브라우저, 핸드폰 어플리케이션 등...

클라이언트 프로그램에서 사용자가 회원가입을 시도하게 되면, 서버로 회원정보를 보내게 되고 서버는 회원 정보를 저장해주기도 한다. 이 과정에서 **클라이언트와 서버 간의 교류가 HTTP라는 규약을 이용하여 발생하게 된다.**

# Request (요청)

**클라이언트가 서버에게 연락하는 것**을 요청이라고 하며 요청을 보낼때는 요청에 대한 정보를 담아 서버로 보낸다.

## Request Method (요청의 종류)

- **GET** : 자료를 **요청**할 때 사용
- **POST** : 자료의 **생성**을 요청할 때 사용
- **PUT** : 자료의 **수정**을 요청할 때 사용
- **DELETE** : 자료의 **삭제**를 요청할 때 사용

### 1. 시작줄 (첫 줄)

첫 줄은 시작줄로 **메서드 구조 버전**으로 구성되었다.

- GET : HTTP Method
- [https://www.naver.com](http://www.naver.com) : 사이트 주소
- HTTP/1.1 : HTTP 버전

### 2. 헤더 (두 번째 줄부터)

두번째 줄부터는 헤더이며 **요청에 대한 정보**를 담고 있다. User-Agent, Upgrade-Insecure-Requests 등등이 헤더에 해당되며 헤더의 종류는 매우 많다.

### 3. 본문 (헤더에서 한 줄 띄고)

본문은 **요청을 할 때 함께 보낼 데이터를 담는 부분**이다. 현재 예시에는 단순히 주소로만 요청을 보내고 있고 따로 데이터를 담아 보내지 않기 때문에 본문이 비어있다.

# Response (응답)

**서버가 요청에 대한 답변을 클라이언트에게 보내는 것**을 응답이라고 한다.

## Status Code (상태 코드)

상태 코드에는 굉장히 많은 종류가 있다. 모두 숫자 세 자리로 이루어져 있으며, 아래와 같이 크게 다섯 부류로 나눌 수 있다.

- **1XX (조건부 응답)** : 요청을 받았으며 작업을 계속한다.
- **2XX (성공)** : 클라이언트가 요청한 동작을 수신하여 이해했고 승낙했으며 성공적으로 처리했음을 가리킨다.
- **3XX (리다이렉션 완료)** : 클라이언트는 요청을 마치기 위해 추가 동작을 취해야 한다.
- **4XX (요청 오류)** : 클라이언트에 오류가 있음을 나타낸다.
- **5XX (서버 오류)** : 서버가 유효한 요청을 명백하게 수행하지 못했음을 나타낸다.

## Resonse HTTP 메시지 예시

```
HTTP/1.1 200 OK														// 시작줄
Connection: keep-alive												 // 헤더
Content-Encoding: gzip
Content-Length: 35653
Content-Type: text/html;

<!DOCTYPE html><html lang="ko" data-reactroot=""><head><title...
```

### 1. 시작줄 (첫 줄)

첫 줄은 **버전 상태코드 상태메시지**로 구성되어 있다. 200은 성공적인 요청이었다는 뜻이다.

### 2. 헤더 (두 번째 줄부터)

두 번째 줄부터는 헤더로 **응답에 대한 정보를 담고 있다.**

### 3. 본문 (헤더 뒤부터)

응답에는 대부분의 경우 본문이 있다. 보통 데이터를 요청하고 응답 메시지에는 **요청한 데이터를 담아서 보내주기 때문이다. 응답 메시지에 HTML이 담겨 있는데 이 HTML을 받아 브라우저가 화면에 렌더링한다.**

# HTTP 1.1 vs HTTP 2.0

### HTTP/1.1 특징

- Persitent Connections - HTTP 요청은 TCP 프로토콜을 이용하기 때문에 요청을 전송하기 위해선 3-way-handshake 과정을 거쳐 연결을 설정해야 한다. 하지만 매 요청마다 3-way-handshake를 하는 과정에서 많은 시간이 소요되기 때문에 HTTP 1.1 이전에는 keep-alive 헤더를 이용하여 일정 시간동안 연결을 끊지않고 여러 요청에 재사용하도록하여 TCP handshake하는 비용을 아끼고 성능을 향상시켰다.
    
    하지만 HTTP 1.1 부터는 기본적으로 요청에 대한 연결을 일정시간동안 끊지않도록 되어있기 때문에 keep-alive 헤더를 사용할 필요가 없다.
    
    ![https://velog.velcdn.com/images%2Fhoo00nn%2Fpost%2F38b2c216-7894-4d0d-aa2a-6c0484484a20%2Fimage.png](https://velog.velcdn.com/images%2Fhoo00nn%2Fpost%2F38b2c216-7894-4d0d-aa2a-6c0484484a20%2Fimage.png)
    
- Pipelining - 기본적으로 HTTP 요청은 순차적이기 때문에 요청에 대한 응답을 받고 나서야 새로운 요청을 보낸다. 하지만 HTTP 1.1 부턴 응답을 기다리지 않고 요청을 연속적으로 보내는 기능인 Pipelining 스펙이 추가되었다.
    
    하지만 모든 요청에 Pipelining이 적용되는 것은 아니다. GET, HEAD, DELETE, PUT과 같은 멱등성이 보장되는 요청에만 적용된다.
    
    Pipelining의 문제점도 있다. 응답순서는 보장되어야하기 때문에 앞의 요청을 처리하는데 많은 시간이 소요된다면 뒤의 요청들이 늦게 응답 받게되는 Head Of Line Blocking이 발생한다.
    
    ![https://velog.velcdn.com/images%2Fhoo00nn%2Fpost%2F3f146d00-0664-4fc6-a0b5-45485dfcc189%2Fimage.png](https://velog.velcdn.com/images%2Fhoo00nn%2Fpost%2F3f146d00-0664-4fc6-a0b5-45485dfcc189%2Fimage.png)
    

### HTTP/1.1 느린 이유?

- 연결당 하나의 요청과 응답을 처리하기 때문에 동시 전송 문제와 다수의 리소스를 처리하기에 속도와 성능 문제가 존재
- Head Of Line Blocking (특정 응답 지연) - HTTP/1.1 사양의 제한으로 Request의 순서와 Response의 응답순서는 동기화 되야한다. 그렇기 때문에 특청 요청을 처리하는데 많은 시간이 걸린다면 다른 요청을 처리하는데 지연이 발생한다.
- 헤더가 크다 (특히 쿠키때문에) - 매 요청마다 중복되는 헤더를 보내게 되기 때문에 비효율적이다.

### HTTP/1.1의 속도 문제를 개선하기 위한 노력

- 이미지 스프라이트 - 다수의 리소스 요청을 보내게되면 HOC Blocking이 발생할 수도 있기 때문에 이미지들을 하나로 합쳐서 리소스 요청을 한 번만 보내는 방식
- CSS/JavaScript 압축

### HTTP/2.0

HTTP/2.0은 새로운 기능이 도입되기 보단 HTTP/1.1의 성능을 개선시키는데 많은 노력을 쏟았다. 즉, 성능 향상에 초점을 맞춘 프로토콜이다.

### HTTP/2.0 특징

**Multiplexed Streams**

한 커넥션에서 여러개의 메세지를 주고받을 수 있으며, 응답은 순서에 상관없이 stream으로 주고 받는다. HTTP/1.1의 Connection Keep-Alive, Pipelining의 개선이라고 보면 된다.

아래의 이미지 처럼, 하나의 커넥션에서 여러 병렬 스트림(3개)이 존재 할 수 있다. stream이 뒤섞여서 전송 될 경우, stream number를 이용해 수신측에서 재조합된다.

![https://velog.velcdn.com/images%2Fhoo00nn%2Fpost%2F19c60958-1e6a-4369-a25a-df67631b2573%2Fimage.png](https://velog.velcdn.com/images%2Fhoo00nn%2Fpost%2F19c60958-1e6a-4369-a25a-df67631b2573%2Fimage.png)

**Server Push**

서버는 클라이언트 요청에 대해 요청하지도 않은 리소스를 마음대로 보낼 수 있다.

한마디로 클라이언트(브라우저)가 HTML문서를 요청했고 해당 HTML에 여러개의 리소스(CSS, Image 등) 가 포함되어 있는 경우 HTTP/1.1에서 클라이언트는 요청한 HTML문서를 수신한 후 HTML문서를 해석하면서 필요한 리소스를 재요청한다.

하지만 HTTP/2.0에선 `Server Push` 기법을 통해서 클라이언트가 요청하지도 않은 (HTML문서에 포함된 리소스) 리소스를 Push 해주는 방법으로 클라이언트의 요청을 최소화 해서 성능 향상을 이끌어 낸다.

이를 PUSH_PROMISE 라고 부르며 PUSH_PROMISE를 통해서 서버가 전송한 리소스에 대해선 클라이언트는 요청을 하지 않는다.

![https://velog.velcdn.com/images%2Fhoo00nn%2Fpost%2Fb3401c29-8e1e-438d-9c7d-2fd14c080ad6%2Fimage.png](https://velog.velcdn.com/images%2Fhoo00nn%2Fpost%2Fb3401c29-8e1e-438d-9c7d-2fd14c080ad6%2Fimage.png)

**Header Compression**

Header의 내용과 중복되는 필드를 재전송 하지 않도록 하여, 데이터를 절약한다. 또한 기존에 HTTP Header가 Plain Text(평문)이었지만, HTTP/2에서는 Hash Table과 Huffman Coding을 사용하는 HPACK이라는 Header 압축방식을 이용하여 데이터 전송 효율을 높였다.

![https://velog.velcdn.com/images%2Fhoo00nn%2Fpost%2Fefd4797b-9074-418b-97ed-1fae5a0165da%2Fimage.png](https://velog.velcdn.com/images%2Fhoo00nn%2Fpost%2Fefd4797b-9074-418b-97ed-1fae5a0165da%2Fimage.png)
