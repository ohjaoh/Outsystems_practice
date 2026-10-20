# 🚀 OutSystems 학습 및 실습 과제 저장소

아웃시스템즈(OutSystems) 로우코드 개발 학습 과정을 일차별로 정리하고 기록하는 저장소입니다.  
각 일차별 실습 결과 링크와 핵심 구현 기능, 상세 시나리오를 포함하고 있습니다.

---

## 📅 일차별 학습 및 과제 현황

* [Day 1: Outsystems 알아보기 및 강의 Ot ]
* [Day 2: Aggregate 필터링 및 변수 바인딩](#-day-2-aggregate-필터링-및-변수-바인딩)
<!-- 추후 Day 3, Day 4 등을 여기에 목록으로 추가할 수 있습니다 -->

---

## 📌 Day 2: Aggregate 필터링 및 변수 바인딩

Aggregate의 Filter 기능, 변수(Local Variable) 바인딩, 그리고 `Refresh Data` 액션을 활용하여 검색 및 동적 데이터 조회 기능을 구현했습니다.

### 🔗 실습 결과 링크
* **Customer Main Page** 👉 [결과 화면 보기](https://personal-fdeqhkkx.outsystemscloud.com/CustomerMainPage/?_ts=639281000459416485)  
  * 단일 키워드 기반 고객 이름 실시간 필터링 기능 구현
* **Course Main Page** 👉 [결과 화면 보기](https://personal-fdeqhkkx.outsystemscloud.com/Course_Main/?_ts=639280999479179617)  
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

> **Note:** 실습 링크는 개인 학습용 OutSystems Personal Environment 환경에서 구동되며, 서버 상태에 따라 접속이 원활하지 않을 수 있습니다.
