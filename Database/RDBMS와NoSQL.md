# 💾RDBMS와 NoSQL

## 🗂️ RDBMS란?
RDBMS(Relational Database Management System)는 데이터를 **테이블(Table)** 형식으로 저장하고 관리하는 데이터베이스 관리 시스템입니다.  
데이터는 행(Row)과 열(Column)로 구성된 테이블에 저장되며, 데이터 간의 관계를 명확히 정의할 수 있습니다.  
SQL(Structured Query Language)을 사용하여 데이터를 관리합니다.

### 주요 특징
- **고정된 스키마**: 데이터 구조가 사전에 정의되어야 합니다.
- **ACID 트랜잭션 지원**: 데이터 무결성과 일관성을 보장합니다.
- **관계형 데이터 모델**: 데이터 간의 관계를 명확히 정의할 수 있습니다.

---

## 🗂️ NoSQL이란?
NoSQL(Not Only SQL)은 관계형 데이터베이스와는 다른 방식으로 데이터를 저장하고 관리하는 데이터베이스입니다.  
문서(Document), 키-값(Key-Value), 그래프(Graph), 컬럼(Column) 기반 등 다양한 데이터 모델을 지원하며, 유연한 데이터 구조를 제공합니다.

### 주요 특징
- **스키마리스(Schema-less)**: 데이터 구조를 유연하게 설계할 수 있습니다.
- **수평적 확장(Sharding)**: 대량의 데이터를 처리하기 위해 여러 서버에 데이터를 분산 저장할 수 있습니다.
- **다양한 데이터 모델**: JSON, BSON, XML 등 다양한 형식으로 데이터를 저장합니다.

---

## 🆚 RDBMS와 NoSQL 비교

<table>
  <thead>
    <tr>
      <th>특징</th>
      <th>RDBMS</th>
      <th>NoSQL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>데이터 모델</td>
      <td>테이블 기반 (행과 열)</td>
      <td>문서, 키-값, 그래프, 컬럼 기반</td>
    </tr>
    <tr>
      <td>스키마</td>
      <td>고정된 스키마</td>
      <td>스키마리스</td>
    </tr>
    <tr>
      <td>확장성</td>
      <td>수직적 확장(Scale-up)</td>
      <td>수평적 확장(Sharding)</td>
    </tr>
    <tr>
      <td>트랜잭션</td>
      <td>ACID 트랜잭션 완벽 지원</td>
      <td>제한적 지원 (최신 버전에서 개선)</td>
    </tr>
    <tr>
      <td>쿼리 언어</td>
      <td>SQL</td>
      <td>데이터베이스마다 고유한 API 또는 쿼리 언어</td>
    </tr>
    <tr>
      <td>성능</td>
      <td>복잡한 쿼리와 트랜잭션 처리에 최적화</td>
      <td>대량의 읽기/쓰기 작업에 최적화</td>
    </tr>
    <tr>
      <td>사용 사례</td>
      <td>금융, ERP, CRM</td>
      <td>실시간 분석, IoT, 소셜 네트워크</td>
    </tr>
    <tr>
      <td>예시 데이터베이스</td>
      <td>MySQL, PostgreSQL, Oracle</td>
      <td>MongoDB, Cassandra, Redis</td>
    </tr>
  </tbody>
</table>

※ 트랜잭션(Transaction)이란 데이터베이스에서 하나의 논리적 작업 단위를 의미  
<sub> 트랜잭션은 여러 작업이 하나의 묶음으로 처리되며, 모두 성공하거나 모두 실패해야 하는 데이터베이스의 중요한 개념.</sub>

---

## 🔍 RDBMS와 NoSQL의 장단점

### RDBMS의 장단점

<table>
  <thead>
    <tr>
      <th>장점</th>
      <th>단점</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>데이터 무결성과 트랜잭션 보장</td>
      <td>스키마 변경이 어려움</td>
    </tr>
    <tr>
      <td>표준화된 SQL 사용</td>
      <td>수평적 확장에 한계</td>
    </tr>
    <tr>
      <td>복잡한 관계형 데이터 처리에 적합</td>
      <td>대량의 비정형 데이터 처리에 비효율적</td>
    </tr>
  </tbody>
</table>

---

### NoSQL의 장단점

<table>
  <thead>
    <tr>
      <th>장점</th>
      <th>단점</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>유연한 데이터 모델</td>
      <td>복잡한 트랜잭션 처리에 약함</td>
    </tr>
    <tr>
      <td>수평적 확장 가능</td>
      <td>표준화된 쿼리 언어 부족</td>
    </tr>
    <tr>
      <td>대량의 비정형 데이터 처리에 적합</td>
      <td>데이터 무결성 보장이 어려움</td>
    </tr>
  </tbody>
</table>
