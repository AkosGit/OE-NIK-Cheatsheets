# Csoportfügvények
- AVG - Átlag
- COUNT- recordok száma
- \* összes recordok száma
- SUM - összegzés
- MIN - minimum
- MAX - maximum
```sql
SELECT round(...(salary),tizedes) 
FROM DEPARTMENTS;
```
## Csoportositás
- csoportok létrehozása
- mindennek szerepelnie kell a groupby-ban ami szerepel a select-ben
- helye ORDER BY elött van
- csoportosítás miatt nem szerepelhet bármi a SELECT-ben! 
	- csak a group by található dolgok lehetnek és csoportfügvények
-   nem listázható olyan attribútum, ami szerint nem csoportosítunk
-   Szerepelhet: **csoportosító attribútum, csoportfüggvény vagy konstans, illetve az ezekből alkotott kifejezések**
```sql
SELECT COUNT(*) from DEPARTMENTS
GROUP BY DEPARTMENT_ID;
```
```sql
SELECT  department_name Részleg, 
   SUM(salary) Összfizetés
FROM employees INNER JOIN departments 
USING (department_id)
GROUP BY department_name
ORDER BY department_name;
```
#### COUNT
- A COUNT a nem-null előfordulásokat számolja, ha paramétert adunk neki:
```sql
SELECT job_id Munkakör, 
COUNT(commission_pct) "Jutalékot kaphat"
FROM EMPLOYEES GROUP BY job_id;
```
## Szürés csoportokra
- HAVING nem tud a SELECT-beli névre hivatkozni
- szürés csoportositás után történik
	- GROUP BY szürője
- Helye a lekérdezésben: a GROUP BY után, az ORDER BY előtt.
```sql
SELECT department_id, COUNT(*) 
FROM EMPLOYEES
GROUP BY department_id
HAVING COUNT(*) >= 10;
```
- Azok a munkakörök, amelyekben a legtöbbet kereső dolgozó fizetése 10.000 dollár fölött van, a hozzájuk tartozó legnagyobb fizetéssel…
```sql
SELECT job_title, MAX(salary)
FROM employees NATURAL JOIN jobs
GROUP BY job_title
HAVING MAX(salary) > 10000;
```
-   Listázzuk főnökönként (főnök vezetékneve) a jutalékban nem részesülő közvetlen beosztottainak összfizetését csökkenő sorrendben, feltéve, hogy ez az érték 20.000 USD-nál több.
```sql
SELECT  e1.last_name Főnök,
   SUM(e2.salary) Összfizetés
FROM employees e1, employees e2
WHERE e1.employee_id=e2.manager_id 
 AND e2.commission_pct IS NULL
GROUP BY e1.last_name
HAVING SUM(e2.salary) > 20000
ORDER BY Összfizetés DESC;
```
