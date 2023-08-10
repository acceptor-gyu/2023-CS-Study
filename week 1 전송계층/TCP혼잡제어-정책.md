혼잡 제어기법은 슬로우 스타트(Slow start), 혼잡 회피(Congestion Avoidance), 빠른 복구(Fast recovery), AIMD(Additivie Increase Multiplicate Decrease)가 있습니다.

슬로우스타트는 연결시작 혹은 혼잡 발생시에 혼잡 윈도우 값을 최소값부터 시작을 하는 것을 말합니다. ACK가 수신될 때마다 혼잡 윈도우를 1씩 증가시키고 RTT(Round Trip Time)마다 혼잡윈도우를 2배씩 증가시킵니다. 

혼잡회피는 혼잡 윈도우가 임계치에 도달하거나 넘어가면 선형적으로 증가하도록 증가 속도를 조절하는 알고리즘입니다. 임계치에 도달하면 RTT 마다 혼잡 윈도우를 1씩 증가시킵니다.

빠른 복구는 3개의 중복 ACK에 의한 빠른 재전송 시에 적용합니다. (경미한 혼잡 상황)

정상 ACK가 수신되어 오류 복구가 완료되면 슬로우 스타트 구간을 건너뛰고 혼잡 회피 단계로 진입합니다. 혼잡 윈도우의 임계치를 현재 혼잡 윈도우의 1/2 로 설정합니다. 이후 손실된 세그먼트를 재전송합니다. 그리고 혼잡 윈도우의 임계치를 현재 임계치의 +3 으로 설정합니다. 여전히 중복 ACK를 수신하면 혼잡윈도우의 크기를 +1 합니다. 정상 ACK를 수신하면 혼잡윈도우를 임계치 값으로 설정합니다. 이후 혼잡 회피 단계로 진입합니다.

그런데 TCP의 버전에 따라서 혼잡 제어 알고리즘이 다릅니다. 버전은 Tahoe, Reno버전이 있습니다.

Tahoe 버전에서 혼잡인식은 Timeout과 3개의 중복 ACK가 발생했을 때 입니다. 임계치를 현재 혼잡 윈도우의 1/2로 설정하고 슬로우 스타트를 개시합니다. 이후 혼잡 윈도우가 임계치에 도달하거나 넘어가면 혼잡 회피를 수행하는 알고리즘을 구현합니다.

Reno 버전에서 혼잡인식은 Tahoe 버전과 동일하게 Timeout과 3개의 중복 ACK 발생 시 혼잡 상황을 인식합니다. 대신 구현 알고리즘이 약간 다릅니다.

Timeout 발생시 Tahoe 버전과 동일하게 혼잡 윈도우의 임계치를 1/2로 줄이고 슬로우 스타트를 실행합니다.

3개의 중복 ACK가 발생했을 때는 fast recovery 알고리즘을 적용합니다.

마지막으로 AIMD 기법은 패킷을 보낼 때 혼잡윈도우의 크기를 선형적으로 증가시키고 혼잡이 발생하면 지수적으로 감소시키는 방법입니다.

# Tahoe vs Reno 요약

**TCP Tahoe**

- 임계 전 Slow start
- 임계 도달 후 AIMD 방식
- 3 ACK Duplicated / Timeout → 임계를 반으로 줄이고 Window 1부터 slow start

![image](https://github.com/CodeSquad-BE-Study/2023-CS-Study/assets/112251635/465a4c53-3506-4d0b-bcad-26054538b35a)


**TCP Reno**

Tahoe와 동일하지만 3 Ack Duplicated 발생 시 Window를 키우는데 많은 시간이 걸린다는 Tahoe의 단점을 보완하는 방식이다.

- 임계 전 Slow start
- 임계 도달 후 AIMD 방식
- 3 ACK Duplicated → 윈도우 크기를 반으로 줄이고 임계를 동일하게 설정
- Timeout → 임계를 반으로 줄이고 Window 1부터 slow start

![image](https://github.com/CodeSquad-BE-Study/2023-CS-Study/assets/112251635/33a5b0b0-d3d0-44fb-bc48-bc6b3111b5e6)
