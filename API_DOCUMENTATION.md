# ToDo List API 문서

## 개요
Django REST Framework를 사용한 실무 기반 ToDo List API입니다.

## 주요 기능
- 할 일 CRUD 작업
- 카테고리 및 태그 관리
- 댓글 및 첨부파일 지원
- 고급 필터링 및 검색
- 사용자별 권한 관리
- 통계 및 대시보드

## 설치 및 실행

### 1. 의존성 설치
```bash
uv sync
```

### 2. 데이터베이스 마이그레이션
```bash
uv run python manage.py migrate
```

### 3. 슈퍼유저 생성
```bash
uv run python manage.py createsuperuser
```

### 4. 서버 실행
```bash
uv run python manage.py runserver
```

## API 엔드포인트

### 인증
- **POST** `/api/auth/token/` - 인증 토큰 발급

### 할 일 (Tasks)
- **GET** `/api/tasks/` - 할 일 목록 조회
- **POST** `/api/tasks/` - 할 일 생성
- **GET** `/api/tasks/{id}/` - 할 일 상세 조회
- **PUT** `/api/tasks/{id}/` - 할 일 전체 수정
- **PATCH** `/api/tasks/{id}/` - 할 일 부분 수정
- **DELETE** `/api/tasks/{id}/` - 할 일 삭제

#### 할 일 특별 엔드포인트
- **GET** `/api/tasks/my_tasks/` - 내 할 일 목록
- **GET** `/api/tasks/overdue/` - 마감일이 지난 할 일
- **GET** `/api/tasks/today/` - 오늘 마감인 할 일
- **GET** `/api/tasks/important/` - 중요한 할 일
- **GET** `/api/tasks/completed/` - 완료된 할 일
- **GET** `/api/tasks/statistics/` - 할 일 통계
- **PATCH** `/api/tasks/{id}/mark_complete/` - 할 일 완료 처리
- **PATCH** `/api/tasks/{id}/mark_in_progress/` - 할 일 진행 중 처리
- **PATCH** `/api/tasks/{id}/update_progress/` - 진행률 업데이트

#### 할 일 댓글
- **GET** `/api/tasks/{id}/comments/` - 댓글 목록
- **POST** `/api/tasks/{id}/comments/` - 댓글 작성

#### 할 일 첨부파일
- **GET** `/api/tasks/{id}/attachments/` - 첨부파일 목록
- **POST** `/api/tasks/{id}/attachments/` - 첨부파일 업로드

### 카테고리 (Categories)
- **GET** `/api/categories/` - 카테고리 목록
- **POST** `/api/categories/` - 카테고리 생성
- **GET** `/api/categories/{id}/` - 카테고리 상세
- **PUT** `/api/categories/{id}/` - 카테고리 수정
- **DELETE** `/api/categories/{id}/` - 카테고리 삭제
- **GET** `/api/categories/{id}/tasks/` - 카테고리별 할 일 목록

### 태그 (Tags)
- **GET** `/api/tags/` - 태그 목록
- **POST** `/api/tags/` - 태그 생성
- **GET** `/api/tags/{id}/` - 태그 상세
- **PUT** `/api/tags/{id}/` - 태그 수정
- **DELETE** `/api/tags/{id}/` - 태그 삭제
- **GET** `/api/tags/{id}/tasks/` - 태그별 할 일 목록

## 필터링 및 검색

### 할 일 필터링 옵션
- `status` - 상태 (todo, in_progress, review, done, cancelled)
- `priority` - 우선순위 (low, medium, high, urgent)
- `category` - 카테고리 ID
- `tags` - 태그 ID들 (복수 선택 가능)
- `is_important` - 중요 여부 (true/false)
- `is_archived` - 보관 여부 (true/false)
- `is_overdue` - 마감일 지남 여부 (true/false)
- `due_date_before` - 마감일 이전
- `due_date_after` - 마감일 이후
- `progress_min` - 최소 진행률
- `progress_max` - 최대 진행률
- `due_today` - 오늘 마감 (true/false)
- `due_this_week` - 이번 주 마감 (true/false)
- `due_this_month` - 이번 달 마감 (true/false)
- `search` - 통합 검색 (제목, 설명, 카테고리, 태그)

### 정렬 옵션
- `ordering` - 정렬 필드 (created_at, updated_at, due_date, priority, status)
- `-` 접두사로 내림차순 정렬 가능

### 페이지네이션
- 기본 페이지 크기: 20개
- `page` 파라미터로 페이지 번호 지정

## 데이터 모델

### Task (할 일)
```json
{
  "id": 1,
  "title": "할 일 제목",
  "description": "할 일 설명",
  "user": {
    "id": 1,
    "username": "user1"
  },
  "category": {
    "id": 1,
    "name": "업무",
    "color": "#007bff"
  },
  "tags": [
    {
      "id": 1,
      "name": "긴급",
      "color": "#dc3545"
    }
  ],
  "priority": "high",
  "status": "in_progress",
  "due_date": "2024-01-15T10:00:00Z",
  "completed_at": null,
  "is_important": true,
  "is_archived": false,
  "progress": 50,
  "estimated_hours": 8.0,
  "actual_hours": 4.0,
  "created_at": "2024-01-10T09:00:00Z",
  "updated_at": "2024-01-12T14:30:00Z",
  "is_overdue": false,
  "days_until_due": 3,
  "comment_count": 2
}
```

### Category (카테고리)
```json
{
  "id": 1,
  "name": "업무",
  "color": "#007bff",
  "description": "업무 관련 할 일",
  "created_at": "2024-01-10T09:00:00Z",
  "updated_at": "2024-01-10T09:00:00Z",
  "task_count": 5
}
```

### Tag (태그)
```json
{
  "id": 1,
  "name": "긴급",
  "color": "#dc3545",
  "created_at": "2024-01-10T09:00:00Z",
  "task_count": 3
}
```

## 인증

### 토큰 인증
```bash
# 토큰 발급
curl -X POST http://localhost:8000/api/auth/token/ \
  -H "Content-Type: application/json" \
  -d '{"username": "admin", "password": "admin123"}'

# 토큰 사용
curl -H "Authorization: Token YOUR_TOKEN_HERE" \
  http://localhost:8000/api/tasks/
```

## 사용 예시

### 1. 할 일 생성
```bash
curl -X POST http://localhost:8000/api/tasks/ \
  -H "Authorization: Token YOUR_TOKEN_HERE" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "새로운 할 일",
    "description": "할 일 설명",
    "priority": "high",
    "due_date": "2024-01-15T10:00:00Z",
    "is_important": true,
    "category_id": 1,
    "tag_ids": [1, 2]
  }'
```

### 2. 할 일 필터링
```bash
# 오늘 마감인 중요한 할 일
curl "http://localhost:8000/api/tasks/?due_today=true&is_important=true" \
  -H "Authorization: Token YOUR_TOKEN_HERE"

# 진행 중인 할 일을 우선순위 순으로 정렬
curl "http://localhost:8000/api/tasks/?status=in_progress&ordering=-priority" \
  -H "Authorization: Token YOUR_TOKEN_HERE"
```

### 3. 통계 조회
```bash
curl http://localhost:8000/api/tasks/statistics/ \
  -H "Authorization: Token YOUR_TOKEN_HERE"
```

## Django Admin

관리자 페이지: http://localhost:8000/admin/
- 기본 계정: admin / admin123

## 주요 특징

1. **실무 기반 설계**: 실제 프로젝트에서 사용할 수 있는 수준의 기능
2. **권한 관리**: 사용자별 데이터 격리 및 권한 제어
3. **고급 필터링**: 다양한 조건으로 할 일 검색 및 필터링
4. **성능 최적화**: select_related, prefetch_related 사용
5. **확장성**: 모듈화된 구조로 기능 추가 용이
6. **한국어 지원**: 한국어 라벨 및 메시지
7. **파일 업로드**: 첨부파일 지원
8. **댓글 시스템**: 할 일별 댓글 기능
9. **통계 기능**: 진행률, 완료율 등 통계 제공
10. **마감일 관리**: 마감일 추적 및 알림 기능
