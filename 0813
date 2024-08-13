USE PRACTICE;

/***************INNER JOIN***************/
/* INNER JOIN: 두 테이블의 공통 값이 매칭되는 데이터만 결합 */

/* Customer + Sales Inner JOIN */
SELECT  *
  FROM  CUSTOMER AS A
 INNER
  JOIN  SALES AS B
    ON  A.MEM_NO = B.MEM_NO;

/* Customer 및 Sales 테이블은 mem_no(회원번호) 기준으로 1:N 관계 */
SELECT  *
  FROM  CUSTOMER AS A
 INNER
  JOIN  SALES AS B
    ON  A.MEM_NO = B.MEM_NO
 WHERE  A.MEM_NO = '1000970';
 
 
/***************LEFT JOIN***************/
/* LEFT JOIN: 두 테이블의 공통 값이 매칭되는 데이터만 결합 + 왼쪽 테이블의 매칭되는 않는 데이터는 NULL */

/* Customer + Sales LEFT JOIN */
SELECT  *
  FROM  CUSTOMER AS A
  LEFT
  JOIN  SALES AS B
    ON  A.MEM_NO = B.MEM_NO;

/* NULL은 회원가입만하고 주문은 하지 않는 회원을 의미 */


/***************RIGHT JOIN***************/
/* RIGHT JOIN: 두 테이블의 공통 값이 매칭되는 데이터만 결합 + 오른쪽 테이블의 매칭되는 않는 데이터는 NULL */

/* Customer + Sales RIGHT JOIN */
SELECT  *
  FROM  CUSTOMER AS A
  RIGHT
  JOIN  SALES AS B
    ON  A.MEM_NO = B.MEM_NO
 WHERE  A.MEM_NO IS NULL;

/* 회원번호(9999999)는 비회원 */
/* IS NULL: 비교 연산자 / NULL인 값만 */


/***************테이블 결합(JOIN) + 데이터 조회(SELECT)***************/

/* 회원(Customer) 및 주문(Sales) 테이블 Inner JOIN 결합 */
SELECT  *
  FROM  CUSTOMER AS A
 INNER
  JOIN  SALES AS B
    ON  A.MEM_NO = B.MEM_NO;

/* 임시테이블 생성 */
CREATE TEMPORARY TABLE CUSTOMER_SALES_INNER_JOIN
SELECT  A.*
		,B.ORDER_NO
  FROM  CUSTOMER AS A
 INNER
  JOIN  SALES AS B
    ON  A.MEM_NO = B.MEM_NO;

/* 임시테이블 조회 */
SELECT * FROM CUSTOMER_SALES_INNER_JOIN;

/* 임시테이블(TEMPORARY TABLE)은 서버 연결 종료시 자동으로 삭제됩니다. */
   

/* 성별이 남성 조건으로 필터링하여 */
SELECT  *
  FROM  CUSTOMER_SALES_INNER_JOIN
 WHERE  GENDER = 'MAN';


/* 거주지역별로 구매횟수 집계 */
SELECT  ADDR
		,COUNT(ORDER_NO) AS 구매횟수
  FROM  CUSTOMER_SALES_INNER_JOIN
 WHERE  GENDER = 'MAN'
 GROUP
    BY  ADDR;


/* 구매횟수 100회 미만 조건으로 필터링 */
SELECT  ADDR
		,COUNT(ORDER_NO) AS 구매횟수
  FROM  CUSTOMER_SALES_INNER_JOIN
 WHERE  GENDER = 'MAN'
 GROUP
    BY  ADDR
HAVING  COUNT(ORDER_NO) < 100;


/* 모든 열 조회 */
/* 구매횟수가 낮은 순으로 */
SELECT  ADDR
		,COUNT(ORDER_NO) AS 구매횟수
  FROM  CUSTOMER_SALES_INNER_JOIN
 WHERE  GENDER = 'MAN'
 GROUP
    BY  ADDR
HAVING  COUNT(ORDER_NO) < 100
 ORDER
    BY  COUNT(ORDER_NO) ASC;
    
    
/***************3개 이상 테이블 결합***************/
/* 주문(Sales) 테이블 기준, 회원(Customer) 및 상품(Product) 테이블 LEFT JOIN 결합 */

SELECT  *
  FROM  SALES AS A
  LEFT
  JOIN  CUSTOMER AS B
    ON  A.MEM_NO = B.MEM_NO
  LEFT
  JOIN  PRODUCT AS C
    ON  A.PRODUCT_CODE = C.PRODUCT_CODE;
