<!-- i18n-source: 03-skills/doc-generator/SKILL.md -->
<!-- i18n-source-sha: TBD -->
<!-- i18n-date: 2026-07-01 -->

---
name: api-documentation-generator
description: 소스 코드에서 종합적이고 정확한 API 문서를 생성합니다. API 문서를 만들거나 업데이트할 때, OpenAPI 명세를 생성할 때, 또는 사용자가 API 문서, 엔드포인트, 문서화를 언급할 때 사용합니다.
---

# API 문서 생성기 스킬

## 생성 항목

- OpenAPI/Swagger 명세
- API 엔드포인트 문서
- SDK 사용 예제
- 통합 가이드
- 오류 코드 참조
- 인증 가이드

## 문서 구조

### 각 엔드포인트에 대해

```markdown
## GET /api/v1/users/:id

### 설명
이 엔드포인트가 하는 일에 대한 간략한 설명

### 매개변수

| 이름 | 타입 | 필수 | 설명 |
|------|------|----------|-------------|
| id | string | 예 | 사용자 ID |

### 응답

**200 성공**
```json
{
  "id": "usr_123",
  "name": "John Doe",
  "email": "john@example.com",
  "created_at": "2025-01-15T10:30:00Z"
}
```

**404 찾을 수 없음**
```json
{
  "error": "USER_NOT_FOUND",
  "message": "사용자가 존재하지 않습니다"
}
```

### 예제

**cURL**
```bash
curl -X GET "https://api.example.com/api/v1/users/usr_123" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

**JavaScript**
```javascript
const user = await fetch('/api/v1/users/usr_123', {
  headers: { 'Authorization': 'Bearer token' }
}).then(r => r.json());
```

**Python**
```python
response = requests.get(
    'https://api.example.com/api/v1/users/usr_123',
    headers={'Authorization': 'Bearer token'}
)
user = response.json()
```
```
