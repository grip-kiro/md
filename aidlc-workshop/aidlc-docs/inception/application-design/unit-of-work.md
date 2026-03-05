# 테이블오더 서비스 — Unit of Work 정의

## 분해 전략
모놀리식 백엔드 + 별도 프론트엔드 2개 구조. 총 3개 유닛으로 분해.
백엔드가 핵심 의존성이므로 백엔드를 먼저 개발하고, 프론트엔드 2개는 병렬 개발 가능.

---

## Unit 1: Backend API (백엔드)

**유형**: Service (독립 배포 가능)
**기술**: Java 17+ / Spring Boot 3.x / MySQL / Flyway
**범위**: 모든 비즈니스 로직, 데이터 관리, 인증/인가, SSE

**모듈 구성**:
- auth — 인증/인가 (JWT, 로그인 시도 제한)
- menu — 메뉴/카테고리 CRUD, 캐싱
- order — 주문 생성/조회/삭제/상태 변경
- table — 테이블 등록/관리/세션 관리
- sse — Server-Sent Events 발행
- common — 공통 설정, 에러 처리, DTO

**산출물**:
- Spring Boot 프로젝트 (Gradle)
- Flyway 마이그레이션 스크립트
- REST API 엔드포인트
- SSE 엔드포인트

**개발 순서**: 1순위 (프론트엔드의 의존성)

---

## Unit 2: Customer App (고객용 앱)

**유형**: Module (별도 배포, 백엔드 의존)
**기술**: React 18+ / TypeScript / Vite
**범위**: 고객 대면 기능 (메뉴 탐색, 장바구니, 주문, 주문 내역)

**주요 페이지/컴포넌트**:
- SetupPage — 태블릿 초기 설정 (마스터 PIN → 테이블 연결)
- MenuPage — 메뉴 탐색 (사이드바 + 3열 그리드)
- MenuDetailModal — 메뉴 상세 모달
- CartDrawer/CartPage — 장바구니
- OrderConfirmPage — 주문 확인/확정
- OrderSuccessPage — 주문 성공 (5초 표시)
- OrderHistoryPage — 주문 내역 조회

**산출물**:
- React 프로젝트 (Vite + TypeScript)
- API 클라이언트 (axios/fetch)
- SSE 클라이언트 (세션 종료 감지)

**개발 순서**: 2순위 (백엔드 API 완료 후, Admin App과 병렬 가능)

---

## Unit 3: Admin App (관리자용 앱)

**유형**: Module (별도 배포, 백엔드 의존)
**기술**: React 18+ / TypeScript / Vite
**범위**: 관리자 기능 (로그인, 대시보드, 테이블/메뉴/카테고리 관리)

**주요 페이지/컴포넌트**:
- LoginPage — 관리자 로그인
- DashboardPage — 실시간 주문 대시보드 (테이블별 그리드)
- TableDetailModal — 테이블 주문 상세
- TableManagementPage — 테이블 등록/관리
- MenuManagementPage — 메뉴 CRUD (테이블 형태)
- CategoryManagementPage — 카테고리 CRUD
- OrderHistoryModal — 과거 주문 내역

**산출물**:
- React 프로젝트 (Vite + TypeScript)
- API 클라이언트 (axios/fetch)
- SSE 클라이언트 (실시간 주문 업데이트)

**개발 순서**: 2순위 (백엔드 API 완료 후, Customer App과 병렬 가능)

---

## 코드 구조

```
aidlc-workshop/
+-- backend/                    # Unit 1: Spring Boot API
|   +-- src/main/java/com/tableorder/
|   |   +-- auth/
|   |   +-- menu/
|   |   +-- order/
|   |   +-- table/
|   |   +-- sse/
|   |   +-- common/
|   +-- src/main/resources/
|   |   +-- db/migration/       # Flyway
|   |   +-- application.yml
|   +-- build.gradle
+-- customer-app/               # Unit 2: React Customer App
|   +-- src/
|   |   +-- pages/
|   |   +-- components/
|   |   +-- api/
|   |   +-- hooks/
|   |   +-- store/
|   +-- package.json
+-- admin-app/                  # Unit 3: React Admin App
|   +-- src/
|   |   +-- pages/
|   |   +-- components/
|   |   +-- api/
|   |   +-- hooks/
|   |   +-- store/
|   +-- package.json
+-- aidlc-docs/                 # 문서 (코드 아님)
```
