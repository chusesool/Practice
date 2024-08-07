USE PRACTICE;

/****************************************************************************/
/*****************************회원 분석용 데이터 마트******************************/
/****************************************************************************/

/***************회원 구매정보***************/

/* 회원 구매정보 */
SELECT  A.MEM_NO, A.GENDER, A.BIRTHDAY, A.ADDR, A.JOIN_DATE
		,SUM(B.SALES_QTY * C.PRICE) AS 구매금액
        ,COUNT(B.ORDER_NO) 			AS 구매횟수
        ,SUM(B.SALES_QTY)			AS 구매수량
  FROM  CUSTOMER AS A
  LEFT
  JOIN  SALES AS B
    ON  A.MEM_NO = B.MEM_NO
  LEFT
  JOIN  PRODUCT AS C
    ON  B.PRODUCT_CODE = C.PRODUCT_CODE
 GROUP
    BY  A.MEM_NO, A.GENDER, A.BIRTHDAY, A.ADDR, A.JOIN_DATE;

/* 회원 구매정보 임시테이블 */
CREATE TEMPORARY TABLE CUSTOMER_PUR_INFO AS
SELECT  A.MEM_NO, A.GENDER, A.BIRTHDAY, A.ADDR, A.JOIN_DATE
		,SUM(B.SALES_QTY * C.PRICE) AS 구매금액
        ,COUNT(B.ORDER_NO) 			AS 구매횟수
        ,SUM(B.SALES_QTY)			AS 구매수량
  FROM  CUSTOMER AS A
  LEFT
  JOIN  SALES AS B
    ON  A.MEM_NO = B.MEM_NO
  LEFT
  JOIN  PRODUCT AS C
    ON  B.PRODUCT_CODE = C.PRODUCT_CODE
 GROUP
    BY  A.MEM_NO, A.GENDER, A.BIRTHDAY, A.ADDR, A.JOIN_DATE;
 
/* 확인 */ 
SELECT  * FROM CUSTOMER_PUR_INFO;
 

/***************회원 연령대***************/
 
/* 생년월일 -> 나이 */
SELECT  *
		,2021-YEAR(BIRTHDAY) + 1 AS 나이
  FROM  CUSTOMER;

/* 생년월일 -> 나이 -> 연령대 */
SELECT  *
		,CASE WHEN 나이 < 10 THEN '10대 미만'
			  WHEN 나이 < 20 THEN '10대'
              WHEN 나이 < 30 THEN '20대'
              WHEN 나이 < 40 THEN '30대'
              WHEN 나이 < 50 THEN '40대'
              ELSE '50대 이상' END AS 연령대         
  FROM  (
		SELECT  *
				,2021-YEAR(BIRTHDAY) +1 AS 나이
		  FROM  CUSTOMER
		)AS A;

/* CASE WHEN 함수 사용시 주의점(순차적) */
SELECT  *
		,CASE WHEN 나이 < 50 THEN '40대'
			  WHEN 나이 < 10 THEN '10대 미만'
              WHEN 나이 < 20 THEN '10대'
              WHEN 나이 < 30 THEN '20대'
              WHEN 나이 < 40 THEN '30대'
              ELSE '50대 이상' END AS 연령대         
  FROM  (
		SELECT  *
				,2021-YEAR(BIRTHDAY) +1 AS 나이
		  FROM  CUSTOMER
		)AS A; 
 
/* 회원 연령대 임시테이블 */
CREATE TEMPORARY TABLE CUSTOMER_AGEBAND AS
SELECT  A.*
		,CASE WHEN 나이 < 10 THEN '10대 미만'
			  WHEN 나이 < 20 THEN '10대'
              WHEN 나이 < 30 THEN '20대'
              WHEN 나이 < 40 THEN '30대'
              WHEN 나이 < 50 THEN '40대'
              ELSE '50대 이상' END AS 연령대         
  FROM  (
		SELECT  *
				,2021-YEAR(BIRTHDAY) + 1 AS 나이
		  FROM  CUSTOMER
		)AS A;

/* 확인 */ 
SELECT  * FROM CUSTOMER_AGEBAND;


/***************회원 구매정보 + 연령대 임시테이블***************/
CREATE TEMPORARY TABLE CUSTOMER_PUR_INFO_AGEBAND AS
SELECT  A.*
		,B.연령대
  FROM  CUSTOMER_PUR_INFO AS A
  LEFT
  JOIN  CUSTOMER_AGEBAND AS B
    ON  A.MEM_NO = B.MEM_NO;

/* 확인 */ 
SELECT  * FROM CUSTOMER_PUR_INFO_AGEBAND;


/***************회원 선호 카테고리***************/

/* 회원 및 카테고리별 구매횟수 순위 */
SELECT  A.MEM_NO
		,B.CATEGORY
		,COUNT(A.ORDER_NO) AS 구매횟수
		,ROW_NUMBER() OVER(PARTITION BY A.MEM_NO ORDER BY COUNT(A.ORDER_NO) DESC) AS 구매횟수_순위
  FROM  SALES AS A
  LEFT
  JOIN  PRODUCT AS B
	ON  A.PRODUCT_CODE = B.PRODUCT_CODE   
 GROUP
	BY  A.MEM_NO
		,B.CATEGORY;

/* 회원 및 카테고리별 구매횟수 순위 + 구매횟수 순위 1위만 필터링 */
SELECT  *
  FROM  (
		SELECT  A.MEM_NO
				,B.CATEGORY
				,COUNT(A.ORDER_NO) AS 구매횟수
				,ROW_NUMBER() OVER(PARTITION BY A.MEM_NO ORDER BY COUNT(A.ORDER_NO) DESC) AS 구매횟수_순위
		  FROM  SALES AS A
		  LEFT
		  JOIN  PRODUCT AS B
			ON  A.PRODUCT_CODE = B.PRODUCT_CODE
		 GROUP
			BY  A.MEM_NO
				,B.CATEGORY
		)AS A
 WHERE  구매횟수_순위 = 1;

/* 회원 선호 카테고리 임시테이블 */
CREATE TEMPORARY TABLE CUSTOMER_PRE_CATEGORY AS
SELECT  *
  FROM  (
		SELECT  A.MEM_NO
				,B.CATEGORY
				,COUNT(A.ORDER_NO) AS 구매횟수
				,ROW_NUMBER() OVER(PARTITION BY A.MEM_NO ORDER BY COUNT(A.ORDER_NO) DESC) AS 구매횟수_순위
		  FROM  SALES AS A
		  LEFT
		  JOIN  PRODUCT AS B
			ON  A.PRODUCT_CODE = B.PRODUCT_CODE
		 GROUP
			BY  A.MEM_NO
				,B.CATEGORY
		)AS A
 WHERE  구매횟수_순위 = 1;

/* 확인 */ 
SELECT  * FROM CUSTOMER_PRE_CATEGORY;


/***************회원 구매정보 + 연령대 + 선호 카테고리 임시테이블***************/
CREATE TEMPORARY TABLE CUSTOMER_PUR_INFO_AGEBAND_PRE_CATEGORY AS
SELECT  A.*
		,B.CATEGORY AS PRE_CATEGORY
  FROM  CUSTOMER_PUR_INFO_AGEBAND AS A
  LEFT
  JOIN  CUSTOMER_PRE_CATEGORY AS B
    ON  A.MEM_NO = B.MEM_NO;

/* 확인 */ 
SELECT  * FROM CUSTOMER_PUR_INFO_AGEBAND_PRE_CATEGORY;


/***************회원 분석용 데이터 마트 생성(회원 구매정보 + 연령대 + 선호 카테고리 임시테이블)***************/
CREATE TABLE CUSTOMER_MART AS
SELECT  *
  FROM  CUSTOMER_PUR_INFO_AGEBAND_PRE_CATEGORY;
   
/* 확인 */    
SELECT  *
  FROM  CUSTOMER_MART;



/****************************************************************************/
/*********************************데이터 정합성**********************************/
/****************************************************************************/

/***************데이터 마트 회원 수의 중복은 없는가?***************/

SELECT  *
  FROM  CUSTOMER_MART;

SELECT  COUNT(MEM_NO)
		,COUNT(DISTINCT MEM_NO)
  FROM  CUSTOMER_MART;


/***************데이터 마트의 요약 및 파생변수의 오류는 없는가?***************/
  
SELECT  *
  FROM  CUSTOMER_MART;  

/* 회원(1000005)의 구매정보 */
/* 구매금액: 408000 / 구매횟수: 3 구매수량: 14 */

SELECT  SUM(A.SALES_QTY * B.PRICE) AS 구매금액
        ,COUNT(A.ORDER_NO)         AS 구매횟수
        ,SUM(A.SALES_QTY)		   AS 구매수량
  FROM  SALES AS A
  LEFT
  JOIN  PRODUCT AS B
    ON  A.PRODUCT_CODE = B.PRODUCT_CODE
 WHERE  MEM_NO = '1000005';

/* 회원(1000005)의 선호 카테고리 */
/* PRE_CATEGORY: home */
SELECT  *
  FROM  SALES AS A
  LEFT
  JOIN  PRODUCT AS B
    ON  A.PRODUCT_CODE = B.PRODUCT_CODE
 WHERE  MEM_NO = '1000005';
 
 
/***************데이터 마트의 구매자 비중(%)의 오류는 없는가?***************/

/* 회원(Customer) 테이블 기준, 주문(Sales) 테이블 구매 회원번호 LEFT JOIN 결합 */
SELECT  *
  FROM  CUSTOMER AS A
  LEFT
  JOIN  (
		SELECT  DISTINCT MEM_NO
          FROM  SALES
		)AS B
    ON  A.MEM_NO = B.MEM_NO;
   
/* 회원(Customer) 테이블 기준, 주문(Sales) 테이블 구매 회원번호 LEFT JOIN 결합 */
/* 구매여부 추가 */
SELECT  *
		,CASE WHEN B.MEM_NO IS NOT NULL THEN '구매'
			  ELSE '미구매' END AS 구매여부
  FROM  CUSTOMER AS A
  LEFT
  JOIN  (
		SELECT  DISTINCT MEM_NO
          FROM  SALES
		)AS B
    ON  A.MEM_NO = B.MEM_NO;
 
/* 구매여부별, 회원수 */
SELECT  구매여부
		,COUNT(MEM_NO) AS 회원수
  FROM  (
		SELECT  A.*
				,CASE WHEN B.MEM_NO IS NOT NULL THEN '구매'
					  ELSE '미구매' END AS 구매여부
		  FROM  CUSTOMER AS A
		  LEFT
		  JOIN  (
				SELECT  DISTINCT MEM_NO
				  FROM  SALES
				)AS B
			ON  A.MEM_NO = B.MEM_NO
		)AS A
 GROUP
    BY  구매여부; 
 
/* 확인(미구매: 1459 / 구매: 1202) */ 
SELECT  *
  FROM  CUSTOMER_MART
 WHERE  구매금액 IS NULL;
 
SELECT  *
  FROM  CUSTOMER_MART
 WHERE  구매금액 IS NOT NULL; 
