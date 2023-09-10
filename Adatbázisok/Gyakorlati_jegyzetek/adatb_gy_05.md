## DML és DDL utasítások
- DML: Data Manipulation Language    
	- **sorok** beszúrása, módosítása, törlése
-   DDL: Data Definition Language
	- **táblák, nézetek** létrehozása, módosítása, törlése
- SEQUENCE
	- CACHE: Specify how many values of the sequence the database preallocates and keeps in memory for faster access. 
	- NOCACHE: Specify NOCACHE to indicate that values of the sequence are not preallocated. If you omit both CACHE and NOCACHE, the database caches 20 sequence numbers by default.
	-  adatbázis objektum, növekvő egész számok generálásához, segítségével egyedi, elsődleges kulcs értéket lehet generálni
	-  Az első hivatkozás a SEQ_TAB.nextval értékére 1-et ad vissza, a második hivatkozás 2-t. Minden egyes későbbi hivatkozás értéke:  az előző érték + 1.
		```sql
		Create sequence SEQ_TAB
		Minvalue 1
		Maxvalue 99999999
		Start with 1
		Increment by 1
		nocache;
		Select SEQ_TAB.nextval from DUAL;
		```
	- elsődleges kulcs értékek generálására használható a szekvencia:
	```sql
	Create sequence SEQ_TAB
	Minvalue 1
	Maxvalue 99999999
	Start with 1
	Increment by 1
	nocache;

	Create table TAB(id number PRIMARY KEY,
	name 	varchar2(16) not null);
	Select SEQ_TAB.nextval from DUAL;
	Insert into TAB (id, name) 
	VALUES (SEQ_TAB.NEXTVAL, 'CBA'); 
	```
	
- tábla kiirása
```sql
DESCRIBE TABLE_NAME
```
### INSERT
```sql
INSERT INTO TABLE_NAME (column1, column2, column3,…columnN)
VALUES (value1, value2, value3,…valueN);
```
- TO_DATE("datum","formatum")
- Vagy
```sql
INSERT INTO TABLE_NAME VALUES (value1,value2,value3,…valueN);
```
### UPDATE
```sql
UPDATE table_name
SET column1 = value1, column2 = value2…., columnN = valueN
WHERE [condition];
```
 - feltétel nem kötelezö

### DELETE
```sql
DELETE FROM table_name
[WHERE condition];
```
- összes rekord törlése:
```sql
DELETE FROM table_name
```
# Nézetek
-   View-k
-   Logikailag: egy vagy több tábla adatainak egy részhalmaza.
-   Gyakorlatilag: egy lekérdezést „mentünk és úgy használjuk, mintha tábla lenne”.
### Nézet létrehozása
```sql
CREATE VIEW empv1 AS  
  SELECT employee_id, last_name, job_id
  FROM employees 
 WHERE department_id = 50;
```
### Nézet módosítása
- Az ALTER VIEW nem használható a nézet definíciójának módosítására (csak a kapcsolódó megszorítások módosítására).
- 
```sql
CREATE OR REPLACE VIEW empv2 AS  
SELECT employee_id, first_name,
  last_name, job_id
FROM employees 
WHERE department_id BETWEEN 10 AND 50;
```
### Nézet törlése
- Az adatok megmaradnak!
```sql
DROP VIEW view_nevundefined##;
```
### DML nézeten keresztül
-   Bizonyos esetekben lehetséges nézeten keresztül beszúrni, módosítani és törölni
	-   kivéve ha a nézetet WITH READ ONLY opcióval hoztuk létre
	```sql
	create or replace view ketto as 
	select employee_id, last_name, 
	job_id, salary, 	department_name 
	from employees2 natural join departments2;
	select * from ketto;
	update ketto set salary=salary+1 where
	upper(department_name)='ACCOUNTING';
	select * from employees2;
	```