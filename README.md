# ROS2-Autonomous-Driving-Architecture

본 프로젝트는 ROS2 Humble을 기반으로 3개의 자율주행 미션을 수행하는 모듈형 자율주행 시스템을 설계하고 구현한 프로젝트입니다.  
인지-판단-제어의 효율적인 데이터 파이프라인을 노드화하여 구축했으며, 유아용 전동차에 입혀 트랙 주행에 성공했습니다.

---

## Demo

### Video_Link

**Link for driving video**
https://drive.google.com/drive/folders/1upF1mNUlB2NO9lO2neyLbMjiGGSPX5eX

**Link for presentation video**
https://drive.google.com/drive/folders/1GYMJ3wTFHcRrzuolcIzSicu3ZcszoKqJ

---

## Overview

- 인지부: 카메라와 LiDAR 센서 데이터를 융합해 차선, 장애물 차량, 신호등, 횡단보도 실시간 인지
- 판단부: 인지 데이터를 분석해 최적의 조향 각도와 목표 속도 결정
- 제어부: 조향 각도와 목표 속도를 명령 패킷으로 변환하여 차량 구동
- 구동: 유아용 전동차에 아두이노 기반의 제어 명령 전달, 트랙 주행

### Key features

- YOLOv8 Based Detection
- Modular ROS2 Architecture
- Sensor Fusion
- UART Communication

---

## System Architecture

카메라와 LiDAR 센서에서 수집된 데이터는 인지부에서 처리되고, 판단부에서는 미션에 따라 적절한 주행 상태를 결정합니다.  
그 후 제어부는 명령 command를 생성해 UART 통신으로 주행 차량에 명령을 전달합니다.  

<img width="1142" height="920" alt="image" src="https://github.com/user-attachments/assets/4e64a01e-5b45-4ecb-94db-1d88468863eb" />


### 1. 인지부 (Perception Layer)
- Sensing: 카메라, LiDAR 센서 데이터를 활용해 도로 상황 파악
- Computer Vision: OpenCV를 활용한 트랙 내 차선(Lane) 검출 및 신호(Signal) 인지 프로세스 최적화

### 2. 판단부 (Decision Making Layer)
- FSM-based Decision Making: 인지 데이터를 활용해 미션별 Finite State Machine 설계
- Path Planning: Stanley method 기반 조향각 계산

### 3. 제어부 (Control Layer)
- Actuation: 하위 구동체와의 UART 시리얼 통신을 통해 조향 및 구동 제어 명령 하달

---

## Hardware Platform

해당 자율주행 시스템은 유아용 전동차에 탑재됩니다. 

### Vehicle Platform
- Ride-on electric vehicle body
- Drive motor
- Steering motor
- Wheels
- Battery

### Control Unit
- Arduino microcontroller
- Potentiometer (steering control)

### Sensors
- Camera: YOLO 기반 차선/장애물 차량/신호등/횡단보도 감지
- LiDAR: 거리 및 각도별 장애물 유무 감지

### Hardware Stack
- Computer: MSI Cyborg 15 A13VF
- CPU: 13th Gen Intel(R) Core(TM i7-13620H
- GPU: NVIDIA GeForce RTX 4060
- DRAM: DDR5 32GB

---

## Software Framework
- ROS2 기반 Architecture
- 인지부: 도로 상황 파악
- 판단부: 미션별 FSM 기반 알고리즘
- 제어부: 주행 차량의 조향 및 속도 control

### Software Stack
| 분류 | 상세 기술 |
| :--- | :--- |
| OS | Ubuntu 22.04.5 LTS|
| 구동 Framework | ROS2 Humble |
| Node 동작 기술 | Python 3 |
| 딥러닝 연산 처리 | Pytorch |
| YOLO Segmentation 처리 | Ultralytics |
| Libraries | OpenCV, NumPy, SciPy, Sensor-Fusion |

---

## Mission Scenarios

시스템 설계의 유연성을 검증하기 위해 3가지의 상이한 미션을 완수했습니다.

1. 트랙 주파 (Track Traversal)
   - 정해진 트랙 최단 시간 주파 및 곡선 구간 조향 최적화

2. 장애물 회피 및 신호등 인식 (Obstacle Avoidance & Traffic Signal Recognition)
   - 센서 퓨전 기반 동적/정적 장애물 차량 회피
   - YOLO 기반 신호등 인지 및 동기화, 알맞게 정지/출발

3. 주차 (Autonomous Parking)
   - 주차 공간 확인 및 후면 주차 경로 생성 및 실시간 보정

---

## Results

유아용 전동차 트랙 주행 결과, 모든 미션 5회 이상 주행 성공
학부 수업 자율주행캡스톤디자인 자체 경진대회 1위

---

## Repository Structure

```
ROS2-Autonomous-Driving-Architecture
├── code/                             # ROS2 기반 자율주행 시스템 전체 코드
|   └── src/                          # 소스 코드
│       ├── arduino_pkg/              # 아두이노 하위 제어 로직
│       ├── camera_perception_pkg/    # 카메라 기반 도로 상황 인식
│       ├── debug_pkg/                # 시스템 모니터링 및 디버깅
│       ├── decision_making_pkg/      # FSM 기반 판단부 모듈
│       ├── execution_pkg/            # 최종 제어 명령 실행 모듈
│       ├── interfaces_pkg/           # 노드 간 통신을 위한 커스텀 메시지
│       ├── lidar_perception_pkg/     # LiDAR 기반 장애물 유무 감지
│       └── serial_communication_pkg/ # PC-차량 플랫폼 간 UART 통신 인터페이스
|   ├── to_m1.sh                      # 판단부를 트랙 주파 모드로 설정
|   ├── to_m2.sh                      # 판단부를 장애물 회피 및 신호등 인식 모드로 설정
|   └── to_m3.sh                      # 판단부를 주차 모드로 설정
|
├── docs/                             # 프로젝트 기술 문서 및 활동 자료
│   ├── presentation/                 # 교내 온라인 발표회 발표 자료
│   ├── Hardware_Setup.md             # 하드웨어 구성 및 연결
│   └── Terminal_Setting.md           # 시스템 구동을 위한 터미널 가이드
|
└── README.md                         # 프로젝트 개요, 시스템 아키텍처 등 안내 문서
```

---

## Acknowledgement

This project was developed as part of the Autonomous Driving Advanced Course at Sungkyunkwan University.  

The experimental platform and basic framework were provided during the course.  
The perception pipeline using camera and LiDAR, mission algorithm, and the overall system integration were implemented in this project.
