# FUXA PlantUML 다이어그램

이 폴더에는 FUXA 프로젝트의 전체 구조를 설명하는 PlantUML 다이어그램이 포함되어 있습니다.

## 다이어그램 목록

| 파일 | 제목 | 설명 |
|------|------|------|
| [01_system_overview.puml](01_system_overview.puml) | 전체 시스템 아키텍처 개요 | 클라이언트, 서버, 데이터베이스, 디바이스, 외부 서비스 간의 최상위 관계 |
| [02_server_components.puml](02_server_components.puml) | 서버 컴포넌트 상세 | `server/` 디렉토리의 모든 모듈 (API 레이어, 런타임 엔진, 프로토콜 드라이버) |
| [03_client_components.puml](03_client_components.puml) | Angular 클라이언트 컴포넌트 | `client/src/app/` 내의 페이지, 서비스, 모델, 게이지 위젯 구조 |
| [04_data_flow_sequence.puml](04_data_flow_sequence.puml) | 실시간 데이터 흐름 | 초기 연결, 태그 폴링, 알람 체크, 값 쓰기, 이력 조회의 시퀀스 |
| [05_device_protocol_classes.puml](05_device_protocol_classes.puml) | 디바이스 프로토콜 클래스 | 모든 디바이스 드라이버의 공통 인터페이스와 각 프로토콜 구현 클래스 |
| [06_api_endpoints.puml](06_api_endpoints.puml) | REST API 엔드포인트 | 서버가 제공하는 모든 REST API 경로 목록 |
| [07_deployment.puml](07_deployment.puml) | 배포 구성 | Docker/직접 설치 환경에서의 노드, DB, 외부 서비스 배치 |
| [08_alarm_state_machine.puml](08_alarm_state_machine.puml) | 알람 관리자 상태 머신 | AlarmsManager의 INIT → LOAD → IDLE → STOP 상태 전이 |
| [09_project_data_model.puml](09_project_data_model.puml) | 프로젝트 데이터 모델 | ProjectData, HmiData, View, GaugeSettings, Device, Tag, Alarm 등 핵심 엔티티 관계 |
| [10_startup_sequence.puml](10_startup_sequence.puml) | 서버 시작 순서 | main.js에서 시작하여 모든 런타임 모듈이 초기화되는 순서 |

## 사용 방법

### VS Code에서 렌더링
1. **PlantUML** 확장 프로그램 설치: `jebbs.plantuml`
2. `.puml` 파일 열기
3. `Alt+D` 로 미리보기

### 온라인 렌더링
[PlantUML 온라인 서버](https://www.plantuml.com/plantuml/uml/)에 파일 내용 붙여넣기

### CLI 생성
```bash
java -jar plantuml.jar UML/*.puml
```

## FUXA 아키텍처 요약

```
┌─────────────────────────────────────────────────────────────┐
│                    클라이언트 (Angular SPA)                   │
│  HMI 뷰어 | 에디터 | 디바이스 | 알람 | 스크립트 | 스케줄러   │
└──────────────────────┬──────────────────────────────────────┘
                       │ HTTPS REST + Socket.IO
┌──────────────────────▼──────────────────────────────────────┐
│                    서버 (Node.js)                            │
│  ┌─────────────────────────────────────────────────────┐    │
│  │           Express REST API + JWT 인증                │    │
│  └──────────────────────┬──────────────────────────────┘    │
│  ┌───────────────────────▼─────────────────────────────┐    │
│  │               런타임 엔진                            │    │
│  │  DevicesManager | AlarmsManager | ScriptsManager    │    │
│  │  SchedulerService | JobsManager | NotificatorMgr    │    │
│  │  ProjectManager | UsersManager | DAQStorage         │    │
│  └──────────────────────┬──────────────────────────────┘    │
│                          │ 프로토콜 드라이버               │
│  ┌───────────────────────▼─────────────────────────────┐    │
│  │  OPC-UA | Modbus | S7 | BACnet | MQTT | HTTP | ...  │    │
│  └─────────────────────────────────────────────────────┘    │
└──────────────────────┬──────────────────────────────────────┘
                       │
          ┌────────────┴────────────┐
          ▼                         ▼
   SQLite (Project/DAQ)     산업 디바이스/PLC
   InfluxDB/TDengine/QuestDB
```
