---
tags: opre
---
# Parancsok és Bash cheatsheet

# Parancsok
## Fájl és mappa müveletek
- fie létrehopzása/editelése: nano filenév
- script mentése: ctr+o
- mappa létrehozása: mkdir mappanév
- navigation, visszalépés: "cd mappanév";  "cd .."
- enviroment változok: env
- path mappák: $PATH
- kiterjesztés nem számit, futtathatóvá kell tenni:
	-  terminálba: chmod a+x <fájl név>
	-  ez után futtatható így: "./fájl név"
- *ls* Mappa tartalmának kiírása
- *mkdir* Mappa létrehozása
- *rmdir* mappa törlése
- *cat* fájl kiirás
- *touch* fájl létrehozás 
- *mv* átnevezés mozgatás
- *rm* egy file törlése, rm -r rukerziv fájl és almappák
- *cp* másolás
## Szövegfeldolgozás
### Keresés fájlban és szövegben
- Keresés fájlban: 
``` bash
grep keresendő_szöveg fájlnév/fájlnevek
```
- hasznos grep kapcsolók:
	- v – negált keresés (azokat a sorokat adja vissza, amiben nem található meg a keresett szöveg) 
	- h – több fájlban kereséskor azt is megadja, hogy melyik fájlban találta meg
	- i – kis- és nagybetűk között nem tesz különbséget 
	- w – egész szóra keres
	- n – a sorszámot is visszaadja
	- P - Perl tipusu regex -> regulátlt, minta alapján kereshetünk benne
	- o - csak azt irja ki amit keresel fontos! -> nem pontos értékre keres, hanem ha benne van a keresési paraméter, akkor a string maradékját levágja
- példa:  .sh -t tartalmazó fájlnevek listázása
``` bash
ls –l | grep '.sh '
```



### Reguláris kifejezések
- grep –E vagy egrep
- Speciális karakterek a következők: * . , [ ] \ ^ $
- ^ sor elejének kell megfelelnie a keresett kifejezéssek
- $ a sor végének kell megfelelnie a keresett kifejezéssek
- Konkrét sor keresése
``` bash
egrep '^Linux scriptelést tanulunk$'
```
- \\< : szó elejének keresése
- \\> : szó végének keresése
- . : bármilyen karakterre keres
- [] : a két szögletes zárójel között levő karakterek valamelyikére keres
- [\-] : karakter tartományra illeszti rá
- A \[\] és a \[ - \] használható egyszerre is: \[a -záéíóöőüű \]



#### Jelentésmódosító jelek
- A jelentésmódosító jelek a már megismert reguláris kifejezésekkel együtt használhatók
- ? : az előtte álló karakter opcionális
- \* : az előtte álló karakterből tetszőleges számút keres (0 is lehet!)
	```bash
	egrep '1[0-9]*'
	``` 
- \+ : az előtte álló karakterből tetszőleges számút keres, de legalább egyet
- {} : az előtte álló karakterből a kapcsoszárójelben megadott számút keres
- () : karakterek csoportosítására szolgál, így a jelentésmódosító arra a csoportra, karakterláncra vonatkozik, nem egy karakterre
- | : vagy kapcsolat
## Sed
- A sed egy parancssoros szövegszerkesztő program
- A bemeneti szöveget soronként dolgozza fel, az adott sort a mintatérbe olvas be és úgy hajtja végre rajta az utasítás(oka)t
- Feldolgozás után a mintatérben levő sort kiírja
- A kimeneti fájl ne egyezzen meg a bemeneti fájlal!
	```bash
	sed 'utasítás' bemeneti_fájl > kimeneti_fájl
	//vagy
	cat bemeneti_fájl | sed 'utasítás' > kimeneti_fájl
	kimeneti_fájl pl: sed 'p' datum.sh 
	(duplázva soronként kiírja a kódot) 
	pl: sed -n 'p' datum.sh 
	(soronként kiírja a kódot) 
	pl: sed -n '3 p' datum.sh 
	(a harmadik sort írja ki) 
	pl: sed -n '2,4 p' datum.sh 
	(2-4 sorokat írja ki) 
	pl: sed '4 d' datum.sh 
	(törli a 4. sort) 
	pl: sed 's/exit 0/exit 1/' datum.sh 
	(kicseréli az első a sorban megtalált exit 0-t exit 1-re) 
	pl: sed 's/u/U/g' datum.sh 
	(kicseréli a sor minden u-ját, U-ra)
	```
### Sed utasítások - mintatér kiírása
- p : Mintatér aktuális tartalmának kiírása
	```bash
	sed 'p' datum.sh
	out: #!/bin/sh
	```
- Minden sor kétszer jelenik meg, mert egyszer lefut a parancs (ami a kiírás), ezt követően pedig a parancs végrehajtása után a sed kiírja a mintatér tartalmát
- utasítás végrehajtása után ne írja ki az aktuális mintateret, használhatjuk az -n kapcsolót
- Ezen felül megadható, hogy mely sorokon futtassa a parancsot
- p : A \#-edik soron hajtja végre az utasítást
	``` bash
	sed -n '3 p' datum.sh
	```
- #,# p : A két # közötti sorokon hajtja végre az utasítást
	``` bash
	sed -n '2,4 p' datum.sh
	```
- d : Mintatér aktuális tartalmának törlése
	```bash
	sed '4 d' datum.sh
	```
- s : A megadott reguláris kifejezést cseréli a megadott szövegre, szintaktikája: s/mit/mire/
- Ha azt szeretnénk, hogy a sorban ne csak az első találatot cserélje ki, hanem az összeset, akkor azt a g utasítással tudjuk megtenni a következő szintaktika szerint: s/mit/mire/g
- \-i input file megadása
## Date
```bash
date +"%H:%M:%S"
%m: Displays the month of year (01 to 12).
%y: Displays last two digits of the year(00 to 99).
%Y: Display four-digit year. 
%T: Display the time in 24 hour format as HH:MM:SS.
%H: Display the hour.
%M: Display the minute.
%S: Display the seconds.
```
## cut
- A cut program segítségével a sorokra oszlopokra tudjuk bontani
	```bash
	cut kapcsolók bemeneti_fájl
	```
- Kapcsolói: 
	- d – az elválasztó karakter megadása
	- f – a megjelenítendő oszlop száma	
## wc
- A wc megszámolja a bemenetként adott fájlban levő sorokat, szavakat és karaktereket
	```bash
	wc bemeneti_fájl
	wc -l : Prints the number 
	of lines in a file.
	wc -w : prints the number 
	of words in a file.
	wc -c : Displays the count 
	of bytes in a file.
	wc -m : prints the count 
	of characters from a file.
	wc -L : prints only the length of
	the longest line in a file.
	```
## head és tail
- head : A bemenetként megadott fájl első 10 sorát írja ki
	```bash
	head bemeneti_fájl 
	```
- A –n kapcsolóval megadható, hogy az első 10 helyett az első X sort írja ki
- tail : A bemenetként megadott fájl utolsó 10 sorát írja ki
	```bash
	tail bemeneti_fájl
	```
- A –n kapcsolóval megadható, hogy az utolsó 10 helyett az utolsó X sort írja ki


## tac
- A tac a bemenetként kapott fájl sorait fordított sorrendben írja ki (utolsó sor lesz az első)
	```bash
	tac bemeneti_fájl
	```
## Tömörítés
- tar – fájlok összecsomagolására szolgál
- gzip – fájlok tömörítésére szolgál
- Ha a fájl kiterjesztése tar.gz, akkor csomagolva és tömörítve is van
- tar segítségével összecsomagolhatjuk, majd átadhatjuk a gzip-nek tömörítésre
- Becsomagolás:
	```bash
	tar –cvzf cél_fájlnév forrás_fájlnevek
	```
	- c: compress 
	- v: verbose
	- z: gzip
	- A forrás fájlnevek lehetnek felsorolva vagy ha az adott mappa tartalmát szeretnénk tömöríteni, akkor használhatunk *-ot is
- Kicsomagolás:
	```bash
	tar –xvzf forrás_fájlnév	
	```
	- x: extract



## Fájlok és mappák keresése
### find
- A find segítségével fájlokat és mappákat kereshetünk 
- Megadhatjuk a kiindulási mappát 
- Továbbá azt is megadhatjuk, hogy hajtson végre bizonyos parancsokat a találatokon
	```bash
	find kezdő_könyvtár mit_keresünk mit_hajtson_végre
	```
- A find fontosabb kapcsolói: abban a mappában keres amiben éppen vagyunk
	- name – fájl vagy könyvtár neve
	- type – típus (f: file, d: mappa)  -> ilyen típust fopg keresni
	- user - fájl/mappa tulajdonosa
- exec – parancs futtatása a fájlon
	- Az exec kapcsoló használata:
	- exec parancs {} \; 
	- {} – ennek a helyére jön az aktuális fájlnév
	- \; – ezzel zárjuk le a parancsot
    ```bash
    find path -name "szar" -exec cat {} \;    
    ```
### wget - Fájlok letöltése
- A wget egy letöltőprogram, melyel a hálózatról tudunk letölteni fájlokat HTTP-n, HTTPS-en és FTP-n keresztül
	```bash 
	wget elérési_út
	```

# Bash scriptelés

## konzol
 - kilépés konzol programból: CTRL + c
 ### átirányitás
 - Lehetőség van viszont eltérni az előre meghatározott kimenetekről (Standard és Standard Error Output-ok). Ha szeretnénk, akkor átirányíthatjuk a kimentet:
	  - Például ls –l /etc/network > lista . Ez esetben a mappatartalom nem íródik ki a képernyőre, hanem egy fájlba íródik be. Fontos, hogy ez az operátor felülírja a fájl tartalmát!
	  - \>\> operátor szintén Standard Outputot irányítja át, azzal változással, hogy hozzáfűz, nem felülír
	  - 2\> operátorral a Standard Error Outputot irányíthatjuk át
### Parancsösszefűzés
- Parancs kimenetét más parancsok bemenetére is átirányíthatjuk, így parancsokat fűzhetünk egybe. 
-  ls -h | less : ez esetben a ls –h kimentét, a hosszú súgó szöveget átadtuk a less parancs bemenetének, aminek a feladata a lapozhatóvá tétel. Így a less parancs kimenetét, a lapozható szöveget láthatjuk a less parancs kimenetén, a monitoron.

- **interpreter választása script elejére: #!/bin/sh**
- exit 0 – nem volt hiba a script lefutása közben
- exit 1 – hiba volt a script lefutása közben

- változóra való hivatkozás: $változó
	```bash
	szoveg=”egy tál húsleves”
	echo $szoveg
	```
- érték beolvasás: read érték
	```bash
	#!/bin/sh 
	echo ”Mi az ön neve? ” 
	read nev 
	echo ”Hello” $nev 
	exit 0
	```
### Beépített változók
- $? – az előző parancs vagy script lefutásának eredménye
- $# – a megadott paraméterek száma
- $1 , $2 , … , $9 – az n.-edik paraméter értéke -> futtatáskor megadott paraméter
- $0 – az adott script neve
- $* – az összes paraméter egyben (egy stringként)
- $@ – az összes paraméter egyben (egy tömbként)
### Idézőjelek, parancs behelyettesítés
- ’ ’ – az aposztrófok közötti szöveg string-ként van értelmezve és nem helyettesíti be a változók érétkét!
	```bash
	valtozo=”date”
	echo ’$valtozo’
	kimenet: $valtozo
	```
- ” ” – a hagyományos idézőjelek közötti szöveg string-ként van értelmezve, de behelyettesíti a változók értékét!
	```bash
	valtozo=”date”
	echo ”$valtozo”
	kimenet: date
	```
- \` \` – az alt gr+7 típusú aposztróf közé helyezett szöveg behelyettesítéskor parancsként fut le
	```bash
	valtozo=”date”
	echo `$valtozo`
	kimenet: 2018. aug. 23., csütörtök, 10:46:19 CEST
	```
- A kimenet megegyezik, de a scriptből látható, hogy a különféle idézőjelek egymásba ágyazhatók!
- Ha szöveg + változó behelyetesités
    ```bash
    v="szöveg${var}"
    ```
### Összetett parancs futtatása változóból
```bash
	 parancs=”ls -l /home | sort”
	 eval $parancs
```
### Matematikai műveletek
- A változóknak nincsenek típusai, minden string-ként van tárolva:
	```bash
	eredmeny=3+4
	echo $eredmeny
	kimenet: 3+4
	```
- Szám és változó összeadása
    ```bash
    num=$((num1 + 2 + 3))
    ```
- A matematikai műveletek elvégzésére az expr parancsot használhatjuk:
	```bash
	szam1=3
	szam2=4
	eredmeny=`expr $sz1 + $sz2`
	```
### Elágazások
```bash
if [ logikai_kifejezés1 ]
then
# parancsok, ha a logikai_kifejezés1 igaz
elif [ logikai_kifejezés2 ]
then
# parancsok, ha a logikai_kifejezés2 igaz
else
# parancsok, ha sem a logikai_kifejezés1, sem a
# logikai_kifejezés2 nem volt igaz
fi
```
- Stringek vizsgálata
	```bash
	#Igaz, ha a két string megegyezik
	if [ ”$s1” = ”$s2” ]
    if[ STRING1 == STRING2 ]
    # nem egyenlö
    if[ STRING1 != STRING2 ]
    # STRING1 abc-ben utána van a másiknak
    if[ STRING1 > STRING2 ]
    #STRING1 abc-ben elötte van
    if[ STRING1 < STRING2 ]
    #Nem üres string
    if [ -n NONEMPTYSTRING ]
    #Üres string
    if [ -z EMPTYSTRING ]
    
    
- Fájl létezésének vizsgálata
	```bash
	#Igaz, ha a fájl létezik
	if [ -e fájlnév ]
	#Igaz, ha létezik és olvasható 
	if [ -r fájlnév ]
	#Igaz, ha a fájl létezik és írható 
	if [ -w fájlnév ]
    #ha mappa
    if [ -d directory ]
    # 
	```
- 	Egész számok vizsgálata
	```bash
	#Igaz, ha két szám egyenlő
	if [ $sz1 –eq $sz2 ]
	#Igaz, ha két szám nem egyenlő
	if [ $sz1 –ne $sz2 ]
    #NUM1 nagyobb
    if[ NUM1 -gt NUM2 ]
    #Nagyobb vagy egyenlö
    if[ NUM1 -ge NUM2 ]
    #NUM1 kissebb
    if[ NUM1 -lt NUM2 ]
    #NUM1 kissebb vagy egyenlöű
    if[ NUM1 -le NUM2 ] 
	```
- Hagyományos szám hasonlitás
    ```bash
    if(( NUM1 == NUM2 ))
    if(( NUM1 != NUM2 ))
    if(( NUM1 > NUM2 ))
    if(( NUM1 >= NUM2 ))
    if(( NUM1 < NUM2 ))
    if(( NUM1 <= NUM2 ))
    ```
- További operátorok:
	- gt, -ge, -lt, -le
	```
    
    

#### Több feltétel
- Legalább az egyik feltételnek teljesülni kell (OR):
	```bash
	if [ logikai_kifejezés1 ] ||
	 [ logikai_kifejezés2 ]
	then
			# logikai_kifejezés2 nem volt igaz
	fi
	```
- Mindkét feltételnek teljesülnie kell (AND):
	```bash	
		if [ logikai_kifejezés1 ] 
		&& [ logikai_kifejezés2 ]
		then
			# mindkét feltétel igaz volt
		fi
	```
### Case feltételes elágazás
```bash
#!/bin/sh 
echo '***********************' 
echo '* Menü *' 
echo '* 1 - Mappa tartalma *' 
echo '* 2 - Aktuális mappa *' 
echo '* 3 - Kilépés *' 
echo '***********************' 
echo 'Kérem válasszon:' 
read valasz 
case "$valasz" in 
	"1") ls -l ;; 
	"2") pwd ;; 
	"3") echo "Viszlát!" ;; 
	*) echo "Csak 1 -3 értékek között válasszon!" ;;     
esac
```
###  Ciklusok
- for ciklus
	```bash
	for ciklusváltozó in lista
	do
	# parancsok
	done
	```
- c tipusu for loop
    ```bash
    for (( c=1; c<=5; c++ ))
    do 
       echo "Welcome $c times"
    done
    ```
- iterálás listán for looppal
    ```bash
    allThreads=(1 2 4 8 16 32 64 128)
    for t in ${allThreads[@]}; do
        echo "$t"
    done
    ```
- Listák for ciklushoz
	```bash
	#Kézzel beírt listák
	for ciklusváltozó in 1 2 3 4 5 6
	#Generált számlista
	for ciklusváltozó in `seq 6`
	#Számokból álló listát generálni a seq paranccsal tudunk
	seq utolsó – 1-től a megadott paraméterig generál listát, 1-el inkrementálva
	seq első utolsó – a megadott paraméterek között generál listát, 1-el inkrementálva
	seq első inkrementum utolsó – a megadott paraméterek között generál listát, a megadott inkrementummal
	#Fájlok listája
	for ciklusváltozó in `ls`
	```
- While ciklus
	```bash
	while [ logikai_kifejezés ];
	do
	# parancsok
	done
	```
    - while kapcsolók
        - "-le":less then
        - "-gt": greater then 
- Until ciklus
	- while viszont addig megy amedig a feltétel hamis 
	```bash
	until [ logikai_kifejezés ];
	do
		# parancsok
	done
	```




 














