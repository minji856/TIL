```
DESC departments;
DESC employees;
DESC countries;
DESC locations;
DESC regions;

SELECT * FROM departments;
SELECT * FROM employees;
SELECT * FROM countries;
SELECT * FROM locations;
SELECT * FROM regions;
SELECT * FROM jobs;

-- 1. SUBQUERY
-- 1. 80번부서의 평균급여보다 많은 급여를 받는 직원의 이름, 부서id, 급여를 조회하시오.
SELECT FIRST_NAME, DEPARTMENT_ID, SALARY
FROM employees
WHERE SALARY > (SELECT AVG(SALARY) FROM employees WHERE DEPARTMENT_ID = 80);




-- 2. SUBQUERY, JOIN
-- 2. 'South San Francisco'에 근무하는 직원의 최소급여보다 급여를 많이 받으면서 50 번부서의 평균급여보다 많은 급여를 받는 직원의 이름, 급여, 부서명, 
부서id를 조회하시오.
SELECT e.first_name, e.salary, d.department_name, d.department_id, l.city
FROM employees e 
	JOIN departments d ON e.department_id = d.department_id
	JOIN locations l ON d.location_id = l.location_id
WHERE l.city = 'South San Francisco'
AND e.salary > (SELECT AVG(SALARY) FROM employees WHERE DEPARTMENT_ID = 50);

--

SELECT e.first_name, e.salary, d.department_name, d.department_id, l.city
FROM employees e 
	JOIN departments d ON e.department_id = d.department_id
	JOIN locations l ON d.location_id = l.location_id
WHERE e.salary > (SELECT MIN(e.salary)
						FROM employees e 
							JOIN departments d ON e.department_id = d.department_id
							JOIN locations l ON d.location_id = l.location_id
						WHERE l.city = 'South San Francisco')
AND e.salary > (SELECT AVG(SALARY) FROM employees WHERE DEPARTMENT_ID = 50);

-- 'South San Francisco'에 근무하는 직원의 최소급여
SELECT MIN(e.salary)
FROM employees e 
	JOIN departments d ON e.department_id = d.department_id
	JOIN locations l ON d.location_id = l.location_id
WHERE l.city = 'South San Francisco';

SELECT MIN(salary)
FROM employees
WHERE salary = (SELECT );

-- 50번 부서의 평균
SELECT AVG(SALARY) FROM employees WHERE DEPARTMENT_ID = 50;




-- 3-1.
-- 3-1.각 직급별(job_title) 인원수를 조회하되 사용되지 않은 직급이 있다면 해당 직급도 출력결과에 포함시키시오. 
-- IFNULL(COUNT(j.job_title), '미사용') 
SELECT j.job_title, COUNT(j.job_title) AS '인원수'
FROM employees e JOIN jobs j ON e.job_id = j.job_id
GROUP BY j.job_title;



-- 3-2.
-- 3-2. 직급별 인원수가 10명 이상인 직급만 출력결과에 포함시키시오.
SELECT j.job_title, COUNT(j.job_title) AS '인원수'
FROM employees e JOIN jobs j ON e.job_id = j.job_id
GROUP BY j.job_title
HAVING COUNT(j.job_title) >= 10;




-- 4. 각 부서별 최대급여를 받는 직원의 이름, 부서명, 급여를 조회하시오.
-- 
SELECT e.first_name, d.department_name, MAX(e.salary)
FROM employees e JOIN departments d USING(department_id)
WHERE e.department_id = d.department_id
GROUP BY d.department_name
ORDER BY department_id;

-- 
5. 직원의 이름, 부서id, 급여를 조회하시오. 그리고 직원이 속한 해당 부서의 
최소급여를 마지막에 포함시켜 출력 하시오.
SELECT first_name, department_name, salary, d.department_id
FROM employees e
	JOIN departments d ON e.department_id = d.department_id
WHERE salary in (SELECT max(salary) FROM employees GROUP BY department_id)
GROUP BY department_name
ORDER BY department_id;

SELECT salary, department_id, 
		(SELECT AVG(SALARY) FROM employees WHERE e.department_id = department_id)
FROM employees e JOIN departments d USING(department_id)
WHERE SALARY > ANY(SELECT AVG(SALARY) FROM employees WHERE e.department_id = department_id)
ORDER BY department_id ASC;

-- 서브쿼리
SELECT e.first_name, e.department_id, e.salary
FROM employees e
JOIN departments d ON e.department_id = d.department_id
WHERE(d.department_id, e.salary) in (SELECT department_id, MAX(salary) FROM employees GROUP BY department_id)
ORDER BY department_id;

--인라인 뷰
SELECT e.first_name, e.department_id, e.salary FROM employees e
INNER JOIN (SELECT department_id max_id, MAX(salary) max_salary FROM employees GROUP BY department_id) m
ON e.department_id = m.max_id AND e.salary = m.max_salary
ORDER BY e.department_id;

/*
SELECT e.first_name, d.department_id, e.salary
FROM employees e
	JOIN departments d ON e.department_id = d.department_id
GROUP BY d.department_id
HAVING (d.department_id, e.salary) in (SELECT department_id, MAX(salary) FROM employees GROUP BY department_id)
ORDER BY d.department_id;
*/


-- 5.
-- 연관 서브쿼리 사용
SELECT first_name, department_id, salary, 
	(SELECT MIN(salary) FROM employees WHERE e.department_id = department_id) AS '부서최저'
FROM employees e;




-- (50번 부서의) 최소급여 구하기
SELECT department_id, MIN(salary)
FROM employees
WHERE department_id = 50;

SELECT first_name, salary FROM employees
WHERE SALARY = (SELECT MIN(SALARY) FROM employees);



-- 6.
-- 6. 월별 입사자 수를 조회하되, 입사자 수가 10명 이상인 월만 출력하시오.

SELECT month(hire_date), COUNT(month(hire_date))
FROM employees
GROUP BY month(hire_date)
HAVING COUNT(month(hire_date)) >= 10;



-- 7.자신의 관리자(상사)보다 많은 급여를 받는 직원의 이름과 급여를 조회하시오.

SELECT e.first_name, e.salary
FROM employees e
	JOIN employees m ON e.manager_id = m.employee_id
WHERE e.salary > m.salary;



-- 8.'Southlake'에서 근무하는 직원의 이름, 급여, 직책(job_title)을 조회하시오
SELECT e.first_name, e.salary, j.job_title, l.city
FROM employees e 
	JOIN departments d ON e.department_id = d.department_id
	JOIN jobs j ON e.job_id = j.job_id
	JOIN locations l ON d.location_id = l.location_id
WHERE l.city = 'Southlake';



-- 9.국가별 근무 인원수를 조회하시오. 단, 인원수가 3명 이상인 국가정보만 출력되어야함.
SELECT * FROM departments;
SELECT * FROM employees;
SELECT * FROM countries;
SELECT * FROM locations;
SELECT * FROM regions;
SELECT * FROM jobs;

SELECT c.country_id, COUNT(c.country_id)
FROM employees e 
	JOIN departments d ON e.department_id = d.department_id
	JOIN locations l ON d.location_id = l.location_id
	JOIN countries c ON l.country_id = c.country_id
GROUP BY c.country_id
HAVING COUNT(c.country_id) >= 3;



-- 10.직원의 폰번호, 이메일과 상사의 폰번호, 이메일을 조회하시오. 단, 상사가 없는 직원은 '<관리자 없음>'이 출력되도록 해야 한다.
SELECT e.first_name, e.phone_number, e.email, 
	 	 IFNULL(m.first_name, '<관리자 없음>') AS '매니저 이름',
		 IFNULL(m.phone_number, '<관리자 없음>') AS '전화번호',
		 IFNULL(m.email, '<관리자 없음>') AS '이메일'
FROM employees e left outer JOIN employees m ON e.manager_id = m.employee_id;



-- 11.각 부서 이름별로 최대급여와 최소급여를 조회하시오. 단, 최대/최소급여가 동일한 부서는 출력결과에서 제외시킨다.

SELECT d.department_name, MIN(e.salary), MAX(e.salary)
FROM employees e
JOIN departments d ON e.department_id = d.department_id
GROUP BY d.department_name
HAVING MIN(e.salary) != MAX(e.salary);



-- 12.부서별, 직급별, 평균급여를 조회하시오. 
   단, 평균급여가 50번부서의 평균보다 많은 부서만 출력되어야 합니다.


-- 50번 부서의 a직급의 평균급여가 얼마
-- 50번 부서의 b직급의 평균급여가 얼마
-- 평균급여는 50번 부서의 평균보다 높아야 한다.
SELECT department_id, job_id, avg(salary)
FROM employees 
GROUP BY department_id, job_id
HAVING avg(salary) > (SELECT avg(salary)
							FROM employees
							WHERE department_id = 50);

-- 평균급여가 50번부서의 평균보다 많은 부서만 출력
SELECT avg(salary)
FROM employees
WHERE department_id = 50;

-- 부서별 평균급여
SELECT avg(salary)
FROM employees
GROUP BY department_id;

-- 평균급여
SELECT avg(salary)
FROM employees;

-- 직급별 평균급여
SELECT avg(salary)
FROM employees
GROUP BY job_id;



```
