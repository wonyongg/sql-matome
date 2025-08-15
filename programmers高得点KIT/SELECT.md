## 조건에 부합하는 중고거래 댓글 조회하기

```sql
SELECT b.TITLE, b.BOARD_ID, r.REPLY_ID, r.WRITER_ID, r.CONTENTS, DATE_FORMAT(r.CREATED_DATE, '%Y-%m-%d') as CREATED_DATE
FROM USED_GOODS_REPLY r 
LEFT JOIN USED_GOODS_BOARD b ON r.BOARD_ID = b.BOARD_ID 
WHERE b.CREATED_DATE >='2022-10-01' AND b.CREATED_DATE < '2022-11-01'
ORDER BY r.CREATED_DATE ASC, b.TITLE ASC;
```

-  날짜 포맷 함수는 `DATE_FORMAT(컬럼명, '포맷 양식')`을 사용한다.



## 서울에 위치한 식당 목록 출력하기

```sql
SELECT ri.REST_ID, ri.REST_NAME, ri.FOOD_TYPE, ri.FAVORITES, ri.ADDRESS, ROUND(AVG(rr.REVIEW_SCORE), 2) as SCORE 
FROM REST_INFO ri JOIN REST_REVIEW rr ON ri.REST_ID = rr.REST_ID
WHERE ri.ADDRESS LIKE '서울%'
GROUP BY ri.REST_ID, ri.REST_NAME, ri.FOOD_TYPE, ri.FAVORITES, ri.ADDRESS
ORDER BY SCORE DESC, ri.FAVORITES DESC
```

- `ROUND(숫자, 소수점 자리수)` 함수는 반올림 함수이며, AVG() 함수는 평균을 구하는 함수이다.
- SQL에서 `GROUP BY`를 사용할 때는:
  - **집계 함수(`AVG`, `SUM`, `COUNT` 등)** 를 제외하고,
  - `SELECT`에 명시된 모든 컬럼은 `GROUP BY`에 포함되어야 한다.
  - `GROUP BY`는 **그룹별로 한 줄씩 결과를 보여주기 위한 것**인데, 만약 집계 함수 외의 컬럼을 그냥 SELECT에 쓰면 **"어떤 값을 대표로 보여줄지 애매해지기 때문"**이다.



## 과일로 만든 아이스크림 고르기

```sql
SELECT fh.FLAVOR 
FROM FIRST_HALF fh JOIN ICECREAM_INFO ii ON fh.FLAVOR = ii.FLAVOR
WHERE fh.TOTAL_ORDER > 3000 AND ii.INGREDIENT_TYPE = 'fruit_based'
ORDER BY fh.TOTAL_ORDER DESC
```



## 흉부외과 또는 일반외과 의사 목록 출력하기

```sql
SELECT d.DR_NAME, d.DR_ID, d.MCDP_CD, DATE_FORMAT(d.HIRE_YMD, '%Y-%m-%d') as HIRE_YMD
FROM DOCTOR d
WHERE d.MCDP_CD = 'CS' OR d.MCDP_CD = 'GS'
ORDER BY HIRE_YMD DESC, DR_NAME ASC
```

- `Y`는 2000 네 자리, `y`는 20 두 자리



## 12세 이하인 여자 환자 목록 출력하기

```sql
SELECT p.PT_NAME, p.PT_NO, p.GEND_CD, p.AGE, COALESCE(p.TLNO, 'NONE') as TLNO
FROM PATIENT p
WHERE p.AGE <= 12 AND p.GEND_CD = 'W'
ORDER BY p.AGE DESC, p.PT_NAME ASC
```

- 컬럼의 기본값을 지정하려면 `COALESCE` 함수를 사용하면 된다.



## 인기있는 아이스크림

```sql
SELECT fh.FLAVOR
FROM FIRST_HALF fh
GROUP BY fh.FLAVOR
ORDER BY fh.TOTAL_ORDER DESC, fh.SHIPMENT_ID ASC 
```



## 조건에 맞는 도서 리스트 출력하기

```sql
SELECT b.BOOK_ID, DATE_FORMAT(b.PUBLISHED_DATE, '%Y-%m-%d') as PUBLISHED_DATE
FROM BOOK b
WHERE b.PUBLISHED_DATE >= '2021-01-01' AND b.PUBLISHED_DATE <= '2021-12-31' AND b.CATEGORY = '인문'
```



## 평균 일일 대여 요금 구하기

```sql
SELECT ROUND(AVG(c.DAILY_FEE)) as AVERAGE_FEE
FROM CAR_RENTAL_COMPANY_CAR c
WHERE c.CAR_TYPE = 'SUV';
```

- `Round` 함수 사용 시, 소수 첫번째 자리에서 반올림하는 경우 두번째 파라미터 값을 비워두면 된다.



## 3월에 태어난 여성 회원 목록 출력하기

```sql
SELECT m.MEMBER_ID, m.MEMBER_NAME, m.GENDER, DATE_FORMAT(m.DATE_OF_BIRTH, '%Y-%m-%d') as DATE_OF_BIRTH
FROM MEMBER_PROFILE m
WHERE m.GENDER = 'W' AND m.TLNO IS NOT NULL AND MONTH(m.DATE_OF_BIRTH) = 3
```

- 특정 컬럼의 값이 `NULL`인 경우 이를 출력 결과에서 제외하고 싶으면 `IS NOT NULL`을 사용하면 된다.
- 특정 월만 검색하고 싶은 경우 `MONTH()` 함수를 사용하면 된다. `YEAR`도 있다.



## 강원도에 위치한 생산공장 목록 출력하기

```sql
SELECT f.FACTORY_ID, f.FACTORY_NAME, f.ADDRESS
FROM FOOD_FACTORY f
WHERE f.ADDRESS LIKE '강원도%'
ORDER BY f.FACTORY_ID ASC
```



## 재구매가 일어난 상품과 회원 리스트 구하기

```sql
SELECT os.USER_ID, os.PRODUCT_ID
FROM ONLINE_SALE os
GROUP BY os.USER_ID, os.PRODUCT_ID
HAVING count(*) >= 2
ORDER BY os.USER_ID ASC, os.PRODUCT_ID DESC
```

- `GROUP BY`에 적힌 컬럼은 해당 컬럼을 그룹으로 묶는다는 뜻이다.
  - `USER_ID`, `PRODUCT_ID`를 묶어 하나로 보는 것.
- `HAVING`에서 조건을 적을 수 있는데 `count(*)`에서 `*`는 테이블의 전체 행을 의미한다.
  - 즉, 그룹 안의 행의 수를 뜻한다.
  - 여기서는 `USER_ID`, `PRODUCT_ID`를 묶은 것을 기준으로 하는 행의 개수이므로 같은 값을 가진 행의 개수가 2 개 이상인 경우를 찾는 것이다.



## 모든 레코드 조회하기

```sql
SELECT *
FROM ANIMAL_INS ai
ORDER BY ai.ANIMAL_ID ASC
```



## 역순 정렬하기

```sql
SELECT ai.NAME, ai.DATETIME
FROM ANIMAL_INS ai
ORDER BY ai.ANIMAL_ID DESC
```



## 오프라인/온라인 판매 데이터 통합하기

```sql
SELECT DATE_FORMAT(SALES_DATE, '%Y-%m-%d') as SALES_DATE, PRODUCT_ID, USER_ID, SALES_AMOUNT
FROM ONLINE_SALE
WHERE '2022-03-01' <= SALES_DATE AND '2022-03-31' >= SALES_DATE

UNION ALL

SELECT DATE_FORMAT(SALES_DATE, '%Y-%m-%d') as SALES_DATE, PRODUCT_ID, NULL as USER_ID, SALES_AMOUNT
FROM OFFLINE_SALE
WHERE '2022-03-01' <= SALES_DATE AND '2022-03-31' >= SALES_DATE

ORDER BY SALES_DATE ASC, PRODUCT_ID ASC, USER_ID ASC
```

- `UNION ALL`로 판매 데이터를 합치고, 없는 컬럼은 `NULL`을 넣어 표시한다.
- `ORDER BY`의 경우 결과값에 대한 전체 SQL문에 적용된다.



## 아픈 동물 찾기

```sql
SELECT ai.ANIMAL_ID, ai.NAME
FROM ANIMAL_INS ai
WHERE INTAKE_CONDITION = 'Sick'
ORDER BY ai.ANIMAL_ID
```



## 어린 동물 찾기

```sql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS 
WHERE INTAKE_CONDITION != 'Aged'
ORDER BY ANIMAL_ID ASC
```



## 동물의 아이디와 이름

```sql
SELECT ANIMAL_ID, NAME
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```



## 여러 기준으로 정렬하기

```sql
SELECT ANIMAL_ID, NAME, DATETIME
FROM ANIMAL_INS
ORDER BY NAME ASC, DATETIME DESC
```



## 상위 n개 레코드

```sql
SELECT NAME
FROM ANIMAL_INS
ORDER BY DATETIME ASC
LIMIT 1
```

- 상위 n 개의 레코드만 출력하고 싶을 때는 `LIMIT n`을 사용한다.
- 날짜의 경우 과거 우선이면 `ASC`, 미래가 우선이면 `DESC`이다.



## 조건에 맞는 회원수 구하기

```sql
SELECT COUNT(*) AS USERS
FROM USER_INFO
WHERE JOINED BETWEEN '2021-01-01' AND '2021-12-31'
AND AGE BETWEEN 20 AND 29
```

- 총 개수 구하기 `COUNT(*)`
- `BETWEEN`은 `AND`와 함께 사용
- 만약, 시간까지 포함이면 별도의 `>=`를 사용하는 것이 마지막 날 `23:59:59` 를 포함하므로 안전



## 업그레이드 된 아이템 구하기

```sql
SELECT ii.ITEM_ID, ii.ITEM_NAME, ii.RARITY
FROM ITEM_TREE it JOIN ITEM_INFO ii ON it.ITEM_ID = ii.ITEM_ID
WHERE it.PARENT_ITEM_ID IN (
    SELECT iis.ITEM_ID
    FROM ITEM_INFO iis
    WHERE iis.RARITY = 'RARE' 
    )
ORDER BY ii.ITEM_ID DESC
```

- 서브 쿼리를 통해 `ITEM`의 `RARITY`가 `RARE`인 경우를 찾는다. 
- 서브 쿼리의 `ITEM_ID` 값이 메인 쿼리의 `PARENT_ITEM_ID` 값과 같은 경우를 조건으로 건다.
- 이렇게 되면 해당 `PARENT_ITEM_ID`의 같은 행 `ITEM_ID` 값이, 서브 쿼리의 업글레이드 시 `ITEM_ID`가 된다.



## Python 개발자 찾기

```sql
SELECT ID, EMAIL, FIRST_NAME, LAST_NAME
FROM DEVELOPER_INFOS
WHERE SKILL_1 = 'Python' OR SKILL_2 = 'Python' OR SKILL_3 = 'Python'
ORDER BY ID ASC
```



## 조건에 맞는 개발자 찾기

```sql
SELECT DISTINCT d.ID, d.EMAIL, d.FIRST_NAME, d.LAST_NAME
FROM DEVELOPERS d
JOIN SKILLCODES s
  ON (d.SKILL_CODE & s.CODE) > 0
WHERE s.NAME IN ('Python', 'C#')
ORDER BY d.ID ASC;
```

- **`JOIN SKILLCODES s ON (d.SKILL_CODE & s.CODE) > 0`**
   → 개발자의 스킬 코드에 해당 스킬이 켜져 있는지 비트 연산으로 확인
- **`WHERE s.NAME IN ('Python', 'C#')`**
   → Python이나 C# 중 하나라도 해당되면 포함
- **`DISTINCT`**
   → 한 개발자가 Python과 C# 둘 다 있으면 중복 제거
- **`ORDER BY d.ID ASC`**
   → ID 오름차순 정렬



## 잔챙이 잡은 수 구하기

```sql
SELECT COUNT(*) AS FISH_COUNT
FROM FISH_INFO
WHERE LENGTH IS NULL
```

