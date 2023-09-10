---
tags: sztf2
---

# SZTF2: 13.Programozási Paradigmák

## Paradigmák jellemzői

### Programozási paradigma

* Nincs egyértelmű programozási paradigma-nyelv kapcsolat
* Programozási nyelvek (számítási modellek) osztályozása azok jellemzői alapján, amely meghatározza
  * Az alapvető szintaktikát (jelölésmódot)
  * Az adatok és műveletek felépítésének módját
  * A feldolgozás módját
* Választási szempontjai
  * Feladat jellege
  * Hardver nyújtotta lehetőségek
  * Kor aktuális trendjei

### Absztrakciós sziint

* fogalom
  * A gépi megvalósítás részleteitől vett „távolság”
  * Relatív fogalom, csak összehasonlításként értelmezhető
* Trend: egyre magasabb szint elérése
  * Technikai fejlettség egyre többet enged meg
  * Szoftverfejlesztés hatékonysága is ezt igényli (hibatűrés, karbantarthatóság) és a fejlesztés sebessége is gyorsabb
* Hatékonyság kérdése
  * Alacsonyszintű nyelvek
    * Szabad kezet ad
    * A program minőségét a programozó képességei befolyásolják
  * Magasszintű nyelvek
    * Korlátozott funkciókkal bír (feladat orientáltan)
    * A hatékonyságot a végrehajtó motor biztosítja (forditó)
* Magasszintű nyelvek megvalósítása
  * Fordítóprogram használata
  * Interpreter megvalósítás

### Problémaleírás jellege

* Imperatív
  * Megadás módja: hogyan kell megoldani?
  * Program tipikus felépítése
    * Utasítások és parancsok
    * Vezérlési szerkezetek
    * Főprogram (belépési pont)
  * Előnyei
    * Egyszerű/gépközeli megvalósítás (Neumann architektúra)
    * Programkészítés szabadsága
* Deklaratív
  * Megadás módja: mit kell megoldani?
  * Program tipikus felépítése
    * Típusok, szabályok, függvények definíciója
    * Ismert tények/adatok meghatározása
    * Elvárt eredmény/probléma specifikálása
  * Előnyei
    * Általában magasabb absztrakciós szint
    * Optimalizáció/párhuzamosítás lehetősége

### Végrehajtás folyamata

* Közvetlen vezérlésű végrehajtás
  * A végrehajtási sorrendet az utasítások sorrendje határozza meg
  * Tipikusan imperatív nyelvekre jellemző
  * Automatikusan nem optimalizálható (párhuzamos végrehajtás)
* Adatvezérelt végrehajtás (mohó kiértékelés)
  * Egy-egy művelet azonnal végrehajtódik, ha a művelethez szükséges adatok rendelkezésre állnak
  * Ha nincsenek mellékhatások, akkor jól párhuzamosítható
  * Egyszerűen megvalósítható
* Igényvezérelt végrehajtás (lusta kiértékelés)
  * Hatékony mivel a műveletek csak akkor hajtódnak végre, ha azok a végeredmény eléréséhez szükségessé válnak
  * Lehetővé válik a szimbolikus végrehajtás, és ebből adódó optimalizálás??????????
  * Komplex, nagy memóriaigénnyel járhat

### Számítási modellek „versenye”

* Számítási modelleken alapuló architektúrák megjelenése
*  ![](Images/SZTF2_13.Programozási_Paradigmák_1.png)

### Másodlagos paradigmák

* Eseményvezérelt paradigma
  * Események váltódnak ki
  * Ezeket eseménykezelők fogadják
  * Megfelelő interfészen keresztül kommunikálnak egymással
  * Minden egyéb paradigmához kapcsolódhat
* Generikus programozás
  * Algoritmus írása konkrét típusok nélkül
  * Modern OOP nyelvek is támogatják
* Metaprogramozás
  * Programkészítés magas absztrakciós szintje
  * Maga a programkód is változhat futás közben

## Imperatív paradigmák

### Imperatív paradigmák általános jellemzői

* Problémaleírás imperatív
  * Egy algoritmust adunk meg utasítások sorozatával
  * Tehát a program magát a megoldás menetét tartalmazza
* Számítások alapelemei az adatok és a rajtuk végzett műveletek
  * Nevesített adatelemeket változónak nevezzük
  * Meghatározott memória vagy regiszterhelyekben helyezkednek el
  * A műveletek ezen változók értékét módosítják
* Végrehajtás állapotátmenet szemantikát követ
  * A végrehajtás során az aktuális állapotot az összes deklarált változó (és további technikai adatok) aktuális értéke határozza meg
  * A program futása során a rendszer egyik állapotból a másikba lép át
* Absztrakciós szint
  * Tipikusan alacsony
  * Mai nyelvek nagy osztálykönyvtárai ezt elfedik

### Imperatív paradigma korlátai

* Mellékhatások megjelenése
  * Az imperatív nyelvek megengedik a többszöri értékadást
  * Változók aktuális értéke emiatt múltérzékeny (ugyanaz a változó különböző pillanatokban más-más értéket tartalmazhat)
  * Ez mellékhatásokhoz vezethet
* Párhuzamosítás lehetősége
  * Alapvetően soros végrehajtást feltételez
  * Kiegészíthető a párhuzamosan futtatható részek megjelölésének lehetőségével, ez azonban nem automatizálható
    * Kommunikáció biztosítása
    * Szinkronizáció biztosítása
    * Adatmegosztás biztosítása
* Paradigma előnyei
  * Legrégebbi és legelterjedtebb (egyszerű fordítók)
  * Általános számítási feladatokra nagyon jól használható
  * Könnyen és hatékonyan implementálható
  * Neumann-féle számítógép közvetlenül támogatja

### Alacsony szintű nyelvek

* Imperatív megvalósítás
* Gépi kód
  * Adott processzor utasításrendszeréhez igazodik
  * Emberi szemmel nézve csak számok sorozata
  * Előnyei
    * A lehető leghatékonyabb (memória, futásidő, stb.)
    * A hardver minden lehetősége kiaknázható segítségével
  * Hátrányai
    * Minden más (karbantarthatóság, fejlesztés sebessége, stb.)
* Assembly
  * Ugyanaz mint a gépi kód, csak már szöveges utasításokat használ
  * Számos magas szintű funkció (memóriacím helyett címkék, stb.)
  * Még mindig a megadott processzorhoz tartozó utasítások
  * Szükség van valamilyen fordítóprogramra
  * Előnyei
    * A gépi kód minden előnye elérhető itt is
    * Sokkal barátságosabb a fejlesztő számára

### Procedurális programozási paradigma

* Ismétlődő feladatok kezelése
  * Eljárások és függvények (szubrutinok) megjelenése
  * Minden ilyen alprogramnak jól meghatározott feladata van
  * A program folyamatában így megjelenhetnek függvényhívások is
  * Ezzel együtt jár számos technikai jellemző
    * Paraméterek megjelenése
    * Függvény visszatérési értékének megjelenése
    * Verem kezelése
* Program felépítése
  * Megjelenik egy kiemelt „főprogram”, ez indul el a program indításakor
* Programozási stílus ennek megfelelően változik
  * Program felbontása kisebb alkotóelemekre
  * Felülről lefelé bontás
  * Moduláris programozás lehetőségei
* Strukturált és procedurális paradigma
  * A két paradigma tipikusan együtt jár
  * Alapelveik mai napig elfogadottak és számos más paradigmában megjelennek

### Strukturált programozási paradigma

* Cél: nagyobb, áttekinthető programok készítése
* Program felépítése
  * Elemi utasítások
  * Ezeket összekapcsoló struktúrák
    * Szekvencia
    * Elágazás
    * Ciklus
* Megjelenik számos elvi újdonság
  * A fenti szerkezetek egymásba ágyazhatóak
  * Változók életciklusa
* Bizonyítható, hogy a fentiek véges számú alkalmazásával megoldható minden kiszámítható probléma (Böhm-Jacopini 1966, Dijkstra 1968)
* Ezt számos utasítás megszegi (goto, return, break, continue)
* A feladat felbontása tipikusan fa szerkezetet eredményez

### Objektumorientált programozási paradigma

* Szükségessége: szoftverkrízis
* Alapvető elemei az objektumok, amelyek egyben tartalmazzák
  * Az adatokat
  * Az ezeken végezhető műveleteket
* Alulról felfelé való felbontást alkalmaz
* A paradigma újdonságai
  * Egységbezárás
  * Öröklődés/Kompozíció
  * – Polimorfizmus
* Magasabb absztrakciós szintet biztosít, ahol a program felépítése:
* Háttérben viszont ugyanúgy imperatív megvalósítást feltételez
* Osztály alapú/prototípus alapú nyelvek
* Előnyei: karbantarthatóság, újrafelhasználhatóság

## Funkcionális programozás

### Funkcionális programozási paradigma

* A program célja egy matematikai függvény kiértékelése
* Problémaleírás deklaratív
  * Típusdefiníciók
  * Függvénydefiníciók sorozata
    * Függvény neve
    * Függvény paraméterei
    * Függvény törzse
  * Egy kiértékelhető kezdő kifejezés
* Nincsenek parancsok
  * Parancsok helyett csak függvények jelenhetnek meg
  * Nincsenek benne változók, így nyilván nem értelmezhetők ciklusok
  * Ezeket rekurzió segítségével lehet helyettesíteni
* Saját típusok
  * Egyszerű egyedi értékek: számok
  * Megszokott adatszerkezetek: rendezett n-esek, listák, halmazok
  * Speciális szerkezetek: végtelen listák, generátorok
  * A típusokat általában meg tudja állapítani a rendszer

### Függvények szerepe

* Számítások alapelemei a függvények és ezek alkalmazásai
  * Függvény lehet nevesített vagy névtelen
  * Névtelen függvények megadásának módja lehet a lambda-kalkulus
    * absztrakció
    * applikáció
* Függvények elsőosztályú entitások
  * Adatokhoz hasonlóan kezelhetők
  * Eltárolhatók változóban
  * Kliens-szerver rendszerben a művelet is átküldhető adatként
* Függvények magasabbrendűek
  * Függvény átadható paraméterként/lehet visszatérési érték
  * Műveleteket lehet velük végezni
* Mintaillesztés
  * Ugyanaz a függvény megjelenhet különböző paraméterekkel
  * Hasonló mint az overloading, de annál többet tud

### Mellékhatások hiánya

* „Változók” használatának korlátai
  * Csak egyszeri értékadás engedélyezett (érték kötése)
  * Tehát egy változónak a program futása során mindig ugyanannyi az értéke
  * Ezért egy változó bármikor helyettesíthető az értékével
* Hivatkozási helyfüggetlenség
  * Ha a változók értéke nem változhat, akkor ez a kifejezésekre is igaz
  * Tehát egy kifejezés/függvény értéke mindig ugyanannyi
    * A program bármely pontján
    * A futás bármelyik pillanatában
* Ez lehetővé tesz egy magas szintű automatikus ptimalizálást
  * Memoization
  * Párhuzamosítás
* Funkcionális nyelvek típusai
  * Tiszta nyelvek: a változóknak nincsenek mellékhatásai
  * Nem tiszta: bizonyos mellékhatások megjelenhetnek
* Időbeli állapotváltozás
  * Többszöri értékadás nincs, de új változókat létre lehet hozni

### Funkcionális nyelvek működése

* Végrehajtás vezérlés módja tipikusan lusta kiértékelés
  * A program tartalmaz egy kezdeti kiértékelendő kifejezést
  * A végrehajtó motor ez alapján értékeli ki a további kifejezéseket
  * A végeredmény a kezdeti kifejezés értéke
* Kiértékelés módja választható
  * Általában a lusta kiértékelés az alapértelmezett
  * Teljesítményokokból kikényszeríthető a mohó kiértékelés is
* Lusta kiértékelés előnyei
  * A kiértékelő motor felépít(het) egy teljes kifejezésgráfot
  * Ez alapján van lehetősége ezt
    * Szimbolikus módszerekkel egyszerűsíteni
    * Kiértékelés során bizonyos részeket elhagyni
* Párhuzamosítási lehetőség
  * Nagyon jól automatizálható
  * Elhozta a funkcionális nyelvek reneszánszát

### Funkcionális nyelvek specialitásai

* Végtelen rekurzió lehetősége
  * Farok-rekurzió: ha a függvényhívás utolsó lépése a rekurzív hívás
  * Ebben az esetben a hívott függvény használhatja a hívó vermét
  * Ezzel elérhető végtelen rekurzió
  * Ez a gyakorlatban jól kihasználható
    * Generátorok
    * Eseménykezelés végtelen ciklusa
* Előnyök
  * Magas absztrakciós szint, kifejező kód
  * Hatékony kiértékelés és optimalizáció (párhuzamosítás)
* Hátrányai
  * Bizonyos feladatokra nem alkalmas
  * Egyszerű dolgok is nehézséget okozhatnak

### Funkcionális programozás példa

* Egy rendezést megvalósító funkcionális program
   ![](Images/SZTF2_13.Programozási_Paradigmák_2.png)

## Logikai programozás

### Logikai programozás paradigma

* Funkcionális paradigma része, ami elsőrendű logikán alapul
* Számítások alapelemei szabályok és tények
  * Klóz: véges számú literál diszjunkciója
  * pl.  ![](Images/SZTF2_13.Programozási_Paradigmák_3.png)
  * Horn-klóz: egy nem negált tagot tartalmazó klóz
  * pl.  ![](Images/SZTF2_13.Programozási_Paradigmák_4.png)
  *  ![](Images/SZTF2_13.Programozási_Paradigmák_5.png)
  * Szabály (feltétellel rendelkező klóz):
    * fej ← test
    *  ![](Images/SZTF2_13.Programozási_Paradigmák_6.png)
  * Tény (feltétel nélküli klóz)
    * következmény ← igaz
    * következmény
* Program erősen korlátos
  * Nincsenek objektumok, sem megszokott adattípusok
  * Elsőrendű logikát csak részben fedi le
  * Ciklus helyett rekurzió
  * Feltétel helyett a szabályok helyes megválasztása

### Probléma meghatározása

* Problémaleírás deklaratív
  * Szabályok felsorolása (problématér leírása)
  * Tények felsorolása
  * Célállítás/kérdés meghatározása
* Célállítás típusai
  * Egy feltevés igaz-e vagy sem?
  * Létezik-e valami ami a feltételnek megfelel?
* Rekurzió támogatása
  * Kifejezések tartalmazhatnak rekurziót
  * Rekurzív típusok is használhatók (lista)
* Nyelvi specialitások
  * Saját nyelvi elemek támogatják a túl mély rekurziók elkerülését
  * Nyelvi elemekkel lehet irányítani a keresés folyamatát

### Következtető motor működése

* Következtető motor a szabályok és tények alapján dönt
  * Levezetési szabályokat alkalmaz amíg nem jut egy végeredményhez
  * Lehetséges kimenetek
    * Pozitív válasz: a célállítás kikövetkeztethető a megadott tényekből
    * Negatív válasz: a kezdeti cél nem következtethető ki
    * Nincs válasz: végtelen ciklus vagy túl nagy a probléma tér
* Tételbizonyítási módszer: **SLD rezolúció**
  * Egyesítés: két kifejezés egyesíthető, ha változóik helyére Prolog??????? kifejezéseket helyezve azok azonossá tehetők
  * Redukció: egy klóz fej és a célsorozat első céljának egyesítése. Ha sikeres, akkor a klóz törzse a cél helyére kerül???
* A végrehajtás vezérlési módja lusta kiértékelés
  * Célállítás közvetlen ellenőrzése
  * Egyesítés és redukció végrehajtása
  * Visszalépéses keresés
    * ha sikeres a helyettesítés, akkor továbblép a következő célra
    * ha nem, akkor visszalép és másik egyesítéssel próbálkozik

### Logikai programozás használhatósága

* Használhatósága
  * Tudás kifejezése az implementáció ismerete nélkül?????
  * Alkalmas a tudás eltárolására konkrét használati eset nélkül
  * Szakértői rendszerek, mesterséges intelligencia
  * Természetes nyelvi feldolgozás
  * Nagy rendszerek esetén nehezen áttekinthető
* Előnyei
  * Sokféle kérdésre tud választ adni
  * A feladat megoldásán túl levezetést is ad
* Konkrét nyelvi megvalósítások
  * Prolog és variánsai
  * Pl. IBM Watson egy része

### Logikai programozás példa

* Rokonsági kapcsolatokon alapuló szabályok
*  ![](Images/SZTF2_13.Programozási_Paradigmák_7.png)

## Adatfolyam programozás

### Adatfolyam-elvű programozási paradigma

* Az adaton van a hangsúly, a műveletek másodlagosak
* Számítások alapelemei az adatok és a hozzájuk rendelt adatműveletek
* Problémaleírás imperatív
* A végrehajtási szemantika az adatfolyam szemantika
  * Adatok kiértékelése rendelkezésre álláskor
  * Adatvezérelt
* A bemenő adatok áthaladnak az adatáramlási diagramon, ennek eredményeképpen jönnek létre a kimenő adatok
   ![](Images/SZTF2_13.Programozási_Paradigmák_8.png)

### Adatfolyam-elvű programozási paradigma

* Program felépítése
  * Bemenet, kimenet
  * Kettő közötti kapcsolatok kialakítása
* Tipikus felhasználási kör
  * Adatgyűjtő eszközök
  * Műszervezérlés
  * Ipari automatizálás
* Jellemzői
  * Jól párhuzamosítható
  * Bizonyos feladatok látványosan leírhatóak segítségével
* Konkrét nyelv
  * LabVIEW


