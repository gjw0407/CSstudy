# Hash Table

(Key, Value)로 데이터를 저장하는 자료구조

**Array**를 사용하여 데이터를 저장하기 때문에 검색 속도가 빠르다.

## Hash Function

각 key값에 hash function을 적용하여 **고유한 index 생성**

index를 활용하여 bucket(slot, 실제 값이 저장되는 장소)에 value를 저장/검색

![hash_function](https://user-images.githubusercontent.com/55528172/173781643-4a1356d3-8061-46f1-ad79-b9adb8717d38.png)

1. Division Method
    
    주소 = key % 테이블의 크기
    
    테이블의 크기를 소수 & 2의 제곱수와 먼 값을 사용해야 효과가 좋다
    

1. Digit Folding
    
    주소 = key → ASCII 로 변환 후 합한 값
    

1. Multiplication Method
    
    h(k) = (kAmod1) x m
    
    k: 숫자로 된 key
    
    A: 0~1 사이 실수
    
    m: 2의 제곱수
    

1. Universal Hashing
    
    여러 해시함수의 집합 H에서 무작위로 해시함수를 선택한다
    

## Resolve Collision

key1의 해시값과 key2의 해시값이 동일하다면?

### Open Addressing (개방 주소법)

- 해시 테이블의 연속된 공간을 활용한다.
    - 캐시 효율이 높다.
- 데이터의 개수가 충분히 적을 경우

1. Linear Probing
    
    현재 bucket index로부터 고정폭만큼 이동해가며 비어있는 버킷에 저장
    

1. Quadratic Probing
    
    이동폭을 제곱만큼 옮긴다.
    
    기존 index으로부터 1, 2^2, 3^2 만큼 이동해보고 비어있는 버킷에 저장
    

1. Double Hashing Probing
    
    해시된 값을 한 번 더 해싱
    
    해시의 규칙성이 없어진다
    

### Separate Chaining (분리 연결법)

추가 메모리를 활용하여 버킷에 저장된 이전 데이터가 다음 데이터의 주소를 저장, 연결한다.

개방 주소법은 버킷의 밀도가 높아질수록 Worst Case발생 빈도가 높아지는 반면, 분리 연결법은 보조 해시 함수(supplement hash function)를 통해 해시 충돌이 잘 발생하지 않도록 할 수 있다.

1. 연결 리스트
    - 삭제/삽입이 간단하다.
    - 작은 데이터를 저장할 경우 오버헤드가 크다.
    - 한 hash bucket에 할당된 (key, value)의 개수가 적을 경우
        - 6개, 8개를 기준으로 7→8개가 된다면 LinkedList → Tree
    - 트리에 비해 메모리 사용량이 적다.
    
    ![separate_chaining](https://user-images.githubusercontent.com/55528172/173781656-6e866b61-202e-4e19-82af-da9581962db9.png)
    
2. Red-Black Tree
    - Java HashMap은 Self-Balancing Binary Search Tree를 활용해서 Chaining을 구현
    - 한 hash bucket에 할당된 (key, value)의 개수가 많을 경우
        - 6개, 8개를 기준으로 7 → 6개가 된다면 Tree → LinkedList
