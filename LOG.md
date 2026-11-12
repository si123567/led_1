# 개발자 : 신림고 최재원
# 날짜 : 2026.09.21

## 오늘 한 일
- 깃허브에 프로젝트 만들기
- MQ2랑 LED 따로따로 연결해보기

## 어떤 걸 만들까
누가:우리 반 학생들 / 언제:6교시쯤 / 문제:공기가 탁한데 아무도 모르고 넘어감 / 내 장치:MQ2로 공기를 재서 → 나빠지면 LED가 빨갛게 차올라 시각적으로 알림
## 잘 된 거 / 안 된 거
- 잘 됨 : 연결과 코드가 잘 작동한다.
## 다음 시간 할 일
- ...

# 개발자:신림고 최재원, 김은호
# 날짜:2026.10.26
# 1회자
---
#오늘 한 일
- 깃허브에 공동작업 설정하기
- 아이디어 회의
- 센서 사용 방안 대략적 논의
---
#잘 된 거/안 된 거
- ✅ 의견 논의 성공적
- ✅ 공동작업 설정 완료
- ❌센서 사용 방안 아직 불확실
---
#다음 시간 할 일
-센서 사용 반안 논의하기
---
# 개발자:신림고 최재원, 김은호
# 날짜:2026.11.02
# 2회자
---
#오늘 한 일
-허스키 렌즈 연결선 찾기
---
# 개발자:신림고 최재원, 김은호
# 날짜:2026.11.05
# 3회자
---
## 8,9차시 — AI 카메라 허스키렌즈 
---
#오늘 한 일
- 허스키렌즈 연결하기
- 허스키렌즈에 얼굴 학습시키기
- 허스키렌즈에 색 학습시키기
---
# 개발자:신림고 최재원, 김은호
# 날짜:2026.11.09
# 3회자
---
#오늘 한 일
챗gpt와의 논의
- 챗 지피티와의 대화
```
아, 이제 프로젝트의 구조를 정확히 이해했습니다. 그렇다면 앞서 작성한 서보모터 4개로 손가락 4개를 움직이는 코드와는 다른 방식으로 설계해야 합니다.

보내주신 제품은 디바이스마트의 Keyestudio 아두이노 4DOF 아크릴 로봇암 키트(KS0198X) 입니다. 상품 설명에는 서보모터 4개와 V4.0 제어 보드, 조이스틱 모듈 2개가 안내되어 있습니다.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://www.devicemart.co.kr\&sz=32)

디바이스마트+1



[Toy Robot Arm Kit at Lara Bunton blog](https://images.openai.com/static-rsc-4/vYV_2CkuvrC01NDysvpgpfJDrfKqmOpm5TRycNHzgWJ4Hp86h4DXIVcjNz8LzFRQtqKAHzndsry2DhDSFxSXxo4LNh1nlucW9BGdqKvRFghhPleoGZmCN59WByW28eFrfN8kW-nB_1to1rqWVhptL5kcEdgqR6oUYHMinoXIujI?purpose=inline)

4DOF 집게형 로봇팔

4개의 서보모터로 로봇팔의 관절과 집게를 움직이는 구조입니다.

[제품 페이지](https://www.devicemart.co.kr/goods/view?no=15526613\&utm_source=chatgpt.com)

따라서 장갑의 센서 4개를 로봇팔의 관절 4개에 일대일로 대응시키는 방식이 적합합니다.

## 1. 수정된 하드웨어 구성

이번에는 총 3개의 제어 보드를 사용하는 것을 추천합니다.

| 장치                 | 역할                   |
| ------------------ | -------------------- |
| Pico 2 A           | 플렉스 센서 1, 2 측정       |
| Pico 2 B           | 플렉스 센서 3, 4 측정       |
| Keyestudio V4.0 보드 | 로봇팔 서보모터 4개 제어       |
| 컴퓨터                | 센서 데이터 통합 및 모터 명령 전달 |

여기서 중요한 점은 해당 로봇팔에 V4.0 보드와 서보모터 드라이버 쉴드가 포함된다는 것입니다. 피코가 모터를 직접 구동하도록 복잡하게 배선할 필요 없이, 기존 보드를 모터 제어 전용으로 사용하면 됩니다.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://docs.keyestudio.com\&sz=32)

Robot arm Kit for Arduino documentation+1



### 로봇팔의 실제 모터 연결

설명서에서 확인한 연결 핀은 다음과 같습니다.

| 서보모터    | 기능            | V4.0 보드 핀 |
| ------- | ------------- | --------- |
| Servo 1 | 로봇팔 베이스 좌우 회전 | A1        |
| Servo 2 | 오른쪽 팔 관절      | A0        |
| Servo 3 | 왼쪽 팔 관절       | D6        |
| Servo 4 | 집게 개폐         | D9        |

[image](https://www.google.com/s2/favicons?domain=https://docs.keyestudio.com\&sz=32)

Robot arm Kit for Arduino documentation+1

### 장갑 센서와 로봇팔 연결 관계

우선 다음과 같이 1:1 대응시키겠습니다.

| 장갑의 센서       | 로봇팔 동작 |
| ------------ | ------ |
| 센서 1 (검지)    | 베이스 회전 |
| 센서 2 (중지)    | 오른쪽 관절 |
| 센서 3 (약지)    | 왼쪽 관절  |
| 센서 4 (새끼손가락) | 집게 개폐  |

이렇게 하면 손가락을 구부리는 정도가 로봇팔의 각 관절 움직임으로 변환됩니다.

특히 네 번째 센서는 손가락을 구부릴수록 집게가 닫히도록 설정하겠습니다.

나중에 베이스 회전과 집게 개폐만 사용하고 싶다면 센서 1, 4만 활성화하고 관절 2개는 고정하면 됩니다.

## 2. 브레드보드 배선

불필요하게 복잡한 배선은 생략하겠습니다.

Pico A 브레드보드

| 부품    | 연결       |
| ----- | -------- |
| 센서 1  | GP26     |
| 센서 2  | GP27     |
| 고정 저항 | 10kΩ 2개  |
| 전원    | 3V3, GND |

Pico B 브레드보드

| 부품    | 연결       |
| ----- | -------- |
| 센서 3  | GP26     |
| 센서 4  | GP27     |
| 고정 저항 | 10kΩ 2개  |
| 전원    | 3V3, GND |

로봇팔

- 서보 4개는 기존 드라이버 쉴드에 연결
- V4.0 보드는 USB로 컴퓨터에 연결
- 모터 전원은 키트의 드라이버 사양에 맞는 외부 전원 사용

플렉스 센서는 디지털 입력이 아니라 아날로그 입력으로 읽어야 합니다. 따라서 GP26과 GP27을 `ADC()`로 설정합니다.

로봇팔 설명서의 드라이버 전원 사양은 VIN DC 7\~9V, 5A이며 5V 출력도 제공한다고 안내되어 있습니다. 서보모터 전원을 피코의 3.3V 핀에서 공급하지 마세요.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://docs.keyestudio.com\&sz=32)

Robot arm Kit for Arduino documentation



## 3. Pico A와 Pico B 코드

두 피코는 거의 동일한 코드를 사용합니다. 구별을 위해 A와 B만 변경합니다.

Pico A에 업로드

```
from machine import ADC, Pinfrom time import sleep_msBOARD = "A"flex1 = ADC(Pin(26))flex2 = ADC(Pin(27))def read_average(sensor):    total = 0    for _ in range(10):        total += sensor.read_u16()        sleep_ms(1)    return total // 10while True:    value1 = read_average(flex1)    value2 = read_average(flex2)    print("A,{},{}".format(value1, value2))    sleep_ms(100)
```

Pico B에 업로드

```
from machine import ADC, Pinfrom time import sleep_msBOARD = "B"flex3 = ADC(Pin(26))flex4 = ADC(Pin(27))def read_average(sensor):    total = 0    for _ in range(10):        total += sensor.read_u16()        sleep_ms(1)    return total // 10while True:    value3 = read_average(flex3)    value4 = read_average(flex4)    print("B,{},{}".format(value3, value4))    sleep_ms(100)
```

두 피코 모두 USB를 통해 컴퓨터에 측정값을 전달합니다.

## 4. 로봇팔 제어 코드

이제 키트에 포함된 V4.0 보드에 Arduino 코드를 업로드합니다.

이 보드는 Arduino UNO 호환 ATmega328P 기반이므로 MicroPython이 아닌 Arduino C++ 코드를 사용합니다.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://docs.keyestudio.com\&sz=32)

Robot arm Kit for Arduino documentation



```

#include <Servo.h>

// 로봇팔 서보모터 4개
Servo servo1;  // 베이스 회전
Servo servo2;  // 오른쪽 관절
Servo servo3;  // 왼쪽 관절
Servo servo4;  // 집게

char buffer[64];
int index = 0;


// 서보모터 초기 설정
void setup() {

  Serial.begin(115200);

  servo1.attach(A1);
  servo2.attach(A0);
  servo3.attach(6);
  servo4.attach(9);

  // 시작 자세
  servo1.write(80);
  servo2.write(60);
  servo3.write(120);
  servo4.write(100);
}


// 모터 각도 제어
void controlServo(int a, int b, int c, int d) {

  // 초기 테스트용 안전 범위
  a = constrain(a, 40, 140);
  b = constrain(b, 60, 110);
  c = constrain(c, 60, 120);
  d = constrain(d, 5, 100);

  servo1.write(a);
  servo2.write(b);
  servo3.write(c);
  servo4.write(d);
}


// 컴퓨터 명령 처리
void processCommand(char *command) {

  int a, b, c, d;

  // S,베이스,관절2,관절3,집게
  if (sscanf(command, "S,%d,%d,%d,%d",
             &a, &b, &c, &d) == 4) {

    controlServo(a, b, c, d);
  }
}


void loop() {

  while (Serial.available()) {

    char ch = Serial.read();

    if (ch == '\n') {

      buffer[index] = '\0';

      processCommand(buffer);

      index = 0;
    }

    else if (ch != '\r') {

      if (index < sizeof(buffer) - 1) {
        buffer[index++] = ch;
      }
      else {
        index = 0;
      }
    }
  }
}

```

컴퓨터에서 다음과 같은 데이터를 보내면 네 개의 모터가 각각 움직입니다.

```
S,80,60,120,100
```

집게 모터는 0도에 가까워질수록 닫히고, 100도에 가까워질수록 열리는 방향으로 설정했습니다. 실제 키트의 기본 동작 방향은 설명서에 기재된 내용에 따랐습니다.&#x20;

[image](https://www.google.com/s2/favicons?domain=https://docs.keyestudio.com\&sz=32)

Robot arm Kit for Arduino documentation+1



## 5. 컴퓨터 통합 제어 코드

마지막으로 두 피코의 데이터를 모아 로봇팔을 움직이는 Python 코드입니다.

컴퓨터에는 PySerial을 설치해야 합니다.

```
pip install pyserial
```

컴퓨터에는 총 3개의 USB 연결이 필요합니다.

| 장치             | 예시 포트 |
| -------------- | ----- |
| Pico A         | COM3  |
| Pico B         | COM4  |
| V4.0 로봇팔 제어 보드 | COM5  |

실제 포트 번호는 Windows 장치 관리자에서 확인하세요.

```
time.sleep(2)pico_a.reset_input_buffer()pico_b.reset_input_buffer()# =================================# 2. 센서 데이터 저장# =================================latest = {    "A": None,    "B": None}lock = threading.Lock()# =================================# 3. 피코 데이터 수신# =================================def receive_data(port, board):    while True:        try:            line = port.readline().decode(                "utf-8", errors="ignore"            ).strip()            parts = line.split(",")            if len(parts) != 3:                continue            if parts[0] != board:                continue            v1 = int(parts[1])            v2 = int(parts[2])            with lock:                latest[board] = (                    [v1, v2],                    time.time()                )        except (ValueError, serial.SerialException):            time.sleep(0.05)# 수신 스레드 실행threading.Thread(    target=receive_data,    args=(pico_a, "A"),    daemon=True).start()threading.Thread(    target=receive_data,    args=(pico_b, "B"),    daemon=True).start()# =================================# 4. 센서 4개 통합
```

## 6. 최종 동작 예시

장갑의 동작에 따른 로봇팔 반응

| 센서 | 움직임   | 로봇 동작    |
| -- | ----- | -------- |
| 검지 | 굽힘 증가 | 베이스 회전   |
| 중지 | 굽힘 증가 | 팔 관절 움직임 |
| 약지 | 굽힘 증가 | 팔 관절 움직임 |
| 새끼 | 굽힘 증가 | 집게 닫힘    |

실제 테스트 시 주의할 점: 처음에는 집게에 물체를 넣지 않고, 로봇팔 주변에 손을 두지 않은 상태에서 낮은 속도와 제한된 각도로 테스트하세요. 각 관절이 반대 방향으로 움직이면 컴퓨터 코드의 `ANGLE_STRAIGHT`, `ANGLE_BENT` 값을 해당 센서에 대해 서로 바꾸면 됩니다.

또한 플렉스 센서는 손가락의 굽힘만 감지합니다. 손목을 비트는 동작까지 감지해서 베이스를 회전시키고 싶은 경우에는 별도의 자이로/가속도 센서가 필요합니다. 지금 코드에서는 검지를 구부리는 정도를 베이스의 회전 각도로 변환했습니다.

제가 추천하는 다음 작업은 실제 로봇팔의 네 서보모터 작동 범위를 확인하고 센서 보정값을 맞추는 것입니다. 실물 테스트 때 각도를 기록해 주시면 기구부에 맞게 제어 범위를 조정할 수 있습니다.

🦾로봇팔 각도 보정 점검눌러서 설정하세요
```
---
# 개발자:신림고 최재원, 김은호
# 날짜:2026.11.12
# 4회자
---
#오늘 한 일
- 아두이노 IDE 설치
- 아두이노 작동 확인 및 센서 연결
---
#잘 된 거/안 된 거
- ✅ 아두이노 작동 확인
---
#다음 시간 할 일
- 센서 회로 구상하기
---

