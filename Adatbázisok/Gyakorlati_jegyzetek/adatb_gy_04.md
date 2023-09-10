# DDL utasítások, megszorítások
## SQL utasítások kategóriái
- DQL (Data Query Language)
	- SELECT
- DDL (Data Definition Language)
	- táblák, nézetek létrehozása, módosítása, törlése
	- CREATE, ALTER, DROP, TRUNCATE, RENAME
- DML (Data Manipulation Language)
	- Data Manipulation Language sorok beszúrása, módosítása, törlése
	- INSERT, UPDATE, DELETE
- DCL (Data Control Language)
	- GRANT, REVOKE
- TCL (Transaction Control Language)
	- SAVEPOINT, COMMIT, ROLLBACK

## Tábla készitése
 - másolással
	```sql
	create table táblanév as
	(select * from 	EMPLOYEES where SALARY>3000;)	
	```
- vagy leirással
	```sql
	create table név
	{
	oszlop adattipus(adat) default("alapértelmezett érték")
	oszlop adattipus(adat) CONSTRAINT megszoritás
	megszoritás
	}
	```
- adattipusok:	
	```sql
	CHAR(n): fix (n karakter) hosszú szöveg 
	VARCHAR2(n): változó, legfeljebb n karakter hosszú zöveg
	VARCHAR(n): a VARCHAR2 adattípussal ekvivalens,de jelentése változhat, ezért VARCHAR2 javasolt
	CLOB: változó, kb. 8 TB méretű szöveg
	NUMBER(n, m): fixpontos szám, max. n decimális számjegy, ebből m tizedesjegy
	NUMBER(n): max. n számjegyes egész, gyanaz, mint 		NUMBER(n, 0)
	NUMBER: lebegőpontos, 38 számjegy pontosság
	DATE: dátum és idő (másodperc pontosság)
	TIMESTAMP(n): dátum és idő (a másodperc n izedesjeggyel, default: n=6)
	BLOB: binary large object, „nyers” bináris adat max. 4 GB)
	```


## Tábla módositás
```sql
alter table táblanév ...
```
-  adat változtatás: 
	```sql
	... MODIFY tul.név value
	```
- oszlop átnevezése
	```sql
	... rename column oszlop to új_név
	```
- oszlop törlése
	```sql
	... drop column oszlop
	```
- oszlop hozzáadása
	```sql
	... add(oszlop_név adat)
	```
- tábla üritése
	```sql
	truncate table név
	```
## Tábla kiirása
-  tábla adatok kiirása:
	```sql
	describe táblanév
	```
- kiirás metaadatokkal:
	```sql
	select * from user_table  
	```
## Constrains
- A rendszer a kényszerek teljesülését minden rekord létrehozásakor, törlésekor vagy módosításakor ellenőrzi
- Tömeges adatmódosításnál (pl. adat-import) sok időt vesz igénybe
- A kényszerek átmenetileg kikapcsolhatók!
## Megszorítás hozzáadása
```sql
	ALTER TABLE táblanév
	ADD CONSTRAINT minimálbér CHECK (salary>2000);
```
	vagy
```sql
	ALTER TABLE táblanév
	MODIFY salary CHECK (salary>1000);
```
- Egyéb ALTER TABLE utasításhoz kapcsolódó megszorítás műveletek:
	- Megszorítás törlése  
		- DROP CONSTRAINT megszorításnév;
	- Megszorítás engedélyezése  
		- ENABLE CONSTRAINT megszorításnév;
	- Megszorítás tiltása  
		- DISABLE CONSTRAINT megszorításnév;
## Megszorítások lekérdezése
- A beépített user_constraints nézetből lekérdezhetők a megszorítások, azok típusa és állapota.
- Lényeges mezők:
	- constraint_name: a megszorítás neve
	- constraint_type: a megszorítás típusa
	- table_name: melyik táblához kapcsolódik
```sql
SELECT  constraint_name, constraint_type,  table_name
FROM  user_constraints
WHERE  lower(table_name)  IN
('employees', 'departments');
```
## Megszoritások
```sql
CONSTRAINT megszoritas_neve 
megszoritas ....
```
- PRIMARY KEY
	```sql
	department_id NUMBER(4),
	CONSTRAINT d_pk
	PRIMARY KEY (department_id)
	```
	```sql
	department_id NUMBER(4)  
	CONSTRAINT  d_pk PRIMARY KEY,
	```
- UNIQUE
	- ugyan az mint a primary viszont egy vagy több oszlopból álhat és ezek közül csak egy nem lehet NULL
	- minden érték csak egyszer szerepel
	```sql
	email VARCHAR2(25),
	CONSTRAINT emp_email_uk  UNIQUE  (email));
	```
```sql
	email VARCHAR2(25),
	name VARCHAR2(25),
	CONSTRAINT key  UNIQUE  (email,name));
```
- FOREIGN KEY
	```sql
	manager_id NUMBER(6),
	FOREIGN KEY (manager_id)
	REFERENCES employees
	(employee_id)
	```
	- ha egy UNIQUE kulcsra referenciál akkor UNIQUE kulcsot alkotó öszzes oszlopot meg kell adni
	- ON DELETE CASCADE
		- A szülő rekord törlésekor a gyermek rekordok is törlődnek
	- ON DELETE SET NULL
		- A szülő rekord törlésekor a gyermek rekordokban NULL értékre állítódik a szülőre hivatkozó mező.
	```sql
	CONSTRAINT fk_supplier 
	FOREIGN KEY (supplier_id)
	REFERENCES supplier(supplier_id) 
	ON DELETE CASCADE );
	```
- NOT NULL
	- az értéke nem lehet NULL
	```sql
	CONSTRAINT  dept_name_nn3  NOT NULL,
	```
- CHECK
	- az értéknek meg kell felelnie a megadott feltételnek
	```sql
	CONSTRAINT emp_salary_min 
	CHECK (salary > 0) );
	```
## Indexek
- Az indexek az adott mező(k) szerinti keresést és rendezést gyorsító segédobjektumok (nem adattáblák!)
- Azokat a mezőket indexeljük, amik szerint gyakran keresünk vagy rendezünk
- A jó index gyorsítja a lekérdezést
- A kulcs szerint automatikusan készül index is
- A felesleges index lassítja az adatbázist - az indexek karbantartása is időbe kerül!
- Az idegen kulcsokat általában érdemes indexelni (join-okra tekintettel)
- A rendszer az indexeket minden rekord létrehozásakor, törlésekor vagy módosításakor aktualizálja
- Tömeges adatmódosításnál (pl. adat-import) sok időt vesz igénybe
- Sokszor érdemesebb az indexeket törölni, majd az adatmódosítások után újra létrehozni!
```sql
CREATE [UNIQUE] INDEX indexnév  
ON táblanév (mezőnév [sorrend]  
[, mezőnév [sorrend] … ]);
```
### Értékek egyediségének ellenőrzése indexeléssel
- A mezőre UNIQUE indexet készítünk, pl. CREATE UNIQUE INDEX hallg_nev_idx ON hallgato (nev);
- 1:N kapcsolat esetén az idegen kulcshoz sose készítsünk UNIQUE indexet!
### Index törlése
```sql
DROP INDEX hallg_nevsor_idx;
```

