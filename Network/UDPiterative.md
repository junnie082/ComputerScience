# UDP 기반 서버/클라이언트

## [[Socket 프로그래밍] UDP 기반 서버/클라이언트](https://andjjip.tistory.com/283)

#### 2024.05.29.(수)

## UDP 기반 서버/클라이언트

### UDP 소켓의 특성

UDP를 편지로 예시를 들어 동작원리를 설명하고자 한다.
편지를 보내기 위해서는 편지봉투에다가 보내는 사람과 받는 사람의 주소 정보를 써야 한다. 그리고 우표를 붙여서 우체통에 넣으면 끝이다. 대신 편지의 특성상 편지를 보내고 나서 상대방의 수신여부를 확인할 방법은 없다.

물론 전송 도중에 편지가 분실될 확률도 존재한다. 즉, 편지는 신뢰할 수 없는 전송방법이라고 할 수 있다. 이와 마찬가지로 UDP 소켓은 신뢰할 수 없는 전송방법을 제공한다.

대용량 데이터를 보낼 때는 UDP를 사용한다.
UDP는 TCP보다 속도가 빠르지만 안정성이 조금 떨어진다.
TCP는 에러가 있으면 안되는 문서 전송(데이터 주고받을 때)
UDP는 데이터가 분실되더라도 우리가 취급하는데 이상이 없는 경우(영상 같은 거)

### UDP의 내부 동작 원리

UDP는 TCP와 달리 흐름제어를 하지 않는다. 그럼 이번에는 UDP의 역할이 어디까지인지 구체적으로 언급해보겠다.

위 그림에서 보이듯이 호스트 B를 떠난 UDP 패킷이 호스트 A에게 전달되도록 하는 것은 IP의 역할이다. 그런데 이렇게 전달된 UDP 패킷을 호스트 A 내에 존재하는 UDP 소켓 중 하나에게 최종 전달하는 것은 IP의 역할이 아니다. 이는 바로 UDP의 역할이다. 즉, UDP의 역할 중 가장 중요한 것은 호스트로 수신된 패킷을 PORT 정보를 참조하여 최종 목적지인 UDP 소켓에 전달하는 것이다.

### UDP의 효율적 사용

네트워크 프로그래밍의 대부분이 TCP를 기반으로 구현될 것 같지만, UDP를 기반으로 구현되는 경우도 흔히 볼 수 있다.
그럼 언제 UDP를 사용하는 것이 효율적인지 생각해보자.

만약 1만 개의 패킷을 보냈는데 그 중 1개만 손실 되어도 문제가 발생하는 압축파일의 경우에는 반드시 TCP를 기반으로 송수신이 이루어져야할 것이다. 왜냐하면 압축 파일은 그 특성상 파일의 일부만 손실되어도 압축의 해제가 어렵기 때문이다.

그러나 인터넷 기반으로 실시간 영상 및 음성을 전송하는 경우에는 얘기가 조금 다르다. 멀티미디어 데이터는 그 특성상 일부가 손실되어도 크게 문제되지 않는다. 잠깐의 화면 떨림, 또는 아주 작은 잡음 정도는 그냥 넘어갈 만하다. 하지만 실시간으로 서비스를 해야하므로 속도가 중요한 요소가 된다. 때문에 저번 포스팅에서 보인 흐름제어의 과정이 귀찮게 느껴질 수 있다.

이러한 경우가 UDP 기반의 구현을 고려할만한 상황이다. 그러나 UDP가 TCP에 비해서 언제나 빠른 속도를 내는 것은 아니다. TCP가 UDP에 비해 느린 이유 2가지만 말해 달라고 하면 다음 2가지를 들 수 있다.

- 데이터 송수신 이전, 이후에 거치는 연결 설정 및 해제 과정
- 데이터 송수진 과정에서 거치는 신뢰성보장을 위한 흐름제어

따라서 송수신하는 데이터의 양은 작으면서 잦은 연결이 필요한 경우에는 UDP가 TCP보다 훨씬 효율적이고 빠르게 동작한다.

## UDP 기반 서버 / 클라이언트의 구현

이론만 언급했던 UDP를 구현 관점에서 살펴보자. 그런데 UDP는 앞서 설명한 내용만 이해하면 구현은 그리 문제는 없다.

### UDP에서의 서버와 클라이언트는 연결되어 있지 않다.

UDP 서버, 클라이언트는 TCP와 같이 연결된 상태로 데이터를 송수신하지 않는다. 때문에 TCP와 달리 연결 설정의 과정이 필요 없다. 따라서 TCP 서버 구현 과정에서 거쳤던 listen 함수와 accept 함수의 호출은 불필요하다.
UDP 소켓의 생성과 데이터의 송수신 과정만 존재할 뿐이다.

### UDP에서는 서버건 클라이언트건 하나의 소켓만 있으면 된다.

TCP에서는 소켓과 소켓의 관계가 1:1 이었다. 때문에 서버에서 열 개의 클라이언트에게 서비스를 제공하려면 문지기 역할을 하는 서버 소켓을 제외하고도 열 개의 소켓이 더 필요했다. 그러나 UDP는 서버건 클라이언트건 하나의 소켓만 있으면 된다. 앞서 UDP를 말할 때 편지를 예시로 들었는데, 편지를 주고받기 위해 필요한 우체통을 UDP 소켓에 비유할 수 있다. 우체통이 근처에 하나 있다면, 이를 이용해서 어디건 편지를 보낼 수 있을 것이다.
마찬가지로 UDP 소켓이 하나 있다면 어디건 데이터를 전송할 수 있다.

UDP 소켓은 하나만 있으면 둘 이상의 호스트와의 통신이 가능하다.

### UDP 기반의 데이터 입출력 함수

TCP 소켓을 생성하고 나서 데이터를 전송하는 경우, 주소 정보를 따로 추가하는 과정이 필요 없다. 왜냐하면 TCP 소켓은 목적지에 해당하는 소켓과 연결된 상태이기 때문이다. 즉, TCP 소켓은 목적지의 주소 정보를 이미 알고 있는 상태이다.

그러나 UDP 소켓은 연결 상태를 유지하지 않으므로 (UDP 소켓은 단순히 우체통 역할만 한다). 데이터를 전송할 때마다 반드시 목적지의 주소 정보를 별도로 추가해야 한다.

이는 우체통에 넣을 우편물을 주소정보를 써 넣는 것에 비유할 수 있다. 그럼 여기 주소 정보를 써 넣으면서 데이터를 전송할 때 호출하는 UDP 관련 함수를 알아보자.

```C
#include <sys/socket.h>
ssize_t sendto(int sock, void *buff, size_t nbytes, int flags, struct sockaddr *to, socklen_t addrlen);

// 성공시 전송된 바이트 수, 실패시 -1 반환
```

- sock: 데이터 전송에 사용될 UDP 소켓의 파일 디스크립터를 인자로 전달
- buff: 전송할 데이터를 저장하고 있는 버퍼의 주소 값 전달
- nbytes: 전송할 데이터 크기를 바이트 단위로 전달
- flags: 옵션 지정에 사용되는 매개변수, 지정할 옵션이 없다면 0 전달
- to: 목적지 주소 정보를 담고 있는 sockaddr 구조체 변수의 주소 값 전달
- addrlen: 매개변수 to로 전달된 주소 값의 구조체 변수 크기 전달

sendto 함수는 TCP 기반의 출력함수와 비교되는 것이 목적지 주소정보를 요구하고 있다는 점에서 차이가 있다.

이어서 UDP 데이터 수신에 사용되는 함수를 알아보자.
UDP 데이터는 발신지가 일정하지 않기 때문에 발신지 정보를 얻을 수 있도록 함수가 정의되어 있다. 즉, 이 함수는 UDP 패킷에 담겨 있는 발신지 정보를 함께 반환한다.

```C
#include <sys/socket.h>

ssize_t recvfrom(int sock, void *buff, size_t nbytes, int flags, struct sockaddr *from, socklen_t *addrlen);

// 성공 시 수신한 바이트 수, 실패 시 -1 반환
```

- sock: 데이터 수신에 사용될 UDP 소켓의 파일 디스크립터를 인자로 전달
- buff: 데이터 수신에 사용될 버퍼의 주소 값 전달
- nbytes: 수신할 최대 바이트 수 전달, 때문에 매개변수 buff가 가리키는 버퍼의 크기를 넘을 수 없다.
- flags: 옵션 지정에 사용되는 매개변수, 지정할 옵션이 없다면 0 전달
- from: 발신지 정보를 채워 넣을 sockaddr 구조체 변수의 주소 값 전달
- addrlen: 매개변수 from으로 전달된 주소에 해당하는 구조체 변수의 크기 정보를 담고 있는 변수의 주소값 전달

UDP 프로그램 작성의 핵심은 지금 언급한 sendto, recvfrom 함수이다.
데이터 송수신에서 이 두 함수만 알아도 될 정도로 차지하는 위치가 매우 크다고 볼 수 있다.

### UDP 기반의 에코 서버와 에코 클라이언트

방금 설명한 함수를 종합해서 에코 서버를 구현해보자.
참고로 UDP에서는 TCP와 달리 연결요청과 그에 따른 수락의 과정이 존재하기 때문에 서버 및 클라이언트라는 표현이 적절치 않은 부분도 있다. 다만 서비스를 제공한다는 측면에서 서버라 하는 것이니 이 부분에 오해 없도록 하자.

```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <sys/socket.h>

#define BUF_SIZE 30
void error_handling(char* message);

int main(int argc, char* argv[])
{
    int serv_sock;
    char message[BUF_SIZE];
    int str_len;
    socklen_t clnt_adr_sz;

    struct sockaddr_in serv_adr, clnt_adr;
    if (argc != 2)
    {
        printf("Usage: %s <port> \n", argv[0]);
        exit(1);
    }

    serv_sock = socket(PF_INET, SOCK_DGRAM, 0);
    if (serv_sock == -1)
        error_handling("UDP socket creation error");

    memset(&serv_adr, 0, sizeof(serv_adr));
    serv_adr.sin_family = AF_INET;
    serv_adr.sin_addr.s_addr = htonl(INADDR_ANY);
    serv_adr.sin_port = htons(atoi(argv[1]));

    if (bind(serv_sock, (struct sockaddr*)&serv_adr, sizeof(serv_adr)) == -1)
        error_handling("bind() error");

    while (1) {
        clnt_adr_sz = sizeof(clnt_adr);
        str_len = recvfrom(serv_sock, message, BUF_SIZE, 0,
        (struct sockaddr*)&clnt_adr, &clnt_adr_sz);

        sendto(serv_sock, message, str_len, 0,
        (struct sockaddr*)&clnt_adr, clnt_adr_sz);
    }
    close(serv_sock);
    return 0;
}

void error_handling(char* message)
{
    fputs(message, stderr);
    fputc('\n', stderr);
    exit(1);
}
```

위 예제에서 주목해야할 부분은 다음과 같다.

```C
serv_sock = socket(PF_INET, SOCK_DGRAM, 0);
```

UDP 소켓의 생성을 위해 socket 함수의 두 번째 인자로 SOCK_DGRAM을 전달한다.

```C
str_len = recvfrom(serv_sock, message, BUF_SIZE, 0,
        (struct sockaddr*)&clnt_adr, &clnt_adr_sz);
```

bind를 통해 할당된 주소로 전달되는 모든 데이터를 수신한다. 물론 데이터의 전달 대상에는 제한이 없다.

```C
sendto(serv_sock, message, str_len, 0,
        (struct sockaddr*)&clnt_adr, clnt_adr_sz);
```

위의 str_len 함수 호출을 통해 데이터를 전송한 이의 주소 정보도 함께 얻게 되는데, 바로 이 주소 정보를 이용해서 수신된 데이터를 역으로 재전송하고 있다.

```C
close(serv_sock);
```

while문이 무한 루프문이고, 이 루프를 빠져나가기 위한 break 문이 삽입되지 않았기 때문에 닫는 이 문장은 실행되지 않는다. 따라서 큰 의미는 없다고 볼 수 있다.

이어서 위의 서버와 함께 동작하는 클라이언트를 작성해보자. 이 코드에는 TCP 클라이언트와 달리 connect 함수의 호출이 존재하지 않음에 주목해보자.

```C
// uecho_client.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <sys/socket.h>

#define BUF_SIZE 30
void error_handling(char* message);
int main(int argc, char* argv[])
{
    int sock;
    char message[BUF_SIZE];
    int str_len;
    socklen_t adr_sz;

    struct sockaddr_in serv_adr, from_adr;

    if (argc != 3) {
        printf("Usage: %s <IP> <port>\n", argv[0]);
        exit(1);
    }

    sock = socket(PF_INET, SOCK_DGRAM, 0);
    if (sock == -1)
        error_handling("socket() error");

    memset(&serv_adr, 0, sizeof(serv_adr));
    serv_adr.sin_family = AF_INET;
    serv_adr.sin_addr.s_addr = inet_addr(argv[1]);
    serv_adr.sin_port = htons(atoi(argv[2]));

    while (1)
    {
        fputs("Insert message(q to quit): ", stdout);
        fgets(message, sizeof(message), stdin);
        if (!strcmp(message, "q\n") || !strcmp(message, "Q\n"))
            break;
        sendto(sock, message, strlen(message), 0,
                (struct sockaddr*)&serv_adr, sizeof(serv_adr));
        adr_sz = sizeof(from_adr);
        str_len = recvfrom(sock, message, BUF_SIZE, 0,
                (struct sockaddr*)&from_adr, &adr_sz);
        message[str_len] = 0;
        printf("Message from server: %s", message);
    }
    close(sock);
    return 0;
}

void error_handling(char* message)
{
    fputs(message, stderr);
    fputc('\n', stderr);
    exit(1);
}
```

모든 소켓에는 IP와 PORT가 할당되어야 한다. 다만 직접 할당하느냐, 자동으로 할당되느냐의 문제만 있을 뿐이다.
우리 나름대로 UDP 소켓에 IP와 PORT 정보가 할당되는 시기를 유추해볼 수도 있을 것이다.

실행과정에서 누가 먼저 실행되느냐는 중요하지 않지만, sendto 함수 호출 이전에 sendto 함수의 목적지에 해당하는 프로그램이 실행되어 있기만 하면 된다.

### UDP 클라이언트 소켓의 주소 정보 할당

UDP 기반의 서버, 클라이언트 프로그램 구현에 대해 설명하였다. 그런데 UDP 클라이언트 프로그램을 보면 IP와 PORT를 소켓에 할당하는 부분이 눈에 띄지 않는다. TCP 클라이언트의 경우에는 connect 함수 호출시에 자동으로 할당되었는데, UDP 클라이언트의 경우 그러한 기능을 대신할만한 함수 호출문조차 보이지 않는다.

UDP 프로그램에서는 데이터를 전송하는 sendto 함수 호출 이전에 해당 소켓에 주소정보가 할당되어 있어야 한다.
따라서 sendto 함수 호출 이전에 bind 함수를 호출해서 주소정보를 할당해야 한다. 물론 bind 함수는 TCP 프로그램의 구현에서 호출된 함수이다.
그러나 이 함수는 TCP와 UDP를 가리지 않으므로 UDP 프로그램에서도 호출 가능하다. 그리고 만약 sendto 함수 호출시까지 주소정보가 할당되지 않았다면 sendto 함수가 처음 호출되는 시점에 해당 소켓에 IP와 PORT 번호가 자동으로 할당된다.

또한 이렇게 한번 할당되면 프로그램이 종료될 때까지 주소정보가 그대로 유지되기 때문에 다른 UDP 소켓과 데이터를 주고받을 수 있다. 물론 IP는 호스트 IP로, PORT는 사용하지 않는 PORT 번호 하나를 임의로 골라서 할당하게 된다.

이렇듯 sendto 함수 호출 시 IP와 PORT 번호가 자동으로 할당되기 때문에 일반적으로 UDP의 클라이언트 프로그램에서는 주소 정보를 할당하는 별도의 과정이 불필요하다. 그래서 앞서 보인 예제에서도 별도의 주소 정보 할당 과정을 생략하였으며, 이것이 일반적인 구현방법이다.

### UDP의 데이터 송수신 특성과 UDP에서의 connect 함수 호출

TCP 기반에서 송수신하는 데이터는 경계가 존재하지 않지만, UDP 기반에서는 송수신하는 데이터에 경계가 존재함을 확인해보자.

### 데이터의 경계가 존재하는 UDP 소켓

TCP 기반에서 송수신하는 데이터에는 경계가 존재하지 않는다고 하였는데, 이는 다음의 의미를 가진다.

"데이터 송수신 과정에서 호출하는 입출력함수의 호출횟수는 큰 의미를 지니지 않는다."

반대로 UDP는 데이터의 경계는 존재하는 프로토콜이므로, 데이터 송수신 과정에서 호출하는 입출력함수의 호출횟수가 큰 의미를 지닌다. 때문에 입력 함수의 호출횟수와 출렴함수의 호출횟수가 완벽히 일치해야 송신된 데이터를 전부 수신할 수 있다. 예를 들어 세번의 출력함수를 통해 전송된 데이터는 반드시 세번의 입력함수 호출이 있어야 데이터 전부를 수신할 수 있다.

```C
// bount_host1.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <sys/socket.h>
#define BUF_SIZE 30
void error_handling(char *message);

int main(int argc, char *argv[])
{
    int sock;
    char message[BUF_SIZE];
    struct sockaddr_in my_adr, your_adr;
    socklen_t adr_sz;
    int str_len, i;

    if (argc != 2)
    {
        printf("Usage : %s <port>\n", argv[0]);
        exit(1);
    }

    sock = socket(PF_INET, SOCK_DGRAM, 0);
    if (sock == -1)
        error_handling("socket() error");

    memset(&my_adr, 0, sizeof(my_adr));
    my_adr.sin_family = AF_INET;
    my_adr.sin_addr.s_addr = htonl(INADDR_ANY);
    my_adr.sin_port = htons(atoi(argv[1]));

    if (bind(sock, (struct sockaddr*)&my_adr, sizeof(my_adr)) == -1)
        error_handling("bind() error");

    for (i = 0; i < 3; i++)
    {
        sleep(5); // 5초 딜레이
        adr_sz = sizeof(your_adr);
        str_len = recvfrom(sock, message, BUF_SIZE, 0,
                (struct sockaddr*)&your_adr, &adr_sz);

        printf("Message %d : %s\n", i+1, message);
    }
    clock(sock);
    return 0;
}

void error_handling(char* message)
{
    fputs(message, stderr);
    fputc('\n', stderr);
    exit(1);
}
```

위 예제에서 눈여겨볼 부분은 for문이다. 우선 sleep 함수를 호출하는데, 인자로 전달된 시간만큼(초단위) 프로그램을 멈추는 기능을 제공한다. 즉, for문 안에서는 5초 간격으로 recvfrom 함수를 호출하고 있다.

```C
for (i = 0; i < 3; i++)
{
    sleep(5); // 5초 딜레이
    adr_sz = sizeof(your_adr);
    str_len = recvfrom(sock, message, BUF_SIZE, 0,
            (struct sockaddr*)&your_adr, &adr_sz);

    printf("Message %d : %s\n", i+1, message);
}
```

그리고 이 함수가 몇번 호출되었는지 확인하기 위한 문장이 삽입되었다. 프로그램을 지연시키는 이유는 나중에 알아보도록 하자.

이어서 작성할 예제는 bound_host1.c에 데이터를 전송하는 예제이다.
이 예제에서는 sendto 함수호출을 통해 문자열 데이터를 전송한다.

```C
// bound.host2.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <sys/socket.h>
#define BUF_SIZE 30
void error_handling(char* message);

int main(int argc, char* argv[])
{
    int sock;
    char msg1[] = "Hi!";
    char msg2[] = "I'm another UDP host!";
    char msg3[] = "Nice to meet you";

    struct sockaddr_in your_adr;
    socklen_t your_adr_sz;
    if (argc != 3)
    {
        printf("Usage : %s <IP> <Port> \n", argv[0]);
        exit(1);
    }

    sock = socket(PF_INET, SOCK_DGRAM, 0);
    if (sock == -1)
        error_handling("sock() error");

    memset(&your_adr, 0, sizeof(your_adr));
    your_adr.sin_family = AF_INET;
    your_adr.sin_addr.s_addr = inet_addr(argv[1]);
    your_adr.sin_port = htons(atoi(argv[2]));

    sendto(sock, msg1, sizeof(msg1), 0,
            (struct sockaddr*)&your_adr, sizeof(your_adr));
    sendto(sock, msg2, sizeof(msg2), 0,
            (struct sockaddr*)&your_adr, sizeof(your_adr));
    sendto(sock, msg3, sizeof(msg3), 0,
            (struct sockaddr*)&your_adr, sizeof(your_adr));

    close(sock);
    return 0;
}

void error_handling(char* message)
{
    fputs(message, stderr);
    fputc('\n', stderr);
    exit(1);
}
```

bound_host2.c는 총 3회의 sendto 함수 호출을 통해 데이터를 전송하고, bound_host1.c는 총 3회의 recvfrom 함수 호출을 통해서 데이터를 수신한다.
그런데 recvfrom 함수호출에는 5초간의 지연시간이 존재하기 때문에 recvfrom 함수가 호출되기 이전에 3회의 sendto 함수 호출이 진행되어서 데이터는 이미 bound_host1.c에 전송된 상태에 놓이게 된다.

TCP라면 이 상황에서 단 한번의 입력함수 호출을 통해 모든 데이터를 읽어 들일 수 있다.

그러나 UDP는 다르다. 이 상황에서도 3회의 recvfrom 함수 호출이 요구된다.

결국 bound_host2.c는 총 3회의 sendto 함수호출을 통해서 데이터를 전송하고, bount_host1.c는 총 3회의 recvfrom 함수 호출을 통해 데이터를 수신한다. 그런데 recvfrom 함수호출에는 5초간의 지연 시간이 존재하기 때문에, recvfrom 함수가 호출되기 이전에 3회의 sendto 함수 호출이 진행되어서, 데이터는 이미 bount_host1.c에 전송된 상태에 놓이게 된다.

TCP라면 이 상황에서 단 한번의 입력함수 호출을 통해서 bount_host1.c에 전송된 상태에 놓이게 된다.
그러나 UDP는 다르다. 이 상황에서도 3회의 recvfrom 함수호출이 요구된다.

위의 실행결과 중 bound.host1.c 실행결과는 recvfrom 함수의 호출이 3회 진행되었음을 보이고 있다.
이로써 UDP 기반의 데이터 송수신 과정에서는 입출력 함수의 호출횟수를 일치시켜야 함을 확인할 수 있다.

### UDP 데이터그램

UDP 소켓이 전송하는 패킷을 가리켜 데이터그램이라고 표현하기도 하는데, 사실 데이터그램도 패킷의 일종이다.

다만 TCP 패킷과 달리 데이터의 일부가 아닌, 그 자체가 하나의 데이터로 의미를 가질 때 데이터그램이라 표현할 뿐이다. 이는 UDP의 데이터 전송특성과 관계가 있다. UDP는 데이터의 경계가 존재하기 때문에 하나의 패킷이 하나의 데이터로 간주된다. 따라서 데이터그램이라고 부른다.

그냥 샌드투를 보냈다 => UDP 데이터그램이라고 생각하자.

### connected UDP 소켓, unconnected UDP 소켓

TCP 소켓에는 데이터를 전송할 목적지의 IP와 PORT 번호를 등록하는 반면,
UDP 소켓에는 데이터를 전송할 목적지의 IP와 PORT 번호를 등록하지 않는다.
때문에 sendto 함수 호출을 통한 데이터의 전송과정은 다음과 같이 크게 3가지로 나눌 수 있다.

1. UDP 소켓에 목적지의 IP와 PORT 번호 등록
2. 데이터 전송
3. UDP 소켓에 등록된 목적지 정보 삭제

즉, sendto 함수가 호출될 때마다 위의 과정을 반복하게 된다. 이렇듯 목적지의 주소정보가 계속해서 변하기 때문에 하나의 UDP 소켓을 이용해서 다양한 목적지로 데이터 전송이 가능한 것이다.

그리고 이렇게 목적지 정보가 등록되어 있지 않은 소켓을 가리켜 `unconnected 소켓`이라고 하고,
반면 목적지 정보가 들어있는 소켓을 가리켜 `connected 소켓`이라 한다.
물론 UDP 소켓은 기본적으로 unconnected 소켓이다. 그런데 이러한 UDP 소켓은 다음과 같은 상황에서는 매우 불합리하게 동작한다.

`IP 211.210.147.82, PORT 82번으로 준비된 총 3개의 데이터를 3번의 sendto 함수 호출을 통해 전송한다`

이 경우 위에서 정리한 전송 3단계를 총 3번 반복해야 한다. 그래서 하나의 호스트와 오랜 시간 데이터를 송수신 해야한다면 UDP 소켓을 connected 소켓으로 만드는 것이 효율적이다. 참고로 1단계와 3단계가 UDP 데이터 전송과정의 약 1/3에 해당한다고 하니, 이 시간만 줄이면 성능향상을 기대할 수 있다.

### connected UDP 소켓 생성

connected UDP 소켓을 생성하는 방법은 의외로 간단하다. UDP 소켓을 대상으로 connect 함수만 호출하면 된다.

```C
sock = socket(PF_INET, SOCK_DGRAM, 0);
memset(&adr, 0, sizeof(adr));
adr.sin_family = AF_INIET;
adr.sin_addr.s_addr = ....
adr.sin_port = ....
connect(sock, (struct sockaddr*)&adr, sizeof(adr));
```

코드를 언뜻보면 TCP 소켓 생성 과정의 일부처럼 보일 수 있다. 하지만 socket 함수의 두번째 인자가 SOCK_DGRAM 이기 때문에 UDP 소켓의 생성과정이다. 이때 UDP 소켓을 대상으로 connect 함수를 호출했다고 해서 목적지의 UDP 소켓과 연결 설정을 거친다거나 하지는 않는다. 다만 UDP 소켓에 목적지의 IP와 PORT 정보가 등록될 뿐이다.

이로써 이후부터는 TCP 소켓과 마찬가지로 sendto 함수가 호출될 때마다 데이터 전송의 과정만 거치게 된다.
뿐만 아니라 송수신의 대상이 정해졌기 때문에 sendot, recvfrom 함수가 아닌 write, read 함수의 호출로도 데이터를 송수신 할 수 있다.

확인을 위해 간단한 예제를 작성해보자. 앞서 작성했던 uecho_client.c를 connected UDP 소켓 기반으로 재구현 했다.

때문에 uecho_server.c와 함께 실행이 가능하다. 그리고 비교의 편의를 위해 기존 코드에 주석을 처리하였다.

```C
// uecho_con_client.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <sys/socket.h>

#define BUF_SIZE 30
void error_handling(char* message);
int main(int argc, char* argv[])
{
    int sock;
    char message[BUF_SIZE];
    int str_len;
    socklen_t adr_sz; // 불필요해진 변수

    struct sockaddr_in serv_adr, from_adr; // from_adr는 필요 없음

    if (argc != 3)
    {
        printf("Usage : %s <IP> <port>\n", argv[0]);
        exit(1);
    }

    sock = socket(PF_INET, SOCK_DGRAM, 0);
    if (sock == -1)
        error_handling("socket() error");

    memset(&serv_adr, 0, sizeof(serv_adr));
    serv_adr.sin_family = AF_INET;
    serv_adr.sin_addr.s_addr = inet_addr(argv[1]);
    serv_adr.sin_port = htons(atoi(argv[2]));

    connect(sock, (struct sockaddr*)&serv_adr, sizeof(serv_adr)); // 커넥트함수 추가

    while (1)
    {
        fputs("Insert message(q to quit): ", stdout);
        fgets(message, sizeof(message), stdin);
        if (!strcmp(message, "q\n") || !strcmp(message, "Q\n"))
            break;

        // sendto(sock, message, strlen(message), 0,
        // (struct sockaddr*)&serv_adr, sizeof(serv_adr));

        write(sock, message, strlen(message)); // write 함수 추가

        // adr_sz = sizeof(from_adr);
        // str_len = recvfrom(sock, message, BUF_SIZE, 0,
        // (struct sockaddr*)&from_adr, &adr_sz);

        str_len = read(sock, message, sizeof(message) = 1); // read 함수 추가

        message[str_len] = 0;
        printf("Message from server: %s", message);
    }
    close(sock);
    return 0;
}

void error_handling(char* message)
{
    fputs(message, stderr);
    fputc('\n', stderr);
    exit(1);
}
```

실행결과는 전에처럼 변화가 없이 입력한 문자열이 그대로 출력되는 것을 확인할 수 있다.
코드를 놓고 봤을 때 sendto, recvfrom 함수의 호출을 write, read 함수가 대신한 것에 주목하면 될 듯하다.
