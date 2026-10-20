# 🚀 OutSystems 학습 및 실습 과제 저장소

아웃시스템즈(OutSystems) 로우코드 개발 학습 과정을 일차별로 정리하고 기록하는 저장소입니다.  
각 일차별 실습 결과 링크와 핵심 구현 기능, 상세 시나리오를 포함하고 있습니다.

---

## 📅 일차별 학습 및 과제 현황

- [Day 1: OutSystems 개요 및 OT](#)
- [Day 2: Aggregate 필터링 및 변수 바인딩](#-day-2-aggregate-필터링-및-변수-바인딩)
- [Day 3: Entity 1:1 관계 및 CRUD (상세 화면 / 저장 / 삭제)](#-day-3-entity-11-관계-및-crud-상세-화면--저장--삭제)
- [Day 4: 대시보드 레이아웃 및 위젯/액션 실습](#-day-4-대시보드-레이아웃-및-위젯액션-실습)
<!-- 추후 Day 5 등을 여기에 목록으로 추가할 수 있습니다 -->

---

## 📌 Day 2: Aggregate 필터링 및 변수 바인딩

Aggregate의 Filter 기능, 변수(Local Variable) 바인딩, 그리고 `Refresh Data` 액션을 활용하여 검색 및 동적 데이터 조회 기능을 구현했습니다.

### 🔗 실습 결과 링크
- **Customer Main Page** 👉 [결과 화면 보기](https://personal-fdeqhkkx.outsystemscloud.com/CustomerMainPage/?_ts=639281000459416485)  
  * 단일 키워드 기반 고객 이름 실시간 필터링 기능 구현
- **Course Main Page** 👉 [결과 화면 보기](https://personal-fdeqhkkx.outsystemscloud.com/Course_Main/?_ts=639280999479179617)  
  * 키워드 및 상태(Dropdown) 복합 조건 필터링 및 데이터 갱신 기능 구현

### 🛠️ 핵심 구현 기능
- **단일 조건 검색/필터링 (`CustomerMainPage`)**
  - `SearchKeyword` Local Variable과 Input 위젯 바인딩
  - Aggregate Filter 조건 설정 (`CustomerName like "%" + SearchKeyword + "%"`)
- **복합 조건 검색 및 데이터 새로고침 (`Course_Main`)**
  - Multi-filter 구현 (검색어 + 상태 Dropdown 복합 조건)
  - `SearchAction` 내 `RefreshData`를 통한 동적 데이터 재조회

<details>
<summary><b>🔍 Day 2 상세 실습 시나리오 보기 (클릭)</b></summary>

#### 🧩 Mission 1: Customer 조회 및 검색 구현
1. Customer Entity 속성 확인
2. `SearchKeyword` Local Variable 선언
3. Input 위젯 배치 및 `SearchKeyword` 바인딩
4. `GetCustomerFiltered` Aggregate 생성 및 Filter 조건 설정 (`Customer.CustomerName like "%" + SearchKeyword + "%"`)
5. Table 위젯 배치 및 `GetCustomerFiltered.List` 바인딩
6. 검색 Button 배치 및 `SearchCustomerAction` 연결

#### 🧩 Mission 2: Course 복합 필터링 구현
1. Course Entity 속성 확인
2. `SearchKeyword` + `FilterStatus` Local Variable 생성
3. `GetCourseFiltered` Aggregate 생성 및 복합 Filter 조건 설정
4. Input + Dropdown 위젯 배치 및 각 변수 바인딩
5. Table 배치 및 Source 바인딩
6. 검색 Button → `SearchAction` → `RefreshData(GetCourseFiltered)` 연결

</details>

---

## 📌 Day 3: Entity 1:1 관계 및 CRUD (상세 화면 / 저장 / 삭제)

Entity 간의 1:1 관계(Relationship)를 이해하고, Parameter를 활용한 화면 이동 및 데이터의 **C**reate / **R**ead / **U**pdate / **D**elete 액션을 구현했습니다.

### 🔗 실습 결과 링크
- **Customer Management App** 👉 [결과 화면 보기](https://personal-fdeqhkkx.outsystemscloud.com/CustomerFrontend/)  
  * 고객 목록 조회, 상세 정보 및 연관 정보 Form 조회/수정/삭제 기능 구현

### 🛠️ 핵심 구현 기능
- **1:1 관계 데이터 모델링 및 조회 (`Customer` & `CustomerDetail`)**
  - CustomerId를 식별자(Identifier)로 사용하는 1:1 관계 연동
  - Input Parameter(`CustomerId`) 기반의 Aggregate 필터링 및 Single Record 조회 (`Max Records = 1`)
- **화면 간 데이터 전달 & Form 바인딩**
  - List의 Name 컬럼을 Link로 변경하여 `CustomerDetail` 화면으로 `CustomerId` 전달
  - 2개의 Aggregate 결과 레코드를 단일/복수 Form 컴포넌트에 바인딩
- **데이터 CUD 처리 및 예외/순서 제어**
  - **Save**: `Form.Valid` 검증 후 `UpdateCustomer` ➔ `UpdateCustomerDetail` 연속 실행 및 알림(Feedback Message)
  - **Delete**: 참조 무결성(FK 관계)을 고려하여 **`DeleteCustomerDetail` ➔ `DeleteCustomer` 순서로 삭제** 처리

<details>
<summary><b>🔍 Day 3 상세 실습 시나리오 보기 (클릭)</b></summary>

#### 🧩 Mission: Customer 1:1 관계 CRUD 및 상세 화면 구현
1. Customer / CustomerDetail Entity 속성 및 1:1 관계 설정 확인
2. `CustomerList` 화면 생성 + `GetCustomers` Aggregate + Table 위젯 배치
3. `CustomerDetail` 화면 생성 및 Input Parameter `CustomerId` (Customer Identifier, Mandatory) 추가
4. Table의 Name 컬럼을 Link로 변경 (Destination: `CustomerDetail`, Parameter: `GetCustomers.List.Current.Customer.Id`)
5. `CustomerDetail` 화면 내 2개 Aggregate 생성:
   - `GetCustomerById` (Filter: `CustomerId = CustomerId`, Max Records = 1)
   - `GetCustomerDetailByCustomerId` (Filter: `CustomerDetail.CustomerId = CustomerId`, Max Records = 1)
6. 기본 정보 및 상세 정보 Form 배치 후 두 Aggregate의 Current 레코드 각각 바인딩
7. **Save Action 구현**: `If Form.Valid` ➔ `UpdateCustomer` ➔ `UpdateCustomerDetail` ➔ Feedback Message ➔ Navigate to `CustomerList`
8. **Delete Action 구현**: `DeleteCustomerDetail` ➔ `DeleteCustomer` (삭제 순서 유의) ➔ Feedback Message ➔ Navigate to `CustomerList`

</details>

---

## 📌 Day 4: 대시보드 레이아웃 및 위젯/액션 실습

Container를 활용한 화면 레이아웃 구성, 집계 데이터 표현(Count), 그리고 위젯과 Local Variable 간의 바인딩 및 Client Action 상호작용을 실습했습니다.

### 🔗 실습 결과 링크
- **Order Dashboard App** 👉 [결과 화면 보기](https://personal-fdeqhkkx.outsystemscloud.com/Order/?_ts=639281961359138569)  
  * 주문 대시보드 화면 구성, 총 주문 건수 집계, 상품명 링크 및 최근 주문 목록 구현
- **Task Widget App** 👉 [결과 화면 보기](https://personal-fdeqhkkx.outsystemscloud.com/TaskWidget/?_ts=639281961979691147)  
  * 위젯 배치, Local Variable 바인딩 및 Client Action을 통한 동적 값 변경 확인

### 🛠️ 핵심 구현 기능
- **대시보드 레이아웃 및 데이터 통계 (`OrderDashboard`)**
  - 제공된 샘플 Excel을 통한 `Order` 엔티티 Bootstrap 데이터 적재
  - Container 및 Columns(2단 분할)를 활용한 대시보드 구조 설계
  - `GetOrders.Count`를 이용한 총 주문 건수 표현 및 `GetRecentOrders` (최신 2건 정렬)를 활용한 최근 주문 목록 구성
- **위젯 상호작용 및 Client Action 제어 (`TaskWidget`)**
  - 화면 내 위젯 배치 및 Local Variable 선언 후 속성 바인딩
  - Button Client Action을 연결하여 버튼 클릭 시 Variable 값이 변경되는 로직 구현

<details>
<summary><b>🔍 Day 4 상세 실습 시나리오 보기 (클릭)</b></summary>

#### 🧩 Mission 1: Order 대시보드 구현 (`OrderDashboard`)
1. Order 엔티티 구조 확인 및 제공된 샘플 Excel 데이터 Bootstrap 실행
2. `OrderDashboard` 화면 생성 및 `GetOrders` Aggregate 배치
3. Container 위젯 배치 후 Columns(2단)를 사용하여 좌우 레이아웃 분할
4. **총 주문 건수 표시**: Expression 위젯을 배치하고 `GetOrders.Count` 바인딩
5. **상품명 링크**: Table 위젯(Source=`GetOrders.List`)을 배치하고 상품명 컬럼을 Link로 변경 (클릭 시 Message 출력)
6. **최근 주문 목록**: 별도의 `GetRecentOrders` Aggregate 생성 (OrderDate 내림차순 정렬, Max Records = 2) 후 최신 2건 표시 List 위젯 연동
7. **검색 영역**: Form + Input(검색어) + Button(클릭 시 Message) 조합 배치

#### 🧩 Mission 2: 위젯 및 Local Variable 실습 (`TaskWidget`)
1. 화면에 각종 위젯 배치 및 Local Variable 선언
2. 위젯 속성 설정 및 변수(`Variable`) 바인딩 수행
3. Button Client Action 작성 후 버튼 클릭 시 Variable 값이 새로운 값(`NewValue`)으로 정상 변경되는지 확인

</details>

---

> **Note:** 실습 링크는 개인 학습용 OutSystems Personal Environment 환경에서 구동되며, 서버 상태에 따라 접속이 원활하지 않을 수 있습니다.
