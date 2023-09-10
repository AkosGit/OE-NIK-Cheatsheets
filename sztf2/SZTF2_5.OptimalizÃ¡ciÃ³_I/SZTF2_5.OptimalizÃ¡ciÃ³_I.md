---
tags: sztf2
---
# SZTF2: 5.Optimalizáció I

* A megoldandó feladat
  * 0-1 hátizsák probléma
  * Bemenete
    * $W_{max}$: hátizsák mérete
    * $N$: rendelkezésre álló tárgyak száma
    * $w_i$: i. tárgy súlya
    * $p_i$: i. tárgy értéke
  * kimenet
    * $P_{max}$: egy optimális pakolás értéke
  * Pakolás: tárgyanként meghatározzuk, hogy bekerül-e a zsákba vagy sem
  * Pakolás összsúlya/összértéke: a pakolás által a hátizsákba választott tárgyak súlyának/értékének összege
  * Érvényes pakolás: olyan pakolás, amelynek összsúlya nem nagyobb mint a hátizsák mérete
  * Optimális pakolás: legnagyobb összértékü érvényes pakolás

## ==Nyers erő módszere (brute force)==

* ==Alapelv: egyszerűen végigpróbálgatjuk az összes lehetséges megoldást==
* $2^N$: az összes kombináció, mert bináris számok kombinációjának összege csak páros lehet
* Amennyiben érvényes pakolást találunk, akkor ellenőrizzük, hogy jobb-e, mint az eddig talált legjobb
* 
*  ![](Images/SZTF2_5.Optimalizáció_I_1.png)
* Előnyei
  * Jól párhuzamosítható
  * Gyakran csak ez használható
  * Tárhelyigénye nem jelentős
* Hátrányai
  * Futásideje nem kedvező (jelen esetben 2^N lépés)

## ==Oszd meg és uralkodj==

* Prog 1 en már tanultuk
  * Összefésülő rendezés
  * Quickshort
  * K-adik legkisebb elem kiválasztása
* Általában rekurzív megközelítést használ
  * ==Ha a probléma egyszerű akkor ad egy megoldást==
  * ==Ha a probléma túl bonyolult==
    * ==Felosztjuk több kisebb részproblémára==
    * ==A részproblémákat megoldjuk==
    * ==Az ezk után kapott eredmények egyesítése és a nagy feladat megoldása==
* Hátizsák probléma rekurzívan, változók
	* t darab tárgyunk
	* h szabad helyünk(nem db hanem inkább súly)
	* $w_t$: jelenlegi tárgy súlya
	* $p_t$: jelenlegi tárgy értéke
* Rekurzívan az optimális pakolás értéke:
  * ha a tárgyak vagy a szabad hely 0 akkor  a pakolás értéke 0
  * ha az utolsó tárgy nem fér be a zsákba akkor az optimális pakolás értéke annyi mint amennyi az előtte elérhető optimális érték ugyanekkora helyen
  * ha befér, akkor az alábbiak közül a nagyobbik érték
    * az előtte lévő tárgyakkal elérhető optimális érték ugyanekkora helyen
    * az előtte lévő tárgyakkal elérhető érték az utolsó tárgy méretével csökkentett helyen + az utolsó tárgy értékbe
* Mit csinál magyarúl
	* ha tárgyak száma 0 vagy a helyek száma 0.
	* ha $P_nem$: kiszámoljuk a rendezést ha ezt a tárgyat nem veszük bele:
		* hivásnál csökkentjük a tárgyak számát de a jelenlegi tárgy súlyát vonjuk le
	* $P_igen$: bele rakjuk a jelenlegi tárgyat
		* hivásnál csökkentjük a tárgyak számát és levonjuk a jelenlegi tárgy súlyát
		* és ehezz hozzáadjuk a jelenlegi tárgy értékét
	* 
* Kód:
   ![](Images/SZTF2_5.Optimalizáció_I_2.png)
* Main:
   ![](Images/SZTF2_5.Optimalizáció_I_3.png)
* Előnyei:
  * A keresés jobban irányítható mint az előző esetben, nem vizsgál meg eleve reménytelen útvonalakat
  * Gyakran nagyon jól párhuzamosítható
  * A lépésszáma alacsonyabb lehet mint a nyers erő módszer esetén
* Hátrányai:
  * Csak bizonyos szerkezetű megoldásoknál hatékony ez a technika
  * Amennyiben az egyes részproblémák egymást átfedik, akkor nagyon sok felesleges számítást végez. Lásd Fibonacci számok rekurzív számítása

## ==Feljegyzéses módszer (Memorization)==

* ==Az oszd meg és uralkodj müködését úgy képzeljük el hogy a feladatot részfeladatokra bontja viszont, valóságban ugyan azokat a részfeladatokat töbször is megoldja ezért ezeknek a megoldásait fell kell jegyezni==
*  ![](Images/SZTF2_5.Optimalizáció_I_4.png)
* Előnyei
  * Jelentősen tudja csökkenteni a lépésszámot, ha sok egymást átfedő részproblémát kell megoldani
  * Egyszerűen implementálható
* Hátrányai
  * jobb futásidőt kapunk nagyobb tárhelyért cserébe
- Mit csinál magyarúl
	- A rekurziv hivás megtörtént a végén elmentjük az eredményt a jelenlegi t,h paraméterre
	- Az elején megvizsgáljuk hogy a jelenlegi p,t paraméterre volt e ereményünk-e már
## ==Dinamikus programozás==

* A gyakorlatban a rekurzív függvény először csak hívogatja magát, amíg el nem jut valamelyik triviális megoldásig. Ezt követően ez alapján elkezdi el felépíteni a teljes megoldást
* A „feljegyzéses módszer” esetében ez jól látható részmegoldás tárolóba először a legegyszerűbb részproblémák kerülnek bele, majd ezek alapján épülnek fel a komplexebb részproblémák eredményei
* ==Aluról felfelé oldja meg a problémát, Egy tárhelyen tároljuk el a triviális megoldásokat, majd ezek alapján kezdjük el felépíteni az egyre összetettebbeket==
* Hátizsákpakolási feladat megoldása
  * Az algoritmus egy T tömböt tölt fel
    * ennek mérete (N+1) × (Wmax+1)
    * T[t,h] értéke azt mutatja, hogy az első t darab tárgy elhelyezése h szabad helyre milyen optimális pakolási összértékkel járhat
  * A feltöltés menete
    * ha nincs szabad hely, az érték 0
    * ha nincs tárgy, az érték 0
    * Ezt követően a már megismert képlettel feltöltjük a többi elemet
* $p_t$ : adott elem értéke
* $w_t$ : adott elem értéke
* A feltöltött táblázat T[N, Wmax] eleme tartalmazza a teljes feladat megoldását (tehát egy optimális pakolás értékét)
*  ![](Images/SZTF2_5.Optimalizáció_I_5.png)
* a tömböket kivételesen 0-tól indexeljük
* Egy optimális pakolást állít elő az alábbi algoritmus:
*  ![](Images/SZTF2_5.Optimalizáció_I_6.png)
* Ha F[t,h] = F[t-1,h] Ha a két érték nem egyezik, akkor a t. elem benne van a zsákban
* Ezt követően vizsgáljuk a megelőző tárgyakból elérhető optimális pakolást
* A „dinamikus programozás” módszere általánosan
  * A módszer hatékony használatának előkövetelményei:
    * A feladat legyen optimális részstruktúrájú
    * a részfeladatokra bontás során többször is merüljön fel ugyanannak a részfeladatnak a megoldása
* Előnyei
  * Futásideje általában kedvező
  * Nincs szükség egy részprobléma többszöri megoldására
  * Nem rekurzív
  * Minden, az előfeltételeknek megfelelő feladat megoldható vele
  * Tárigénye és lépésszáma előre tudható
* Hátrányai
  * Az alulról felfelé való megoldás miatt néha felesleges részproblémákat is megold
  * A többi megoldáshoz viszonyítva a tárigénye magasabb lehet
* A tárigény csökkenthető, ha a már szükségtelen részmegoldások eredményeit töröljük

## ==Mohó algoritmusok==

* Mohó algoritmus:
  * ==a feladat megoldása során felmerülő részproblémák megoldásakor mindig az aktuálisan legjobbnak tűnő reszeredményt adja vissza==
* Lejobbnak tűnő:
  * a rendelkezésre álló információk alapján próbálja megbecsülni, hogy mit érdemes választani. Ebből adódóan a futásideje általában nagyon jó
* A mohó stratégia az alábbi két előfeltételt igényli
  * ==Optimális reszproblémák:==
    * ==az optimális megoldás felépíthető optimális részproblémák optimális megoldásából==
  * ==Mohó választás :==
    * ==a globális optimális megoldás elérhető lokális optimumk választásán keresztül==
* Mohó algoritmusok típusai
  * Az előzőknek megfelelő problémák esetén mindig optimális megoldást ad
  * Bizonyos feladatoknál nem mindig ad optimális megoldást, csak egy közel optimálisat
* Egy közel optimális pakolást állít elő az alábbi algorutmus
   ![](Images/SZTF2_5.Optimalizáció_I_7.png)
* ==Először rendezzük a tárgyakat valamilyen szempont szerint==, pl
  * Súly szerint növekvő sorrend
  * Érték szerint csökkenő sorrend
  * Érték/Súly szerint csökkenő sorrend
* ==Ezt követően a fenti sorrendben kezdjük vizsgálni a tárgyakat==
  * ==Ha még belefér a zsákba akkor belerakjuk, egyébként nem==
  * Megállunk ha tele a zsák, vagy elfogytak a tárgyak
* Előnyei:
  * Futási ideje nagyon jó (gyakran lináris)
  * Ha nem ad optimális értéket akkor is egy közel optimálisat ad
* Hátránya:
  * csak a feladatok egy szűk körénél használható


