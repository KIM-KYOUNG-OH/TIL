# 트랜잭션

- 데이터베이스를 조작하는 하나의 논리적 기능을 구성하는 쿼리의 집합
- MyISAM은 트랜잭션 지원X

# 트랜잭션을 왜 쓸까?

- 데이터 정합성을 보장하기 위함
    - 데이터 정합성은 데이터들이 모순없이 일치하는 상태를 의미
    - 이상현상이 발생하면 정합성을 보장하지 못함
    - ex) ‘술먹고 운전은 했지만 음주운전은 하지 않았다’
- ACID 보장
    - Atomicity
        - 작업중 일부만 동작하는 경우 없이 모든 작업이 전부 반영되거나 아예 반영되지 않음을 보장
    - Consistency
        - 예기치 못한 에러로 인해 작업이 일시 중단되더라도 롤백으로 일관성 유지
    - Isolation
        - 트랜잭션 동작중 다른 트랜잭션의 요청이 오더라도 앞의 트랜잭션을 먼저 처리하고 나중에 처리함
    - Durability
        - 트랜잭션을 완료하면 커밋하여 서버를 재시작하더라도 영구 반영함

# 잠금

- 잠금은 동시성을 제어하기 위한 기능이다

# 인덱스와 잠금

- InnoDB의 잠금은 레코드를 잠그는게 아니라 인덱스를 잠그는 방식으로 처리함
- 인덱스 설계가 안되있다면 where 조건으로 특정 row를 찾을 때 풀 스캔을 해서 성능이 떨어질 수 있다

    ![스크린샷 2022-03-03 오후 10 53 16](https://user-images.githubusercontent.com/66231761/156578218-2621f1e1-1c51-473c-b7b8-a16b07db038e.png)  

---

# MySQL 격리 수준

- 격리 수준이란 여러 트랜잭션이 동시에 처리될 때 특정 트랜잭션이 다른 트랜잭션에서 변경하거나 조회하는 데이터를 볼 수 있게 허용할지 말지를 결정하는 것이다
- 오라클에선 READ COMMITTED 수준이 DEFAULT, MySQL에서는 REPEATABLE READ이 DEFAULT

# READ UNCOMMITTED

![스크린샷 2022-03-03 오후 10 53 54](https://user-images.githubusercontent.com/66231761/156578335-749dddcc-afb9-4674-8457-4ad157169cfd.png)  

- 아직 커밋되지 않았는데 트랜잭션의 변경 내용을 다른 트랜잭션에서 조회 가능한 Dirty Read 발생
- 문제가 많아 트랜잭션 격리 수준으로 인정하지 않음

# READ COMMITTED

![스크린샷 2022-03-03 오후 10 54 12](https://user-images.githubusercontent.com/66231761/156578377-6bf254e3-f1b2-46d3-b5e3-609436cc6169.png)  

- 트랜잭션이 쓰기 작업중이더라도 undo log 영역에 백업된 데이터를 다른 트랜잭션에서 조회 가능한 격리 수준
- 같은 데이터를 여러번 조회할 때마다 결과가 다르게 나오는 NON-REPEATABLE READ 현상이 발생
    - 같은 쿼리를 실행했을 때 항상 같은 결과가 나와야 하는 정합성을 위반
    - 금전적인 정보 처리시 문제를 야기할 수 있다
        
        ![스크린샷 2022-03-03 오후 10 54 33](https://user-images.githubusercontent.com/66231761/156578448-59a66f39-00d0-472d-9910-db20eefbee80.png)  

    
    # REPEATABLE READ
    
    ![스크린샷 2022-03-03 오후 10 54 49](https://user-images.githubusercontent.com/66231761/156578516-05297b56-e958-41a5-843b-482240d74bb3.png)  
    
    - InnoDB 스토리지 엔진에서 기본으로 사용되는 격리 수준
    - 트랜잭션마다 번호를 매김
    - MVCC를 보장하기 위해 자신의 트랜잭션 번호보다 작은 번호에서 변경한 것만 볼 수 있다(커밋되더라도 말이다)
    - 같은 쿼리를 실행했을 때 없던 레코드가 생겼다 없어졌다 하는 PHANTOM READ 현상 발생
        
        ![스크린샷 2022-03-03 오후 10 55 09](https://user-images.githubusercontent.com/66231761/156578580-e79f3038-441f-482b-b956-7276f4a9e600.png)  
    
    # REPEATABLE READ와 READ COMMITTED의 차이
    
    - undo log의 여러 버전 가운데 몇 번째 이전 버전까지 찾아 들어가야 하냐에 있다
    - read committed
        - 트랜잭션이 완료될 때까지 write lock을 유지함
        - 읽기 작업에 read lockd을 걸지만 다른 읽기 작업이 실행되면 read lock이 해제됨
    - repeatable read
        - 트랜잭션이 완료될 때까지 read lock과 write lock을 유지함
        - where 조회시 range lock을 걸지 않음
    - serializable
        - 트랜잭션이 완료될 때까지 read lock과 write lock을 유지함
        - where 조회시 range lock을 검
    
    # SERIALIZABLE
    
    - 가장 엄격한 격리 수준
    - 쓰기 작업중에는 읽기/쓰기 작업이 절대 접근할 수 없다
