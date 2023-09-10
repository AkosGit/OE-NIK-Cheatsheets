# Allekérdezések(folyt.)
## IN,  ANY/SOME,  ALL
- EXISTS (Semijoin)
	- a feltétel akkor érvényesül ha az Allekérdezés vissza add legalább egy sort vissza ad(benne van)
- Antijoin
	- Where id NOT IN (lekérdezés)
	- Visszaadja azokat, amelyeknek **nincs** megfelelője az allekérdezésben.
- Mindháromnak közös tulajdonsága:
	- egy értéket (bal oldal) hasonlítanak egy értékhalmazhoz (jobb oldal)
	- a kapcsolódó allekérdezés csak egyetlen oszlopot adhat vissza
- IN
	- Igazat ad vissza, ha a bal oldalán levő érték benne van a jobb oldalán levő halmazban
- ANY (vagy SOME)
	- Igazat ad vissza, ha a jobb oldalán levő halmaz legalább egyik elemére igaz a reláció(operátorokkal használatos)
- ALL
	- Igazat ad vissza, ha a jobb oldalán levő halmaz minden elemére igaz a reláció
#### IN
- azon részlegek nevét és városát, ahol Programmer-ek dolgoznak.
```sql
SELECT  department_name Részlegnév,
   city Város
FROM departments NATURAL JOIN locations
WHERE department_id IN
(SELECT department_id 
FROM employees NATURAL JOIN jobs
WHERE job_title = 'Programmer')
ORDER BY Részlegnév;
```
#### Antijoin
```sql
SELECT * FROM employees
WHERE department_id NOT IN 
(SELECT department_id 
FROM departments 
WHERE location_id = 1700)
ORDER BY last_name;
```
#### ANY
- Listázzuk azon dolgozókat,  akikre igaz, hogy van olyan részlegfőnök, akinél többet keresnek.
```sql
SELECT * FROM employees WHERE salary > ANY
(SELECT salary FROM departments d 
inner join employees e on 
d.manager_id = e.employee_id)
```
#### ALL
- Listázzuk azon dolgozókat, akiknek a fizetése minden részleg átlagfizetésénél nagyobb.
```sql
SELECT * FROM employees
WHERE salary > ALL
(SELECT AVG(salary) FROM employees
GROUP BY department_id)
```
#### EXISTS
- Listázzuk azokat a részlegeket, ahol dolgozik 15.000 dollárnál magasabb fizetésű dolgozó.
```sql
SELECT * FROM departments d
WHERE EXISTS 
(SELECT * FROM employees e WHERE
d.department_id=e.department_id 
AND e.salary > 15000)
ORDER BY department_name;
```
## Halmazműveletek
- A halmazművelet mindkét oldalán egy-egy lekérdezés áll.
- A lekérdezések által visszaadott oszlopoknak egyezniük kell darabszámban és típusban (névben nem).
- UNION
	- Unió: mindazon elemek, amelyek legalább az egyik halmazban megtalálhatók (ismétlődés nélkül)
	- UNION ALL: ismétlődésekkel
- INTERSECT
	- Metszet: mindazon elemek, amelyek mindkét halmazban megtalálhatók
- MINUS
	- Különbség: mindazon elemek, amelyek megtalálhatók az első halmazban, de a másodikban nem.
#### Union
- Mely régió azonosítók találhatók meg a regions vagy a countries táblában?
```sql
SELECT region_id FROM regions
UNION
SELECT region_id FROM countries;
SELECT region_id FROM regions
UNION ALL
SELECT region_id FROM countries;
```
#### Intersect
- Kik azok, akik már töltöttek be másik munkakört (azaz szerepelnek a job_history táblában)?
```sql
SELECT employee_id FROM employees
INTERSECT
SELECT employee_id FROM job_history;
```
#### Minus
- Kik azok, akik részlegfőnökök, de nincs nekik közvetlen beosztottjuk?
```sql
SELECT manager_id FROM departments
MINUS
SELECT manager_id FROM employees;
```
## ROWNUM
- A lekérdezésekben elérhető pszeudo-oszlop, amely a rekord "sorszáma" a lekérdezésben (NEM a táblában!)
- A WHERE feltétel kiértékelésekor kerül kiosztásra, csoportosítás és rendezés előtt.
- MIUTÁN kiosztásra került egy ROWNUM, azaz találtunk egy rekordot, amire igaz a WHERE, utána fog inkrementálódni, hogy a következő eggyel nagyobb sorszámot kapjon.
- Listázzuk ki a munkakörök nevét és a hozzájuk tartozó maximális fizetéseket, utóbbi szerinti csökkenő sorrendben, az első 3 kivételével.
```sql
SELECT job_title, max_salary FROM jobs
   ORDER BY max_salary;

SELECT al.*, ROWNUM Sorszám FROM
  (SELECT job_title, max_salary FROM jobs
   ORDER BY max_salary)

SELECT job_title, max_salary FROM
 (SELECT al.*, ROWNUM Sorszám FROM
  (SELECT job_title, max_salary FROM jobs
   ORDER BY max_salary) al
  )
WHERE Sorszám>3;
```
## FETCH, OFFSET
- egyszerübb ROWNUM alternativa
- ... FETCH FIRST n ROWS ONLY vagy .. OFFSET n ROWS \[FETCH NEXT m ROWS ONLY\]
```sql
SELECT * FROM employees
ORDER BY salary DESC
FETCH FIRST 5 ROWS ONLY;
```


