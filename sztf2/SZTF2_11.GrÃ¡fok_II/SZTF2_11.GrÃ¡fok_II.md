---
tags: sztf2
---
# SZTF2: 11.Gráfok II

## Legrövidebb utak keresése

### Adott csúcsból induló legrövidebb utak

* Súlyozatlan gráfok
  * A szélességi keresés megadta a súlyozatlan gráfokban a legrövidebb utakat(alkotó élek számát)
* Súlyozott gráfok
  * az egyes élekhez súlyokat rendelünk, és azt az utat keressük, ahol az azt alkotó élek súlyainak az összege minimális
  * Megfontolandó
    * Negatív súlyú éleket is tartalmazó gráfok esete
    * Negatív összsúlyú köröket is tartalmazó gráfok esete

### Fokozatos közelítés elve

* A fokozatos közelítés technikája során
  * csúcsokhoz a kiindulópontból eddig talált legrövidebb út hosszát (d-be tároljuk), ami egy felső becslés a végeredményre
  * Minden csúcsnál eltároljuk azt, hogy ezen az úton melyik csúcsból értük el az adott csúcsot (π)
* A közelítés elve
  * Kiinduló állapot
    * A kezdőcsúcs esetében d[start] = 0 és π[start] = semmi
    * A többi csúcshoz még nem találtunk utat, ezért ezeknél a távolság értékeket végtelenre állítjuk, a megelőző csúcsokat pedig semmi-re
  * Közelítés
    * Ha találunk olyan (u, v) élt: d[u] + u.Súly(v) < d[v]
    * akkor találtunk a b csúcshoz egy eddiginél rövidebb utat és módositjuk a d és π értékeket
      * hossz: d[v] <- d[u] + u.Súly(v)
      * megelőző csúcs π[v] <- u

### Dijkstra algoritmusa
![](Images/SZTF2_11.Gráfok_II_1.png))
### Az algoritmus eredménye

* Az algoritmus eredménye
  - mutatja minden x csúcs esetén az oda vezető legrövidebb út hosszát (vagy végtelen, ha nincs ilyen)
  - mutatja a fenti úton a megelőző csúcsot (kivéve a kiinduló és az el nem ért csúcsokat)
* Tényleges utak előállítása
  * A cél csúcsból elindulva a π-ben található adatok alapján visszafelé lépkedve A kezdőpontig vagy ahol a π semmi, előállíthatjuk az egyes csúcsokba vezető utakat
* Legrövidebb utakat tartalmazó fa előállítása
  * Az algoritmus a kezdőcsúcsból minden elérhető csúcsba előállítja a legrövidebb utat
  * A π értékek alapján előállítható egy olyan fa, amely ezeket az utakat tartalmazza

### További megoldható feladatok

* Egy csúcsból az összes többi csúcsba vezető legrövidebb utak megkeresése
  * Nincsenek negatív élek: Dijkstra algoritmus
  * Lehetnek negatív élek: Bellman-Ford algoritmus (nem tárgyaljuk)
* Egy csúcsba vezető legrövidebb utak problémája
  * Az összes csúcsból a cél csúcsba vezető legrövidebb utakat keressük
  * Élek irányításának megfordításával könnyen megoldható
* Két adott csúcs közötti legrövidebb út problémája
  * A Dijkstra algoritmus megoldja
  * Ennél sokkal jobb megoldásunk nincs
* Összes csúcspár közötti legrövidebb utak keresése
  * A gráf minden u és v csúcsa között megkeressük a legrövidebb utat
  * Lefuttathatjuk a Dijkstra algoritmust minden csúcsból kiindulva
* Irányítatlan gráfok esete
  * Ha nincs negatív él, akkor a Dijkstra használható kis módosítással
  * Negatív él egyben negatív kört is jelent

## Minimális feszítőfa keresése

### Minimális feszítőfa előállítása

* Olyan feszítőfát keresünk, ahol az élek összsúlya minimális
  * Ennek megfelelően a gráf legyen súlyozott
  * A gráf legyen irányítatlan
  * Legyen összefüggő
* Egy gráfnak több minimális feszítőfája is lehet, ezek közül elég az egyiket megtalálnunk

### 

### Feszítőfa növelésének elve

* Feszítőfa növelése során
  * Egy alapállapotból kiindulva, egyesével adjuk hozzá az éleket a leendő feszítőfához
  * Tehát az algoritmus minden állapotában a feszítőfa egy részét tartja nyilván
* A feszítőfa lépésenkénti növelésének elve
  * Kiinduló állapot
    * Az A kezdőértéke legyen egy olyan állapot, ami biztosan a leendő feszítőfa része lesz majd (pl. egy darab csúcs, vagy egy jól megválasztott él)
  * Növelés
    * Ezt követően olyan éleket keresünk, amelyeket hozzáadva az eddigi A állapothoz, továbbra is fennáll a fenti állítás
    * Ezeket biztonságos éleknek nevezzük
    * Egy ilyen élet hozzáadunk az A-hoz
  * Addig ismételjük, amíg A nem feszítőfa
* Biztonságos él megállapítása

### Biztonságos él keresése

* Vágásnak nevezzük a gráf csúcsainak a kettéosztását két csoportba pl. ábrán a zöld vonal bal és jobb oldalán lévő elemekre
* Azt mondjuk, hogy egy él keresztezi a vágást, ha a két végpontja a két különböző csoportba került
   ![](Images/SZTF2_11.Gráfok_II_2.png)
* Egy él könnyű egy vágásban, ha a vágást keresztező élek közül neki van a legkisebb a súlya
* Egy vágás kikerül egy A halmazt, ha az A egyik éle sem keresztezi a vágást

### Prim algoritmusa

 ![](Images/SZTF2_11.Gráfok_II_3.png)

### Kruskal algoritmusa

 ![](Images/SZTF2_11.Gráfok_II_4.png)

### Kruskal segédműveletek

* Halmaz-létrehozás(x)
  * Létrehoz egy új halmazt
  * Ebben elhelyezi az x elemet (ami egy csúcs)
* Tartalmazó-halmaz(x)
  * Az előzőleg létrehozott halmazok közül megkeresi azt, amelyikben az x szerepel
  * Csak egy ilyen halmaz lehet, a visszatérési értéke ez a halmaz
* Halmaz-összevonás
  * Az előzőleg létrehozott halmazok közül összevon kettőt
  * Tehát pl. az A legyen a két halmaz uniója, a B-t pedig megszünteti
* Megvalósítás
  * Tényleges halmaz adatszerkezetekkel
    * Objektumorientáltan (halmaz objektumok, illetve halmazok halmazai)
    * A halmazok maximális száma és maximális elemszáma ismert (mind a kettő értéke |V|), így tömbök egyszerűen használhatók
    * Láncolt lista az összevonásnál praktikus
  * Másik megoldás, ha halmazok helyett csak a csúcsokhoz rendelünk egy azonosítót, hogy melyik halmazban van (összevonás csak ennek a másolása)


