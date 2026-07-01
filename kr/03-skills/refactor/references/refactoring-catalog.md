<!-- i18n-source: 03-skills/refactor/references/refactoring-catalog.md -->
<!-- i18n-source-sha: TBD -->
<!-- i18n-date: 2026-07-01 -->

# 리팩토링 카탈로그

Martin Fowler의 *Refactoring* (2판)에서 발췌한 리팩토링 기법의 큐레이팅된 카탈로그입니다. 각 리팩토링에는 동기, 단계별 메커닉스 및 예제가 포함됩니다.

> "리팩토링은 그 메커닉스—변경을 수행하기 위해 따르는 정확한 단계 순서—에 의해 정의됩니다." — Martin Fowler

---

## 이 카탈로그 사용 방법

1. 코드 스멜 참조를 사용하여 **스멜 식별**
2. 이 카탈로그에서 **일치하는 리팩토링 찾기**
3. **메커닉스를 단계별로 따르기**
4. **각 단계 후 테스트**하여 동작이 보존되는지 확인

**황금률**: 어떤 단계가 10분 이상 걸리면 더 작은 단계로 나누세요.

---

## 가장 일반적인 리팩토링

### 메서드 추출

**사용 시기**: 긴 메서드, 중복 코드, 개념에 이름을 붙여야 할 때

**동기**: 코드 조각을 목적을 설명하는 메서드로 변환.

**메커닉스**:
1. 하는 일(방법이 아님)을 기준으로 이름이 지정된 새 메서드 생성
2. 코드 조각을 새 메서드로 복사
3. 조각에서 사용된 로컬 변수 확인
4. 로컬 변수를 매개변수로 전달 (또는 메서드에서 선언)
5. 반환 값을 적절히 처리
6. 원래 조각을 새 메서드 호출로 대체
7. 테스트

**이전**:
```javascript
function printOwing(invoice) {
  let outstanding = 0;

  console.log("***********************");
  console.log("**** Customer Owes ****");
  console.log("***********************");

  // 미지급액 계산
  for (const order of invoice.orders) {
    outstanding += order.amount;
  }

  // 세부사항 출력
  console.log(`name: ${invoice.customer}`);
  console.log(`amount: ${outstanding}`);
}
```

**이후**:
```javascript
function printOwing(invoice) {
  printBanner();
  const outstanding = calculateOutstanding(invoice);
  printDetails(invoice, outstanding);
}

function printBanner() {
  console.log("***********************");
  console.log("**** Customer Owes ****");
  console.log("***********************");
}

function calculateOutstanding(invoice) {
  return invoice.orders.reduce((sum, order) => sum + order.amount, 0);
}

function printDetails(invoice, outstanding) {
  console.log(`name: ${invoice.customer}`);
  console.log(`amount: ${outstanding}`);
}
```

---

### 메서드 인라인

**사용 시기**: 메서드 본문이 이름만큼 명확할 때, 과도한 위임

**동기**: 메서드가 가치를 추가하지 않을 때 불필요한 간접 참조 제거.

**메커닉스**:
1. 메서드가 다형성이 아닌지 확인
2. 메서드에 대한 모든 호출 찾기
3. 각 호출을 메서드 본문으로 대체
4. 각 대체 후 테스트
5. 메서드 정의 제거

**이전**:
```javascript
function getRating(driver) {
  return moreThanFiveLateDeliveries(driver) ? 2 : 1;
}

function moreThanFiveLateDeliveries(driver) {
  return driver.numberOfLateDeliveries > 5;
}
```

**이후**:
```javascript
function getRating(driver) {
  return driver.numberOfLateDeliveries > 5 ? 2 : 1;
}
```

---

### 변수 추출

**사용 시기**: 이해하기 어려운 복잡한 표현식

**동기**: 복잡한 표현식의 일부에 이름 부여.

**메커닉스**:
1. 표현식에 부작용이 없는지 확인
2. 불변 변수 선언
3. 표현식(또는 일부)의 결과로 설정
4. 원래 표현식을 변수로 대체
5. 테스트

**이전**:
```javascript
return order.quantity * order.itemPrice -
  Math.max(0, order.quantity - 500) * order.itemPrice * 0.05 +
  Math.min(order.quantity * order.itemPrice * 0.1, 100);
```

**이후**:
```javascript
const basePrice = order.quantity * order.itemPrice;
const quantityDiscount = Math.max(0, order.quantity - 500) * order.itemPrice * 0.05;
const shipping = Math.min(basePrice * 0.1, 100);
return basePrice - quantityDiscount + shipping;
```

---

### 변수 인라인

**사용 시기**: 변수 이름이 표현식보다 더 많은 정보를 전달하지 않음

**동기**: 불필요한 간접 참조 제거.

**메커닉스**:
1. 우변에 부작용이 없는지 확인
2. 변수가 불변이 아니면 불변으로 만들고 테스트
3. 첫 번째 참조를 찾아 표현식으로 대체
4. 테스트
5. 모든 참조에 대해 반복
6. 선언 및 할당 제거
7. 테스트

---

### 변수 이름 변경

**사용 시기**: 이름이 목적을 명확히 전달하지 않음

**동기**: 좋은 이름은 깔끔한 코드에 중요.

**메커닉스**:
1. 변수가 널리 사용되면 캡슐화 고려
2. 모든 참조 찾기
3. 각 참조 변경
4. 테스트

**팁**:
- 의도를 드러내는 이름 사용
- 약어 피하기
- 도메인 용어 사용

```javascript
// 나쁨
const d = 30;
const x = users.filter(u => u.a);

// 좋음
const daysSinceLastLogin = 30;
const activeUsers = users.filter(user => user.isActive);
```

---

### 함수 선언 변경

**사용 시기**: 함수 이름이 목적을 설명하지 않음, 매개변수 변경 필요

**동기**: 좋은 함수 이름은 코드를 자기 문서화하게 만듦.

**메커닉스 (단순)**:
1. 필요하지 않은 매개변수 제거
2. 이름 변경
3. 필요한 매개변수 추가
4. 테스트

**메커닉스 (마이그레이션 - 복잡한 변경용)**:
1. 매개변수를 제거하는 경우 사용되지 않는지 확인
2. 원하는 선언으로 새 함수 생성
3. 이전 함수가 새 함수를 호출하게 함
4. 테스트
5. 호출자가 새 함수를 사용하도록 변경
6. 각각 후 테스트
7. 이전 함수 제거

**이전**:
```javascript
function circum(radius) {
  return 2 * Math.PI * radius;
}
```

**이후**:
```javascript
function circumference(radius) {
  return 2 * Math.PI * radius;
}
```

---

### 변수 캡슐화

**사용 시기**: 여러 곳에서 데이터에 직접 접근

**동기**: 데이터 조작을 위한 명확한 접근 지점 제공.

**메커닉스**:
1. getter 및 setter 함수 생성
2. 모든 참조 찾기
3. 읽기를 getter로 대체
4. 쓰기를 setter로 대체
5. 각 변경 후 테스트
6. 변수의 가시성 제한

**이전**:
```javascript
let defaultOwner = { firstName: "Martin", lastName: "Fowler" };

// 여러 곳에서 사용
spaceship.owner = defaultOwner;
```

**이후**:
```javascript
let defaultOwnerData = { firstName: "Martin", lastName: "Fowler" };

function defaultOwner() { return defaultOwnerData; }
function setDefaultOwner(arg) { defaultOwnerData = arg; }

spaceship.owner = defaultOwner();
```

---

### 매개변수 객체 도입

**사용 시기**: 자주 함께 사용되는 여러 매개변수

**동기**: 자연스럽게 함께 속하는 데이터 그룹화.

**메커닉스**:
1. 그룹화된 매개변수를 위한 새 클래스/구조체 생성
2. 테스트
3. 함수 선언 변경을 사용하여 새 객체 추가
4. 테스트
5. 그룹의 각 매개변수를 함수에서 제거하고 새 객체 사용
6. 각각 후 테스트

**이전**:
```javascript
function amountInvoiced(startDate, endDate) { ... }
function amountReceived(startDate, endDate) { ... }
function amountOverdue(startDate, endDate) { ... }
```

**이후**:
```javascript
class DateRange {
  constructor(start, end) {
    this.start = start;
    this.end = end;
  }
}

function amountInvoiced(dateRange) { ... }
function amountReceived(dateRange) { ... }
function amountOverdue(dateRange) { ... }
```

---

### 함수를 클래스로 묶기

**사용 시기**: 여러 함수가 동일한 데이터에서 작동

**동기**: 작동하는 데이터와 함께 함수 그룹화.

**메커닉스**:
1. 공통 데이터에 레코드 캡슐화 적용
2. 각 함수를 클래스로 이동
3. 각 이동 후 테스트
4. 데이터 인자를 클래스 필드 사용으로 대체

**이전**:
```javascript
function base(reading) { ... }
function taxableCharge(reading) { ... }
function calculateBaseCharge(reading) { ... }
```

**이후**:
```javascript
class Reading {
  constructor(data) { this._data = data; }

  get base() { ... }
  get taxableCharge() { ... }
  get calculateBaseCharge() { ... }
}
```

---

### 단계 분할

**사용 시기**: 코드가 두 가지 다른 것을 처리

**동기**: 명확한 경계로 코드를 별도의 단계로 분리.

**메커닉스**:
1. 두 번째 단계를 위한 두 번째 함수 생성
2. 테스트
3. 단계 간 중간 데이터 구조 도입
4. 테스트
5. 첫 번째 단계를 자체 함수로 추출
6. 테스트

**이전**:
```javascript
function priceOrder(product, quantity, shippingMethod) {
  const basePrice = product.basePrice * quantity;
  const discount = Math.max(quantity - product.discountThreshold, 0)
    * product.basePrice * product.discountRate;
  const shippingPerCase = (basePrice > shippingMethod.discountThreshold)
    ? shippingMethod.discountedFee : shippingMethod.feePerCase;
  const shippingCost = quantity * shippingPerCase;
  return basePrice - discount + shippingCost;
}
```

**이후**:
```javascript
function priceOrder(product, quantity, shippingMethod) {
  const priceData = calculatePricingData(product, quantity);
  return applyShipping(priceData, shippingMethod);
}

function calculatePricingData(product, quantity) {
  const basePrice = product.basePrice * quantity;
  const discount = Math.max(quantity - product.discountThreshold, 0)
    * product.basePrice * product.discountRate;
  return { basePrice, quantity, discount };
}

function applyShipping(priceData, shippingMethod) {
  const shippingPerCase = (priceData.basePrice > shippingMethod.discountThreshold)
    ? shippingMethod.discountedFee : shippingMethod.feePerCase;
  const shippingCost = priceData.quantity * shippingPerCase;
  return priceData.basePrice - priceData.discount + shippingCost;
}
```

---

## 기능 이동

### 메서드 이동

**사용 시기**: 메서드가 자신의 클래스보다 다른 클래스의 기능을 더 많이 사용

**동기**: 가장 많이 사용하는 데이터와 함께 함수 배치.

**메커닉스**:
1. 메서드가 자신의 클래스에서 사용하는 모든 프로그램 요소 검사
2. 메서드가 다형성인지 확인
3. 메서드를 대상 클래스에 복사
4. 새 컨텍스트에 맞게 조정
5. 원래 메서드가 대상에 위임하게 함
6. 테스트
7. 원래 메서드 제거 고려

---

### 필드 이동

**사용 시기**: 필드가 다른 클래스에 의해 더 많이 사용됨

**동기**: 사용하는 함수와 함께 데이터 유지.

**메커닉스**:
1. 아직 캡슐화되지 않은 경우 필드 캡슐화
2. 테스트
3. 대상에 필드 생성
4. 참조를 대상 필드 사용으로 업데이트
5. 테스트
6. 원래 필드 제거

---

### 함수로 문장 이동

**사용 시기**: 동일한 코드가 항상 함수 호출과 함께 나타남

**동기**: 반복되는 코드를 함수로 이동하여 중복 제거.

**메커닉스**:
1. 아직 함수가 아닌 경우 반복되는 코드를 함수로 추출
2. 해당 함수로 문장 이동
3. 테스트
4. 호출자가 더 이상 독립형 문장이 필요하지 않으면 제거

---

### 호출자로 문장 이동

**사용 시기**: 공통 동작이 호출자 간에 다름

**동기**: 동작이 달라야 할 때 함수 밖으로 이동.

**메커닉스**:
1. 이동할 코드에 메서드 추출 사용
2. 원래 함수에 메서드 인라인 사용
3. 인라인된 호출 제거
4. 추출된 코드를 각 호출자로 이동
5. 테스트

---

## 데이터 구성

### 객체로 원시 타입 대체

**사용 시기**: 데이터 항목이 단순 값보다 더 많은 동작 필요

**동기**: 데이터를 그 동작과 함께 캡슐화.

**메커닉스**:
1. 변수 캡슐화 적용
2. 단순 값 클래스 생성
3. setter가 새 인스턴스를 생성하도록 변경
4. getter가 값을 반환하도록 변경
5. 테스트
6. 새 클래스에 더 풍부한 동작 추가

**이전**:
```javascript
class Order {
  constructor(data) {
    this.priority = data.priority; // string: "high", "rush", etc.
  }
}

// 사용
if (order.priority === "high" || order.priority === "rush") { ... }
```

**이후**:
```javascript
class Priority {
  constructor(value) {
    if (!Priority.legalValues().includes(value))
      throw new Error(`유효하지 않은 우선순위: ${value}`);
    this._value = value;
  }

  static legalValues() { return ['low', 'normal', 'high', 'rush']; }
  get value() { return this._value; }

  higherThan(other) {
    return Priority.legalValues().indexOf(this._value) >
           Priority.legalValues().indexOf(other._value);
  }
}

// 사용
if (order.priority.higherThan(new Priority("normal"))) { ... }
```

---

### 임시 변수를 질의로 대체

**사용 시기**: 임시 변수가 표현식의 결과를 보유

**동기**: 표현식을 함수로 추출하여 코드를 더 명확하게.

**메커닉스**:
1. 변수가 한 번만 할당되는지 확인
2. 할당의 우변을 메서드로 추출
3. 임시 변수 참조를 메서드 호출로 대체
4. 테스트
5. 임시 변수 선언 및 할당 제거

**이전**:
```javascript
const basePrice = this._quantity * this._itemPrice;
if (basePrice > 1000) {
  return basePrice * 0.95;
} else {
  return basePrice * 0.98;
}
```

**이후**:
```javascript
get basePrice() {
  return this._quantity * this._itemPrice;
}

// 메서드에서
if (this.basePrice > 1000) {
  return this.basePrice * 0.95;
} else {
  return this.basePrice * 0.98;
}
```

---

## 조건부 로직 단순화

### 조건문 분해

**사용 시기**: 복잡한 조건문 (if-then-else)

**동기**: 조건과 동작을 추출하여 의도를 명확하게.

**메커닉스**:
1. 조건에 메서드 추출 적용
2. then-브랜치에 메서드 추출 적용
3. else-브랜치에 메서드 추출 적용 (있는 경우)

**이전**:
```javascript
if (!aDate.isBefore(plan.summerStart) && !aDate.isAfter(plan.summerEnd)) {
  charge = quantity * plan.summerRate;
} else {
  charge = quantity * plan.regularRate + plan.regularServiceCharge;
}
```

**이후**:
```javascript
if (isSummer(aDate, plan)) {
  charge = summerCharge(quantity, plan);
} else {
  charge = regularCharge(quantity, plan);
}

function isSummer(date, plan) {
  return !date.isBefore(plan.summerStart) && !date.isAfter(plan.summerEnd);
}

function summerCharge(quantity, plan) {
  return quantity * plan.summerRate;
}

function regularCharge(quantity, plan) {
  return quantity * plan.regularRate + plan.regularServiceCharge;
}
```

---

### 조건부 표현식 통합

**사용 시기**: 동일한 결과를 가진 여러 조건

**동기**: 조건이 단일 검사임을 명확히.

**메커닉스**:
1. 조건에 부작용이 없는지 확인
2. `and` 또는 `or`를 사용하여 조건 결합
3. 결합된 조건에 메서드 추출 고려

**이전**:
```javascript
if (employee.seniority < 2) return 0;
if (employee.monthsDisabled > 12) return 0;
if (employee.isPartTime) return 0;
```

**이후**:
```javascript
if (isNotEligibleForDisability(employee)) return 0;

function isNotEligibleForDisability(employee) {
  return employee.seniority < 2 ||
         employee.monthsDisabled > 12 ||
         employee.isPartTime;
}
```

---

### 중첩 조건문을 보호 구문으로 대체

**사용 시기**: 깊게 중첩된 조건문으로 흐름 따라가기 어려움

**동기**: 특수 케이스에 보호 구문을 사용하여 일반 흐름을 명확히.

**메커닉스**:
1. 특수 케이스 조건 찾기
2. 일찍 반환하는 보호 구문으로 대체
3. 각 변경 후 테스트

**이전**:
```javascript
function payAmount(employee) {
  let result;
  if (employee.isSeparated) {
    result = { amount: 0, reasonCode: "SEP" };
  } else {
    if (employee.isRetired) {
      result = { amount: 0, reasonCode: "RET" };
    } else {
      result = calculateNormalPay(employee);
    }
  }
  return result;
}
```

**이후**:
```javascript
function payAmount(employee) {
  if (employee.isSeparated) return { amount: 0, reasonCode: "SEP" };
  if (employee.isRetired) return { amount: 0, reasonCode: "RET" };
  return calculateNormalPay(employee);
}
```

---

### 조건문을 다형성으로 대체

**사용 시기**: 타입 기반 switch/case, 타입별로 다른 조건부 로직

**동기**: 객체가 자신의 동작을 처리하도록 함.

**메커닉스**:
1. 클래스 계층 구조 생성 (없는 경우)
2. 객체 생성을 위한 팩토리 함수 사용
3. 조건부 로직을 수퍼클래스 메서드로 이동
4. 각 케이스에 대한 서브클래스 메서드 생성
5. 원래 조건부 제거

**이전**:
```javascript
function plumages(birds) {
  return birds.map(b => plumage(b));
}

function plumage(bird) {
  switch (bird.type) {
    case 'EuropeanSwallow':
      return "average";
    case 'AfricanSwallow':
      return (bird.numberOfCoconuts > 2) ? "tired" : "average";
    case 'NorwegianBlueParrot':
      return (bird.voltage > 100) ? "scorched" : "beautiful";
    default:
      return "unknown";
  }
}
```

**이후**:
```javascript
class Bird {
  get plumage() { return "unknown"; }
}

class EuropeanSwallow extends Bird {
  get plumage() { return "average"; }
}

class AfricanSwallow extends Bird {
  get plumage() {
    return (this.numberOfCoconuts > 2) ? "tired" : "average";
  }
}

class NorwegianBlueParrot extends Bird {
  get plumage() {
    return (this.voltage > 100) ? "scorched" : "beautiful";
  }
}

function createBird(data) {
  switch (data.type) {
    case 'EuropeanSwallow': return new EuropeanSwallow(data);
    case 'AfricanSwallow': return new AfricanSwallow(data);
    case 'NorwegianBlueParrot': return new NorwegianBlueParrot(data);
    default: return new Bird(data);
  }
}
```

---

### 특수 케이스 도입 (Null 객체)

**사용 시기**: 특수 케이스에 대한 반복적인 null 검사

**동기**: 특수 케이스를 처리하는 특수 객체 반환.

**메커닉스**:
1. 예상 인터페이스로 특수 케이스 클래스 생성
2. isSpecialCase 검사 추가
3. 팩토리 메서드 도입
4. null 검사를 특수 케이스 객체 사용으로 대체
5. 테스트

**이전**:
```javascript
const customer = site.customer;
// ... 많은 곳에서 검사
if (customer === "unknown") {
  customerName = "occupant";
} else {
  customerName = customer.name;
}
```

**이후**:
```javascript
class UnknownCustomer {
  get name() { return "occupant"; }
  get billingPlan() { return registry.defaultPlan; }
}

// 팩토리 메서드
function customer(site) {
  return site.customer === "unknown"
    ? new UnknownCustomer()
    : site.customer;
}

// 사용 - null 검사 필요 없음
const customerName = customer.name;
```

---

## API 리팩토링

### 질의와 수정자 분리

**사용 시기**: 함수가 값을 반환하면서 부작용도 가짐

**동기**: 어떤 작업에 부작용이 있는지 명확히.

**메커닉스**:
1. 새 질의 함수 생성
2. 원래 함수의 반환 로직 복사
3. 원래 함수를 void 반환으로 수정
4. 반환 값을 사용하는 호출 대체
5. 테스트

**이전**:
```javascript
function alertForMiscreant(people) {
  for (const p of people) {
    if (p === "Don") {
      setOffAlarms();
      return "Don";
    }
    if (p === "John") {
      setOffAlarms();
      return "John";
    }
  }
  return "";
}
```

**이후**:
```javascript
function findMiscreant(people) {
  for (const p of people) {
    if (p === "Don") return "Don";
    if (p === "John") return "John";
  }
  return "";
}

function alertForMiscreant(people) {
  if (findMiscreant(people) !== "") setOffAlarms();
}
```

---

### 함수 매개변수화

**사용 시기**: 여러 함수가 다른 값으로 유사한 작업 수행

**동기**: 매개변수를 추가하여 중복 제거.

**메커닉스**:
1. 하나의 함수 선택
2. 변하는 리터럴을 위한 매개변수 추가
3. 본문이 매개변수를 사용하도록 변경
4. 테스트
5. 호출자가 매개변수화된 버전을 사용하도록 변경
6. 더 이상 사용되지 않는 함수 제거

**이전**:
```javascript
function tenPercentRaise(person) {
  person.salary = person.salary * 1.10;
}

function fivePercentRaise(person) {
  person.salary = person.salary * 1.05;
}
```

**이후**:
```javascript
function raise(person, factor) {
  person.salary = person.salary * (1 + factor);
}

// 사용
raise(person, 0.10);
raise(person, 0.05);
```

---

### 플래그 인자 제거

**사용 시기**: 함수 동작을 변경하는 불리언 매개변수

**동기**: 별도의 함수를 통해 동작을 명시적으로.

**메커닉스**:
1. 각 플래그 값에 대한 명시적 함수 생성
2. 각 호출을 적절한 새 함수로 대체
3. 각 변경 후 테스트
4. 원래 함수 제거

**이전**:
```javascript
function bookConcert(customer, isPremium) {
  if (isPremium) {
    // 프리미엄 예약 로직
  } else {
    // 일반 예약 로직
  }
}

bookConcert(customer, true);
bookConcert(customer, false);
```

**이후**:
```javascript
function bookPremiumConcert(customer) {
  // 프리미엄 예약 로직
}

function bookRegularConcert(customer) {
  // 일반 예약 로직
}

bookPremiumConcert(customer);
bookRegularConcert(customer);
```

---

## 상속 처리

### 메서드 올리기

**사용 시기**: 여러 서브클래스에 동일한 메서드

**동기**: 클래스 계층 구조에서 중복 제거.

**메커닉스**:
1. 메서드가 동일한지 검사
2. 시그니처가 같은지 확인
3. 수퍼클래스에 새 메서드 생성
4. 하나의 서브클래스에서 본문 복사
5. 하나의 서브클래스 메서드 삭제, 테스트
6. 다른 서브클래스 메서드 삭제, 각각 테스트

---

### 메서드 내리기

**사용 시기**: 서브클래스의 하위 집합에만 관련된 동작

**동기**: 사용되는 곳에 메서드 배치.

**메커닉스**:
1. 필요한 각 서브클래스에 메서드 복사
2. 수퍼클래스에서 메서드 제거
3. 테스트
4. 필요하지 않은 서브클래스에서 제거
5. 테스트

---

### 서브클래스를 위임으로 대체

**사용 시기**: 상속이 잘못 사용됨, 더 많은 유연성 필요

**동기**: 적절할 때 상속보다 컴포지션 선호.

**메커닉스**:
1. 위임을 위한 빈 클래스 생성
2. 위임을 보유하는 호스트 클래스에 필드 추가
3. 호스트에서 호출되는 위임 생성자 생성
4. 기능을 위임으로 이동
5. 각 이동 후 테스트
6. 상속을 위임으로 대체

---

## 클래스 추출

**사용 시기**: 여러 책임을 가진 큰 클래스

**동기**: 단일 책임 유지를 위해 클래스 분할.

**메커닉스**:
1. 책임 분할 방법 결정
2. 새 클래스 생성
3. 원래 클래스에서 새 클래스로 필드 이동
4. 테스트
5. 원래 클래스에서 새 클래스로 메서드 이동
6. 각 이동 후 테스트
7. 두 클래스 검토 및 이름 변경
8. 새 클래스 노출 방법 결정

**이전**:
```javascript
class Person {
  get name() { return this._name; }
  set name(arg) { this._name = arg; }
  get officeAreaCode() { return this._officeAreaCode; }
  set officeAreaCode(arg) { this._officeAreaCode = arg; }
  get officeNumber() { return this._officeNumber; }
  set officeNumber(arg) { this._officeNumber = arg; }

  get telephoneNumber() {
    return `(${this._officeAreaCode}) ${this._officeNumber}`;
  }
}
```

**이후**:
```javascript
class Person {
  constructor() {
    this._telephoneNumber = new TelephoneNumber();
  }
  get name() { return this._name; }
  set name(arg) { this._name = arg; }
  get telephoneNumber() { return this._telephoneNumber.toString(); }
  get officeAreaCode() { return this._telephoneNumber.areaCode; }
  set officeAreaCode(arg) { this._telephoneNumber.areaCode = arg; }
}

class TelephoneNumber {
  get areaCode() { return this._areaCode; }
  set areaCode(arg) { this._areaCode = arg; }
  get number() { return this._number; }
  set number(arg) { this._number = arg; }
  toString() { return `(${this._areaCode}) ${this._number}`; }
}
```

---

## 빠른 참조: 스멜 to 리팩토링

| 코드 스멜 | 주요 리팩토링 | 대안 |
|------------|-------------------|-------------|
| 긴 메서드 | 메서드 추출 | 임시 변수를 질의로 대체 |
| 중복 코드 | 메서드 추출 | 메서드 올리기 |
| 큰 클래스 | 클래스 추출 | 서브클래스 추출 |
| 긴 매개변수 목록 | 매개변수 객체 도입 | 전체 객체 보존 |
| 기능 선망 | 메서드 이동 | 메서드 추출 + 이동 |
| 데이터 덩어리 | 클래스 추출 | 매개변수 객체 도입 |
| 원시 타입 집착 | 객체로 원시 타입 대체 | 타입 코드 대체 |
| Switch 문 | 조건문을 다형성으로 대체 | 타입 코드 대체 |
| 임시 필드 | 클래스 추출 | Null 객체 도입 |
| 메시지 체인 | 위임 숨기기 | 메서드 추출 |
| 중개자 | 중개자 제거 | 메서드 인라인 |
| 발산적 변경 | 클래스 추출 | 단계 분할 |
| 샷건 수술 | 메서드 이동 | 클래스 인라인 |
| 죽은 코드 | 죽은 코드 제거 | - |
| 투기적 일반화 | 계층 구조 축소 | 클래스 인라인 |

---

## 추가 읽기

- Fowler, M. (2018). *Refactoring: Improving the Design of Existing Code* (2nd ed.)
- 온라인 카탈로그: https://refactoring.com/catalog/
