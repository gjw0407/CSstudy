## 정의

> 테이블간의 관계성에 따라 여러 개의 테이블을 결합하고, 하나의 테이블인 것처럼 결과를 출력한다
> 

## 종류

![image](https://user-images.githubusercontent.com/55528172/178146338-6b0d8f48-5b65-4293-b00b-78f822ba375f.png)


### 1. INNER JOIN

![image](https://user-images.githubusercontent.com/55528172/178146358-9a9be238-7fe7-44df-a9f9-f11d7c469c6e.png)

- 조인하는 두 개 테이블 모두에 데이터가 공통적으로 존재하는 행에 대해서만 결과를 가져온다
- 교집합 연산
- SQL
    ```sql
    -- ANSI-SQL
    SELECT 컬럼명, ...
    FROM TABLE_A
    [INNER] JOIN TABLE_B
    ON TABLE_A.조인키컬럼 = TABLE_B.조인키컬럼
    ;
    ```
    ```sql
    -- ORACLE SQL
    SELECT 컬럼명, ...
    FROM TABLE_A, TABLE_B
    WHERE TABLE_A.조인키컬럼 = TABLE_B.조인키컬럼
    ;
    ```
- 예시
    - inner join 전
    ![image](https://user-images.githubusercontent.com/55528172/178146364-b50e830a-2bfb-4531-a950-efb3e871ce62.png)
    - inner join 후
    ![image](https://user-images.githubusercontent.com/55528172/178146452-828069f7-4a54-4562-88ba-0d856113df0c.png)


### 2. OUTER JOIN

- 매칭되는 행이 없어도 결과를 가져오고 매칭되는 행이 없으면 NULL로 표시
- LEFT JOIN, RIGHT JOIN

### 2-1.  LEFT (OUTER) JOIN

![image](https://user-images.githubusercontent.com/55528172/178146575-ac9227a5-48bd-44b4-bdc8-80188c56c3ac.png)

- 가장 많이 사용되는 조인 형태
- 교집합 연산 + 차집합 연산 ( (A ∩ B) ∪ (A - B) )
- SQL
    ```sql
    -- ANSI-SQL
    SELECT 컬럼명, ...
    FROM TABLE_A
    LEFT OUTER JOIN TABLE_B
    ON TABLE_A.조인키컬럼 = TABLE_B.조인키컬럼
    ;
    ```
    ```sql
    -- ORACLE SQL
    SELECT 컬럼명, ...
    FROM TABLE_A, TABLE_B
    WHERE TABLE_A.조인키컬럼 = TABLE_B.조인키컬럼(+)
    ;
    ```
    - 오른쪽 테이블(TABLE_B)의 조인 키 컬럼명 뒤에 (+) 입력
- 예시
    - left outer join 전
    ![image](https://user-images.githubusercontent.com/55528172/178146364-b50e830a-2bfb-4531-a950-efb3e871ce62.png)
    - left outer join 후
    ![image](https://user-images.githubusercontent.com/55528172/178146577-24a2f6a3-5c64-49ee-9902-65059a85db92.png)

    

### 2-2. RIGHT (OUTER) JOIN

![image](https://user-images.githubusercontent.com/55528172/178146632-5e9c561a-317b-4e33-8f29-05e1b7003404.png)

- 교집합 연산 + 차집합 연산 ( (A ∩ B) ∪ (A - B) )
- LEFT OUTER JOIN의 반대
- SQL
    ```sql
    -- ANSI-SQL
    SELECT 컬럼명, ...
    FROM TABLE_A
    RIGHT OUTER JOIN TABLE_B
    ON TABLE_A.조인키컬럼 = TABLE_B.조인키컬럼
    ;
    ```
    ```sql
    -- ORACLE SQL
    SELECT 컬럼명, ...
    FROM TABLE_A, TABLE_B
    WHERE TABLE_A.조인키컬럼(+) = TABLE_B.조인키컬럼
    ;
    ```
    - 왼쪽 테이블(TABLE_A)의 조인 키 컬럼명 뒤에 (+) 입력
- 예시
    - right outer join 전
    ![image](https://user-images.githubusercontent.com/55528172/178146364-b50e830a-2bfb-4531-a950-efb3e871ce62.png)
    - right outer join 후
    ![image](https://user-images.githubusercontent.com/55528172/178146636-bfb23632-8992-471f-a4ae-1aa3db702ae1.png)

    

### 2-3. FULL OUTER JOIN

![image](https://user-images.githubusercontent.com/55528172/178146664-05cd1035-4dad-43a9-a4be-0f956d2a9d14.png)

- 합집합 연산 (A ∩ B)
- SQL
    ```sql
    -- ANSI-SQL
    SELECT 컬럼명, ...
    FROM TABLE_A
    FULL OUTER JOIN TABLE_B
    ON TABLE_A.조인키컬럼 = TABLE_B.조인키컬럼
    ;
    ```
    ```sql
    -- ORACLE SQL
    SELECT 컬럼명, ...
    FROM TABLE_A
    FULL OUTER JOIN TABLE_B
    WHERE TABLE_A.조인키컬럼 = TABLE_B.조인키컬럼
    ;
    ```
- 예시
    - full outer join 전
    ![image](https://user-images.githubusercontent.com/55528172/178146364-b50e830a-2bfb-4531-a950-efb3e871ce62.png)
    - full outer join 후
    ![image](https://user-images.githubusercontent.com/55528172/178146669-2f1c10e7-2240-4cfe-b2df-0b57885472d0.png)

