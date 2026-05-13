# Bizkit

> 팀의 스프린트와 이슈를 한눈에 관리하는 프로젝트 협업 도구

![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.4.3-6DB33F?logo=springboot&logoColor=white)
![Java](https://img.shields.io/badge/Java-17-007396?logo=openjdk&logoColor=white)
![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.7-3178C6?logo=typescript&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8-4479A1?logo=mysql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-blue?logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?logo=kubernetes&logoColor=white)

---

## 목차

- [프로젝트 소개](#프로젝트-소개)
- [주요 기능](#주요-기능)
- [기술 스택](#기술-스택)
- [서비스 아키텍처](#서비스-아키텍처)
- [시작하기](#시작하기)
- [CI/CD](#cicd)
- [팀 구성](#팀-구성)

---

## 프로젝트 소개

**Bizkit**은 소프트웨어 팀이 프로젝트를 효율적으로 운영할 수 있도록 돕는 이슈 트래킹 및 스프린트 관리 도구입니다.

에픽(Epic) → 스프린트(Sprint) → 이슈(Issue) 계층 구조로 작업을 체계적으로 관리하고, 드래그앤드롭 보드를 통해 이슈 상태를 직관적으로 조작할 수 있습니다.

> SSAFY 12기 S반 최종 프로젝트 (S12P31D207)

---

## 주요 기능

- **프로젝트 관리** — 프로젝트 생성, 멤버 초대 및 권한 관리
- **스프린트 보드** — 드래그앤드롭으로 이슈 상태 이동 (할 일 → 진행 중 → 완료)
- **백로그** — 스프린트에 할당되지 않은 이슈 관리 및 스프린트 배치
- **에픽 / 컴포넌트** — 이슈를 주제별로 분류하는 그룹 단위 관리
- **내 작업** — 나에게 할당된 이슈를 프로젝트 전체에서 한눈에 조회
- **이슈 상세** — 담당자, 우선순위, 스프린트, 에픽, 컴포넌트 인라인 편집
- **무한 스크롤** — 대량 이슈 목록의 커서 기반 페이지네이션

---

## 기술 스택

### Frontend

| 분류                 | 기술                        |
| -------------------- | --------------------------- |
| Language / Framework | TypeScript 5.7, React 19    |
| Build                | Vite 6.3                    |
| Styling              | Tailwind CSS 4.1            |
| State Management     | Zustand 5                   |
| Routing              | React Router 7              |
| HTTP                 | Axios 1.9                   |
| Architecture         | Feature-Sliced Design (FSD) |

### Backend

| 분류                 | 기술                        |
| -------------------- | --------------------------- |
| Language / Framework | Java 17, Spring Boot 3.4.3  |
| Build                | Gradle (멀티 모듈)           |
| Security             | Spring Security + JWT       |
| Database             | JPA + MySQL                 |
| File Storage         | AWS S3                      |
| Email                | Spring Mail + Thymeleaf     |
| API Docs             | Spring REST Docs + AsciiDoc |
| Monitoring           | SonarQube, Sentry           |

### Infrastructure

| 분류          | 기술                |
| ------------- | ------------------- |
| CI/CD         | Jenkins             |
| Container     | Docker (Kaniko)     |
| Orchestration | Kubernetes (GitOps) |
| Notification  | Mattermost          |

---

## 서비스 아키텍처

```
                        ┌─────────────────────────────────┐
                        │            GitLab               │
                        │  (소스 코드 / MR 트리거)         │
                        └────────────┬────────────────────┘
                                     │ Webhook
                                     ▼
                        ┌────────────────────────┐
                        │        Jenkins         │
                        │  lint → build → test   │
                        │  → SonarQube → Docker  │
                        └────────────┬───────────┘
                                     │ image push
                    ┌────────────────▼──────────────────┐
                    │           Docker Hub              │
                    └────────────────┬──────────────────┘
                                     │ GitOps update
                    ┌────────────────▼──────────────────┐
                    │           Kubernetes              │
                    │  ┌──────────────┐  ┌───────────┐  │
                    │  │  API Server  │  │ API Docs  │  │
                    │  │ Spring Boot  │  │   Nginx   │  │
                    │  └──────┬───────┘  └───────────┘  │
                    │         │                         │
                    │  ┌──────▼───────┐                 │
                    │  │    MySQL     │                 │
                    │  └──────────────┘                 │
                    └───────────────────────────────────┘

 User ──── HTTPS ──── React SPA (Vite) ──── /api proxy ──── API Server
```

---

## 시작하기

각 서비스의 상세한 실행 방법은 하위 디렉토리 README를 참고하세요.

### Frontend

```bash
cd frontend
npm install
npm run dev
```

→ 자세한 내용: [frontend/README.md](./frontend/README.md)

### Backend

```bash
cd backend
./gradlew :core:core-api:bootRun
```

**환경 변수 (application.yml 또는 시스템 환경 변수)**

| 변수             | 설명               |
| ---------------- | ------------------ |
| `DB_URL`         | MySQL 접속 URL     |
| `DB_USERNAME`    | DB 사용자명        |
| `DB_PASSWORD`    | DB 비밀번호        |
| `JWT_SECRET`     | JWT 서명 키        |
| `AWS_S3_BUCKET`  | S3 버킷 이름       |
| `AWS_ACCESS_KEY` | AWS Access Key     |
| `AWS_SECRET_KEY` | AWS Secret Key     |
| `MAIL_USERNAME`  | 이메일 발송 계정   |
| `MAIL_PASSWORD`  | 이메일 앱 비밀번호 |

---

## CI/CD

`backend` 브랜치에 push 또는 MR이 생성되면 Jenkins 파이프라인이 자동으로 실행됩니다.

```
1. Check Lint       Spotless 코드 스타일 검사
2. Build            Gradle 빌드
3. Test             JUnit 테스트 실행
4. SonarQube        코드 품질 분석
5. Quality Gate     SonarQube 품질 게이트 통과 여부 확인
6. API Docs         AsciiDoc API 문서 생성
7. Docker Build     API 서버 / API Docs 이미지 빌드 & Push
8. GitOps Update    Kubernetes 매니페스트 이미지 태그 업데이트
```

빌드 결과는 Mattermost로 알림이 전송됩니다.

---

## 팀 구성

| 이름 | 역할           |
| ---- | -------------- |
|   김덕윤   | 팀장 / Backend |
|   채용수   | Backend        |
|   이효미   | Backend        |
|   최재영   | Backend       |
|   이성욱   | Frontend       |
|   전준표   | Frontend       |
