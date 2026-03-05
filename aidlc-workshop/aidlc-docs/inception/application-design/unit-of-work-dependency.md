# 테이블오더 서비스 — Unit of Work 의존성

## 의존성 매트릭스

```
              Backend API   Customer App   Admin App
Backend API       -             -              -
Customer App    REST+SSE        -              -
Admin App       REST+SSE        -              -
```

- Backend API: 독립 (다른 유닛에 의존하지 않음)
- Customer App: Backend API에 의존 (REST API 호출, SSE 수신)
- Admin App: Backend API에 의존 (REST API 호출, SSE 수신)
- Customer App ↔ Admin App: 직접 의존성 없음 (병렬 개발 가능)

## 개발 순서

```
Phase 1: Backend API (필수 선행)
    |
    +---> Phase 2a: Customer App (병렬)
    |
    +---> Phase 2b: Admin App (병렬)
```

## 통합 포인트

| 통합 포인트 | 관련 유닛 | 계약 |
|-------------|-----------|------|
| REST API | Backend ↔ Customer/Admin | API 엔드포인트 (component-methods.md 참조) |
| SSE 주문 업데이트 | Backend → Admin | /api/sse/admin/{storeId} |
| SSE 세션 종료 | Backend → Customer | /api/sse/table/{tableId} |
| JWT 토큰 | Backend ↔ Customer/Admin | Access Token + Refresh Token (쿠키) |

## 리스크

| 리스크 | 영향 | 완화 방안 |
|--------|------|-----------|
| Backend API 지연 | 프론트엔드 개발 차단 | API 명세 먼저 확정, Mock 서버 활용 |
| SSE 연결 불안정 | 실시간 업데이트 누락 | 재연결 로직, 폴링 fallback 고려 |
| JWT 토큰 설계 변경 | 전체 인증 플로우 영향 | 토큰 구조 먼저 확정 후 개발 |
