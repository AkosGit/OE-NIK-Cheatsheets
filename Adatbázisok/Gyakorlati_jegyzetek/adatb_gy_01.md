## Munkakörnyezet 
- Virtuális gép (otthonra letölthető): SQL Developer, SQL*Plus + szövegszerkesztő Felhasználók: hr / hr magyar / Tigris-1 
- Online munkakörnyezet: apex.oracle.com (a gyakorló séma tábláit scripttel kell létrehozni benne)
## Lekérdezések
- Minden: SELECT * FROM employees;
-  A SELECT és a FROM kötelező elemei a lekérdezésnek. Akkor is kell FROM, ha nem táblával dolgozunk: 
```sql
SELECT sysdate FROM dual;
SELECT 25+17 FROM dual;
```
-  Az employees táblából csak a dolgozó nevét, fizetését, és a részlegének azonosítóját szeretnénk látni. 
```sql 
SELECT first_name, last_name, salary, department_id 
FROM employees;
```
- Összefűzés: 
```sql
SELECT first_name||' '||last_name FROM employees;
``` 
- Oszlopok másodlagos neve (alias): 
```sql
SELECT employee_id AS "Azonosító", first_name AS "Keresztnév" 
FROM employees;
```
- Feltételek: 
	```sql
	SELECT last_name, job_id, salary 
	FROM employees 
	WHERE salary > 10000;
	```
	- NVL(attribútum,helyettesítő_érték) 
		- Ahol NULL érték szerepel, ott az NVL a megadott értéket behelyettesíti a rekord megfelelő attribútumába.
	- …és ki kap 2000 dollárnál több jutalékot? Mennyit?
	```sql
	SELECT first_name, NVL(commission_pct,0)*salary Jutalék 
	FROM employees 
	WHERE NVL(commission_pct,0)*salary>2000; 
	```
	- vigyázat! a "WHERE Jutalék>2000" nem működik!
- Összetett feltételek:
	- Írassuk ki azon dolgozók teljes nevét, munkakörét és fizetését, akiknek a fizetése **2400 és 3000 USD között** van. A lista fejléce legyen „Név”, „Munkakör”, „Fizetés”, rendezzen a dolgozók fizetése szerint csökkenő sorrendbe.
	```sql
	SELECT first_name||' '||last_name Név,
  	job_id Munkakör, 
  	salary Fizetés
	FROM employees
	WHERE salary BETWEEN 2400 AND 3000
	ORDER BY salary DESC;
	```
	- Írassuk ki azon dolgozók nevét, munkakörét és fizetését, akiknek a fizetése **NINCS** **2400 és 3000 USD között**. A lista fejléce legyen „Név”, „Munkakör”, „Fizetés”, rendezzen a dolgozók fizetése szerint csökkenő sorrendben.
	```sql
	SELECT first_name||' '||last_name Név,
  	job_id Munkakör, 
  	salary Fizetés
	FROM employees
	WHERE salary NOT between 2400 and 3000
	ORDER BY salary DESC;
	```
	- Kik azok a 80-es részlegben dolgozók, akiknek a fizetése legalább 10.000 USD?
	```sql
	SELECT * FROM employees 
	WHERE department_id=80 
	AND	salary>=10000;
	```
- Rendezés: 
	```sql
	SELECT * FROM employees 
	ORDER BY salary;
	```
	- és csökkenő sorrendbe?
	```sql
	SELECT * FROM employees 
	ORDER BY salary DESC;
	```
	- Rendezzük a munkakörök tábláját megnevezés szerinti sorrendbe
	```sql
	SELECT * FROM jobs 
	ORDER BY job_title DESC;
	```
	- Rendezzük a dolgozókat részlegazonosító szerint csökkenő, azon belül keresztnév szerint növekvő sorrendbe!
	```sql
	SELECT * FROM employees 
	ORDER BY department_id DESC, first_name;
	```
	- Rendezésnél hivatkozhatunk az oszlopok sorszámára (amilyen sorrendben a SELECT listában megjelennek). A sorszámozás itt 1-től indul!
	```sql
	SELECT * FROM employees 
	ORDER BY 11 DESC, 2;
	```
- Stringek illesztése, LIKE és kis,nagy betük:
	- Írassuk ki azon dolgozók kereszt és vezetéknevét akiknek a keresztnevében szerepel „EV”
	```sql
	SELECT first_name, last_name
	FROM employees
	WHERE LOWER(first_name) LIKE '%ev%';
	```
	vagy
	```sql
	SELECT first_name, last_name
	FROM employees
	WHERE UPPER(first_name) LIKE '%EV%';
	```
- IN,OR
	```sql
	SELECT first_name, last_name
	FROM employees 
	WHERE LOWER(first_name) IN ('kevin','steven'); 
	```
	
	```sql
	SELECT first_name,last_name
	FROM employees
	WHERE first_name='Kevin' OR first_name='Steven';
	```
- DISTINCT	
	-	Milyen részlegazonosítók léteznek ennél a cégnél?
		SELECT department_id 
		FROM employees;
		De nekem elég, ha egy részleget csak egyszer listáz…
	```sql
	SELECT DISTINCT department_id
	FROM employees;
	```
- IS NULL / IS NOT NULL
	- A commission_pct>0 nem feltétlenül helyes.
	```sql
	SELECT first_name, last_name,
	commission_pct
	FROM employees
	WHERE commission_pct IS NOT NULL;
	```
- Műveletek dátumokkal
	- Parancssorban átállíthatjuk az aktuális munkamenetre...
	- alter session set nls_date_format='YYYY.MM.DD';
	- vagy helyben jelezzük függvénnyel: -   to_date('1981.05.01','YYYY.MM.DD');
- TO_CHAR
```sql
SELECT first_name,last_name, 
TO_CHAR(hire_date,'YYYY.MM.DD') "Belépés dátuma"
FROM employees
WHERE TO_CHAR(hire_date,'YY')='01';
```
- EXTRACT
```sql
SELECT first_name, last_name, hire_date,
EXTRACT(YEAR FROM hire_date) Be_Év,
EXTRACT(MONTH FROM hire_date) Be_Hónap,
EXTRACT(DAY FROM hire_date) Be_Nap
FROM employees
WHERE hire_date>TO_DATE('2001.01.13','YYYY.MM.DD');
```
- LENGTH
```sql
SELECT * FROM employees
WHERE LENGTH(first_name)>5;
```
- SUBSTR
	- SUBSTR(honnantól, mennyit)
	- (-) jel hátulról kezdi a számolást 
```sql
SELECT * FROM employees
WHERE SUBSTR(first_name,2,1)='a';
```
- CASE
	- Logikai elágazás a lekérdezésben
	```sql
	SELECT last_name Név,
	CASE	
	WHEN salary<5000 THEN 'Alacsony'
	WHEN salary BETWEEN 5000 AND 12000 THEN 'Közepes'
	ELSE 'Magas'
	END Fizetés,
	department_id Részleg
	FROM employees;
	```
	- Listázzuk ki a dolgozók teljes nevét, belépési dátumát, a vállalatnál eltöltött éveik számát, valamint a tapasztalatukat: akik legalább 15 éve dolgoznak a cégben, őket jelöljük SENIOR szóval (különben JUNIOR). Rendezzünk részleg, azon belül vezetéknév szerint.
	```sql
	SELECT first_name||' '||last_name Név, hire_date Belépés,
	ROUND(months_between(sysdate, hire_date)/12) Évek,
	CASE
	WHEN ROUND(months_between(sysdate, hire_date)/12)>=15 THEN 	'SENIOR'
	ELSE 'JUNIOR'
	END Tapasztalat
	FROM employees
	ORDER BY department_id, last_name;
	```

