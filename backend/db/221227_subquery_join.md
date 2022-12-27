```
DESC employees;

SELECT * FROM employees WHERE first_name LIKE 'k%';


-- 단일행 : first_name이 kelly 사원과 동일한 부서에서 일하는 근무자의 부서코드, 급여, 이름 조회
SELECT department_id, salary, first_name 
FROM employees 
WHERE department_id = (SELECT department_id 
							  FROM employees 
							  WHERE first_name = 'kelly');

-- 다중행 : first_name이 peter 사원과 동일한 부서에서 일하는 근무자의 부서코드, 급여, 이름 조회
SELECT department_id, salary, first_name 
FROM employees 
WHERE department_id in (SELECT department_id 
							  FROM employees 
							  WHERE first_name = 'peter');


-- 최대급여 부서별 조회
SELECT FIRST_NAME, MAX(SALARY) FROM employees;
SELECT FIRST_NAME, MIN(SALARY) FROM employees;

SELECT department_id, MAX(SALARY) 
FROM employees
GROUP BY department_id;

SELECT first_name, salary FROM employees
WHERE SALARY = (SELECT MIN(SALARY) FROM employees);


-- PETER와 같은 부서이고, 같은 입사일인 사원의 부서코드, 급여, 이름 조회
SELECT JOB_ID, department_id, salary, first_name 
FROM employees 
WHERE department_id in (SELECT department_id FROM employees WHERE first_name = 'peter')
AND JOB_ID IN (SELECT JOB_ID FROM employees WHERE first_name = 'peter');

-- 다중열 서브쿼리
SELECT JOB_ID, department_id, salary, first_name
FROM employees 
WHERE (department_id, JOB_ID) IN
										(SELECT department_id, JOB_ID
										 FROM employees 
										 WHERE first_name = 'peter');


-- subquery

-- kelly와 급여가 같은 사원의 이름, 급여 조회
SELECT first_name, salary
FROM employees
WHERE salary = (SELECT salary FROM employees WHERE FIRST_name='kelly');


-- kelly와 급여가 같은 사원의 이름, 급여 조회
SELECT first_name, salary
FROM employees
WHERE salary > (SELECT salary FROM employees WHERE FIRST_name='KELLY');

-- 모든 WILLIAM보다 급여를 많이 받는 사원의 이름, 급여 조회
SELECT first_name, salary
FROM employees
WHERE salary > ALL (SELECT salary FROM employees WHERE FIRST_name='WILLIAM');

-- 어떤  WILLIAM보다 급여를 많이 받는 사원의 이름, 급여 조회
-- any = in
-- WHERE salary in (10, 20)
-- WHERE salary > ANY (10, 20)
SELECT first_name, salary
FROM employees
WHERE salary > ANY (SELECT salary FROM employees WHERE FIRST_name='WILLIAM');

-- 모든 WILLIAM보다 급여를 많이 받는 사원의 이름, 급여 조회
SELECT first_name, salary
FROM employees
WHERE salary = ALL (SELECT salary FROM employees WHERE FIRST_name='WILLIAM');

-- 어떤  WILLIAM보다 급여를 많이 받는 사원의 이름, 급여 조회
SELECT first_name, salary
FROM employees
WHERE salary = ANY (SELECT salary FROM employees WHERE FIRST_name='WILLIAM');

-- 변수 사용
SET @NAME = 'Kelly';

SELECT first_name, salary
FROM employees
WHERE salary > ANY (SELECT salary FROM employees WHERE FIRST_NAME=@NAME);


-- 부서의 최대급여 사원 이름 조회
SELECT first_name, salary, department_id
FROM employees
WHERE (department_id, SALARY) 
	IN (SELECT department_id, MAX(SALARY) FROM employees GROUP BY department_id)
ORDER BY department_id ASC;


SELECT department_id, MAX(SALARY) FROM employees GROUP BY department_id;


--  ----------------------------------------------------------
-- 서브쿼리 중첩
--  ----------------------------------------------------------

SELECT * FROM locations;
SELECT * FROM departments;

-- 1. 런던 도시코드 조회
SELECT LOCATION_ID FROM locations WHERE CITY='LONDON';

-- 2. 런던 도시코드와 같은 도시코드의 부서코드 조회
SELECT DEPARTMENT_ID FROM departments WHERE LOCATION_ID = 2400;

-- 3. 2번 부서코드에 있는 사람 조회
SELECT FIRST_NAME, DEPARTMENT_ID
FROM employees
WHERE DEPARTMENT_ID = 40;

-- 4. 합체
SELECT FIRST_NAME, DEPARTMENT_ID
FROM employees
WHERE DEPARTMENT_ID = (SELECT DEPARTMENT_ID 
								FROM departments 
								WHERE LOCATION_ID = (SELECT LOCATION_ID 
															FROM locations 
															WHERE CITY='SOUTH SAN FRANCISCO'));
															
--  ----------------------------------------------------------
-- 연관 서브쿼리
--  ----------------------------------------------------------		
													
-- 부서의 최대급여 사원 이름 조회
SELECT first_name, salary, department_id
FROM employees
WHERE (department_id, SALARY) 
	IN (SELECT department_id, MAX(SALARY) FROM employees GROUP BY department_id)
ORDER BY department_id ASC;															

-- 부서의 평균급여보다 많이 받는 사원의 급여, 부서번호 조회
SELECT salary, department_id, 
		(SELECT AVG(SALARY) FROM employees WHERE e.department_id = department_id)
FROM employees e
WHERE SALARY > ANY(SELECT AVG(SALARY) FROM employees WHERE e.department_id = department_id)
ORDER BY department_id ASC;	

--  ----------------------------------------------------------
--  inline view 
--  ----------------------------------------------------------	

-- inline view 
-- from절에 들어가는 서브쿼리이다.
-- from절은 서브쿼리의 결과를 1개의 가상테이블처럼 대한다.

-- employees가 대상이다.
SELECT AVG(salary)
FROM employees
WHERE salary >= 10000;

-- SAL_TBL라는 가상테이블의 AVG_SAL라는 평균값을 사용한다.
-- 제한된 정보를 제공한다. 범위를 준다.
SELECT SAL_TBL.AVG_SAL AS '고액월급평균'
FROM (SELECT AVG(salary) AVG_SAL
		FROM employees
		WHERE salary >= 10000) SAL_TBL;

--  ----------------------------------------------------------
--  IFNULL
--  ----------------------------------------------------------	

-- 급여 수준에 따른 직급 조회
-- EMPLOYEES 테이블에 급여컬럼이 있다. 직급컬럼은 없다.
-- 직급 20000 이상이면 임원, 15000ㅇ 이상이면 부장, 10000 이상이면 과장, 
-- 5000 이상이면 대리, 이하는 사원
-- 급여 : SALARY + SALARY * COMMISSION_PCT

-- 그런데 STEVE는 SALARY가 20000이지만 PCT가 NULL이다.
-- STEVE는 결과값이 NULL이 되어 ELSE로 반환된다.
-- NULL값 컬럼 연산식 결과도 NULL이다. IFNULL함수로 다른 값으ㅗ 변경해 연산하자.

SELECT MAX(salary), MIN(salary) FROM employees;

SELECT FIRST_NAME,
	CASE
		WHEN IMSISAL >= 20000 THEN '임원'
		WHEN IMSISAL >= 15000 THEN '부장'
		WHEN IMSISAL >= 10000 THEN '과장'
		WHEN IMSISAL >= 5000 THEN '대리'
		ELSE '사원'
	END 직급
FROM (SELECT FIRST_NAME, salary + SALARY * IFNULL(COMMISSION_PCT, 0.1) AS IMSISAL FROM employees) IMSITABLE;


/*
SELECT FIRST_NAME,
	CASE
		WHEN SALARY + SALARY * IFNULL(COMMISSION_PCT, 0.1) >= 20000 THEN '임원'
		WHEN SALARY + SALARY * COMMISSION_PCT >= 15000 THEN '부장'
		WHEN SALARY + SALARY * COMMISSION_PCT >= 10000 THEN '과장'
		WHEN SALARY + SALARY * COMMISSION_PCT >= 5000 THEN '대리'
		ELSE '사원'
	END 직급
	FROM EMPLOYEES;
*/


SELECT first_name, salary, (SELECT MIN(salary) FROM employees) AS 'ms' FROM employees;


--  ----------------------------------------------------------
--  where절 + update에 서브쿼리사용하기
--  ----------------------------------------------------------	
-- where절 + update에 서브쿼리사용하기
-- update
-- emp_copy

-- kelly와 같은 부서인 사원을 부서 100번으로 이동시킨다
SELECT first_name, department_id FROM emp_copy WHERE department_id = 50;
SELECT first_name, department_id FROM emp_copy WHERE department_id = 100;
SELECT first_name, department_id FROM emp_copy WHERE department_id = 20;

UPDATE emp_copy
SET department_id = 100
WHERE department_id = (SELECT department_id FROM emp_copy WHERE first_name='kelly');

-- 100번 부서원을 susan 부서로이동
UPDATE emp_copy
SET department_id = (SELECT department_id FROM emp_copy WHERE first_name='susan')
WHERE department_id = 100;


--  ----------------------------------------------------------
--  테이블 조합
-- 2개의 물리적인 테이블로 나뉘어잇다가 select시 합쳐서 조회한다.
-- 조합 테이블의 컬럼, 갯수, 타입, 순서가 일치해야한다.
-- union, union all, intersect, minus(마리아db에서는 except)
-- 서브쿼리아님. 멀티쿼리라고 보자.
--  ----------------------------------------------------------	

-- 50번 부서의 모든 부서원을 복사한 emp_dept_50 테이블 생성
CREATE TABLE emp_dept_50
(SELECT * FROM employees WHERE department_id=50);

-- manager 계열 직종 사원들을 복사한 emp_job_man 테이블 생성
CREATE TABLE emp_job_man
(SELECT * FROM employees WHERE job_id LIKE '%man%');

SELECT * FROM emp_dept_50;
SELECT * FROM emp_job_man;

-- 재난 지원금을 지원하려고 한다.
-- 대상은 50번 부서원 혹은 manager 직종이다.
SELECT employee_id, first_name, department_id, job_id FROM emp_dept_50
UNION
SELECT employee_id, first_name, department_id, job_id FROM emp_job_man
ORDER BY 1;

-- 재난 지원금을 지원하려고 한다.
-- 대상은 50번 부서원 혹은 manager 직종이다.
-- 50번 부서이면서 manager 직종이면 중복으로 받을 수 있다.
SELECT employee_id, first_name, department_id, job_id FROM emp_dept_50
UNION all
SELECT employee_id, first_name, department_id, job_id FROM emp_job_man
ORDER BY 1;

-- 재난 지원금을 지원하려고 한다.
-- 대상은 50번 부서원이며 manager직종이어야 한다.
SELECT employee_id, first_name, department_id, job_id FROM emp_dept_50
INTERSECT
SELECT employee_id, first_name, department_id, job_id FROM emp_job_man
ORDER BY 1;

-- 재난 지원금을 지원하려고 한다.
-- 대상은 50번 부서원이며, manager직종이 아니어야 한다.
SELECT employee_id, first_name, department_id, job_id FROM emp_dept_50
except
SELECT employee_id, first_name, department_id, job_id FROM emp_job_man
ORDER BY 1;

-- 재난 지원금을 지원하려고 한다.
-- 대상은 manager직종이며, 50번 부서원이 아니어야 한다.
SELECT employee_id, first_name, department_id, job_id FROM emp_job_man
except
SELECT employee_id, first_name, department_id, job_id FROM emp_dept_50
ORDER BY 1;











```
