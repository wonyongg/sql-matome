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
