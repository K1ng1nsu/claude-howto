<!-- i18n-source: 03-skills/code-review-specialist/templates/finding-template.md -->
<!-- i18n-source-sha: TBD -->
<!-- i18n-date: 2026-07-01 -->

# 코드 리뷰 발견 사항 템플릿

코드 리뷰 중 발견된 각 이슈를 문서화할 때 이 템플릿을 사용하세요.

---

## 이슈: [TITLE]

### 심각도
- [ ] Critical (배포 차단)
- [ ] High (병합 전 수정 필요)
- [ ] Medium (곧 수정 필요)
- [ ] Low (있으면 좋음)

### 카테고리
- [ ] 보안
- [ ] 성능
- [ ] 코드 품질
- [ ] 유지보수성
- [ ] 테스팅
- [ ] 디자인 패턴
- [ ] 문서화

### 위치
**파일:** `src/components/UserCard.tsx`

**줄:** 45-52

**함수/메서드:** `renderUserDetails()`

### 이슈 설명

**내용:** 이슈가 무엇인지 설명합니다.

**중요성:** 영향과 이것이 수정되어야 하는 이유를 설명합니다.

**현재 동작:** 문제가 있는 코드나 동작을 보여줍니다.

**예상 동작:** 대신 무엇이 발생해야 하는지 설명합니다.

### 코드 예제

#### 현재 (문제 있음)

```typescript
// N+1 쿼리 문제를 보여줍니다
const users = fetchUsers();
users.forEach(user => {
  const posts = fetchUserPosts(user.id); // 사용자당 쿼리!
  renderUserPosts(posts);
});
```

#### 제안된 수정

```typescript
// JOIN 쿼리로 최적화
const usersWithPosts = fetchUsersWithPosts();
usersWithPosts.forEach(({ user, posts }) => {
  renderUserPosts(posts);
});
```

### 영향 분석

| 측면 | 영향 | 심각도 |
|--------|--------|----------|
| 성능 | 20명의 사용자에 100개 이상의 쿼리 | High |
| 사용자 경험 | 느린 페이지 로드 | High |
| 확장성 | 대규모에서 중단됨 | Critical |
| 유지보수성 | 디버깅 어려움 | Medium |

### 관련 이슈

- `AdminUserList.tsx` 120번 줄의 유사한 이슈
- 관련 PR: #456
- 관련 이슈: #789

### 추가 자료

- [N+1 쿼리 문제](https://en.wikipedia.org/wiki/N%2B1_problem)
- [데이터베이스 조인 문서](https://docs.example.com/joins)

### 리뷰어 참고 사항

- 이 코드베이스에서 흔한 패턴입니다
- 코드 스타일 가이드에 추가하는 것을 고려하세요
- 헬퍼 함수를 만드는 것이 좋을 수 있습니다

### 작성자 응답 (피드백용)

*코드 작성자가 작성:*

- [ ] 커밋에서 수정 구현됨: `abc123`
- [ ] 수정 상태: 완료 / 진행 중 / 논의 필요
- [ ] 질문 또는 우려사항: (설명)

---

## 발견 사항 통계 (리뷰어용)

여러 발견 사항을 검토할 때 추적:

- **발견된 총 이슈:** X
- **Critical:** X
- **High:** X
- **Medium:** X
- **Low:** X

**권장:** ✅ 승인 / ⚠️ 변경 요청 / 🔄 논의 필요

**전반적 코드 품질:** 1-5점
