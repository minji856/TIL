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
SELECT FIRST_NAME, DEPARTMENT_ID, SALARY
FROM employees
WHERE SALARY > (SELECT AVG(SALARY) FROM employees WHERE DEPARTMENT_ID = 80);




-- 2. SUBQUERY, JOIN
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
-- IFNULL(COUNT(j.job_title), '미사용') 
SELECT j.job_title, COUNT(j.job_title) AS '인원수'
FROM employees e JOIN jobs j ON e.job_id = j.job_id
GROUP BY j.job_title;



-- 3-2.
SELECT j.job_title, COUNT(j.job_title) AS '인원수'
FROM employees e JOIN jobs j ON e.job_id = j.job_id
GROUP BY j.job_title
HAVING COUNT(j.job_title) >= 10;




-- 4. 각 부서별 최대급여를 받는 직원의 이름, 부서명, 급여를 조회하시오.

SELECT e.first_name, d.department_name, MAX(e.salary)
FROM employees e JOIN departments d USING(department_id)
WHERE e.department_id = d.department_id
GROUP BY d.department_name
ORDER BY department_id;


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
SELECT month(hire_date), COUNT(month(hire_date))
FROM employees
GROUP BY month(hire_date)
HAVING COUNT(month(hire_date)) >= 10;



-- 7.
SELECT e.first_name, e.salary
FROM employees e
	JOIN employees m ON e.manager_id = m.employee_id
WHERE e.salary > m.salary;



-- 8.
SELECT e.first_name, e.salary, j.job_title, l.city
FROM employees e 
	JOIN departments d ON e.department_id = d.department_id
	JOIN jobs j ON e.job_id = j.job_id
	JOIN locations l ON d.location_id = l.location_id
WHERE l.city = 'Southlake';



-- 9.
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



-- 10.
SELECT e.first_name, e.phone_number, e.email, 
	 	 IFNULL(m.first_name, '<관리자 없음>') AS '매니저 이름',
		 IFNULL(m.phone_number, '<관리자 없음>') AS '전화번호',
		 IFNULL(m.email, '<관리자 없음>') AS '이메일'
FROM employees e left outer JOIN employees m ON e.manager_id = m.employee_id;



-- 11.
SELECT d.department_name, MIN(e.salary), MAX(e.salary)
FROM employees e
JOIN departments d ON e.department_id = d.department_id
GROUP BY d.department_name
HAVING MIN(e.salary) != MAX(e.salary);



-- 12.
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
