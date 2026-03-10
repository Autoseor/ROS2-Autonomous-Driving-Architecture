# ROS2-Autonomous-Driving-Architecture

본 프로젝트는 ROS2 기반으로 3개의 자율주행 미션을 수행하는 모듈형 자율주행 시스템을 설계하고 구현한 프로젝트입니다.  
인지-판단-제어의 효율적인 데이터 파이프라인을 노드화하여 구축했으며, 유아용 전동차에 입혀 트랙 주행에 성공했습니다.

---

## Demo


---

## Overview

이 시스템은 카메라와 LiDAR 센서 데이터를 융합해 차선, 장애물 차량, 신호등을 실시간으로 인지합니다.  
정해진 미션에 따라, 판단부에서 인지 데이터를 분석해 최적의 조향 각도와 목표 속도를 결정하며, 제어부는 이를 명령 패킷으로 변환하여 차량을 구동합니다.  
유아용 전동차에 아두이노 기반의 제어 명령을 전달하여 트랙을 주행합니다.

### Key features

- Lane/vehicle/traffic light detection using YOLO
- ROS2 based modular architecture
- Sensor fusion
- UART communication

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

## 수행 미션

시스템 설계의 유연성을 검증하기 위해 3가지의 상이한 미션을 완수했습니다.

1. 트랙 주파 (Track Driving)
   - 비전 알고리즘을 통한 차선 추종 및 곡선 구간 최적 조향 제어

2. 장애물 회피 및 신호 인식
   - LiDAR 센서 퓨전을 통한 동적/정적 장애물 판단 및 회피 경로 생성
   - 비전 기반 신호 체계 인지 및 주행 상태 동기화

3. 자동 주차 (Automatic Parking)
   - 목표 구역 진입을 위한 경로 생성 및 실시간 보정 로직을 통한 주차 시나리오 완수

---

## Results

학부 수업 자율주행캡스톤디자인 자체 경진대회 1위

---

## Repository Structure

```
ROS2-Autonomous-Driving-Architecture
├── code/                            # ROS2 기반 자율주행 시스템 소스 코드
|   ├── src/
│       ├── arduino_pkg/             # 
│       ├── camera_perception_pkg/   # 이미지 데이터를 활용한 차선 및 차량 인식
│       ├── debug_pkg/               #
│       ├── decision_making_pkg/     # FSM 기반 판단부
│       ├── execution_pkg/           # 
│       ├── interfaces_pkg/          # 노드 간 통신을 위한 커스텀 메시지
│       ├── lidar_perception_pkg/    # LiDAR 데이터를 활용한 전후방 장애물 유무 감지
│       └── serial_communication_pkg/ # PC와 차량 플랫폼 간의 UART 통신 인터페이스
|   ├── to_m1.sh
|   ├── to_m2.sh
|   ├── to_m3.sh
|
├── docs/                        # 연구 관련 문서 및 학술 활동 자료
│   ├── presentation/            # IEEE ICCE 국제 학회 채택 논문 및 포스터 자료
│   ├── Hardware_Setup.md        # 산학프로젝트 논문 및 우수 논문 발표 자료
│   └── Setting.md               # 산학프로젝트 논문 및 우수 논문 발표 자료
|
├── media/                       # 프로젝트 시연 및 시각화 자료
│   └── Video_Link/              # 산학 프로젝트 성과교류회 발표 영상
|
└── README.md                    # 프로젝트 개요, 시스템 아키텍처 등 안내 문서
```

---

## Acknowledgement

This project was developed as part of the Autonomous Driving Advanced Course at Sungkyunkwan University.

The experimental platform and basic framework were provided during the course. The perception pipeline using camera and LiDAR, the FSM-based lane-change decision algorithm, and the overall system integration were implemented in this project.
