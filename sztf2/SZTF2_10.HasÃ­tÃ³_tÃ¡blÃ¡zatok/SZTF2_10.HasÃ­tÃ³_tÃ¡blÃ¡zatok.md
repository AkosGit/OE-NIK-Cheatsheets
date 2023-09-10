---
tags: sztf2
---

# SZTF2: 10.Hasító táblázatok

## Felépítése

### Közvetlen címzés

* A kulcs az index amit kivételesen 0 tól számolunk

### Hasító táblázat

* A hasító táblázat hasonló elveken alapul, azonban bevezetünk még egy új függvényt, ami a kulcsokat leképezi tömb indexekre

### Tökéletes hasító táblázat alapműveletei

* Nincs benne kulcsütközés
* Beszúrás
   ![](Images/SZTF2_10.Hasító_táblázatok_1.png)
* Keresés
   ![](Images/SZTF2_10.Hasító_táblázatok_2.png)
* Törlés
   ![](Images/SZTF2_10.Hasító_táblázatok_3.png)

### Hasító táblázat elemzése

* Előnye
  * Nagy méretű kulcshalmaz esetén is használható lehet
  * Ha a tényleges elemszám kisebb mint a kulcshalmaz mérete, akkor is hatékony memóriafoglalás érhető el vele
* Hátránya
  * Fennáll a lehetőség, hogy két (egyébként különböző) kulcs ugyanoda képződik le, tehát foglalkoznunk kell az esetleges ütközésekkel is
  * Nehéz megtalálni a tökéletes hasítófüggvényt
* A választott hasítófüggvénynek mindig az adott feladatnak megfelelőnek kell lennie, általános megoldásunk nincs
* Azonban megadható néhány általános szabály
  * Egyenletesen ossza szét a kulcsokat
  * Véletlenszerűen ossza szét a hasonló kulcsokat
  * Kevés ütközés produkáljon
  * Egyszerűen kiszámolható legyen

## Tipikus hasító függvények

### Osztó módszer

* h(k) : k mod m
  * k a kulcs, ami csak pozitív egész szám lehet
  * m tetszőleges pozitív egész szám, tömb mérete
* Az egyenletes szétszórás az m megválasztásától függ
* Tipikusan nem ajánlott 2 vagy 10 hatványait választani, mivel akkor az eredmény csak az utolsó néhány helyiértéktől függ
* Célszerű m-nek prímszámot választani

### Szorzó módszer

* h(k) : Közepe^M(Y * k)
  * k a kulcs, ami csak pozitív egész szám lehet
  * Y egy tetszőleges pozitív egész szám
  * Közepe^M(x) függvény visszaadja az x középső M darab számjegyét
* A paramétert itt is célszerű prímnek választani
* Nevezetes megoldás a négyzetközép módszer
  * h(k) : Közepe^M(k * k)

### Számjegyes módszer

* h(k) : Helyiértéki (k) mod m
  * k a kulcs, ami csak pozitív egész szám lehet
  * m tetszőleges pozitív egész szám (tömb mérete is egyben)
  * Helyiértéki (x) visszaadja az x szám i. számjegyét
* Hátránya, hogy érzéketlen a számjegyek cseréjére, amely bizonyos kulcsok esetén ütközésekhez vezethet
* Emiatt célszerű lehet súlyozni a helyiértékeket
  * h(k) : Súlyi * Helyiértéki(k) mod m
  * k a kulcs, ami csak pozitív egész szám lehet
  * m tetszőleges pozitív egész szám
  * Súlyi az i. számjegyhez tartozó súlyozó tényező (p. szám)
  * Helyiértéki (x) visszaadja az x szám i. számjegyét

## További megfontolások

### Szöveges típusú kulcsok

* Az eddigi módszerek mind egész szám típusú kulcsot feltételeztek, ez azonban nem mindig elég
* Ha a kulcs karakter típus, valamilyen számot kell hozzárendelni mint az ASCI ban
* Ha nem csak egy karakter, hanem több karakterből álló szöveg, akkor pedig
* Előre definiált kulcsok esetén (pl. természetes nyelv szavai) gyakran ismert a karakterek eloszlása, ilyenkor súlyozással lehet finomítani

### Univerzális hasítási technika

* Az előzőleg megismert módszerek esetén, ha a paraméter előre rögzített, akkor könnyen megállapíthatók olyan kulcsok, amelyek mind ugyanoda képződnek le
* Hatásos módszer, ha a hasító függvényt a kulcsoktól független módon véletlenül választjuk ki
* Ez általános esetben kielégítő eredményt ad, és elkerüli a legrosszabb esetet

### Összetett kulcsok

* Összetett kulcs egy személy esetén pl. a személy neve és a születési dátuma együttesen
* Összetett kulcsok esetén általánosan jól használható módszer nem állapítható meg
* Általában célszerű az összetett kulcsot megfelelő byte-méretű darabokra vágni, majd ezeket a darabokat számként kezelve valamelyik előzőleg ismert transzformációt alkalmazni
* Amennyiben ismert a kulcs felépítése célszerű lehet súlyozni az egyes darabokatv

## Kulcsütközés

### Kulcsütközés

* Kulcsütközésnek tekintjük, ha a kulcstranszformációs-függvény két különböző kulcsú elemet azonos indexre képez
* kulcshalmaz nagyobb, mint az indexhalmaz, akkor az ütközés elkerülhetetlen

### Kulcsütközések kezelése

* Túlcsordulási területtel
  * A hasítótáblázat szerkezetét kiegészítjük egy további memóriaterülettel
  * A kulcsütközés miatt az alap szerkezetbe nem férő elemeket ezen a túlcsordulási területen tároljuk
* Láncolt listával
  * A hasító táblázatban az elemek helyett egy-egy láncolt lista fejet tárolunk
* Nyílt címzéssel
  * A hasító függvényt kiegészítjük egy további paraméterrel, ami lehetőséget ad az elemek elcsúsztatására
  * Ennek megfelelően többszöri próbálkozással más-más helyre próbáljuk meg elhelyezni az elemet

### Beszúrás hasító táblázatba (túlcsordulási területtel)

* Ha az A tömb hasító függvény által megadott helye szabad, akkor ide helyezzük az új elemet
* Ha nem, akkor a túlcsordulási területre
*  ![](Images/SZTF2_10.Hasító_táblázatok_4.png)

### Keresés hasító táblázatban (túlcsordulási területtel)

* Megnézzük, hogy az A tömbben van-e a keresett elem (a „helyén”)
* Ha ott nincs, akkor megvizsgáljuk a túlcsordulási területet
* Ha egyik helyen sincs, akkor hibát jelzünk
*  ![](Images/SZTF2_10.Hasító_táblázatok_5.png)

### Törlés hasító táblázatból (túlcsordulási területtel)

* Ha a keresett elem az A tömbben van, akkor töröljük
* Ha ott nincs, akkor ellenőrizzük a túlcsordulási terüeltet is. Ha ott van, akkor töröljük innen
* Ha egyik helyen sincs, akkor hibát jelzünk
*  ![](Images/SZTF2_10.Hasító_táblázatok_6.png)

### Láncolt altáblák

* Ebben az esetben az A tömb nem közvetlenül az eltárolni kívánt elemeket és kulcsokat tartalmazza, hanem csak láncolt listák fejeit
* Célszerű lehet rendezni a láncolt listát hozzáférési gyakoriság szerint
* minden müveletet (törlés,beszurás,keresés) a A[h(kulcs)] kezdetű listában végzünk
*  ![](Images/SZTF2_10.Hasító_táblázatok_7.png)

### Nyílt címzés

* Egy előre definiált szisztematikus üres hely keresést végzünk
  * Tehát ha egy kulcs hasító függvény által megadott helye nem szabad, akkor egy másik helyet keresünk neki (ugyanezt követjük keresés és törlés során)
  * Ezek az úgynevezett kipróbálási stratégiák
* Ehhez bevezetünk egy kétparaméteres hasító függvényt, aminek első paramétere a kulcs, második pedig a próbálkozás száma:
  * Lineáris próba: h(k, j) : (h(k) + zj) mod m
  * Négyzetes próba: h(k, j) : (h(k) + z1j + z2j2) mod m
  * k – a hasítani kívánt kulcs
  * j – az aktuális próbálkozás száma
  * h(k) – egy már ismert hasító függvény
  * m – az osztó módszernél már megismert paraméter
  * z illetve z1 ,z2 – tetszőleges egész konstansok
* A különféle próbák különféle problémákkal járhatnak
  * Lineáris próba: egy területen halmozódó kulcsok rontják a futásidőt
  * Négyzetes próba: kihagyhat szabad helyeket
* Célszerűen bevezetünk az üres () mellett egy törölt (⊗) állapotot is

### Beszúrás hasító táblázatba (nyílt címzéssel)

* Beszúrás művelete
  * Első körben j = 0 értékkel vizsgáljuk meg, hogy szabad-e a hasító függvény által adott hely (tehát üres vagy törölt)
  * Ha nem, akkor növeljük a j paraméter értékét, hátha találunk szabad helyet
  * Ha találtunk szabad helyet, akkor oda felvesszük az új elemet
  * Ha nem (m próbálkozás után ezt valószínűsítjük), akkor hibát jelzünk
*  ![](Images/SZTF2_10.Hasító_táblázatok_8.png)

### Keresés hasító táblázatban (nyílt címzéssel)

* Keresés művelete
  * a beszúráshoz hasonló utat járjuk végig, amíg
  * megtaláljuk a keresett kulcsot → megvan a keresett elem
  * üres helyre lépünk → nincs ilyen kulcs
* törölt elemnél megyünk tovább, hiszen utána még lehet a keresett kulcs
*  ![](Images/SZTF2_10.Hasító_táblázatok_9.png)

### Törlés hasító táblázatból (túlcsordulási területtel)

* Törlés művelete
  * Az első ciklus azonos a keresésnél látottal (hiszen elsőként meg kell találnunk a törlendő elemet egy kereséssel)
  * Ha a keresés eredményes volt, akkor a vizsgált elemet töröljük. Ez ebben az esetben csak a törölt tulajdonság beállítását jelenti
  * Ha a keresés eredményes volt, akkor a vizsgált elemet töröljük. Ez ebben az esetben csak a törölt tulajdonság beállítását jelenti
*  ![](Images/SZTF2_10.Hasító_táblázatok_10.png)

### Többszörös hasítás
* Tulajdonképpen nyiltcimzés egy fajtája, mert ugyan úgy másik helyre próbálja rakni kúlcsütközés esetén
* Használjunk két vagy akár több hasító függvényt (h1 , h2,…, hr ) ! 
* Ezek lehetnek 
  * egymástól teljesen független hasítófüggvények
* pl: hj(k) : KözepeM(Y * k * j * j)
  * k a kulcs, ami csak pozitív egész szám lehet
  * j a hasítófüggvény száma
  * Y, k, Közepe^M a már ismert paraméterek
* Beszúrás:
  * Ha a hi kulcstranszformációs függvény alkalmazásával kulcsütközés lépne fel, próbáljuk a hi+1-el, ami máshova képez (i=1-től indulva)
* Keresés:
  * A beszúráshoz hasonló utat járunk végig itt is
* Törlés:
  * A fenti kereséssel megtalált elemet töröljük


