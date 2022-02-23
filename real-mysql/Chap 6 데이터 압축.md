# 왜 데이터 압축을 사용할까?

- 데이터 파일이 클수록 쿼리 처리 시간과 백업, 복구 시간이 길어진다
- 데이터 압축은 이런 문제를 해결하기 위한 방법중 하나이다

# 데이터 압축 방식

- 테이블 압축
- 페이지 압축

# 페이지 압축(Transparent Page Compression)

- MySQL 서버가 데이터를 디스크에 저장하는 시점에 데이터를 압축해서 저장
- MySQL 서버가 디스크에서 페이지를 읽어올 때 압축이 해제

# 페이지 압축후 디스크에 저장하는 과정

- 데이터를 압축 데이터와 빈 공간으로 나누고 실제 디스크에선 압축 데이터 공간 만큼만 디스크에 기록
- 빈 공간에 대한 펀치 홀을 생성
- 파일 시스템은 펀치 홀 만큼의 공간을 OS로 반납

# 페이지 압축을 사용하지 않는 이유

- 하드웨어에서 펀치 홀 기능을 지원해야 함
- 아직 파일 시스템 관련 명령어가 펀치 홀을 지원하지 않음

# 테이블 압축

- OS나 하드웨어의 제약 없이 사용 가능

# 테이블 압축의 단점

- 버퍼 풀 공간 활용률이 낮음
- 쿼리 처리 성능이 낮음(??)
- 데이터를 읽거나 쓸때 압축을 해제하고 다시 압축하는 오버헤드가 발생하므로 빈번한 데이터 변경시 압축을 고려하지 않는 것이 좋다

# 압축 테이블 생성

- innodb_file_per_table=ON 으로 설정
- 테이블 압축을 사용할 테이블을 생성할 때 ROW_FORMAT=COMPRESSED와 KEY_BLOCK_SIZE 옵션을 명시

```bash
mysql> SET GLOBAL innodb_file_per_table=ON;

mysql> CREATE TABLE compressed_table (
    c1 INT PRIMARY KEY
  )
  ROW_FORMAT=COMPRESSED
  KEY_BLOCK_SIZE=8;
```

# KEY_BLOCK_SIZE

<img width="650" alt="스크린샷 2022-02-23 오후 8 56 41" src="https://user-images.githubusercontent.com/66231761/155314887-c0d66d0b-e4be-4fb3-9696-eba36f46ffff.png">  

- 압축될 페이지 크기를 의미(KB 단위)
- 핵심은 원본 데이터 압축 결과가 KEY_BLOCK_SIZE보다 작을 때까지 반복해서 페이지를 쪼개는 것이다
- KEY_BLOCK_SIZE를 잘못 설정하면 MySQL 처리 성능이 떨어질 수 있다
