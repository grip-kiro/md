# AI-DLC State Tracking

## Project Information
- **Project Type**: Greenfield
- **Start Date**: 2026-03-05T09:00:00Z
- **Current Stage**: INCEPTION - Workflow Planning

## Workspace State
- **Existing Code**: No
- **Reverse Engineering Needed**: No
- **Workspace Root**: aidlc-workshop/

## Code Location Rules
- **Application Code**: Workspace root (NEVER in aidlc-docs/)
- **Documentation**: aidlc-docs/ only
- **Structure patterns**: See code-generation.md Critical Rules

## Extension Configuration
| Extension | Enabled | Decided At |
|---|---|---|
| Security Baseline | No | Requirements Analysis |

## Execution Plan Summary
- **Total Stages**: 12
- **Stages to Execute**: 9 (WD, RA, US, WP, AD, UG, FD, CG, BT)
- **Stages to Skip**: 3 (NFR Req, NFR Design, Infra Design)

## Stage Progress
- [x] INCEPTION - Workspace Detection
- [x] INCEPTION - Requirements Analysis
- [x] INCEPTION - User Stories
- [x] INCEPTION - Workflow Planning
- [x] INCEPTION - Application Design
- [x] INCEPTION - Units Generation
- [x] CONSTRUCTION - Functional Design (per-unit) — 4/4 완료
- [ ] CONSTRUCTION - NFR Requirements — SKIP
- [ ] CONSTRUCTION - NFR Design — SKIP
- [ ] CONSTRUCTION - Infrastructure Design — SKIP
- [ ] CONSTRUCTION - Code Generation (per-unit) — Unit 1 완료, Unit 2~4 대기
- [ ] CONSTRUCTION - Build and Test

## Current Status
- **Lifecycle Phase**: CONSTRUCTION
- **Current Stage**: Code Generation — Unit 1: Customer Backend ✅ 완료
- **Next Stage**: Code Generation — Unit 2: Admin Backend
- **Status**: Unit 1 구현 완료, Unit 2 진행 대기

## Unit 1 (Customer Backend) 구현 요약
- **위치**: `table-order-server/`
- **기술 스택**: Spring Boot 3.2.3, JPA, H2 (MySQL 모드), Flyway, JJWT
- **구현 완료 API**:
  - `POST /api/auth/table/login` — 태블릿 로그인 (기기 제한 포함)
  - `POST /api/auth/refresh` — Access Token 갱신
  - `GET /api/stores/{storeId}/menus` — 카테고리별 메뉴 조회
  - `GET /api/menus/{menuId}` — 메뉴 상세 조회
  - `POST /api/orders` — 주문 생성 (가격 스냅샷, 품절 검증)
  - `GET /api/tables/{tableId}/orders` — 세션별 주문 내역 조회
- **인증**: JWT 필터 (`JwtAuthFilter`) — `/api/auth/**` 제외 전체 적용
- **SSE**: MVP에서 스킵 (클라이언트 401 fallback 사용)
- **DB**: Flyway V1 (스키마), V2 (시드 데이터)
