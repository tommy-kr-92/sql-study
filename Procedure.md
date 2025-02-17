# 프로시저(Procedure)란?
데이터 베이스에 대한 일련의 작업을 정리한 절차를 관계형 데이터베이스 관리 시스템에 저장한 것으로 영구저장모듈(Persistent Storage Module)이라고도 불림.


보통 저장 프로시저를 프로시저라고 부르며 일련의 쿼리를 마치 하나의 함수처럼 실행하기 위한 쿼리의 집합

즉 특정 작업을 위한 쿼리들의 블록

## 장점

1. 하나의 요청으로 여러 SQL문을 실행.(네트워크 부하를 줄일 수 있음)
2. 네트워크 소요 시간을 줄여 성능을 개선
3. 여러 어플리케이션과 공유가 가능(API처럼 제공 가능)
4. 기능 변경이 편함(특정 기능을 변경할 때 프로시저만 변경하면 됨)

## 단점

1. 문자나 숫자열 연산에 사용하면 오히려 C, Java보다 느린 성능을 보일 수 있음
2. 유지보수가 어려움(프로시져가 앱의 어디에 사용되는지 확인이 어려움)

## 생성
```sql
CREATE OR REPLACE PROCEDURE 프로시져이름 (파라미터1, 파라미터2, ...);

IS 
변수

BEGIN
쿼리문

END 프로시져 이름;
```

### 소환사의 티어를 알아내는 프로시저
``` sql 
CREATE OR REPLACE PROCEDURE GET_TIER(in_name IN VARCHA2, out_tier OUT VARCHAR2)

IS 

BEGIN
SELECT TIER INTO out_tier FROM SUMMONER_TB WHERE NAME = in_name;

EXCEPTION
    -- 소환사를 찾을 수 없을 때
    WHEN NO_DATA_FOUND THEN
        out_tier := 'NO_SUMMONER_FOUND';
END GET_TIER;
```

- 파라미터 갑은 IN, OUT, INOUT으로 총 세가지 종류로 작성 가능
- IN: 전달될 데이터
- OUT: 결과로 나갈 데이터
- IN OUT: IN과 OUT 모두 가능한 데이터

## 조회
```sql
DECLARE
출력될 변수 선언
실행할 프로시저
출력문(Optional)
END
```

### faker의 티어를 출력하는 프로시저
```sql
DECLARE
out_tier VARCHAR2(10);
BEGIN get_tier('faker', out_tier);
DBMS_OUTPUT.PUT_LINE(out_tier);
END;

-- C1
```

## 수정
수정은 create or replace 구문을 사용하면 해당 프로시저명이 있다면 수정, 없다면 생성

```sql
CREATE OR REPLACE PROCEDURE GET_TIER(in_name IN VARCHAR2, out_tier OUT VARCHAR2)

IS
BEGIN
    SELECT NAME INTO out_tier FROM SUMMONER_TB WHERE NAME = in_name;

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        out_tier:='NO_DATA_FOUND';
END get_tier;
```

## 삭제
```sql
DROP PROCEDURE 프로시저명;
```
### GET_TIER 프로시저 삭제
```sql
DROP PROCEDURE GET_TIER;
```

## 실행
> ### EXEC 혹은 EXCUTE 프로시저명(매개변수1 값, 매개변수2 값, ...);