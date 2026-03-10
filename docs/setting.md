# setting

## 초기 설정
1. 먼저 빌드 작업이 수행되어야 함. 코드는 아래와 같음.  
```
colcon build --symlink-install
```
2. image_publisher.py에서 FRAME_SRC를 사용자 환경에 따라 수정해야함.  
e.g.
```
FRAME_SRC = "/home/[사용자 이름]/ros2_merge/src/camera_perception_pkg/camera_perception_pkg/lib/test_video.mp4"  
```

---

## 시스템 구동 방법

### 사전 명령어
```
cd ~/ros2_merge
colcon build --symlink-install   
sudo chmod 777 *.sh  
source ./install/setup.bash
```

### 트랙 주파 모드
```
./to_m1.sh  
ros2 launch execution_pkg m1.py
ros2 run serial_communication_pkg bridge
```

### 장애물 회피 및 신호등 인식 모드

```
./to_m2.sh  
ros2 launch execution_pkg m2.py
ros2 run camera_perception_pkg image_publisher
```


### 주차 모드
```
./to_m3.sh  
ros2 launch execution_pkg m3.py
ros2 run camera_perception_pkg image_publisher_rear
```
