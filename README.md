# 🚀 OutSystems 학습 및 실습 과제 저장소

아웃시스템즈(OutSystems) 로우코드 개발 학습 과정을 일차별로 정리하고 기록하는 저장소입니다.  
각 일차별 실습 결과 링크와 핵심 구현 기능, 상세 시나리오를 포함하고 있습니다.

---

## 📅 일차별 학습 및 과제 현황

- [Day 1: OutSystems 개요 및 OT](#day-1)
- [Day 2: Aggregate 필터링 및 변수 바인딩](#day-2)
- [Day 3: Entity 1:1 관계 및 CRUD (상세 화면 / 저장 / 삭제](#day-3)
- [Day 4: 대시보드 레이아웃 및 위젯/액션 실습](#day-4)
- [Day 5: 엔터티 관계(1:N, N:1) 및 마스터-디테일 CRUD 종합 실습](#day-5)
- [Day 6: Entity 1:1 관계 상세 화면 및 참조 무결성 삭제 제어 실습](#day-6)
- [Day 7: 장바구니(Cart) 다중 선택, 페이징 및 자바스크립트 연동 실습](#day-7)
- [Day 8: Data Action을 활용한 서버사이드 복합 검색 및 리스트 매핑 실습](#day-8)
- [Day 9: 매출 대시보드(KPI/별점 블록) 및 직원 관리(서버사이드 정렬/페이징/파일 업로드) 실습](#day-9)
- [Day 10: Advanced SQL 및 Server Action SQL 쿼리 실습](#day-10)

---

<div id="day-2"></div>

## 📌 Day 2: Aggregate 필터링 및 변수 바인딩

Aggregate의 Filter 기능, 변수(Local Variable) 바인딩, 그리고 `Refresh Data` 액션을 활용하여 검색 및 동적 데이터 조회 기능을 구현했습니다.

### 🔗 실습 결과 링크
- **Customer Main Page** 👉 <a href="https://personal-fdeqhkkx.outsystemscloud.com/CustomerMainPage" target="_blank" rel="noopener noreferrer">결과 화면 보기</a>  
  * 단일 키워드 기반 고객 이름 실시간 필터링 기능 구현
- **Course Main Page** 👉 <a href="https://personal-fdeqhkkx.outsystemscloud.com/Course_Main" target="_blank" rel="noopener noreferrer">결과 화면 보기</a>  
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

<div id="day-3"></div>

## 📌 Day 3: Entity 1:1 관계 및 CRUD (상세 화면 / 저장 / 삭제)

Entity 간의 1:1 관계(Relationship)를 이해하고, Parameter를 활용한 화면 이동 및 데이터의 **C**reate / **R**ead / **U**pdate / **D**elete 액션을 구현했습니다.

### 🔗 실습 결과 링크
- **Customer Management App** 👉 <a href="https://personal-fdeqhkkx.outsystemscloud.com/CustomerFrontend/" target="_blank" rel="noopener noreferrer">결과 화면 보기</a>  
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

<div id="day-4"></div>

## 📌 Day 4: 대시보드 레이아웃 및 위젯/액션 실습

Container를 활용한 화면 레이아웃 구성, 집계 데이터 표현(Count), 그리고 위젯과 Local Variable 간의 바인딩 및 Client Action 상호작용을 실습했습니다.

### 🔗 실습 결과 링크
- **Order Dashboard App** 👉 <a href="https://personal-fdeqhkkx.outsystemscloud.com/Order" target="_blank" rel="noopener noreferrer">결과 화면 보기</a>  
  * 주문 대시보드 화면 구성, 총 주문 건수 집계, 상품명 링크 및 최근 주문 목록 구현
- **Task Widget App** 👉 <a href="https://personal-fdeqhkkx.outsystemscloud.com/TaskWidget" target="_blank" rel="noopener noreferrer">결과 화면 보기</a>  
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

<div id="day-5"></div>

## 📌 Day 5: 엔터티 관계(1:N, N:1) 및 마스터-디테일 CRUD 종합 실습

Customer(9개 속성), Product, Order 간의 1:N(`Customer` → `Order`) 및 N:1(`Order` → `Product`) 관계를 설정하고, 마스터-디테일 구조의 통합 관리 및 연관 데이터 CUD 처리를 구현했습니다.

### 🔗 실습 결과 링크
- **Customer & Order Management App** 👉 <a href="https://personal-fdeqhkkx.outsystemscloud.com/CustomerOrder" target="_blank" rel="noopener noreferrer">결과 화면 보기</a>  
  * 고객 목록 및 선택된 고객의 연관 주문 목록(카드 형태)을 연동하여 조회, 수정, 등록, 삭제 기능 구현

### 🛠️ 핵심 구현 기능
- **마스터-디테일 양방향 연동 (`CustomerList`)**
  - Table 위젯(Name, Email, Grade)을 통한 고객 마스터 목록 제공 및 행 클릭 시 `SelectCustomerOnClick` 액션을 통해 `CurrentCustomerId` 갱신
  - Order와 Product를 Join한 `GetOrdersByCustomerId` Aggregate를 활용하여 우측 패널에 선택된 고객의 주문 내역 동적 표시 (미선택 및 주문 없음 상태별 If 분기 처리)
- **고객(Customer) 등록 및 수정 화면 분기 (`CustomerDetail`)**
  - Input Parameter `CustomerId` (`Mandatory=No`) 및 `GetCustomerById` (Filter: `Customer.Id = CustomerId`, Max Records=1) 활용
  - 8개 필드를 포함한 Form 구성 및 Name·Email 필수값 Validation 설정
  - 신규 등록(`CustomerId = NullIdentifier()`)과 기존 정보 수정 처리 로직 구현
- **주문(Order) 관리 및 디테일 처리 (`OrderDetail`)**
  - 상품 드롭다운(`GetProducts.List` 소스) 및 수량·상태 입력을 위한 Form 구성
  - Save 시 `Form.Valid` 검증 후 `Order.CustomerId` 할당 및 `CreateOrUpdateOrder` 실행
  - 카드 단위 삭제 기능(`DeleteOrder` 서버 액션 연동 및 `GetOrdersByCustomerId` 새로고침) 구현

<details>
<summary><b>🔍 Day 5 상세 실습 시나리오 보기 (클릭)</b></summary>

#### 🧩 Mission: Customer-Order 마스터 디테일 및 CRUD 종합 구현
1. **데이터 모델링 확인**: Customer(9개 속성), Product, Order 엔터티 구조 및 1:N(`Customer`→`Order`), N:1(`Order`→`Product`) 관계 설정 및 Delete Rule 확인
2. **CustomerList 화면 구성**:
   - `GetCustomers` Aggregate 및 Table 위젯(Name, Email, Grade 컬럼) 배치
   - CustomerList에 Input Parameter `SelectedCustomerId` 및 Local Variable `CurrentCustomerId` 추가
3. **고객 상세/등록 화면 (`CustomerDetail`) 구현**:
   - CustomerDetail 화면 생성 및 Input Parameter `CustomerId` (`Customer Identifier`, `Mandatory=No`) 추가
   - Name 컬럼을 Link(`CustomerDetail`)로 변경하고, 상단에 `+ 신규 등록` 버튼 배치 (`CustomerId = NullIdentifier()`)
   - `GetCustomerById` Aggregate 생성 (Filter: `Customer.Id = CustomerId`, Max Records=1)
   - CustomerDetail Form(8필드) 배치 및 Name·Email 필수값 Validation 설정
   - **Save 액션**: `If Form.Valid` ➔ `CreateOrUpdateCustomer` ➔ Message ➔ Navigate to `CustomerList` (SelectedCustomerId = 반환 ID 전달)
4. **목록 내 삭제 및 인터랙션 처리**:
   - 목록 Delete 컬럼 추가 ➔ `DeleteCustomer` 서버 액션 연동 ➔ `GetCustomers` 새로고침
   - 행 클릭 시 `SelectCustomerOnClick`: `CurrentCustomerId` Assign ➔ `GetOrdersByCustomerId` 새로고침 (오른쪽 패널 표출)
   - 우측 패널 If 분기 처리: 미선택 시(`CurrentCustomerId = NullIdentifier()`) 안내 문구 및 주문 없음(`List.Empty`) 안내 문구 표시
5. **주문 카드 목록 및 상세(`OrderDetail`) 연동**:
   - 주문 카드 List(Product.Name, Quantity, Status, Price) 배치 및 카드 Delete (`DeleteOrder` 서버 액션 ➔ `GetOrdersByCustomerId` 새로고침)
   - `+ 주문 추가` 버튼 클릭 시 `OrderDetail` 화면 이동
   - `OrderDetail` 화면 구성: Input 파라미터(`OrderId`, `CustomerId`), `GetOrderById`, `GetProducts` (드롭다운 소스) Aggregate 배치
   - Form 구성: Product Dropdown(`Source=GetProducts.List`), Quantity, Status 배치
   - **Save 주문 액션**: `If Form.Valid` ➔ `Order.CustomerId` Assign ➔ `CreateOrUpdateOrder` ➔ Navigate to `CustomerList` (SelectedCustomerId 전달)

</details>

---

<div id="day-6"></div>

## 📌 Day 6: Entity 1:1 관계 상세 화면 및 참조 무결성 삭제 제어 실습

Customer와 CustomerDetail 간의 1:1 관계 모델링을 이해하고, 상세 화면 분기 처리와 연관 엔티티 간 참조 무결성을 고려한 삭제(Delete) 순서 제어 로직을 구현했습니다.

### 🔗 실습 결과 링크
- **Customer 1:1 Detail Practice App** 👉 <a href="https://personal-fdeqhkkx.outsystemscloud.com/OutsystemsPractice6/" target="_blank" rel="noopener noreferrer">결과 화면 보기</a>  
  * 1:1 관계로 연결된 고객 기본/상세 정보 조회, 수정, 그리고 안전한 순서의 데이터 삭제 처리 구현

### 🛠️ 핵심 구현 기능
- **1:1 관계 엔티티 구성 및 화면 이동 (`CustomerList`)**
  - Customer와 CustomerDetail 엔티티 속성 및 1:1 관계 설정 확인
  - Table 위젯의 Name 컬럼을 Link로 변경하여 `CustomerDetail` 화면으로 이동 (`CustomerId = GetCustomers.List.Current.Customer.Id`)
- **다중 Aggregate 바인딩 및 상세 Form 구성 (`CustomerDetail`)**
  - Input Parameter `CustomerId` (`Mandatory`) 설정
  - 두 개의 Aggregate (`GetCustomerById`, `GetCustomerDetailByCustomerId`)를 각각 생성하여 Filter 조건 적용 (`Max Records=1`)
  - 기본 정보 및 상세 정보 Form을 배치하고 두 Aggregate의 Current 레코드 각각 바인딩
- **참조 무결성을 고려한 CRUD 처리 (`Save` & `Delete`)**
  - **Save**: `Form.Valid` 검증 후 `UpdateCustomer`와 `UpdateCustomerDetail` 연속 실행 ➔ Feedback Message ➔ `CustomerList`로 이동
  - **Delete**: FK 관계로 묶인 두 엔티티의 참조 무결성을 고려하여 **`DeleteCustomerDetail` 후 `DeleteCustomer`를 실행하는 순서 제어** 구현

<details>
<summary><b>🔍 Day 6 상세 실습 시나리오 보기 (클릭)</b></summary>

#### 🧩 Mission: Customer 1:1 관계 상세 및 삭제 순서 제어 구현
1. **엔티티 관계 확인**: Customer / CustomerDetail Entity 속성 및 1:1 관계 설정 확인
2. **CustomerList 화면 구성**:
   - `GetCustomers` Aggregate 생성 및 Table 위젯 배치
3. **CustomerDetail 화면 및 파라미터 연동**:
   - CustomerDetail 화면 생성 및 Input Parameter `CustomerId` (Customer Identifier, Mandatory) 추가
   - Table의 Name 컬럼을 Link로 변경 (Destination: `CustomerDetail`, Parameter: `GetCustomers.List.Current.Customer.Id`)
4. **Aggregate 및 Form 바인딩**:
   - `GetCustomerById` Aggregate 생성 (Filter: `CustomerId = CustomerId`, Max Records=1)
   - `GetCustomerDetailByCustomerId` Aggregate 생성 (Filter: `CustomerDetail.CustomerId = CustomerId`, Max Records=1)
   - 기본/상세 정보 Form 배치 후 두 Aggregate의 Current 레코드를 각각 바인딩
5. **Save 액션 구현**:
   - `If Form.Valid` ➔ `UpdateCustomer` ➔ `UpdateCustomerDetail` ➔ Feedback Message ➔ Navigate to `CustomerList`
6. **Delete 액션 구현 (참조 무결성 제어)**:
   - `DeleteCustomerDetail` ➔ `DeleteCustomer` ➔ Feedback Message ➔ Navigate to `CustomerList` (**삭제 순서가 매우 중요**)

</details>

---

<div id="day-7"></div>

## 📌 Day 7: 장바구니(Cart) 다중 선택, 페이징 및 자바스크립트 연동 실습

Customer, Product, Cart 엔터티 간의 관계(`Cart` → `Customer` N:1, `Cart` → `Product` N:1)를 설정하고, 체크박스 다중 선택, 페이징네이션, JavaScript를 활용한 컨펌 다이얼로그 및 장바구니 저장/관리 기능을 구현했습니다.

### 🔗 실습 결과 링크
- **Cart & Order App** 👉 <a href="https://personal-fdeqhkkx.outsystemscloud.com/CartOrderApp/" target="_blank" rel="noopener noreferrer">결과 화면 보기</a>  
  * 상품 목록 다중 선택(페이지 유지), 페이징, JavaScript 확인창 연동 및 장바구니 일괄 저장/상세 내역 관리 구현

### 🛠️ 핵심 구현 기능
- **다중 선택 상태 유지 및 페이징네이션 (`CartOrder`)**
  - 상품 20건 Excel Bootstrap 및 페이징 처리를 위한 `GetProducts` Aggregate (`Max Records=PageSize`, `Start Index=(CurrentPage-1)*PageSize`) 구성
  - 체크박스 상태 관리를 위해 `SelectedProductIds` (Product Id List) Local Variable 활용 (`ListIndexOf` 검증 및 `ToggleSelect` 액션을 통한 페이지 이동 시 선택 유지 구현)
- **JavaScript 노드를 활용한 커스텀 컨펌 다이얼로그**
  - `ConfirmDialog` 클라이언트 액션 내 JavaScript 노드 작성 (`$parameters.Result = confirm($parameters.Message);`)을 통해 브라우저 기본 확인창 연동
- **장바구니 추가/제거 및 일괄 저장 처리**
  - `+` 버튼(`AddToCartOnClick`): 확인창 호출 ➔ True 시 루프(`GetAllProducts.List`)를 돌며 체크된 상품을 `CartList`에 추가 및 선택 초기화
  - `-` 버튼(`RemoveFromCartOnClick`): 장바구니에서 체크된 상품 제거
  - `SAVE` 버튼(`SaveCartOnClick`): 확인창 호출 ➔ `SaveCart` 서버 액션(`CustomerId`와 상품 리스트를 전체 Cart 행으로 저장) 실행 ➔ `CartList` 비움
- **장바구니 상세 및 개별 삭제 (`CartDetail`)**
  - 우상단 '장바구니 상세' 링크를 통해 `CartDetail` 화면 이동 (`CustomerId=SelectedCustomerId` 파라미터 전달)
  - `GetCartByCustomer` (Cart + Product Join)를 활용한 내역 조회 및 각 행별 삭제 버튼(`DeleteCart` 서버 액션 및 데이터 새로고침) 구현

<details>
<summary><b>🔍 Day 7 상세 실습 시나리오 보기 (클릭)</b></summary>

#### 🧩 Mission: Cart 다중 선택, 페이징 및 저장/상세 관리 구현
1. **엔티티 모델링 및 데이터 Bootstrap**:
   - Customer / Product / Cart 엔터티 생성 및 관계 설정 확인 (`Cart`→`Customer` N:1, `Cart`→`Product` N:1, Delete Rule 확인)
   - Product 20건 및 Customer 시드 데이터 Excel Bootstrap 실행 (페이지네이션·드롭다운 테스트용)
2. **CartOrder 화면 및 변수 선언**:
   - Local Variables: `SelectedCustomerId`, `SelectedProductIds` (Product Id List), `CartList` (Product List), `CurrentPage=1`, `PageSize=10`
   - 상단 고객 Dropdown: Source=`GetCustomers.List`, 표시=`Customer.Name`, 값=`SelectedCustomerId`
3. **상품 목록 페이징 및 체크박스 인터랙션**:
   - `GetProducts` Aggregate (`Max Records=PageSize`, `Start Index=(CurrentPage-1)*PageSize`), `GetAllProducts`, `GetProductCount` 생성
   - 왼쪽 상품 Table 배치: 첫 컬럼에 체크박스 배치 (`Checked=ListIndexOf(SelectedProductIds, Product.Id)>=0`, `OnChange=ToggleSelect`)
   - `ToggleSelect` 스크린 액션: `SelectedProductIds`에 ProductId 추가/제거 (페이지를 넘겨도 선택 상태가 유지되도록 구현)
   - 페이징네이션 구현 (10개/페이지, 1~N 버튼, 이전/다음) ➔ `GoToPage` ➔ `Refresh Data(GetProducts)`
4. **JavaScript 다이얼로그 및 장바구니 담기/빼기**:
   - `ConfirmDialog` 클라이언트 액션 구현 및 JavaScript 노드 추가: `$parameters.Result = confirm($parameters.Message);` (Output: `Confirmed` Boolean 반환)
   - `+` 버튼 (`AddToCartOnClick`): `ConfirmDialog("장바구니에 등록하시겠습니까?")` ➔ True 시 Loop(`GetAllProducts.List`)를 돌며 체크 상품을 `CartList`에 추가 후 체크 해제
   - `-` 버튼 (`RemoveFromCartOnClick`): 장바구니에서 체크된 상품 제거
5. **장바구니 저장 및 상세 (`CartDetail`) 연동**:
   - 오른쪽 장바구니 Table 배치: Source=`CartList` (상품명, 단가, 합계)
   - Cart 엔터티 + `SaveCart` 서버 액션 생성 (Input: `CustomerId`, `Products=Product List` ➔ 리스트 전체를 Cart 행으로 저장)
   - `SAVE` 버튼 (`SaveCartOnClick`): `ConfirmDialog("저장하시겠습니까?")` ➔ True 시 `SaveCart(SelectedCustomerId, CartList)` 실행 ➔ 메시지 표시 ➔ `CartList` 비움
   - 우상단 '장바구니 상세' 링크: `CartDetail` 화면 이동 (`CustomerId=SelectedCustomerId` 파라미터 전달)
   - `CartDetail` 화면 구성: `GetCartByCustomer` (Cart + Product Join) Aggregate를 이용해 내역 표시, 각 상품 행 끝 삭제 버튼 ➔ `DeleteCart` 서버 액션 ➔ `Refresh Data(GetCartByCustomer)` 연동

</details>

---

<div id="day-8"></div>

## 📌 Day 8: Data Action을 활용한 서버사이드 복합 검색 및 리스트 매핑 실습

ProductCategory와 Product 엔터티 간 1:N 관계를 설정하고, `On Demand`로 동작하는 Data Action을 활용하여 카테고리 필터와 부분 일치(LIKE) 검색을 조합한 서버사이드 복합 검색 기능을 구현했습니다.

### 🔗 실습 결과 링크
- **Product Search App** 👉 <a href="https://personal-fdeqhkkx.outsystemscloud.com/ProductSearch/" target="_blank" rel="noopener noreferrer">결과 화면 보기</a>  
  * 카테고리 드롭다운 및 상품명 키워드 조건에 따른 실시간 서버사이드 검색 및 구조체(Structure) 매핑 구현

### 🛠️ 핵심 구현 기능
- **엔터티 모델링 및 데이터 Bootstrap**
  - ProductCategory(Id, Label) 및 Product(Id, Name, CategoryId, Price) 엔터티 생성 및 1:N 관계 확인 (`Name`, `CategoryId` 필수 설정)
  - Excel Bootstrap을 통한 ProductCategory 시드(7건) 및 Product 시드(약 22건) 적재
- **Data Action 및 On Demand 데이터 페칭**
  - 화면 진입 시 초기 조회를 방지하기 위해 `Fetch = On Demand`로 설정된 `SearchProducts` Data Action 구현 (Input: `CategoryId`, `SearchName` / Output: `ProductRow` List)
  - Data Action 내부에서 Product와 ProductCategory를 Join한 Aggregate 실행 (선택적 분류 필터 및 `Index(Product.Name, SearchName) >= 0` 부분 검색 조건 적용)
  - `For Each` 루프를 활용하여 Join 결과를 `ProductRow` 구조체 리스트로 매핑하여 반환
- **동적 필터 인터랙션 및 화면 분기**
  - 분류 Dropdown의 `OnChange` 이벤트와 조회 Button의 `Click` 이벤트를 통해 `Refresh Data(SearchProducts)` 실행 및 즉시 갱신
  - 초기 진입 시 빈 목록 상태에 대한 안내 문구 표출을 위한 If 분기 처리 구현

<details>
<summary><b>🔍 Day 8 상세 실습 시나리오 보기 (클릭)</b></summary>

#### 🧩 Mission: Data Action 기반 복합 검색 및 데이터 매핑 구현
1. **엔터티 및 시드 데이터 준비**:
   - `ProductCategory`(Id, Label) / `Product`(Id, Name, CategoryId, Price) 엔터티 생성 및 1:N 관계 설정 (`Name`, `CategoryId` 필수)
   - Excel Bootstrap 활용하여 ProductCategory(7건) 및 Product(약 22건) 시드 데이터 적재
2. **ProductSearch 화면 및 Local Variables 선언**:
   - Local Variables: `SelectedCategoryId` (ProductCategory Identifier), `SearchName` (Text)
   - `GetCategories` Aggregate 생성 (`Fetch = At Start`) 및 상단 상품분류 Dropdown 바인딩 (`Source=GetCategories.List`, 표시=`Label`, 값=`SelectedCategoryId`)
3. **ProductRow Structure 정의 및 Data Action 생성**:
   - `ProductRow` Structure 정의 (속성: `ProductName Text`, `CategoryLabel Text`, `Price Currency`)
   - `SearchProducts` Data Action 생성 (`Fetch = On Demand`, Input: `CategoryId`, `SearchName`, Output: `ProductRow List`)
   - Data Action 내부 로직:
     - Product와 ProductCategory를 Join하는 Aggregate (`GetProducts`) 배치 (필터: 분류 선택값 optional 및 `Index(Product.Name, SearchName) >= 0`)
     - Loop (`For Each`)를 사용하여 Join된 결과를 `ProductRow` 리스트에 매핑하여 반환
4. **화면 위젯 및 이벤트 연결**:
   - 결과 Table 배치 (Source=`SearchProducts` 출력 List) 및 컬럼 구성 (상품명, 분류, 단가)
   - 분류 Dropdown `OnChange` 이벤트 연결 ➔ `Refresh Data(SearchProducts)` (변경 즉시 필터링)
   - 상품명 Input + 조회 Button 연결 (`SearchOnClick`) ➔ `Refresh Data(SearchProducts)` (부분 LIKE 검색)
   - **On Demand 동작 확인**: 화면 진입 시 초기 데이터가 없는 빈 목록 상태를 처리하기 위한 If 분기 안내 메시지 구현

</details>

---

<div id="day-9"></div>

## 📌 Day 9: 매출 대시보드(KPI/별점 블록) 및 직원 관리(서버사이드 정렬/페이징/파일 업로드) 실습

상품 매출 데이터 집계 및 **블록(Block) 컴포넌트**를 활용한 별점 상호작용 기능, 그리고 대규모 데이터 처리를 위한 서버사이드 페이징, 동적 정렬, 바이너리 파일 업로드/다운로드를 구현했습니다.

### 🔗 실습 결과 링크
- **Sales Dashboard App** 👉 <a href="https://personal-fdeqhkkx.outsystemscloud.com/SalesDashboardReal/" target="_blank" rel="noopener noreferrer">결과 화면 보기</a>  
  * 매출 KPI 집계, 상품별 판매 집계 및 커스텀 StarRating 블록을 통한 실시간 별점 평가 기능 구현
- **Employee Practice App** 👉 <a href="https://personal-fdeqhkkx.outsystemscloud.com/EmployeePractice/" target="_blank" rel="noopener noreferrer">결과 화면 보기</a>  
  * 직원 60여 명 시드 데이터 기반 서버사이드 페이징, 동적 정렬 헤더, 프로필 사진 업로드/다운로드 및 상세 CRUD 구현

### 🛠️ 핵심 구현 기능
- **매출 대시보드 및 커스텀 별점 블록 (`SalesDashboard`)**
  - Product, ProductSale 엔터티 1:N 관계 설정 및 Excel Bootstrap을 통한 시드 데이터 적재
  - `GetSalesKpi` Aggregate를 통한 총 수량·금액 및 평균 금액 집계 및 KPI 카드 3개 구성
  - `StarRating` UI 블록 생성 (Input: `Rating`, `IsEditable` / Output Event: `OnRate`), 아이콘 위젯 및 조건부 이벤트 바인딩을 통한 인터랙티브 별점 시스템 구축
- **대규모 직원 데이터 페이징 및 동적 정렬 (`EmployeePractice`)**
  - 11개 속성을 가진 `Employee` 엔터티 생성 및 약 58~60명 시드 데이터 Excel Bootstrap 적재
  - `GetEmployeesPaged` 서버 액션을 구현하여 서버사이드 페이징 (`StartIndex`, `MaxRecords`) 및 동적 정렬 (`SortColumn`, `SortAscending`) 처리
  - OutSystems UI Pagination 패턴 적용 및 헤더 클릭 시 정렬 토글/교체 및 페이지네이션 리셋 로직 구현
- **프로필 사진 업로드 및 다운로드 제어**
  - Upload 위젯을 활용하여 프로필 사진의 Binary 데이터와 파일명(`ProfilePhoto`, `PhotoFileName`) 바인딩 및 저장
  - 사진 유무에 따른 조건부 If 분기를 적용하여 Download 노드를 통한 파일 다운로드(`DownloadPhoto` 액션) 기능 구현

<details>
<summary><b>🔍 Day 9 상세 실습 시나리오 보기 (클릭)</b></summary>

#### 🧩 Mission 1: 매출 대시보드 및 StarRating 블록 구현 (`SalesDashboardReal`)
1. **엔터티 모델링 및 시드 데이터 적재**:
   - `Product` (Id, Name, Category, Rating Integer, 기본 0) / `ProductSale` (수량·금액·판매일) 엔터티 및 1:N 관계 확인/생성
   - Excel Bootstrap 활용하여 Product + ProductSale 시드 데이터 적재 (`Rating` 초기값 0)
2. **대시보드 및 KPI 구성**:
   - `SalesDashboard` 화면 생성 및 상품판매정보 Table 배치 (`GetSales`: ProductSale + Product Join)
   - `GetSalesKpi` Aggregate 생성 (`Sum(Quantity)`, `Sum(Amount)`, `Average(Amount)`) 및 KPI 카드 3개 구성
   - `GetSalesByProduct` Aggregate 생성 (Group By Product, Sum·Sum·Average 집계) 및 상품별 집계 Table 배치
3. **StarRating 블록 및 상호작용 구현**:
   - `StarRating` 블록 생성: Input Parameter (`Rating Integer`, `IsEditable Boolean`), Output Event (`OnRate(NewRating Integer)`) 정의
   - 블록 내부: Icon 위젯 5개 배치, 표시 조건=`If(Rating >= n)`, 편집 모드(`IsEditable`)일 때 별 클릭 시 `OnRate(n)` 이벤트 발생
   - 상품 목록 (`GetProducts`) 배치 및 행 클릭 시 `SelectedProductId` 세팅 (`SelectProduct` 액션 ➔ `GetProductById` 새로고침)
   - 선택 상품 편집 패널에서 `StarRating` 블록의 `IsEditable = Yes` 설정 및 블록 `OnRate` 이벤트 발생 시 `SaveRating(NewRating)` 액션 실행 (`Product.Rating` 세팅 ➔ `CreateOrUpdateProduct` 덮어쓰기 ➔ 데이터 새로고침)

#### 🧩 Mission 2: 직원 관리, 페이징, 정렬 및 파일 처리 구현 (`EmployeePractice`)
1. **Employee 엔터티 및 시드 데이터 준비**:
   - `Employee` 엔터티 (11속성: `EmployeeNo`, `Name`, `Department`, `Position`, `Email`, `Phone`, `HireDate`, `Status`, `ProfilePhoto` Binary Data, `PhotoFileName` 등) 확인 및 생성
   - Excel Bootstrap을 통해 직원 약 58~60명 시드 데이터 적재 (동적 정렬 및 페이징 테스트용)
2. **서버사이드 페이징 및 정렬 액션 구현**:
   - `GetEmployeesPaged` 서버 액션 생성 (Input: `StartIndex`, `MaxRecords`, `SortColumn`, `SortAscending` / Output: `Employees` List, `TotalCount`)
   - `EmployeeList` 화면 생성 및 Local Variables 선언 (`StartIndex=0`, `PageSize=10`, `SortColumn='Name'`, `SortAscending=True`, `TotalPages` 등)
   - `LoadEmployees` 스크린 액션 작성 (`GetEmployeesPaged` 호출 ➔ 결과 Assign ➔ `TotalPages = Trunc((TotalCount + PageSize - 1) / PageSize)` 계산), `OnInitialize`에서 최초 호출
3. **목록 테이블 및 동적 정렬/페이징 UI 연동**:
   - Table 위젯 배치 (Source = `Employees` Local List) 및 컬럼 구성 (사번, 이름, 부서, 직급, 입사일, 상태)
   - 헤더 동적 정렬 구현: `SortBy` 스크린 액션 (같은 컬럼 클릭 시 `SortAscending` 토글, 다른 컬럼 클릭 시 `SortColumn` 교체 및 오름차순 적용, `StartIndex=0` 리셋) 및 헤더 정렬 아이콘 표출
   - OutSystems UI Pagination 패턴 적용 (한 페이지 Max 10건) 및 `PaginationOnNavigate` 액션 연동 (`StartIndex = NewStartIndex` ➔ `LoadEmployees`)
4. **상세 화면, 파일 업로드 및 다운로드 구현**:
   - 행 끝 '상세' 버튼 컬럼 추가 ➔ `EmployeeDetail` 화면 이동 (`EmployeeId = 현재 행 Id`)
   - `EmployeeDetail` 화면 구성: Input Parameter `EmployeeId`, `GetEmployeeById` Aggregate (`Filter Customer.Id = EmployeeId`, `Max Records=1`)
   - Form 구성 (사번, 이름, 부서, 직급, 이메일, 전화, 입사일, 상태 / 사번·이름 필수값 검증)
   - 프로필 사진 업로드: Upload 위젯을 `ProfilePhoto`(Binary)와 `PhotoFileName`에 바인딩
   - 프로필 사진 다운로드: `DownloadPhoto` 스크린 액션(Download 노드 활용) 생성 및 If 분기를 통해 `Employee.ProfilePhoto` 유무에 따른 다운로드 버튼 제어
   - **Save 액션**: `If Form.Valid` ➔ `CreateOrUpdateEmployee` 서버 액션 실행 ➔ Message 표시 ➔ `EmployeeList`로 이동

</details>

---

<div id="day-10"></div>

## 📌 Day 10: Advanced SQL 및 Server Action SQL 쿼리 실습

OutSystems Server Action 내에 SQL 노드를 활용하여 데이터베이스 직접 조회 쿼리를 작성하고, 집계 함수(`GROUP BY`, `HAVING`, `AVG`, `MAX`)를 이용한 부서별 연봉 통계 및 결과 매핑을 구현했습니다.

### 🔗 실습 결과 링크
- **Advanced SQL Practice App** 👉 <a href="https://personal-fdeqhkkx.outsystemscloud.com/SearchProduct" target="_blank" rel="noopener noreferrer">결과 화면 보기</a>  
  * 서버사이드 SQL 노드를 통한 부서별 평균 및 최고 연봉 집계 쿼리 실행 및 Table 위젯 시각화 구현

### 🛠️ 핵심 구현 기능
- **Server Action 내 SQL 노드 활용**
  - Server Action 내부에 SQL 노드를 추가하여 직접 SQL 쿼리 작성 및 실행 제어
- **고급 집계 쿼리 작성 (`GROUP BY`, `HAVING`, `AVG`, `MAX`)**
  - 부서별 그룹화를 수행하고 집계 함수를 적용하여 부서별 평균 연봉(`AVG`)과 최고 연봉(`MAX`) 산출
  - 필요에 따라 `HAVING` 절을 활용한 조건 필터링 구현
- **Output Structure 매핑 및 Table 위젯 시각화**
  - SQL 쿼리 결과를 안전하게 받을 수 있도록 Output Structure를 정의하고 데이터 매핑
  - 화면에 Table 위젯을 배치하여 집계된 부서별 연봉 통계 결과 출력 및 1-Click Publish 후 데이터 정확성 검증

<details>
<summary><b>🔍 Day 10 상세 실습 시나리오 보기 (클릭)</b></summary>

#### 🧩 Mission: 부서별 연봉 통계 SQL 쿼리 및 화면 표시 구현
1. **서버 액션 및 SQL 노드 구성**:
   - Server Action 생성 및 내부 워크플로우에 SQL 노드 추가
2. **SQL 쿼리 작성**:
   - `GROUP BY Department`, `HAVING`, `AVG`, `MAX` 함수를 조합하여 부서별 평균 연봉과 최고 연봉을 계산하는 SQL 문장 작성
3. **Output Structure 정의 및 매핑**:
   - 쿼리 결과를 담을 Output Structure를 정의하고 SQL 노드 Output에 연결하여 결과 매핑
4. **화면 표시 및 검증**:
   - 화면에 Table 위젯을 배치하고 SQL 실행 결과를 소스로 바인딩하여 부서별 연봉 통계 표시
   - 1-Click Publish 실행 후 쿼리 결과의 정확성 검증

</details>

---

> **Note:** 실습 링크는 개인 학습용 OutSystems Personal Environment 환경에서 구동되며, 서버 상태에 따라 접속이 원활하지 않을 수 있습니다.