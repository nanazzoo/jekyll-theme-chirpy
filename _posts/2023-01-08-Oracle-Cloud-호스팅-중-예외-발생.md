---
title: Oracle Cloud 호스팅 중 Chart.js 문제 발생
date: 2023-01-08 10:21:00 +09:00
categories: [Farmfarm, troubleshooting]
tags: [proejct, troubleshooting, oracleCloud]
---



## Oracle Cloud 호스팅 중 Chart.js 문제 발생

Farmfarm 프로젝트에서 SQL문의 문제를 개선하였던 경험을 포스트로 남기려고 합니다.



</br></br>



### 1. 문제 상황

Oracle Cloud에 프로젝트를 배포 후, 관리자 페이지의 Chart.js를 이용한 대시보드 부분이 동작하지 않는 오류를 발견했습니다. **로컬에서 서버를 돌렸을 때** 문제없이 차트가 나왔기 때문에 어떤 문제가 있는 지 확인해야 했습니다.



> 문제의 대시보드

![1](C:\Users\bboya\OneDrive\바탕 화면\1.png)



</br></br></br></br>



### 원인 파악

대시보드의 차트 부분이 모두 나오지 않고 있었기 때문에 첫 번째로 차트 관련 SQL문을 확인했습니다.

제가 만든 기능이 아니라서 차트 기능을 담당한 팀원과 함께 해결을 진행하였습니다.

문제는 가입, 주문 수를 불러오는 차트 sql문에 있었습니다. 

where문 작성 시 두 개의 날짜를 비교할 때 CHAR형 날짜와 DATE형 날짜를 비교했던 부분에서 오류가 발생했음을 인지하였고 수정을 진행했습니다.



</br></br></br></br>



#### 기존 코드

```sql
	 // 1.
     SELECT TO_CHAR(b.OD, 'MM-DD') AS ORDER_DATE
	    	 , NVL(SUM(a.cnt), 0) AS ORDER_COUNT
		FROM ( SELECT TO_CHAR(ORDER_DATE, 'YYYY-MM-DD') AS ORDER_DATE
		              ,COUNT(*) cnt
		        FROM "ORDER"
		        WHERE ORDER_DATE BETWEEN SYSDATE-31
		                             AND SYSDATE
		        GROUP BY ORDER_DATE
		        ) a
		      , (SELECT (TO_DATE(SYSDATE-30,'YY-MM-DD') + LEVEL) AS OD
				FROM dual 
				<![CDATA[CONNECT BY LEVEL <= 31]]>) b
		WHERE b.OD = a.ORDER_DATE(+)
		GROUP BY b.OD
		ORDER BY b.OD
		
		// 2.
        SELECT TO_CHAR(b.SD, 'MM-DD') AS SIGNUP_DATE
		       ,NVL(SUM(a.cnt), 0) AS SIGNUP_COUNT
		FROM ( SELECT TO_CHAR(SIGNUP_DATE, 'YYYY-MM-DD') AS SIGNUP_DATE ,COUNT(*) cnt
		        FROM MEMBER
		        WHERE SIGNUP_DATE BETWEEN SYSDATE-31 AND SYSDATE
		        GROUP BY SIGNUP_DATE
		        ) a
		       ,(SELECT (TO_DATE(SYSDATE-30,'YY-MM-DD') + LEVEL) AS SD
						FROM DUAL 
						<![CDATA[CONNECT BY LEVEL <= 31]]>) b
		WHERE b.SD = a.SIGNUP_DATE(+)
		GROUP BY b.SD
		ORDER BY b.SD 
```

기존 코드의 WHERE절을 보면 **CHAR 타입 데이터와 DATE 타입 데이터를 형변환 없이 비교**하고 있음을 알 수 있었습니다.

1번 SQL문에서 ORDER_DATE의 형식은 CHAR, OD의 형식은 DATE 타입이었고, 2번 SQL문에서 SIGNUP_DATE는 CHAR 타입, SD는 DATE 타입임에도 로컬에서 작동할 때에는 oracle에서 자동 형변환을 하여 문제없이 Chart가 표시 되었으나 호스팅 한 서버에서는 다른 환경 조건 때문에 타입이 달라 오류가 발생하였습니다.



</br></br></br></br>



### 해결 방안

비교되는 두 컬럼의 타입만 맞춰주면 해결될 수 있었지만 조금 더 고민해보기로 했습니다.

기존의 코드에서 사용한 방식이 조금 복잡하고 가독성이 떨어진다는 단점이 있었기 때문에 조금 더 간결하고 직관적인 코드로 변경하고자 하였습니다.



</br></br></br></br>



#### 수정 후 코드

``` sql
	// 1.
	SELECT ORDER_DATE, 
        (SELECT COUNT(*) 
        FROM "ORDER" o 
        WHERE TO_CHAR(o.ORDER_DATE , 'YYYY-MM-DD') = a.ORDER_DATE) ORDER_COUNT
    FROM (SELECT TO_CHAR(CURRENT_DATE - 31 + LEVEL, 'YYYY-MM-DD') ORDER_DATE 
    	FROM DUAL CONNECT BY LEVEL <=31) a
		
	// 2.	
    SELECT SIGNUP_DATE, 
    	(SELECT COUNT(*) 
         FROM MEMBER m 
         WHERE TO_CHAR(m.SIGNUP_DATE, 'YYYY-MM-DD') = a.SIGNUP_DATE) SIGNUP_COUNT
    FROM (SELECT TO_CHAR(CURRENT_DATE - 31 + LEVEL, 'YYYY-MM-DD') SIGNUP_DATE 
          FROM DUAL CONNECT BY LEVEL <=31) a
```

기존의 복잡한 방식에서 벗어나 현 시점으로부터 31일 전까지에 대한 가입 데이터를 불러오는 형식으로 변경하였습니다.

아래는 31일간의 가입 추이를 불러오는 2번 SQL문에 대한 설명입니다.



1. `DUAL` 테이블과 `CONNECT BY` 구문을 사용하여 31일 동안의 모든 일자를 조회합니다.
   - `CONNECT BY` 절에서 `LEVEL` 함수를 사용하여 1부터 31까지의 숫자 시퀀스를 생성합니다.
   - `CURRENT_DATE - 31`을 사용하여 현재 날짜에서 31일을 뺀 날짜를 기준으로 `LEVEL` 함수가 생성하는 숫자 시퀀스에 일자를 더하여, 31일 동안의 모든 일자를 조회합니다.
   - `TO_CHAR` 함수를 사용하여 일자를 `YYYY-MM-DD` 형식의 문자열로 변환합니다.
2. 서브쿼리에서 각 일자별로 회원 가입한 회원의 수를 조회합니다.
   - `MEMBER` 테이블에서 `SIGNUP_DATE` 컬럼을 `TO_CHAR` 함수를 사용하여 `YYYY-MM-DD` 형식의 문자열로 변환하고, 이 문자열이 `a.SIGNUP_DATE`와 일치하는 행의 개수를 조회합니다.
   - `SIGNUP_COUNT` 컬럼에 조회된 회원 수를 저장합니다.
3. 메인쿼리에서 서브쿼리를 사용하여 각 일자별로 회원 가입한 회원의 수를 조회합니다.
   - `SIGNUP_DATE` 컬럼에는 서브쿼리에서 조회한 일자 정보가 저장됩니다.
   - `SIGNUP_COUNT` 컬럼에는 서브쿼리에서 조회한 각 일자별 회원 가입 수가 저장됩니다.



주문 SQL문도 위와 같은 방식으로 동작하도록 변경하였습니다.



</br></br></br></br>



### 결과



수정 결과, SQL문의 가독성이 눈에 띄게 증가하였습니다.

성능상으로는 10회 실행 평균 기존 코드가 평균 38ms, 수정 후 코드가 평균 37.8ms로 유의미한 속도의 개선은 이루어지지 않았다는 평가입니다.





![스크린샷 2023-03-13 125853](C:\Users\bboya\OneDrive\사진\스크린샷\스크린샷 2023-03-13 125853.png)



SQL문을 수정하고 다시 배포한 후 관리자 페이지 화면입니다.

Chart.js로 만든 그래프가 잘 동작하는 것을 확인하였습니다.



</br></br></br></br>



### 마무리

제가 맡은 기능도 아니었고, 로컬에서 정상적으로 수행이 됐기 때문에 코드의 성능이나 문제에 대해서 제대로 짚어보지 않았었는데, 실제 서비스 하려는 프로젝트였다면 큰 문제를 일으킬 수 있었던 부분이었기 때문에 앞으로 코드를 작성함에 있어서 타입, 코드의 가독성, 성능 부분에서 신경을 써야겠다는 생각을 하게 됐던 경험이었습니다.

잘 되는 부분도 다시 한번 돌아봐야 할 것 같습니다! 



</br></br></br></br>

