# Bizkit Frontend

> 팀의 스프린트와 이슈를 한눈에 관리하는 프로젝트 협업 도구

![React](https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-5.7-3178C6?logo=typescript&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-6.3-646CFF?logo=vite&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-4.1-06B6D4?logo=tailwindcss&logoColor=white)
![Zustand](https://img.shields.io/badge/Zustand-5.0-433E38)

---

## 목차

- [기술 스택](#기술-스택)
- [시작하기](#시작하기)
- [프로젝트 구조](#프로젝트-구조)
- [페이지 목록](#페이지-목록)
- [개발 규칙](#개발-규칙)

---

## 기술 스택

| 분류 | 기술 |
|------|------|
| Framework | React 19, TypeScript 5.7 |
| Build | Vite 6.3 |
| Styling | Tailwind CSS 4.1 |
| State Management | Zustand 5 |
| Routing | React Router 7 |
| HTTP | Axios 1.9 |
| Drag & Drop | @hello-pangea/dnd |
| Icons | Lucide React |
| Architecture | Feature-Sliced Design (FSD) |
| Linting | ESLint 9, Prettier 3 |

---

## 시작하기

### Prerequisites

- **Node.js** 18.0.0 이상
- **npm** 9.0.0 이상

```bash
node -v  # v18.x.x 이상 확인
```

### Installation

```bash
# 저장소 클론 후 frontend 디렉토리로 이동
cd frontend

# 의존성 설치
npm install
```

### 개발 서버 실행

```bash
# FSD 아키텍처 검사 + 타입 체크 + 개발 서버 동시 실행
npm run dev
```

개발 서버: `http://localhost:5173`

> API 요청(`/api/*`)은 Vite 프록시를 통해 `https://api.bizkit.dukcode.org`로 자동 전달됩니다.

### 그 외 스크립트

```bash
npm run build          # 프로덕션 빌드
npm run preview        # 빌드 결과 미리보기
npm run lint           # ESLint 검사
npm run format         # Prettier 포맷팅 적용
npm run format:check   # Prettier 포맷팅 검사
npm run check:fsd      # FSD 아키텍처 구조 검사
```

---

## 프로젝트 구조

[Feature-Sliced Design (FSD)](https://feature-sliced.design/) 아키텍처를 따릅니다.
레이어 간 의존 방향은 **단방향**으로 고정됩니다: `app → pages → widgets → features → entities → shared`

```
src/
├── app/          # 앱 진입점, 전역 라우터, 레이아웃, 전역 스타일
├── pages/        # 각 URL에 대응하는 페이지 컴포넌트
├── widgets/      # 여러 페이지에서 재사용하는 복합 UI 블록
├── features/     # 단일 비즈니스 기능 단위 (사용자 액션 포함)
├── entities/     # 도메인 모델과 그에 대한 UI/API/상태
└── shared/       # 도메인 비종속 공용 컴포넌트, 유틸, API 설정
```

각 레이어 내부의 슬라이스는 다음 세그먼트로 구성됩니다.

```
<slice>/
├── api/    # API 호출
├── model/  # 타입 정의, 상태(store)
├── lib/    # 유틸 함수, 훅
└── ui/     # React 컴포넌트
```

### 주요 레이어 상세

```
pages/
├── backlog/      # 백로그 관리
├── sprint/       # 스프린트 보드
├── my-works/     # 내 이슈 목록
├── home/         # 홈 (프로젝트 선택)
├── onboarding/   # 프로젝트 생성/참여
├── profile/      # 프로필
├── settings/     # 프로젝트 설정
├── signin/       # 로그인
├── signup/       # 회원가입
├── invitation/   # 초대 처리
└── error/        # 에러 페이지

entities/
├── issue/        # 이슈 도메인
├── sprint/       # 스프린트 도메인
├── epic/         # 에픽 도메인
├── component/    # 컴포넌트 도메인
├── member/       # 멤버 도메인
├── project/      # 프로젝트 도메인
└── user/         # 유저 도메인

widgets/
├── issue-detail-modal/    # 이슈 상세 모달
└── top-navigation-bar/    # 상단 네비게이션 바
```

---

## 페이지 목록

| 경로 | 페이지 | 설명 |
|------|--------|------|
| `/signin` | 로그인 | 이메일/비밀번호 로그인 |
| `/signup` | 회원가입 | 계정 생성 |
| `/` | 홈 | 프로젝트 목록 |
| `/onboarding` | 온보딩 | 프로젝트 생성 또는 참여 |
| `/:projectId/sprint` | 스프린트 보드 | 드래그앤드롭 이슈 관리 |
| `/:projectId/backlog` | 백로그 | 미완료 이슈 목록 |
| `/:projectId/my-works` | 내 작업 | 내게 할당된 이슈 |
| `/:projectId/settings` | 설정 | 프로젝트 설정 및 멤버 관리 |
| `/profile` | 프로필 | 내 정보 수정 |
| `/invitation` | 초대 | 프로젝트 초대 수락 |

---

## 개발 규칙

### 코드 스타일

- **ESLint** + **Prettier**로 코드 스타일을 통일합니다.
- PR 전 반드시 포맷팅을 확인하세요.

```bash
npm run format        # 포맷팅 자동 수정
npm run lint          # 린트 검사
```

- Print width: 100
- Tailwind 클래스는 Prettier 플러그인이 자동 정렬

### FSD 아키텍처 규칙

- 상위 레이어가 하위 레이어를 import 하는 것만 허용됩니다.
- 같은 레이어 내 슬라이스 간 cross-import는 금지합니다 (`shared` 제외).
- PR 전 FSD 검사를 통과해야 합니다.

```bash
npm run check:fsd
```

### 경로 별칭

`@/`는 `src/` 디렉토리의 절대 경로 별칭입니다.

```ts
// ✅ 올바른 import
import { Button } from '@/shared/ui';

// ❌ 상대 경로 지양
import { Button } from '../../../shared/ui';
```

### 커밋 메시지 컨벤션

```
<type>: <subject>

feat     새로운 기능
fix      버그 수정
refactor 코드 리팩토링 (기능 변경 없음)
style    코드 스타일 수정 (포맷팅 등)
chore    빌드, 설정 파일 수정
docs     문서 수정
test     테스트 추가/수정
```

예시:
```
feat: 스프린트 보드 드래그앤드롭 이슈 이동 구현
fix: 백로그 무한 스크롤 중복 호출 버그 수정
```

### 브랜치 전략

```
main          배포 브랜치
└── frontend  프론트엔드 통합 브랜치
    └── fe/<issue-id>-<description>  기능 개발 브랜치
```

예시:
```
fe/S12P31D207-42-sprint-board-drag-drop
```
