## Allekérdezések
- lekérdezés eredményét használni új lekérdezésben
- allekérdezésben használhatjuk a from-ban elnevezett táblát
	- ha inner joint használunk akkor csak akkor lehet táblát elnevezni ha a másik tábla egy alllekérdezés
- allekérdezésből csak azt tudjuk elérni ami a SELECT-be volt
- ajánlott először megcsinálni az allekérdezést utána a főt
- feltétel kifejezésben
	```sql
	SELECT job_title FROM EMPLOYEES 
	NATURAL JOIN jobs
	WHERE salary = (SELECT MIN(salary) 
	FROM EMPLOYEES);
	```
- tábla helyett (inline view)
```sql
SELECT vnev||' '||knev Oktató, 
fizetes Fizetés, Tárgyak 
FROM oktatok NATURAL JOIN 
(SELECT oktatoID, COUNT(*) Tárgyak 
FROM leckekonyv GROUP BY oktatoID 
HAVING AVG(ertekeles) >= 3) 
FETCH FIRST 5 ROWS ONLY;
```
- mező helyett
	```sql
	SELECT  last_name, salary,
  	department_id, 
	(SELECT AVG(salary) FROM employees 
	WHERE department_id=e.department_id
	) atlag
	FROM employees e;
	```
- join-nal is használható
```sql
SELECT last_name Név,
       job_title Munkakör,
       salary Fizetés
FROM employees NATURAL JOIN jobs
WHERE job_id = 
 (SELECT job_id FROM employees
  WHERE salary=
  (SELECT MIN(salary) FROM employees)
 );
```
- Listázzuk azon dolgozók vezetéknevét, fizetését és részlegük nevét, akik többet keresnek, mint amennyi a részlegük átlagfizetése.
```sql
SELECT  last_name, salary,
   department_name
FROM employees INNER JOIN departments 
USING (department_id) NATURAL JOIN 
(SELECT department_id,
ROUND(AVG(salary)) részlegátlag
FROM employees 
GROUP BY department_id)
WHERE salary > részlegátlag; 
```
- vagy
```sql
SELECT  last_name, salary,
   department_name
FROM employees e, departments d
WHERE e.department_id=d.department_id 
AND salary > 
(SELECT AVG(salary) FROM employees
WHERE e.department_id=department_id
);
```
- Hogy hívják és hány tanítványa van a legjobban kereső oktatónak?
```sql
SELECT vnev||' '||knev Oktató, 
(SELECT COUNT(neptunID) FROM leckekonyv 
WHERE oktatoID = o.oktatoID) 
Tanítványok FROM oktatok o WHERE fizetes = (SELECT MAX(fizetes) FROM oktatok);
```
- Kik azok az oktatók, akiknek a kurzusait senki nem vette fel?
```sql
SELECT o.oktatoID, vnev||' '||knev Oktató 
FROM oktatok o INNER JOIN 
(SELECT oktatoID FROM oktatok 
MINUS 
SELECT oktatoID FROM leckekonyv) s ON o.oktatoID = s.oktatoID;
```