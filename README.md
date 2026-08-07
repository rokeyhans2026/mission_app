# 🐢 Mission App — ROS TurtleBot 프로젝트

ROS(Robot Operating System)와 **TurtleBot3** 기반의 완전 자율 이동 로봇 애플리케이션.
실내 환경에서 지능형 내비게이션, 맵 작성(SLAM), 임무 수행을 위한 소프트웨어 스택입니다.

## 🎯 프로젝트 개요

Mission App은 TurtleBot3를 스스로 판단하고 이동하는 **임무 수행 로봇**으로 만드는 ROS 기반 소프트웨어 스택입니다.
실시간 맵 작성과 위치 추정부터 자율 주행, 목표 지점 순차 임무 수행까지 전 과정을 처리합니다.

## ✨ 주요 기능

- 🗺️ **SLAM 기반 맵 작성** — `gmapping`을 이용한 실시간 지도 구축
- 📍 **AMCL 위치 추정** — 적응형 몬테카를로 위치추정 기반 정밀 자세 추적
- 🧭 **자율 주행** — 동적 장애물 회피를 포함한 move_base 기반 내비게이션
- 🎯 **다중 목표 임무 수행** — 임무를 위한 순차 경유지(Waypoint) 추종
- 📊 **실시간 시각화** — RViz 기반 상태 모니터링
- 🛰️ **모듈형 아키텍처** — 패키지 분리로 확장이 용이한 구조

## 🏗️ 아키텍처

```
mission_app/
├── mission_bringup/       # Launch 파일 & 설정
├── mission_nav/           # 내비게이션 스택 연동
├── mission_slam/          # SLAM & 위치 추정 노드
├── mission_mission/       # 임무/경유지 실행기
└── mission_msgs/          # 커스텀 메시지 정의
```

## 🚀 빠른 시작

### 사전 요구사항

- Ubuntu 20.04 LTS (이상)
- ROS Noetic
- TurtleBot3 (Burger / Waffle)
- Python 3.8+

### 설치

```bash
# 1. 저장소 클론
git clone https://github.com/yourname/mission_app.git
cd mission_app

# 2. 워크스페이스 빌드
catkin_make

# 3. 환경 설정
source devel/setup.bash
```

### 로봇 실행

```bash
# 터미널 1: 로봇 실행 (실제 하드웨어 또는 Gazebo)
export TURTLEBOT3_MODEL=burger
roslaunch mission_bringup turtlebot3_bringup.launch

# 터미널 2: SLAM 맵 작성 시작
roslaunch mission_slam slam.launch

# 터미널 3: RViz 시각화 실행
rosrun rviz rviz -d $(rospack find mission_slam)/rviz/mission.rviz
```

### 임무 실행

```bash
# 임무 전송 (예: 3개 경유지 순찰)
rosrun mission_mission run_mission.py --goals "1,1 2,2 3,3"
```

## 🛠️ 패키지 상세

| 패키지 | 설명 |
|---------|-------------|
| `mission_bringup` | 로봇 bring-up, URDF, launch 파일 |
| `mission_nav` | move_base 설정, costmap, 플래너 |
| `mission_slam` | SLAM + AMCL 위치 추정 |
| `mission_mission` | 경유지 실행기 및 임무 로직 |
| `mission_msgs` | 커스텀 ROS 메시지 타입 |

## 🧪 테스트

```bash
# 유닛 테스트 실행
catkin_make run_tests
```

---

## 🤖 다중 로봇 협업 비전 (MATE)

Mission App의 다음 단계 목표는 **여러 협동 로봇(코봇)이 하나의 임무를 나누어 수행하는 다중 로봇 협업 시스템**입니다.
서로 다른 역할을 가진 로봇들이 ROS 네트워크를 통해 실시간으로 협업하며, 물류 피킹, 공장 생산라인 보조, 실내 탐사 등 다양한 시나리오를 지원합니다.

```
┌─────────────────────────────────────────────────────────┐
│                     임무 스케줄러                        │
│              mission_app (중앙 제어 노드)                 │
└───────────────┬─────────────────────┬───────────────────┘
                │  임무 분배 (task)    │  상태 수집 (status)
   ┌────────────▼─────────┐   ┌────────▼────────────┐
   │  매니퓰레이터 에이전트 │   │     AGV 에이전트      │
   │  (arm_robot)         │   │  (mobile_robot)     │
   └────────────┬─────────┘   └────────┬────────────┘
                │                      │
        ┌───────▼────────┐     ┌───────▼────────┐
        │  MoveIt /      │     │  Nav2 /        │
        │  동작 계획       │     │  자율 주행      │
        └────────────────┘     └────────────────┘
```

### 협업 시나리오

- 🤖 **물류 피킹**: AGV(이동 로봇)와 매니퓰레이터(팔 로봇)가 협업하여 주문 상품을 자동으로 집어 나르는 임무
- 🏭 **생산라인 보조**: 고정형 코봇이 부품 조립, 무거운 물건 리프팅 등 반복 작업을 담당
- 🗺️ **실내 탐사**: 여러 이동 로봇이 영역을 분할 탐색하여 지도를 병합

### 핵심 설계 요소

| 기능 | 설명 |
|------|------|
| 임무 분해 & 분배 | 하나의 큰 임무를 서브태스크로 분해하여 에이전트에게 자동 배분 |
| 협업 조정 | 로봇 간 작업 완료 신호를 기반으로 다음 동작 동기화 |
| 상태 모니터링 | 각 로봇의 진행률·배터리·오류 상태를 실시간 수집 |
| 충돌 회피 | 작업 영역이 겹칠 때 우선순위 기반으로 동작 순서 결정 |
| 장애 복구 | 특정 로봇이 실패하면 다른 로봇이 해당 작업을 인수 |

### 추가 예정 기술

- **ROS 2 (Humble)** — 차세대 ROS 기반으로 전환
- **MoveIt 2** — 매니퓰레이터 역기구학 및 동작 계획
- **Nav2** — 이동 로봇 자율 주행
- **TF2** — 좌표계 변환 및 로봇 간 상대 위치 인식

---

## 📌 로드맵

- [x] 기본 내비게이션 스택
- [x] SLAM 기반 맵 작성
- [ ] 객체 인식 통합 (YOLO)
- [ ] 다중 로봇 협업 (MATE) 시작
- [ ] 웹 기반 원격 대시보드

## 📄 라이선스

MIT License. See [LICENSE](LICENSE) for details.

---

*ROS, TurtleBot3 그리고 많은 커피와 함께 만들어졌습니다 ☕*
