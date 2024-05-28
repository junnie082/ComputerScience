# Socket2

## [[소켓과 웹소켓 (2)] 소켓과 웹소켓의 차이점, 웹소켓의 모든 것, http-tcp-소켓의 상관관계](https://velog.io/@rhdmstj17/%EC%86%8C%EC%BC%93%EA%B3%BC-%EC%9B%B9%EC%86%8C%EC%BC%93-%ED%95%9C-%EB%B2%88%EC%97%90-%EC%A0%95%EB%A6%AC-2)

#### 2024.05.28.(화)

## TCP/IP 소켓과 웹소켓의 차이

소켓과 웹소켓은 **_IP와 포트를 통한 통신_**을 한다는 점에서는 비슷하다. 또한, 둘 다 양방향 통신을 한다는 비슷한 특징을 갖고 있기도 하다.

차이점은, **_웹 소켓은 HTTP 레이어에서 작동하는 소켓으로 TCP/IP 소켓의 레이어가 다르다!_**
그러니 두 개념은 엄연히 다르다. 같다고 혼동하지 않아야 한다.

## WebSocket

웹소켓은 http 에서 실시간 통신을 할 수 없다는 문제를 해결하기 위해 나온 기술이다! 웹소켓의 탄생 배경이 되는, http의 특징에 대해 간략히 살펴보자.

### HTTP로는 실시간 통신을 할 수 없어요

웹소켓의 탄생배경을 알기 위해선 http의 특징에 대해 짚고 넘어가야 한다.

- 비연결성 (단방향)
- 매번 연결 **_맺고 끊는_** 과정의 비용
- **_request-response_**의 구조
- 헤더의 비중이 너무 큼 -> 실시간성으로 많은 데이터를 주고 받고자 하는 경우에는 매우 부담이 되는 요소 중 하나

http는 위와 같은 특징을 갖고 있기 때문이다. 즉, 요청을 보내면 응답이 오는 **_단방향적 구조로 통신_**하기 때문에 TCP/IP 프로토콜을 사용하는 소켓처럼 계속 connection이 유지되는 **_실시간 통신을 할 수 없다_**.

이의 문제점을 해결하기 위해 나온 것이 웹소켓 프로토콜이다.

### 웹소켓은 실시간 통신을 할 수 있어요

- 연결지향 (양방향)
- 한 번 **_연결 맺은 뒤 유지_**
- 핸드쉐이크 과정에서는 헤더의 비중이 크지만, 한 번 연결이 되면 간단한 메세지들만 오고 감 -> 굉장히 경제적임

웹소켓은 위와 같은 특징으로 실시간 통신을 할 수 있다. 웹소켓은 http를 대체할 수 있는 개념까진 아니고, 그저 **_웹에서 실시간 통신을 해야하는 상황에서 잘 쓰이는 프로토콜 중 하나_**라고 생각하면 된다. 위에서 언급했지만 이름부터 웹소켓이기에 http 레이어 위에서 작동한다.

### 웹소켓 정확히 언제 쓰지?

당연한 얘기지만 실시간 통신을 해야하는 상황에 쓴다.

case1 | B유저와 게임을 할 때 타유저가 게임 action을 줄 때마다 새로고침을 해야함 -> 매우 불편하다. 즉, 게임을 할 때.

case2 | 채팅을 해야할 때

case3 | 실시간 주식 거래 사이트를 만들 때... 등등

## 웹 소켓 동작 방법

### 1. 일단 손을 맞잡는다. 핸드쉐이킹!

채널에 대한 정상적인 통신이 시작되기 전에 두 개의 실체 간에 확립된 통신 채널의 변수를 동적으로 설정하는 자동화된 협상 과정

```
Websocket API

데이터 요청자 -> 요청 -> 데이터 제공자

=> 채널 형성
```

### 2. 이제 데이터를 넣을 프레임을 구성하자! Frame 구성

최초 접속에서만 http 프로토콜 위에서 핸드쉐이킹을 하기 때문에 http 헤더를 사용한다.
웹소켓을 위한 별도의 포트는 없으며 기존 포트(http-80, https-443)를 사용(호환된다는 의미)
프레임으로 구성된 메세지라는 논리적 단위로 송수신
메세지에 포함될 수 있는 교환 가능한 메세지(frame)는 텍스트와 바이너리 뿐!

## 웹소켓 한계

웹소켓은 html5 이후에 나왔다. html5 이전 기술로 구현된 서비스에서는 어떻게 해야할까?

### 1. Socket.io, SockJS

Socker.io, SockerJS가 html5 이전의 기술로 구현된 서비스에서 웹 소켓처럼 사용할 수 있도록 도와주는 기술임. 이걸로 실시간 통신을 도와준다.

결국 브라우저와 웹서버의 종류와 버전을 파악하여 가장 적합한 기술을 선택해 웹소켓처럼 보여지게 기능을 만든다!

### 2. STOMP

## 웹 소켓 이전의 비슷한 기술

http로도 실시간 통신을 하기 위한 다양한 방법이 존재한다.

### 1. http polling

서버로 일정 주기 요청 송신

cf. polling

프로그램이 충돌 회피 또는 동기화 처리 등을 목적으로 다른 프로그램의 상태를 주기적으로 검사하여 일정한 조건을 만족할 때 송수신 등의 잘 처리를 하는 방식

- real-time 통신에서는 언제 통신이 발생할지 예측이 불가능하다.
- 불필요한 request와 connection을 생성
- 사진을 보면, 이벤트가 없는데도 요청을 하게됨. 그러면 이벤트 발생 '후' 응답을 받을 때까지의 대기 간극이 생김. (-) 불필요한 시간
- real-time 통신이라고 하기 애매할 정도의 실시간성

### 2. http long-polling

서버에 요청을 보내고 이벤트가 생겨 응답 받을 때까지 서버 측에서 연결을 종료하지 않는 것. 응답을 받으면 끊고 재요청.

cf. long-polling

클라이언트가 웹 서버에서 새로운 내용이 있는지 물어보았을 때 웹 서버에서 새로운 이벤트가 없다면 대답해 주지 않다가 새로운 내용이 생기면 이때 대답해 주는 방식

- 불필요한 request를 없앰(polling의 단점 일부 보완)
- 결국 많은 양의 메세지가 쏟아질 경우 polling과 같아짐...

### 3.https streaming

서버에 요청 보내고 끊기지 않은 연결상태에서 끊임없이 데이터를 수신한다.

- 클라이언트에서 서버로의 데이터 송신이 어렵다

### 하지만...

결과적으로 이 모든 방법이 HTTP를 통해 통신하기 때문에 Request, Response 둘다 Header가 불필요하게 큼(빠르게 데이터를 주고 받아야할 때에는, 이게 걸림돌이 될 수 있다는 의미)

근데... http는 왜 단방향 통신일까?

http는 tcp 위에서 만들어진다. 즉 소켓을 양끝단으로 하는 tcp 레이어 위에 존재하는 프로토콜이다. 아무리 'http vs socket'에 관하여 설명하는 글이 많다 할지라도 http가 tcp 위에 만들어졌으니 HTTP 또한 소켓 통신을 활용한 방식이라고 볼 수 있지 않느냐에 대한 물음에 대한 답은 '그렇다'일 것이다.

하지만 통신에는 계층이 있다. http와 소켓은 다른 프로토콜을 가지고 있고, 엄연히 다른 계층에서 동작한다.
일반적으로 사용하는 소켓통신(tcp통신)은 4계층에서 시작하여 패킷이 생성되고, http 통신의 경우 7 계층에서부터 시작하여 패킷이 생성된다.

따라서 **_4계층 관점으로 본다면 이 둘은 연결지향적 통신이 되는 것이고, 7계층 관점에서 본다면 http는 비연결, socket 통신은 연결지향적이 되는 것_**이다.

[채팅 서버 만들기 - WebSocket](https://velog.io/@rbdus96/WebSocket%EC%9D%84-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EC%B1%84%ED%8C%85%EC%84%9C%EB%B2%84-%EB%A7%8C%EB%93%A4%EA%B8%B0-%EA%B8%B0%EB%B3%B8%EA%B5%AC%EC%A1%B0)

## WebSocket이란?

```
HTTP 환경에서 클라이언트와 서버 사이에 하나의 TCP 연결을 통해 실시간으로 전이중 통신을 가능하게 하는 프로토콜이다. 여기서 전이중 통신이란, 일방적인 송신 또는 수신만이 가능한 단방향 통신과는 달리 가정에서의 전화와 같이 양방향으로 송신과 수신이 가능한 것을 말한다. 클라이언트가 접속 요청을 하고 웹서버가 응답한 후 연결을 끊는 것이 아니라 연결을 그대로 유지하고 클라이언트의 요청 없이도 데이터를 전송할 수 있는 프로토콜이다.
```

### WebSocket의 특징

```
1. 양방향 통신 가능 (Full-Duplex)
   http 통신은 Client가 요청을 보내는 경우에만 Server가 응답하는 단방향 통신이다. 하지만 웹소켓을 사용하면 데이터 송수신을 동시에 처리할 수 있다.

2. 실시간 네트워킹 가능 (Real Time Networking)
   연속된 데이터를 빠르게 노출할 수 있다.
```

### WebSocket이 나오기 전 통신 방식

```
1. Polling
   Client가 평범한 HTTP Request를 서버로 계속 텍스트 요청해 이벤트 내용을 전달받는 방식.
   setTimeout, setInterval 등으로 일정 주기마다 서버에 request를 보내면 된다.
```

<단점>

(1) 불필요한 Request와 Connection을 생성하여 서버에 부담을 주게 된다.
(2) 요청 주기가 짧을 수록 부하가 커진다.
(3) 일정 주기마다 요청을 보내는 것이기 때문에 실시간이라고 보기에 애매하다.
(4) HTTP 통신을 하기 때문에 Request, Response 헤더가 불필요하게 크다.

<Polling을 사용하는 경우>

(1) 응답을 실시간으로 받지 않아도 되는 경우
(2) 다수의 사용자가 동시에 사용하는 경우

```
2. Long Polling
polling과 비슷하게 일정 주기마다 요청을 보내지만 서버가 응답을 바로 전달하지 않는 방식, 즉 요청을 보냈을 때 서버가 응답을 바로 보내지 않고 특정 이벤트나 타임아웃이 발생했을 때 응답을 전달하는 방식
```

<단점>

(1) 불필요한 요청을 보내지 않아 Polling 보다 좋아보이지만, Long Polling도 동시 다발적인 요청과 응답이 생기면 부하가 발생할 수 있다.
(2) HTTP 통신을 하기 때문에 Request, Response 헤더가 불필요하게 크다.

<Long Polling을 사용하는 경우>

(1) 응답을 실시간으로 받아야 하는 경우
(2) 적은 수의 사용자가 동시에 사용하는 경우

```
3. Streaming

이벤트가 발생했을 때 응답을 내려주되, 응답을 완료시키지 않고 계속 연결을 유지하는 방식
```

<단점>

(1) Long Polling에 비해 응답마다 다시 요청을 하지 않아도 되므로 효율적이지만, 연결 시간이 길어질수록 연결 유효성 관리의 부담이 발생한다.
(2) HTTP 통신을 하기 때문에 Request, Response 헤더가 불필요하게 크다.

```
WebSocket 동작 과정은 크게 세가지로 나눌 수 있다.
Opening Handshake, Data transfer, Closing Handshake
```

```
1. Handshake

Opening Handshake와 Closing Handshake는 일반적인 HTTP TCP 통신의 과정 중 하나이다.
접속 요청은 HTTP로 한 뒤, 웹소켓 프로토콜로 변경된다. (WS) 웹소켓 프로토콜로 변경되기 위한 HTTP 헤더는 아래와 같이 구성되어 있다.
```

#### Request Header

```
GET /chat HTTP/1.1  // 웹소켓 통신 요청에서 HTTP 버전은 1.1 이상이어야 하고 GET 메서드를 사용해야 한다.
Host: localhost:8080 // 웹소켓 서버 주소
Upgrade: websocket // 프로토콜을 전환하기 위해 사용하는 헤더. websocket으로 전환
Connection: Upgrade // 현재의 전송이 완료된 후 접속을 유지할 것인가에 대한 정보
Sec-WebSocket-Key: PxYc0tlXHXDojXci2qkTIQ== // 유효한 요청인지 확인하기 위해 사용하는 키 값. 길이가 16Byte인 임의로 선택된 숫자를 base64 인코딩한 값이,다.
Sec-WebSocket-Protocol: chat, superchat // 사용하고자 하는 하나 이상의 웹소켓, 즉 하위 프로토콜 지정
Sec-WebSocket-Version: 13 // 클라이언트가 사용하고자 하는 웹소켓 프로토콜 버전. 현재 최신 버전 13
Origin: http://localhost:8080  // 클라이트의 주소
```

#### Response Header

```
HTTP/1.1 101 Switching Protocols // 101은 HTTP에서 WS로 프로토콜 전환이 승인 되었다는 응답코드이다.
Upgrade: websocket
Connection: Upgrade
Sec-Websocket-Accept: GPxAkIJNOSs6TNM2pBI8qXYiWZ0= // 웹소켓 연결이 개시되었음을 알린다. 서로 간의 신원 인증키가 클라이언트에서 계산한 값과 일치하지 않으면 연결 수립이 안된다.
Sec-WebSocket-Protocol: chat
```

```
2. Data Transfer
Opening HandShake에서 승인이 나고 나면, 웹소켓 프로토콜로 Data Transfer이 진행된다. 여기서 데이터는 메세지라는 단위로 전달된다.
```

<Message>

여러 frame이 모여서 구성하는 하나의 논리적 메세지 단위

<frame>

communication에서 가장 작은 단위의 데이터. 작은 header와 payload로 구성되어 있다.

### WebSocket과 HTTP의 차이

```
HTTP는 클라이언트인 웹 브라우저와 웹 서버 간 소통하기 위한 프로토콜이다. 예를 들어 클라이언트가 HTTP를 통해 특정 페이지, 정보를 요청하게 되면 서버는 요청에 응답하여 필요한 정보를 클라이언트에게 해당 정보를 전달하게 된다. 위의 그림에서도 알 수 있듯이 Response가 있기 전에 무조건 Request가 있어야 한다.
```

```
WebSocket은 하나의 TCP 접속에 전이중 통신 채널을 제공하는 컴퓨터 통신 프로토콜이다. 이를 사용함으로써 서버와 사용자 간의 실시간 데이터 전송을 용이하게 한다. 예를 들어 코인 거래와 같은 트레이딩 시스템, SNS 애플리케이션 등이 대표적인 예이다.
```

즉, WebSocket과 HTTP의 차이는 양방향 통신인지, 단방향 통신인지에 대한 차이가 되겠다.
HTTP는 클라이언트가 요청을 보내고 서버가 응답하는 단방향 통신으로 연결상태가 유지되지 않는다(stateless). 하지만 WebSocket은 서버 역시 클라이언트한테 요청을 보낼 수 있는 양방향 통신으로 연결상태가 유지된다(stateful).

## WebSocket을 이용하여 채팅서버 만들어보기

### 1. 라이브러리 추가

필자는 Maven을 사용하여 구현하였기 때문에 pom.xml에 websocket을 dependency를 추가해 주었다.

```
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-websocket</artifactId>
    <version>${org.springframework-version}</version>
</dependency>
```

### 2. WebSocket Handler 작성

서버는 다수의 클라이언트가 보낸 메세지를 처리할 Handler가 필요하다. 따라서 텍스트 기반의 채팅을 구현해볼 것이므로 "TextWebSocketHandler"를 상속받아서 작성했다.

```Java
package site.workforus.forus.chat.websocket;

import lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Component;
import org.springframework.web.socket.CloseStatus;
import org.springframework.web.socket.TextMessage;
import org.springframework.web.socket.WebSocketSession;
import org.springframework.web.socket.handler.TextWebSocketHandler;

import java.util.ArrayList;
import java.util.List;

@Component
@Slf4j
public class ChatHandler extends TextWebSocketHandler {
    private List<WebSocketSession> sessionList = new ArrayList<WebSocketSession>();

    @Override
    public void afterConnectionEstablished(WebSocketSession session) throws Exception {
        log.info("#ChattingHandler, afterConnectionEstablished");
        sessionList.add(session);

        log.info(session.getPrincipal().getName() + "님이 입장하셨습니다.");
    }

    @Override
    protected void handleTextMessage(WebSocketSession session, TextMessage message) throws Exception {
        log.info("#ChattingHandler, handleMessage");

        log.info(session.getId() + ": " + message);

        String payload = message.getPayload();
        log.info("payload : " + payload);

        log.info("session : " + session);
        log.info("message : " + message);

        for (WebSocketSession s : sessionList) {
            s.sendMessage(new TextMessage(session.getPrincipal().getName() + ":" + message.getPayload()));
        }
    }

    @Override
    public void afterConnectionClosed(WebSocketSession session, CloseStatus status) throws Exception {
        log.info("status: " + status);
        log.info("session: " + session);

        sessionList.remove(session);

        log.info(session.getPrincipal().getName() + "님이 퇴장하셨습니다.");
    }
}
```

### 3. WebSocket Config 작성

@EnableWebSocket 어노테이션을 사용해 WebSocket을 활성화하였다. WebSocket에 접속하기 위한 Endpoint는 /chat로 설정하였고, setAllowedOrigins("\*")을 추가해주어 도메인이 다른 서버에서도 접속 가능하도록 해준다.

```Java
package site.workforus.forus.chat.config;

import lombok.RequiredArgsConstructor;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.socket.config.annotation.EnableWebSocket;
import org.springframework.web.socket.config.annotation.WebSocketConfigurer;
import org.springframework.web.socket.config.annotation.WebSocketHandlerRegistry;
import site.workforus.forus.chat.websocket.ChatHandler;

@Configuration
@RequiredArgsConstructor
@EnableWebSocket

public class WebSocketConfig implements WebSocketConfigurer {
    private final ChatHandler chatHandler;

    @Override
    public void registerWebSocketHandlers(WebSocketHandlerRegistry registry) {
        registry.addHandler(chatHandler, "ws/chat").setAllowedOrigins("*");
    }
}
```

> > Endpoint?

```
Endpoint란 API가 서버에서 자원에 접근할 수 있도록 하는 URL이다.
```

### 4. ChatController 작성

필자는 Spring Security를 사용했기 때문에 아래와 같은 방식으로 user 정보를 가져왔다.

```Java
package site.workforus.forus.chat.controller;

import lombok.extern.slf4j.Slf4j;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotatin.RequestMapping;
import site.workforus.forus.employee.model.LoginVO;

@Controller
@Slf4j
@RequestMapping(value="/chat")
public class ChatController {

    @GetMapping("")
    public String chat(Model model) {
        LoginVO user = (LoginVO) SecurityContextHolder.getContext().getAuthentication().getPrincipal();
        log.info("@ChatController, chat GET()");

        model.addAttribute("userid", user.getUsername());

        return "chat/chat";
    }
}
```

### 5. chat.jsp 작성

```html
<div class="chat-center-layout">
  <section class="section">
    <div class="card">
      <div class="card-header">
        <div class="media d-flex align-items-center">
          <div class="avatar me-3">
            <img src="static/images/faces/1.jpg" alt="" srcset="" />
            <span class="avatar-status bg-success"></span>
          </div>
          <div class="name flex-grow-1">
            <h6 class="mb-0">Fred</h6>
            <span class="text-xs">Online</span>
          </div>
          <button class="btn btn-sm">
            <i data-feather="x"></i>
          </button>
        </div>
      </div>
      <div class="card-body pt-4 bg-grey" id="id_chat">
        <div id="chat-content"></div>
      </div>
      <div class="card-footer">
        <div
          class="message-form d-flex flex-direction-column align-items-center"
        >
          <a href="http://" class="black"><i data-feather="smile"></i></a>
          <div class="d-flex flex-grow-1 ml-4">
            <input
              type="text"
              class="form-control"
              id="msg"
              name="context"
              placeholder="Type your message.."
            />
            <button type="submit" class="submit-btn" id="button-send">
              전송
            </button>
          </div>
        </div>
      </div>
    </div>
  </section>
</div>
```

### 6. chat.js 작성

```javascript
<script type="text/javascript">
	$("#button-send").on("click", function(e) {
		sendMessage();
		$('#msg').val('');
	});

	var ws = new WebSocket("ws://localhost:8080/ws/chat");

	ws.onmessage = onMessage;
	ws.onopen = onOpen;
	ws.onclose = onClose;

	function sendMessage() {
		ws.send($("#msg").val());
	}

	function onMessage(msg) {
		var data = msg.data;
		console.log(data);
		var sessionId = null;
		var message = null;

		var arr = data.split(":");

		for(var i = 0; i < arr.length; i++) {
			console.log('arr[' + i + ']: ' + arr[i]);
		}

		var cur_session = '${userid}';
		console.log("cur_session : " + cur_session);

		sessionId = arr[0];
		message = arr[1];

		console.log("sessionID : " + sessionId);
		console.log("cur_session : " + cur_session);

		if(sessionId == cur_session) {
			var str = "<div class='chat'><div class='chat-body'><div class='chat-message' id='id_chat'>";
			str += sessionId + " : " + message;
			str += "</div></div></div>";
			$("#chat-content").append(str);
		} else {
			var str = "<div class='chat chat-left'><div class='chat'><div class='chat-body'><div class='chat-message' id='id_chat'>";
			str += sessionId + " : " + message;
			str += "</div></div></div></div>";
			$("#chat-content").append(str);
		}
	}
    function onClose(evt) {
		var str = '${userid}' + " 님이 방을 나가셨습니다.";
		$("#chat-content").append(str);
	}

	function onOpen(evt) {
		var str = '${userid}' + " 님이 입장하셨습니다.";
		$("#chat-content").append(str);
	}
</script>

```
