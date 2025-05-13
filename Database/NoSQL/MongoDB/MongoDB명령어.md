# 🗄️ MongoDB 명령어 정리

## 📂 기본 개념

- **데이터베이스(Database)**: MongoDB에서 데이터가 저장되는 가장 큰 단위입니다.
- **컬렉션(Collection)**: 여러 문서(Document)를 그룹화한 것으로, 관계형 데이터베이스의 테이블과 유사합니다.
- **문서(Document)**: MongoDB에서 데이터를 저장하는 기본 단위로, JSON 또는 BSON 형식으로 표현됩니다.
- **필드(Field)**: 문서 내의 키(Key)입니다.
- **값(Value)**: 필드에 저장된 데이터입니다.

---

## 🛠️ 데이터베이스 명령어

- 🆕 **데이터베이스 생성 및 삭제**
```
show dbs  <!-- 데이터베이스 리스트 출력 -->

use <database_name> <!-- 데이터베이스 생성 또는 선택 -->

db <!-- 현재 사용중인 데이터베이스 확인 -->

db.stats() <!-- 현재 사용중인 데이터베이스 정보 출력 -->

db.dropDatabase() <!-- 데이터베이스 삭제 -->
```

## 📂 컬렉션 명령어

- **컬렉션 생성 및 삭제**

```
show collections <!-- 컬렉션 목록 확인 -->

db.createCollection("collection_name") <!-- 컬렉션 생성 -->

db.collection_name.drop() <!-- 컬렉션 삭제 -->

```

## 📄 문서(Document) 명령어 (CRUD)

<sub>※ filter는 MongoDB의 쿼리 문법을 사용하여 특정 문서를 선택하는 조건입니다.<br>
filter는 JSON 형식으로 작성되며, 조건에 따라 문서를 선택할 수 있습니다.</sub>

- **문서 삽입**

```
db.collection_name.insertOne({field:value})
db.collection_name.insertMany([{field:value},{field:value}]) <!-- 여러개를 넣을때는 배열로 묶는다 -->
```

- **문서 조회**
```
db.collection_name.find()
db.collection_name.find({<filter>})
```

- **문서 수정**

<sub>기본적으로 하나의 쿼리만 수정 <br></sub>
<sub>$set을 해야 해당 필드만 바뀐다.</sub>

```
db.collection_name.updateOne({<filter>} ,{$set: {field:value}}) 
<!-- updateOne은 매칭되는 다큐먼트 중 첫 번째만 수정 -->


db.collection_name.updateMany({<filter>} ,{$set: {field:value}})
<!-- updateMany는 매칭되는 모든 다큐먼트를 수정. -->


db.collection_name.replaceOne({<filter>}, {new_field:new_value})
<!-- $set을 안 썼을 때 상황과 유사. -->
```
- <sub>`replaceOne` 메소드는 문서를 통째로 대체합니다. 기존 문서의 모든 필드가 대체되므로, 주의가 필요합니다.</sub>

- **문서 삭제**

```
db.collection_name.remove()
<!-- 전체 삭제 (MongoDB 4.2부터는 권장되지 않음. deleteMany를 사용하는 것이 좋음.) -->

db.collection_name.remove({<filter>})
<!-- 조건에 맞는 문서를 삭제. 조건에 맞는 여러 문서를 삭제할 수 있음 (MongoDB 4.2부터는 deleteMany를 사용하는 것이 좋음.) -->

db.<collection_name>.deleteOne({ <filter> })
<!-- 조건에 맞는 첫 번째 문서를 삭제 -->
db.<collection_name>.deleteMany({ <filter> })
<!-- 조건에 맞는 모든 문서를 삭제 -->
```

## ⚙️ 연산자 (Operators)

MongoDB에서 `filter`를 작성할 때 사용할 수 있는 주요 연산자는 다음과 같습니다:

### 1. 비교 연산자 (Comparison Operators)
- `$eq` (equal): 값이 같은 문서를 찾습니다.  
```
{ field: { $eq: value } }
```

- `$ne` (not equal): 값이 같지 않은 문서를 찾습니다.
```
{ field: { $ne: value } }
```

- `$gt` (greater than): 값이 큰 문서를 찾습니다.
```
{ field: { $gt: value } }
```

- `$gte` (greater than or equal): 값이 크거나 같은 문서를 찾습니다.
```
{ field: { $gte: value } }
```

- `$lt` (less than): 값이 작은 문서를 찾습니다.
```
{ field: { $lt: value } }
```

- `$lte` (less than or equal): 값이 작거나 같은 문서를 찾습니다.
```
{ field: { $lte: value } }
```

### 2. 논리 연산자 (Logical Operators)
- `$and` : 모든 조건을 만족하는 문서를 찾습니다.  
```
{ $and: [ { age: { $gt: 25 } }, { status: "active" } ] }
```
  <sub>예시: 나이가 25보다 크고 상태가 "active"인 문서를 찾습니다.</sub>
- `$or` : 하나 이상의 조건을 만족하는 문서를 찾습니다.
```
{ $or: [ { age: { $lt: 18 } }, { status: "inactive" } ] }
```
<sub>예시: 나이가 18보다 작거나 상태가 "inactive"인 문서를 찾습니다.</sub>

- `$not`: 조건을 만족하지 않는 문서를 찾습니다.
```
{ age: { $not: { $gte: 30 } } }
```
<sub>예시: 나이가 30 이상이 아닌 문서를 찾습니다.</sub>

- `$nor` : 모든 조건을 만족하지 않는 문서를 찾습니다.
```
{ $nor: [ { age: { $gt: 40 } }, { status: "active" } ] }
```
<sub>예시: 나이가 40보다 크지 않고 상태가 "active"가 아닌 문서를 찾습니다.</sub>

### 3. 배열 연산자 (Array Operators)
- `$in` : 배열 내의 값 중 하나와 일치하는 문서를 찾습니다.
```
{ status: { $in: [ "active", "pending" ] } }
```
<sub>예시: 상태가 "active" 또는 "pending"인 문서를 찾습니다.</sub>

- `$nin` (not in array): 배열 내의 값과 일치하지 않는 문서를 찾습니다.
```
{ status: { $nin: [ "inactive", "banned" ] } }
```

- `$all` (all elements match): 배열의 모든 요소가 조건과 일치하는 문서를 찾습니다.
```
{ tags: { $all: [ "mongodb", "database" ] } }
```
<sub>예시: `tags` 배열에 "mongodb"와 "database"가 모두 포함된 문서를 찾습니다.</sub>

- `$size` (array size): 배열의 크기가 특정 값과 같은 문서를 찾습니다.
```
{ tags: { $size: 3 } }
```

- `$elemMatch` (element match): 배열의 요소 중 하나가 여러 조건을 동시에 만족하는 문서를 찾습니다.

```
{ scores: { $elemMatch: { $gt: 80, $lt: 90 } } }
```
<sub>예시: `scores` 배열에 80보다 크고 90보다 작은 값이 포함된 문서를 찾습니다.</sub>

### 4. 정규식 연산자 (Regex Operators)

- `/pattern/` (regex pattern): 특정 패턴과 일치하는 값을 찾습니다.
```
{ name: /john/i }
```
<sub>예시: 이름에 "john"이 포함된 문서를 찾습니다 (대소문자 구분 없음).</sub>
- `$regex` (regex with options): 정규식을 사용하여 값을 찾습니다. $options를 통해 대소문자 구분(i), 멀티라인(m) 등의 옵션을 추가할 수 있습니다.

```
{ name: { $regex: "^A", $options: "i" } }
```
<sub>예시: 이름이 "A"로 시작하는 문서를 찾습니다.</sub>

### 5. 존재 연산자 (Element Operators)
- `$exists` (field existence): 필드가 존재하거나 존재하지 않는 문서를 찾습니다.
```
{ email: { $exists: true } }
```
<sub>예시: email 필드가 존재하는 문서를 찾습니다.</sub>
- `$type` (field type): 필드의 데이터 타입이 특정 타입인 문서를 찾습니다.
```
{ age: { $type: "number" } }
```