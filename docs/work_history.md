# 📝 Open-RMF Web 프로젝트 작업 이력 (Work History)

이 문서는 Open-RMF Web 커스텀 관제 프로젝트의 작업 진행 상황, 환경 설정 변경 내역, 주요 마일스톤 및 다음 계획을 시간 순으로 누적 기록하는 공간입니다.  
**새로운 세션을 시작하는 에이전트는 반드시 이 문서를 가장 먼저 확인하여 작업의 연속성을 유지해야 합니다.**

---

## 📅 작업 이력 로그

### 2026-08-13 (목)

#### 🕒 [16:00 ~ 16:35] Git 브랜치 관리 체계 구축 및 Fork 원격 저장소 설정
* **작업 배경:** 공식 저장소(`open-rmf/rmf-web`)의 `jazzy` 브랜치에 로컬 VM 설정 및 커스텀 로고 작업이 커밋되지 않은 채 혼재되어 있어 정석적인 Git 워크플로우로 정리 필요.
* **작업 내용:**
  1. **작업 브랜치 분리:** `demo_test` 브랜치 생성 및 워킹 트리 변경 사항 이동.
  2. **빌드 산출물 제외 (`.gitignore` 수정):** `build/`, `install/`, `log/`, `.vscode/` 디렉토리를 Git 추적에서 제외.
  3. **로컬 작업 커밋:** 패키지 설정(`package.json`), 대시보드 리소스/예제 코드 수정본 및 VM 가이드 문서(`ln_my_command.md`) 커밋 완료 (`492758fd`).
  4. **원격 저장소 추가 및 푸시:**
     * `origin`: `https://github.com/open-rmf/rmf-web.git` (공식 원본)
     * `my-fork`: `git@github.com:ykkim-temaat/rmf-web.git` (개인 Fork)
     * `demo_test` 브랜치 푸시 완료 (`git push -u my-fork demo_test`).
  5. **원본 브랜치 확인:** `jazzy` 브랜치는 원본 clone 상태(Clean)로 온전히 보존됨을 검증.

#### 🕒 [16:35 ~ 16:45] 에이전트 행동 규칙 수립 및 작업 이력 체계 구축
* **작업 내용:**
  1. **`.agents/AGENTS.md` 생성:**
     * 한국어 응답 원칙
     * 채팅창 LaTeX 수식 깨짐 방지 표기 규칙
     * 코드 수정 전 **사전 승인 철칙**
     * ROS 2 환경(`get_ros`) 로드 규칙
     * Git 브랜치(`demo_test` 작업 / `my-fork` 푸시) 관리 규칙
     * `ln_my_command.md` 가이드 최신화 및 `pnpm` 모노레포 관리 지침
     * **세션 시작 시 `docs/work_history.md` 우선 확인 및 이력 누적 갱신 규칙** 추가
  2. **`docs/work_history.md` 생성:** 향후 세션 간 연속성 유지를 위한 이력 템플릿 및 초기 로그 작성.

#### 🕒 [16:55 ~ 17:20] 시스템 아키텍처 분석, rmf_ws 연동 에러 원인 규명 및 세션 정리
* **작업 배경:** 
  1. `ros2 launch rmf_demos_gz office.launch.xml` 실행 시 발생한 `FileNotFoundError (simulation.launch.xml)` 분석 요청.
  2. `rmf_ws/`(로봇 Core)와 `rmf-web/`(웹 모노레포)의 역할 차이 및 공식 원본 대비 수정 현황 파악.
* **작업 내용:**
  1. **런치 파일 에러 원인 및 해결책 규명:**
     * `~/ros/rmf_ws/src/rmf_demos/rmf_demos_gz/CMakeLists.txt`에서 `install(DIRECTORY launch ...)`에 슬래시(`/`)가 빠져 `share/rmf_demos_gz/launch/` 하위로 빌드됨.
     * `install(DIRECTORY launch/ ...)`로 수정 후 `colcon build --packages-select rmf_demos_gz --symlink-install` 필요함을 확인.
     * `get_ros` 외에 `source ~/ros/rmf_ws/install/setup.bash` 로드 필수화 (`ln_my_command.md` 반영).
  2. **`rmf-web` 공식 저장소 대비 수정 현황 종합 분석:**
     * 수정 파일: 총 12개 (116 lines 추가, 4 lines 수정).
     * 로고 에셋 교체, pnpm 의존성 설정, 가이드 문서/규칙 추가 외에 코어 API/대시보드 로직은 순수 오픈소스 원본 그대로 온전히 보존됨을 검증.
  3. **모노레포 및 `packages/` 6개 서브 패키지 역할 정립:**
     * `rmf-dashboard-framework` (프론트엔드), `api-server` (백엔드 브릿지), `api-client` (SDK), `rmf-models` (데이터 모델), `ros-translator` (변환기), `dashboard-e2e` (통합 테스트).
  4. **가이드 문서(`ln_my_command.md`) 최신화:**
     * IP 변경(`192.168.10.103`), 포트포워딩 포트 추가, `rmf_ws` setup source 명령어 반영.

---

## 📌 현재 시스템 상태 요약
* **현재 작업 브랜치:** `demo_test` (원격 추적: `my-fork/demo_test`)
* **공식 원본 브랜치:** `jazzy` (`origin/jazzy`와 Clean 동기화 유지)
* **VM IP 주소:** `192.168.10.103` (`ens33`)
* **주요 실행 매뉴얼:** [ln_my_command.md](file:///home/yoonki/ros/rmf-web/ln_my_command.md) (Headless Gazebo + API Server + Vite 대시보드 + SSH 포트포워딩)
* **에이전트 규칙:** [.agents/AGENTS.md](file:///home/yoonki/ros/rmf-web/.agents/AGENTS.md)

---

## 🎯 다음 세션 인계 및 진행 예정 작업 (Handover)
> **💡 다음 세션은 `rmf_ws`에서 시작될 예정입니다.**
1. **`rmf_ws` 빌드 오류 수정:**
   * `~/ros/rmf_ws/src/rmf_demos/rmf_demos_gz/CMakeLists.txt`의 8번 라인 `launch`를 `launch/`로 수정.
   * `cd ~/ros/rmf_ws && colcon build --packages-select rmf_demos_gz --symlink-install` 실행.
2. **시뮬레이션 및 웹 대시보드 연동 테스트:**
   * 터미널 1 (Gazebo Headless), 터미널 2 (API Server), 터미널 3 (Dashboard) 순차 실행.
   * 호스트 PC(`http://192.168.10.103:5173`)에서 로봇 위치 렌더링 및 궤적 수신 검증.
3. **커스텀 대시보드 위젯 및 추가 기능 개발 착수:**
   * 요구사항에 따른 모니터링 화면/위젯 커스터마이징.
