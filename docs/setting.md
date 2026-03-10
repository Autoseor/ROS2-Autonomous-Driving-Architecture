# setting

## 초기 설정
colcon build --symlink-install로 빌드 작업을 우선적으로 수행해야 함  
image_publisher.py에서 FRAME_SRC의 경로를 본인의 환경에 맞게 설정해야 정상 동작함  
ex) FRAME_SRC = "/home/[사용자 이름]/ros2_merge/src/camera_perception_pkg/camera_perception_pkg/lib/test_video.mp4"  
(Github로부터 동일한 이름으로 Clone했다는 전제에서, 사용자 이름에 해당하는 부분만 변경하면 동작에 문제가 없을 것임)

---

## 구동

### 사전 실행 명령어
```
cd ~/ros2_merge
colcon build --symlink-install   
sudo chmod 777 *.sh  
source ./install/setup.bash
```

### Mission 1
```
./to_m1.sh  
ros2 launch execution_pkg m1.py
ros2 run serial_communication_pkg bridge
```

### Mission 2

```
./to_m2.sh  
ros2 launch execution_pkg m2.py
ros2 run camera_perception_pkg image_publisher
```


### Mission 3
```
./to_m3.sh  
ros2 launch execution_pkg m3.py
ros2 run camera_perception_pkg image_publisher_rear
```
