## 정의

> 추가적인 쓰기 작업과 작업 공간을 활용하여 데이터베이스 테이블의 **검색 속도를 향상**시키기 위한 자료구조
> 

책의 앞뒤에 있는 색인(index)을 통해 원하는 내용을 쉽고 빠르게 찾을 수 있다.

데이터베이스에서도 데이터와 위치를 포함한 자료구조를 통해 빠르게 조회할 수 있다.

## 장단점

### 장점

- 테이블 조회 속도 향상
    - UPDATE, DELETE도 조회가 필요하기 때문에 성능이 함께 향상된다.
- 전반적인 시스템의 부하를 줄일 수 있다.

### 단점

- DB의 약 10%에 해당하는 저장공간이 필요하다.
- 관리를 위해 추가 작업이 필요하다.
- 잘못 사용할 경우 오히려 성능이 저하된다.

## 관리

인덱스를 **항상 최신 정렬 상태로 유지**해야 빠르게 값을 탐색할 수 있다.

따라서 인덱스가 적용된 컬럼에 INSERT, UPDATE, DELETE가 수행되면 추가 연산이 필요하다.

1. INSERT: 새로운 데이터에 대한 인덱스 추가
2. DELETE: 삭제하는 데이터의 인덱스를 사용하지 않음 처리
3. UPDATE: 기존 인덱스를 사용하지 않음 처리 후, 갱신된 데이터에 대한 인덱스 추가

## 사용 예시

- 규모가 작지 않은 테이블
- INSERT, UPDATE, DELETE가 자주 발생하지 않는 컬럼
    - 인덱스의 크기가 비대해져서 성능이 오히려 저하되는 역효과
- JOIN, WHERE, ORDER BY에 자주 사용되는 컬럼
- 데이터 중복도가 낮은 컬럼

## 자료구조

### B+ Tree

- leaf node(data node)만 데이터와 인덱스를 가지고 있고, internal node는 인덱스만 가진다.
- leaf node들은 LinkedList로 연결되어 있다.
    - 부등호를 이용한 순차 검색 연산에 최적화
- 데이터 노드 크기는 인덱스 노드 크기와 같지 않아도 된다.
- O(logn)의 시간복잡도

![image](https://user-images.githubusercontent.com/55528172/178146259-250f1cfb-231d-4445-b21a-069629825844.png)


## 종류

### 1. Clustered Index

- Primary Index
- 테이블 생성시 pk를 지정하면 자동으로 clustered index가 생성된다.
- 테이블당 1개씩만 존재 가능하다.
- 삽입 순서와 상관없이 index로 지정된 컬럼에 기준으로 물리적으로 데이터를 재배열한다.
- non-clustered index보다 검색 속도가 더 빠르다.
- 입력, 수정, 삭제 속도는 정렬을 수행하기 때문에 더 느리다.

![image](https://user-images.githubusercontent.com/55528172/178146272-5d8b2b75-8624-46dc-a1d9-4e1cd302defe.png)

### 2. Non-clustered Index

- Secondary Index 보조 인덱스
- unique 제약이 있는 테이블 생성시 자동으로 non-clustered index가 생성된다.
- 데이터를 물리적으로 재배열하지 않는다. 대신 별도로 해당 컬럼에 대해 정렬한 인덱스 페이지를 생성한다.
- 테이블당 여러개 존재 가능하다.
- clustered index보다 검색 속도는 느리지만, 입력, 수정, 삭제는 더 빠르다.

![image](https://user-images.githubusercontent.com/55528172/178146274-d9d1d746-c25e-4c37-a244-d538f9dc673c.png)
