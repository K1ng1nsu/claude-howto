<!-- i18n-source: 07-plugins/documentation/templates/api-endpoint.md -->
<!-- i18n-source-sha: TBD -->
<!-- i18n-date: 2026-07-01 -->

# [METHOD] /api/v1/[endpoint]

## 설명
이 엔드포인트의 기능에 대한 간단한 설명.

## 인증
필요한 인증 방법 (예: Bearer 토큰).

## 파라미터

### 경로 파라미터
| 이름 | 타입 | 필수 | 설명 |
|------|------|----------|-------------|
| id | string | 예 | 리소스 ID |

### 쿼리 파라미터
| 이름 | 타입 | 필수 | 설명 |
|------|------|----------|-------------|
| page | integer | 아니요 | 페이지 번호 (기본값: 1) |
| limit | integer | 아니요 | 페이지당 항목 수 (기본값: 20) |

### 요청 본문
```json
{
  "field": "value"
}
```

## 응답

### 200 OK
```json
{
  "success": true,
  "data": {
    "id": "123",
    "name": "Example"
  }
}
```

### 400 Bad Request
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input"
  }
}
```

### 404 Not Found
```json
{
  "success": false,
  "error": {
    "code": "NOT_FOUND",
    "message": "Resource not found"
  }
}
```

## 예제

### cURL
```bash
curl -X GET "https://api.example.com/api/v1/endpoint" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json"
```

### JavaScript
```javascript
const response = await fetch('/api/v1/endpoint', {
  headers: {
    'Authorization': 'Bearer token',
    'Content-Type': 'application/json'
  }
});
const data = await response.json();
```

### Python
```python
import requests

response = requests.get(
    'https://api.example.com/api/v1/endpoint',
    headers={'Authorization': 'Bearer token'}
)
data = response.json()
```

## 속도 제한
- 인증된 사용자: 시간당 1000개 요청
- 공개 엔드포인트: 시간당 100개 요청

## 관련 엔드포인트
- [GET /api/v1/related](#)
- [POST /api/v1/related](#)
