TIME-WAIT 상태는 4 way handshake의 일부로 TCP 연결 종료 과정에서 중요한 역할을 합니다. Time wait의 주요 목적은 다음과 같습니다.

첫 번째로, 지연된 패킷을 처리하기 위함입니다. 클라이언트와 서버가 FIN 과 ACK 응답을 다 했다고 해도, 네트워크를 떠돌며 도착하지 않은 패킷이 있을 수 있습니다. 만약 time wait 없이 소켓 연결을 종료하게 된다면, 패킷 응답을 받지 못하게 되지만, time wait을 유지한다면 손실되는 패킷을 최소화하며 TCP 연결을 종료할 수 있게 됩니다.

두 번째로, 신뢰성 있는 연결 종료를 위해서 사용됩니다. 클라이언트와 서버는 연결을 종료하기 위해 FIN 과 ACK를 주고받습니다. 클라이언트가 처음 보내는 FIN과 서버에서 처음으로 FIN에 대해 응답하는 ACK의 경우는 바로 연결이 끊기는 것이 아니기 때문에 문제없이 처리될 수 있습니다. 그러나 서버에서 보내는 FIN의 경우, 클라이언트가 ACK를 보내고 바로 연결을 끊게 되면, 클라이언트의 ACK가 유실될 우려가 있습니다. 만약 클라이언트가 ACK를 보내고 연결을 끊었는데, ACK가 유실되면 서버는 계속해서 FIN 요청을 보낼것이고 이는 네트워크 부하로 이어집니다.

이러한 상황을 대비하기 위해 4-way-handshake에서 TIME-WAIT가 필요합니다.

3 way handshake의 경우, TCP 연결을 하기 위해 사용되었다면, 4 way handshake는 TCP 연결을 종료할 때 사용됩니다.


# 4 way handshake

4 way handshake는 다음 4 단계를 거쳐 진행됩니다.
![image](https://github.com/CodeSquad-BE-Study/2023-CS-Study/assets/115435784/847fd377-aad0-44f3-b009-7cbafc6a0643)


### 1 단계

클라리언트가 연결을 종료하기 위해 **FIN** 을 전송한다.

![image](https://github.com/CodeSquad-BE-Study/2023-CS-Study/assets/115435784/74409d00-18b4-4470-b598-92565df6cfb3)


### 2 단계

서버는 클라이언트의 **FIN** 요청을 받고, 확인메시지인 **ACK**를 전송한다.

![image](https://github.com/CodeSquad-BE-Study/2023-CS-Study/assets/115435784/6d6043f2-3cef-4373-96ca-a7b8cbb7acc7)


### 3 단계

서버가 연결을 종료할 준비가 되면, 클라이언트에게 **FIN** 요청을 전송한다.

![image](https://github.com/CodeSquad-BE-Study/2023-CS-Study/assets/115435784/c3393afe-0217-4343-abbe-0907083f6e5e)


### 4 단계

클라이언트는 연결 종료 준비가 되었다는 ACK 메시지를 전송합니다.

![image](https://github.com/CodeSquad-BE-Study/2023-CS-Study/assets/115435784/9526444b-d9ad-48ad-91a0-203fe7aaf59c)


**이 때 클라이언트의 상태는 TIME-WAIT** 가 됩니다.

# TIME - WAIT

클라이언트와 서버는 모두 FIN 과 ACK 를 통해 종료하겠다는 통신을 완료했는데 왜 통신이 바로 끊기지 않고 **TIME - WAIT** 상태가 되는걸까요?

이는 네트워크 환경의 불확실한 요소와 연관이 있습니다.

네트워크 환경은 패킷의 지연, 순서 변경, 손실 등과 같은 여러 불확실한 요소를 포함하고 있기 때문에, TIME-WAIT 상태는 이러한 불확실성을 처리하는 중요한 역할을 합니다.

만약 TIME - WAIT가 존재하지 않았다면 어떻게 됐을까요?

첫 번째 상황을 가정해보겠습니다.

![image](https://github.com/CodeSquad-BE-Study/2023-CS-Study/assets/115435784/6555d870-5eed-4250-bfde-6952c0e0340d)


이렇게 서버가 FIN 요청을 보낸 후 클라이언트가 ACK 응답을 보냈는데, 중간에 패킷이 유실된다면 어떻게 될까요?

서버는 계속해서 FIN 요청을 보낼 것이고, 클라이언트는 연결이 끊어졌기 때문에 FIN 에 대한 ACK를 보낼 수 없게 됩니다. 이는 곧 네트워크 트래픽으로 이어지게 됩니다.

두 번째 상황입니다.

![image](https://github.com/CodeSquad-BE-Study/2023-CS-Study/assets/115435784/529e7c16-6c3f-4a8c-929a-4be613016fb8)


클라이언트가 마지막 요청을 보낸 후, FIN 요청을 주고받게 되는데, 마지막 요청이 시간이 오래 걸려서 CLOSE 된 후 ACK가 도착한다면 어떻게 될까요?

클라이언트는 서버의 응답을 받지 못했지만 재전송을 요청하지 못하고 종료되게 됩니다. 이는 TCP의 reliable 한 특성을 유지하지 못하게 됩니다.

위의 그림과 같이 TIME - WAIT를 활용하면 네트워크 특성으로 인한 불확실한 요소를 처리하며, TCP 연결의 안정성과 신뢰성을 보장할 수 있습니다.
