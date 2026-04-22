# 오렌지보드 BLE (MDBT42Q) 사용설명서

> **발명 자료 | iprise 프로젝트**  
> BLE AT Command 모듈 기반 블루투스 통신 시스템

---

## 목차

1. [프로젝트 개요](#1-프로젝트-개요)
2. [사용 부품](#2-사용-부품)
3. [FTDI 드라이버 설치](#3-ftdi-드라이버-설치)
4. [아두이노 IDE 설치](#4-아두이노-ide-설치)
5. [MDBT42Q BLE 모듈 소개](#5-mdbt42q-ble-모듈-소개)
6. [하드웨어 연결](#6-하드웨어-연결)
7. [AT 명령어 사용법](#7-at-명령어-사용법)
8. [블루투스 디바이스 이름 변경](#8-블루투스-디바이스-이름-변경)
9. [AT 명령어 전체 목록](#9-at-명령어-전체-목록)
10. [참고 자료](#10-참고-자료)

---

## 1. 프로젝트 개요

이 프로젝트는 **오렌지보드 BLE**와 **Raytac MDBT42Q BLE 모듈**을 활용하여 블루투스 저에너지(BLE) 기반 무선 통신 시스템을 구현합니다.  
UART 인터페이스를 통해 AT 명령어로 BLE 모듈을 제어하며, 스마트폰 앱과 연동하여 데이터를 송수신할 수 있습니다.

**주요 특징:**
- Nordic nRF52810 SoC 기반 BLE 5.2 모듈
- AT 명령어를 통한 간편한 블루투스 설정
- 개방 공간 최대 80m 통신 거리 (MDBT42Q 기준)
- FCC, CE, KC 등 주요 국제 인증 취득

---

## 2. 사용 부품

| 부품명 | 모델 | 설명 |
|--------|------|------|
| 메인 보드 | 오렌지보드 BLE | FTDI 칩 기반 아두이노 호환 보드 |
| BLE 모듈 | Raytac MDBT42Q-AT / MDBT42Q-PAT | Nordic nRF52810 BLE 5.2 모듈 |
| PC 드라이버 | CDM212364_Setup.exe | FTDI USB-to-Serial 드라이버 |
| 스마트폰 앱 | Nordic Toolbox | BLE 통신 테스트 앱 |

---

## 3. FTDI 드라이버 설치

오렌지보드를 PC에 연결하기 위해서는 **FTDI USB 드라이버**를 먼저 설치해야 합니다.

### 설치 방법

이 저장소에 포함된 `CDM212364_Setup.exe` 파일을 사용합니다.

1. `CDM212364_Setup.exe` 파일을 다운로드합니다.
2. 파일을 실행하고 설치 마법사의 안내에 따라 설치를 완료합니다.
3. 설치 후 오렌지보드를 USB로 PC에 연결합니다.
4. **장치 관리자**를 열어 `포트(COM & LPT)` 항목에서 **USB Serial Port**가 정상적으로 인식되는지 확인합니다.

> 📌 **최신 드라이버:** FTDI 공식 사이트(https://ftdichip.com)에서 최신 VCP(Virtual COM Port) 드라이버를 다운로드할 수도 있습니다.

### Mac OS 사용 시

Mac에서 장치가 인식되지 않을 경우, 수동으로 드라이버를 설치해야 합니다.  
FTDI 공식 사이트에서 Mac용 드라이버를 다운로드하여 설치하세요.

---

## 4. 아두이노 IDE 설치

1. [Arduino 공식 사이트](https://www.arduino.cc/en/software)에서 IDE를 다운로드하여 설치합니다.
2. IDE 실행 후 `도구 > 보드`에서 사용 중인 보드를 선택합니다.
3. `도구 > 포트`에서 FTDI 드라이버 설치 시 인식된 COM 포트를 선택합니다.

---

## 5. MDBT42Q BLE 모듈 소개

**Raytac MDBT42Q-AT / MDBT42Q-PAT**는 Nordic nRF52810 SoC를 기반으로 한 BLE 5.2 모듈입니다.

### 제품 사양

| 항목 | 사양 |
|------|------|
| Solution | Nordic nRF52810 QFN Package |
| Bluetooth 버전 | BLE 5.2 |
| 모드 | Peripheral / Slave |
| 크기 | 16 x 10 x 2.2 mm |
| 통신 거리 | 최대 80m (MDBT42Q, 개방 공간, 1Mbps) |
| 통신 거리 | 최대 60m (MDBT42Q-PAT, 개방 공간) |
| 인터페이스 | UART |
| 펌웨어 버전 | 1.4 |
| 인증 | FCC(미국), CE(EU), TELEC(일본), SRRC(중국), IC(캐나다), NCC(대만), KC(한국) |

### 모델 구분

| 모델 | 안테나 타입 |
|------|------------|
| MDBT42Q-AT | Chip Antenna (칩 안테나) |
| MDBT42Q-PAT | PCB Antenna (PCB 안테나) |

---

## 6. 하드웨어 연결

오렌지보드 BLE와 MDBT42Q 모듈은 **SoftwareSerial** 라이브러리를 통해 UART로 연결됩니다.

| 오렌지보드 핀 | MDBT42Q 핀 | 기능 |
|--------------|-----------|------|
| 디지털 핀 4 | RX | 데이터 수신 |
| 디지털 핀 5 | TX | 데이터 송신 |
| 3.3V | VCC | 전원 공급 |
| GND | GND | 공통 접지 |

> ⚠️ **주의:** MDBT42Q 모듈은 3.3V 전압으로 동작합니다. 5V를 직접 연결하면 모듈이 손상될 수 있습니다.

---

## 7. AT 명령어 사용법

### 기본 아두이노 코드

아래 코드를 아두이노에 업로드하면 시리얼 모니터를 통해 AT 명령어를 BLE 모듈로 전달할 수 있습니다.

```cpp
#include <SoftwareSerial.h>

// BLE 모듈 연결 핀 설정 (RX: 4번, TX: 5번)
SoftwareSerial BLE(4, 5);

void setup() {
  Serial.begin(9600);    // PC 시리얼 통신 속도
  BLE.begin(9600);       // BLE 모듈 통신 속도 (기본값: 9600bps)
}

void loop() {
  // PC → BLE 모듈로 데이터 전달
  if (Serial.available()) {
    BLE.write(Serial.read());
  }
  // BLE 모듈 → PC로 데이터 전달
  if (BLE.available()) {
    Serial.write(BLE.read());
  }
}
```

### AT 명령어 전송 방법

1. 위 코드를 아두이노에 업로드합니다.
2. 아두이노 IDE의 **시리얼 모니터**를 엽니다 (통신 속도: 9600).
3. 시리얼 모니터에 AT 명령어를 입력하고 전송합니다.
4. 모듈의 응답을 시리얼 모니터에서 확인합니다.

---

## 8. 블루투스 디바이스 이름 변경

기본 디바이스 이름은 **"KocoaFAB_BLE"** 입니다. 아래 절차로 변경할 수 있습니다.

### 변경 절차

```
1. 시리얼 모니터에 입력:
   AT+NAME_새이름
   (예: AT+NAME_MyDevice, 최대 20자)

2. 모듈 재시작:
   AT+RESET

3. 이름 확인:
   AT?NAME
```

> 📌 변경된 이름은 **영구 저장**되어 전원을 꺼도 유지됩니다.

### 스마트폰에서 확인 방법

1. **Nordic Toolbox** 앱을 설치합니다 (iOS / Android).
2. 앱을 실행하고 BLE 스캔을 시작합니다.
3. 변경한 디바이스 이름이 목록에 나타나는지 확인합니다.

---

## 9. AT 명령어 전체 목록

### Write 명령어 (설정)

| No. | 명령어 | 설명 |
|-----|--------|------|
| 1 | `AT+NAME[이름]` | 디바이스 이름 설정 (최대 20자) |
| 2 | `AT+RESET` | 시스템 재시작 |
| 3 | `AT+ADVSTART` | 광고(Advertising) 시작 |
| 4 | `AT+ADVSTOP` | 광고(Advertising) 중지 |
| 5 | `AT+SLEEP` | 딥 슬립 모드 진입 |
| 6 | `AT+BAUDRATE9600` | UART 통신 속도 9600 bps 설정 |
| 7 | `AT+BAUDRATE19200` | UART 통신 속도 19200 bps 설정 |
| 8 | `AT+BAUDRATE38400` | UART 통신 속도 38400 bps 설정 |
| 9 | `AT+BAUDRATE57600` | UART 통신 속도 57600 bps 설정 |
| 10 | `AT+BAUDRATE115200` | UART 통신 속도 115200 bps 설정 |
| 11 | `AT+BAUDRATE230400` | UART 통신 속도 230400 bps 설정 (플로우 컨트롤 권장) |
| 12 | `AT+BAUDRATE460800` | UART 통신 속도 460800 bps 설정 (플로우 컨트롤 권장) |
| 13 | `AT+FLOWCONTROLDIS` | UART 플로우 컨트롤 비활성화 |
| 14 | `AT+FLOWCONTROLEN` | UART 플로우 컨트롤 활성화 |
| 15 | `AT+TXPOWER4DBM` | RF 송신 출력 +4dBm 설정 |
| 16 | `AT+TXPOWER0DBM` | RF 송신 출력 0dBm 설정 |
| 17 | `AT+TXPOWER-4DBM` | RF 송신 출력 -4dBm 설정 |
| 18 | `AT+TXPOWER-8DBM` | RF 송신 출력 -8dBm 설정 |
| 19 | `AT+TXPOWER-20DBM` | RF 송신 출력 -20dBm 설정 |
| 20 | `AT+XTALINTERNAL` | 내부 RC 32.768KHz 발진기 사용 |
| 21 | `AT+XTALEXTERNAL` | 외부 크리스탈 32.768KHz 발진기 사용 |
| 22 | `AT+CONNECTINDICATORLOW` | BT 연결 시 LOW 신호 출력 |
| 23 | `AT+CONNECTINDICATORHIGH` | BT 연결 시 HIGH 신호 출력 |
| 24 | `AT+PHYMODE1MBPS` | PHY 모드 1Mbps 설정 |
| 25 | `AT+PHYMODE2MBPS` | PHY 모드 2Mbps 설정 |
| 26 | `AT+WAKEUPLOW` | 딥 슬립 시 LOW 웨이크업 신호 설정 |
| 27 | `AT+WAKEUPHIGH` | 딥 슬립 시 HIGH 웨이크업 신호 설정 |
| 28 | `AT+ADVTIMEtttt` | 광고 시간 설정 (Hex, 0x001E~0x0E10, 0x0000=무제한) |
| 29 | `AT+DCDCDIS` | DC-DC 컨버터 비활성화 |
| 30 | `AT+DCDCEN` | DC-DC 컨버터 활성화 |
| 31 | `AT+CONNECTINTERVALMODE0` | 연결 간격 모드 0 (iOS/Android, 20~40ms) |
| 32 | `AT+CONNECTINTERVALMODE1` | 연결 간격 모드 1 (nRF52832, 8ms 고정) |
| 33 | `AT+CONNECTINTERVALMODE2` | 연결 간격 모드 2 (프로그래머블, 8~1000ms) |
| 34 | `AT+CONNECTINTERVALTIMEtttttttt` | 연결 간격 시간 설정 (MODE2 활성 시, Hex) |
| 35 | `AT+ADVPATTERNnnnnffff` | LED 광고 패턴 설정 (Hex, ON시간/OFF시간) |
| 36 | `AT+CONNECTPATTERNnnnnffff` | LED 연결 패턴 설정 (Hex, ON시간/OFF시간) |
| 37 | `AT+SERIALNOnnnnnnnn` | 시리얼 번호 설정 (8자리 고정) |
| 38 | `AT+RESPONSEDIS` | Write 명령 응답 비활성화 |
| 39 | `AT+RESPONSEEN` | Write 명령 응답 활성화 |
| 40 | `AT+DISCONNECT` | 연결 종료 |
| 41 | `AT+DEFAULT` | 기본값으로 초기화 |
| 42 | `AT+SETGPIOnnHIGH` | GPIO p0.nn HIGH 출력 (nn: 12~19, ASCII) |
| 43 | `AT+SETGPIOnnLOW` | GPIO p0.nn LOW 출력 (nn: 12~19, ASCII) |
| 44 | `AT+SETGPIOnnOFF` | GPIO p0.nn 미사용 설정 (nn: 12~19, ASCII) |
| 45 | `AT+MACАDDRnnnnnnnnnnnn` | IC MAC 주소 설정 (Hex, MSB→LSB) |

### Read 명령어 (읽기)

| No. | 명령어 | 설명 |
|-----|--------|------|
| 1 | `AT?NAME` | 디바이스 이름 조회 |
| 2 | `AT?VERSION` | 펌웨어 버전 조회 |
| 3 | `AT?MACADDR` | MAC 주소 조회 |
| 4 | `AT?BAUDRATE` | 현재 UART 통신 속도 조회 |
| 5 | `AT?FLOWCONTROL` | 플로우 컨트롤 상태 조회 |
| 6 | `AT?TXPOWER` | RF 송신 출력 조회 |
| 7 | `AT?XTAL` | 발진기 상태 조회 |
| 8 | `AT?CONNECTINDICATOR` | BT 연결 표시 핀 로직 조회 |
| 9 | `AT?PHYMODE` | PHY 모드 상태 조회 |
| 10 | `AT?WAKEUP` | 웨이크업 핀 로직 조회 |
| 11 | `AT?ADVTIME` | 광고 시간 조회 (Hex) |
| 12 | `AT?DCDC` | DC-DC 컨버터 상태 조회 |
| 13 | `AT?CONNECTINTERVALMODE` | 연결 간격 모드 조회 |
| 14 | `AT?ADVPATTERN` | LED 광고 패턴 조회 (Hex) |
| 15 | `AT?CONNECTPATTERN` | LED 연결 패턴 조회 (Hex) |
| 16 | `AT?SERIALNO` | 시리얼 번호 조회 |
| 17 | `AT?ADCVALUE` | 10bit ADC 값 조회 |
| 18 | `AT?RESPONSE` | 응답 상태 조회 |
| 19 | `AT?ALLPARAMETERS` | 전체 파라미터 값 조회 |
| 20 | `AT?CONNECTINTERVALTIME` | MODE2 연결 간격 시간 조회 |

### 기본값 응답 예시

| 명령어 | 기본값 | 설명 |
|--------|--------|------|
| `AT?NAME` | Raytac AT-UART | 기본 디바이스 이름 |
| `AT?BAUDRATE` | 0 (9600bps) | 기본 통신 속도 |
| `AT?TXPOWER` | 0 (4dBm) | 기본 송신 출력 |
| `AT?PHYMODE` | 0 (1Mbps) | 기본 PHY 모드 |
| `AT?ADVTIME` | 0000 (무제한) | 기본 광고 시간 |
| `AT?FLOWCONTROL` | 0 (비활성) | 기본 플로우 컨트롤 |

---

## 10. 참고 자료

| 자료 | 링크 |
|------|------|
| FTDI 드라이버 설치 가이드 | [KocoaFAB 튜토리얼 #489](https://kocoafab.cc/tutorial/view/489) |
| BLE 디바이스 이름 변경 가이드 | [KocoaFAB 튜토리얼 #764](https://kocoafab.cc/tutorial/view/764) |
| MDBT42Q 공식 데이터시트 | [Raytac 제품 승인서 (PDF)](https://www.raytac.com/upload/download_files/c5581093f0b2edd11dd21b3f6afc718c.pdf) |
| Raytac 공식 홈페이지 | [www.raytac.com](https://www.raytac.com) |
| Nordic Semiconductor | [www.nordicsemi.com](https://www.nordicsemi.com) |

---

## 파일 구성

```
iprise/
├── README.md                  # 프로젝트 사용설명서 (이 파일)
└── CDM212364_Setup.exe        # FTDI USB-to-Serial 드라이버 설치 파일
```

---

*본 문서는 iprise 프로젝트의 발명 자료로 작성되었습니다.*
