# mirobot_customs
mirobot for ros noetic service nodes

WLKATA 사의 Mirobot ROS Noetic 의 커스텀 서비스 코드를 만들어놓은 레포지토리입니다.

# custom 폴더 설명

## mirobot controller
### scripts/joint_control_service_node.py
이 파일은 python3 으로 실행 가능하며, 조인트 6개를 수동으로 주면 실행이 가능합니다.
### src/joint_control_node.cpp
서비스 move_joints 를 실행 가능케 하는 코드입니다. 상기한 스크립트의 파이썬 파일을 실행하기 전에 켜져 있어야 합니다. /forward_joint_states 토픽을 송출합니다.
### src/position_monitor_node.cpp
토픽 /joint_states 를 구독받고, 미로봇의 스펙을 기준으로 엔드이펙터(말단 손끝)의 xyz, quaternion 으로 나타내는 코드입니다. /end_effector_pose 토픽을 송출합니다.
### src/inverse_kinematic_solver_node.cpp
토픽 /target_pose 를 받아서 제어에 필요한 joint 6개의 값을 계산하는 코드입니다. /inverse_joint_states 토픽을 송출합니다.
### launch/control_system.launch
cpp 파일 3개를 묶어서 실행하는 코드입니다.

---
현재 문제점

- (해결완료)파라미터 조정

  - (해결완료)영점 조절 후 엔드 이펙터의 위치와 계산된 엔드 이펙터의 포즈가 맞지 않습니다.

- 노드/토픽간 연결
  
  - /inverse_joint_states 토픽을 /move_joints 서비스로 연결하는 코드가 필요합니다.

- (해결완료)정체불명의 연결 문제
  
  - (해결완료) /move_joints 서비스 호출 시 기존 /joint_states 토픽과 충돌하는 경우가 있습니다. 충돌 시 rviz에서 영점 조인트(0,0,0,0,0,0)과 호출시킨 조인트 간 포즈가 왔다갔다 합니다.
  - (해결방법) joint_state_publisher 패키지를 참조하여 source_list 에 받아야하는 토픽을 파라미터로 선언해줍니다. 선언된 토픽을 구독하며, 서비스에서 송출하는 토픽명을 변경하였습니다.
