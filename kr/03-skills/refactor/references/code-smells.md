<!-- i18n-source: 03-skills/refactor/references/code-smells.md -->
<!-- i18n-source-sha: TBD -->
<!-- i18n-date: 2026-07-01 -->

# 코드 스멜 카탈로그

Martin Fowler의 *Refactoring* (2판)에 기반한 코드 스멜의 종합적인 참조 자료입니다. 코드 스멜은 더 깊은 문제의 증상입니다—코드 설계에 문제가 있을 수 있음을 나타냅니다.

> "코드 스멜은 일반적으로 시스템의 더 깊은 문제에 해당하는 표면적 징후입니다." — Martin Fowler

---

## 비대화 (Bloaters)

효과적으로 처리하기에는 너무 커진 것을 나타내는 코드 스멜.

### 긴 메서드

**징후:**
- 메서드가 30-50줄 초과
- 전체 메서드를 보려면 스크롤 필요
- 여러 수준의 중첩
- 섹션이 무엇을 하는지 설명하는 주석

**문제점:**
- 이해하기 어려움
- 격리하여 테스트하기 어려움
- 변경이 의도치 않은 결과 초래
- 내부에 중복 로직 숨겨짐

**리팩토링:**
- 메서드 추출
- 임시 변수를 질의로 대체
- 매개변수 객체 도입
- 메서드를 메서드 객체로 대체
- 조건문 분해

**예제 (이전):**
```javascript
function processOrder(order) {
  // 주문 검증 (20줄)
  if (!order.items) throw new Error('항목 없음');
  if (order.items.length === 0) throw new Error('빈 주문');
  // ... 더 많은 검증

  // 합계 계산 (30줄)
  let subtotal = 0;
  for (const item of order.items) {
    subtotal += item.price * item.quantity;
  }
  // ... 세금, 배송비, 할인

  // 알림 전송 (20줄)
  // ... 이메일 로직
}
```

**예제 (이후):**
```javascript
function processOrder(order) {
  validateOrder(order);
  const totals = calculateOrderTotals(order);
  sendOrderNotifications(order, totals);
  return { order, totals };
}
```

---

### 큰 클래스

**징후:**
- 많은 인스턴스 변수 (7-10개 초과)
- 많은 메서드 (15-20개 초과)
- 클래스 이름이 모호함 (Manager, Handler, Processor)
- 메서드가 모든 인스턴스 변수를 사용하지 않음

**문제점:**
- 단일 책임 원칙 위반
- 테스트 어려움
- 변경이 관련 없는 기능에 영향
- 일부 재사용이 어려움

**리팩토링:**
- 클래스 추출
- 서브클래스 추출
- 인터페이스 추출

**감지:**
```
코드 줄 수 > 300
메서드 수 > 15
필드 수 > 10
```

---

### 원시 타입 집착

**징후:**
- 도메인 개념에 원시 타입 사용 (이메일에 string, 금액에 int)
- 객체 대신 원시 타입 배열
- 타입 코드에 문자열 상수
- 매직 넘버/문자열

**문제점:**
- 타입 수준에서 검증 없음
- 로직이 코드베이스 전체에 분산
- 잘못된 값을 전달하기 쉬움
- 누락된 도메인 개념

**리팩토링:**
- 객체로 원시 타입 대체
- 타입 코드를 클래스로 대체
- 타입 코드를 서브클래스로 대체
- 타입 코드를 상태/전략으로 대체

**예제 (이전):**
```javascript
const user = {
  email: 'john@example.com',     // 그냥 문자열
  phone: '1234567890',           // 그냥 문자열
  status: 'active',              // 매직 문자열
  balance: 10050                 // 센트 단위 정수
};
```

**예제 (이후):**
```javascript
const user = {
  email: new Email('john@example.com'),
  phone: new PhoneNumber('1234567890'),
  status: UserStatus.ACTIVE,
  balance: Money.cents(10050)
};
```

---

### 긴 매개변수 목록

**징후:**
- 4개 이상의 매개변수를 가진 메서드
- 항상 함께 나타나는 매개변수
- 메서드 동작을 변경하는 불리언 플래그
- 자주 전달되는 null/undefined

**문제점:**
- 올바르게 호출하기 어려움
- 매개변수 순서 혼동
- 메서드가 너무 많은 일을 함
- 새 매개변수 추가 어려움

**리팩토링:**
- 매개변수 객체 도입
- 전체 객체 보존
- 매개변수를 메서드 호출로 대체
- 플래그 인자 제거

**예제 (이전):**
```javascript
function createUser(firstName, lastName, email, phone,
                    street, city, state, zip,
                    isAdmin, isActive, createdBy) {
  // ...
}
```

**예제 (이후):**
```javascript
function createUser(personalInfo, address, options) {
  // personalInfo: { firstName, lastName, email, phone }
  // address: { street, city, state, zip }
  // options: { isAdmin, isActive, createdBy }
}
```

---

### 데이터 덩어리

**징후:**
- 동일한 3개 이상의 필드가 반복적으로 함께 나타남
- 항상 함께 전달되는 매개변수
- 함께 속하는 필드 하위 집합이 있는 클래스

**문제점:**
- 중복 처리 로직
- 누락된 추상화
- 확장 어려움
- 숨겨진 클래스 표시

**리팩토링:**
- 클래스 추출
- 매개변수 객체 도입
- 전체 객체 보존

**예제:**
```javascript
// 데이터 덩어리: (x, y, z) 좌표
function movePoint(x, y, z, dx, dy, dz) { }
function scalePoint(x, y, z, factor) { }
function distanceBetween(x1, y1, z1, x2, y2, z2) { }

// Point3D 클래스 추출
class Point3D {
  constructor(x, y, z) { }
  move(delta) { }
  scale(factor) { }
  distanceTo(other) { }
}
```

---

## 객체 지향 남용

OOP 원칙의 불완전하거나 잘못된 사용을 나타내는 스멜.

### Switch 문

**징후:**
- 긴 switch/case 또는 if/else 체인
- 여러 곳에서 동일한 switch
- 타입 코드에 대한 switch
- 새 케이스 추가 시 모든 곳에서 변경 필요

**문제점:**
- 개방-폐쇄 원칙 위반
- 변경이 모든 switch 위치에 영향
- 확장 어려움
- 종종 다형성 누락 표시

**리팩토링:**
- 조건문을 다형성으로 대체
- 타입 코드를 서브클래스로 대체
- 타입 코드를 상태/전략으로 대체

**예제 (이전):**
```javascript
function calculatePay(employee) {
  switch (employee.type) {
    case 'hourly':
      return employee.hours * employee.rate;
    case 'salaried':
      return employee.salary / 12;
    case 'commissioned':
      return employee.sales * employee.commission;
  }
}
```

**예제 (이후):**
```javascript
class HourlyEmployee {
  calculatePay() {
    return this.hours * this.rate;
  }
}

class SalariedEmployee {
  calculatePay() {
    return this.salary / 12;
  }
}
```

---

### 임시 필드

**징후:**
- 일부 메서드에서만 사용되는 인스턴스 변수
- 조건부로 설정되는 필드
- 특정 경우를 위한 복잡한 초기화

**문제점:**
- 혼란 — 필드가 존재하지만 null일 수 있음
- 객체 상태 이해 어려움
- 조건부 로직 숨김 표시

**리팩토링:**
- 클래스 추출
- Null 객체 도입
- 임시 필드를 로컬로 대체

---

### 거절된 유산

**징후:**
- 서브클래스가 상속된 메서드/데이터를 사용하지 않음
- 서브클래스가 아무것도 하지 않도록 재정의
- IS-A 관계가 아닌 코드 재사용을 위한 상속 사용

**문제점:**
- 잘못된 추상화
- 리스코프 치환 원칙 위반
- 오해의 소지가 있는 계층 구조

**리팩토링:**
- 메서드/필드 내리기
- 서브클래스를 위임으로 대체
- 상속을 위임으로 대체

---

### 다른 인터페이스를 가진 대체 클래스

**징후:**
- 비슷한 일을 하는 두 클래스
- 같은 개념에 대한 다른 메서드 이름
- 상호 교환하여 사용 가능

**문제점:**
- 중복 구현
- 공통 인터페이스 없음
- 전환 어려움

**리팩토링:**
- 메서드 이름 변경
- 메서드 이동
- 수퍼클래스 추출
- 인터페이스 추출

---

## 변경 방해자

변경을 어렵게 만드는 스멜 — 한 가지를 변경하려면 다른 많은 것을 변경해야 함.

### 발산적 변경

**징후:**
- 하나의 클래스가 여러 다른 이유로 변경됨
- 다른 영역의 변경이 동일한 클래스 편집 트리거
- 클래스가 "God 클래스"

**문제점:**
- 단일 책임 위반
- 높은 변경 빈도
- 병합 충돌

**리팩토링:**
- 클래스 추출
- 수퍼클래스 추출
- 서브클래스 추출

**예제:**
`User` 클래스가 다음으로 인해 변경됨:
- 인증 변경
- 프로필 변경
- 결제 변경
- 알림 변경

→ 추출: `AuthService`, `ProfileService`, `BillingService`, `NotificationService`

---

### 샷건 수술

**징후:**
- 하나의 변경에 많은 클래스 수정 필요
- 작은 기능에 10개 이상의 파일 수정
- 변경이 분산되어 모두 찾기 어려움

**문제점:**
- 놓치기 쉬움
- 높은 결합도
- 변경이 오류 발생 가능

**리팩토링:**
- 메서드 이동
- 필드 이동
- 클래스 인라인

**감지:**
하나의 필드 추가에 5개 이상의 파일 변경이 필요한지 확인.

---

### 병렬 상속 계층 구조

**징후:**
- 한 계층 구조에서 서브클래스를 만들면 다른 계층 구조에서도 서브클래스 필요
- 클래스 접두사 일치 (예: `DatabaseOrder`, `DatabaseProduct`)

**문제점:**
- 두 배의 유지보수
- 계층 구조 간 결합
- 한쪽을 잊기 쉬움

**리팩토링:**
- 메서드 이동
- 필드 이동
- 하나의 계층 구조 제거

---

## 불필요한 것들

제거해야 할 불필요한 것.

### 주석 (과도한)

**징후:**
- 코드가 무엇을 하는지 설명하는 주석
- 주석 처리된 코드
- 영원히 남아있는 TODO/FIXME
- 주석의 사과

**문제점:**
- 주석은 거짓말 (동기화되지 않음)
- 코드는 스스로 문서화되어야 함
- 죽은 코드는 혼란 유발

**리팩토링:**
- 메서드 추출 (이름이 무엇을 하는지 설명)
- 이름 변경 (주석 없이 명확성)
- 주석 처리된 코드 제거
- 단언 도입

**좋은 주석 vs 나쁜 주석:**
```javascript
// 나쁨: 무엇을 하는지 설명
// 사용자를 반복하며 활성 상태 확인
for (const user of users) {
  if (user.status === 'active') { }
}

// 좋음: 왜 그런지 설명
// 활성 사용자만 - 비활성은 정리 작업이 처리
const activeUsers = users.filter(u => u.isActive);
```

---

### 중복 코드

**징후:**
- 여러 곳의 동일한 코드
- 약간의 변형이 있는 유사한 코드
- 복사-붙여넣기 패턴

**문제점:**
- 여러 곳에서 버그 수정 필요
- 불일치 위험
- 부풀려진 코드베이스

**리팩토링:**
- 메서드 추출
- 클래스 추출
- 메서드 올리기 (계층 구조에서)
- 템플릿 메서드 형성

**감지 규칙:**
3번 이상 중복된 코드는 추출해야 함.

---

### 게으른 클래스

**징후:**
- 클래스가 존재를 정당화할 만큼 충분한 일을 하지 않음
- 추가 가치가 없는 래퍼
- 과도한 엔지니어링의 결과

**문제점:**
- 유지보수 오버헤드
- 불필요한 간접 참조
- 이점 없는 복잡성

**리팩토링:**
- 클래스 인라인
- 계층 구조 축소

---

### 죽은 코드

**징후:**
- 도달할 수 없는 코드
- 사용되지 않는 변수/메서드/클래스
- 주석 처리된 코드
- 불가능한 조건 뒤의 코드

**문제점:**
- 혼란
- 유지보수 부담
- 이해 속도 저하

**리팩토링:**
- 죽은 코드 제거
- 안전한 삭제

**감지:**
```bash
# 사용되지 않는 내보내기 찾기
# 참조되지 않은 함수 찾기
# IDE "사용되지 않음" 경고
```

---

### 투기적 일반화

**징후:**
- 서브클래스가 하나뿐인 추상 클래스
- "미래 사용을 위한" 사용되지 않는 매개변수
- 위임만 하는 메서드
- 하나의 사용 사례를 위한 "프레임워크"

**문제점:**
- 이점 없는 복잡성
- YAGNI (You Ain't Gonna Need It)
- 이해하기 어려움

**리팩토링:**
- 계층 구조 축소
- 클래스 인라인
- 매개변수 제거
- 메서드 이름 변경

---

## 결합자

클래스 간의 과도한 결합을 나타내는 스멜.

### 기능 선망

**징후:**
- 메서드가 자신의 클래스보다 다른 클래스의 데이터를 더 많이 사용
- 다른 객체에 대한 많은 getter 호출
- 데이터와 동작이 분리됨

**문제점:**
- 잘못된 동작 위치
- 낮은 캡슐화
- 유지보수 어려움

**리팩토링:**
- 메서드 이동
- 필드 이동
- 메서드 추출 (그런 다음 이동)

**예제 (이전):**
```javascript
class Order {
  getDiscountedPrice(customer) {
    // 고객 데이터를 많이 사용
    if (customer.loyaltyYears > 5) {
      return this.price * customer.discountRate;
    }
    return this.price;
  }
}
```

**예제 (이후):**
```javascript
class Customer {
  getDiscountedPriceFor(price) {
    if (this.loyaltyYears > 5) {
      return price * this.discountRate;
    }
    return price;
  }
}
```

---

### 부적절한 친밀함

**징후:**
- 클래스가 서로의 private 부분에 접근
- 양방향 참조
- 서브클래스가 부모에 대해 너무 많이 알고 있음

**문제점:**
- 높은 결합도
- 변경이 연쇄적으로 발생
- 하나를 수정하면 다른 것도 수정해야 함

**리팩토링:**
- 메서드 이동
- 필드 이동
- 양방향을 단방향으로 변경
- 클래스 추출
- 위임 숨기기

---

### 메시지 체인

**징후:**
- 긴 메서드 호출 체인: `a.getB().getC().getD().getValue()`
- 클라이언트가 탐색 구조에 의존
- "열차 사고" 코드

**문제점:**
- 취약 — 변경 시 체인 중단
- 디미터 법칙 위반
- 구조에 대한 결합

**리팩토링:**
- 위임 숨기기
- 메서드 추출
- 메서드 이동

**예제:**
```javascript
// 나쁨: 메시지 체인
const managerName = employee.getDepartment().getManager().getName();

// 더 나음: 위임 숨기기
const managerName = employee.getManagerName();
```

---

### 중개자

**징후:**
- 다른 클래스에 위임만 하는 클래스
- 메서드의 절반이 위임
- 추가 가치 없음

**문제점:**
- 불필요한 간접 참조
- 유지보수 오버헤드
- 혼란스러운 아키텍처

**리팩토링:**
- 중개자 제거
- 메서드 인라인

---

## 스멜 심각도 가이드

| 심각도 | 설명 | 조치 |
|----------|-------------|--------|
| **Critical** | 개발 차단, 버그 유발 | 즉시 수정 |
| **High** | 상당한 유지보수 부담 | 현재 스프린트에서 수정 |
| **Medium** | 눈에 띄지만 관리 가능 | 가까운 미래에 계획 |
| **Low** | 사소한 불편함 | 기회가 있을 때 수정 |

---

## 빠른 감지 체크리스트

코드를 스캔할 때 이 체크리스트 사용:

- [ ] 30줄 초과 메서드?
- [ ] 300줄 초과 클래스?
- [ ] 4개 초과 매개변수를 가진 메서드?
- [ ] 중복 코드 블록?
- [ ] 타입 코드에 switch/case?
- [ ] 사용되지 않는 코드?
- [ ] 다른 클래스의 데이터를 많이 사용하는 메서드?
- [ ] 긴 메서드 호출 체인?
- [ ] "무엇"이 아닌 "왜"를 설명하는 주석?
- [ ] 객체여야 하는 원시 타입?

---

## 추가 읽기

- Fowler, M. (2018). *Refactoring: Improving the Design of Existing Code* (2nd ed.)
- Kerievsky, J. (2004). *Refactoring to Patterns*
- Feathers, M. (2004). *Working Effectively with Legacy Code*
