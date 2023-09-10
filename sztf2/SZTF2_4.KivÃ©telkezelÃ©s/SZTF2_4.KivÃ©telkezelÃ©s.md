---
tags: sztf2
---
# SZTF2: 4.Kivételkezelés

## Tesztelés

* ==Tesztelési módszerek két csoportja:==
  * ==Statikus tesztelés – a programot nem futtatjuk==
  * ==Dinamikus tesztelés – a programot futtatjuk==

# ==Statikus tesztelési módszerek==

* ==Kódellenőrzés==
  * Az implementált programkód vizsgálata ==(logika)==
* ==Formai ellenőrzés==
  * ==Szintaktikai hibák kiszűrése==
  * Nem létező kulcsszavak, változók stb. Keresése
* ==Tartalmi ellenőrzés==
  * Nem használt változó, változóérték
  * ==Hiányzó kezdőérték==
  * ==Konstans kifejezések==
  * Visszatérési érték nélküli függvény
  * ==Végtelen ciklusok==
  * Fordítóprogramok ezek jelentős részét szintén jelzik, bár önmagukban nem minősülnek hibának (figyelmeztetések)

# ==Dinamikus tesztelés==

## ==Fekete doboz módszer==

* Fekete doboz módszer = adatvezérelt tesztelés
  * ==Nem veszi figyelembe a program belső szerkezetét==
  * ==Csak a bemenetre és kimentre épit==
* Ekvivalencia osztályok keresése
  * A bemeneti tartományt részekre kell osztani, és ezeken belül jellemző teszteseteket kell választani
  * Tartomány esetén: alatta – közte – fölötte
  * Logikai érték esetén: kielégítő – nem kielégítő feltétel
  * Érvényes és érvénytelen esetekre is kellenek osztályok
  * Egy teszteset lehetőleg minél több bemeneti feltételt elégítsen ki, így kevesebb eset kell
* Határeset elemzés
  * Az ekvivalencia osztályok kiválasztott elemeinek a határértékeket választja

## ==Fehér doboz módszer==

* ==Tesztelést a program ismeretében végezzük el==
  * ==Ismert a pontos implementáció==
  * A módszer három lépését különböző technikák segítik
* ==Kipróbálási stratégiák==
  * Meghatározza, hogy a programgráf melyik útjait kell tesztelni
  * ==Utasítások egyszeri lefedésének elve==
  * ==Döntés lefedés elve (összes if -et kipróbálod)==
  * ==Részfeltétel lefedés elve: minden részfeltéttel lefedése (összes else if -et kiprobálod)==
* Tesztesetek generálása
  * ==Az előzőleg meghatározott utakon végigvezető bemenetek meghatározása==
* Ekvivalencia osztályok megállapítása

## ==Speciális tesztek==

* ==Funkcióteszt==
  * ==Az elkészült program tartalmaz-e minden szükséges funkciót?==
* ==Biztonsági teszt==
  * ==A program ellenőrzi a láthatóságokat==
* ==Stressz teszt==
  * ==Fel tud-e dolgozni nagy sebességgel érkező adatokat?==
  * Alkalmas-e párhuzamosan, több irányból érkező nagymennyiségű adat feldolgozására?
* ==Volumen teszt==
  * ==Alkalmas-e nagy mennyiségű adat feldolgozására?==
  * Ha igen, romlik-e a program hatékonysága?
* ==Modulteszt, összeépítési teszt==
  * ==A program önálló moduljainak egymástól független ellenőrzése==
  * Az önállóan már letesztelt modulok összeépítése utáni teszt

## Hibakeresés

### Hibakeresés és javítás

* ==Hibakeresés: A tesztelés során feltárt hibák helyének megállapítása.==
  * A fejlesztői környezetek általában nagy segítséget nyujtanak a hibakereséshez és nyomonkövetéshez
  * A hibák száma általában a program méretével együtt nő.
* ==Tipikus hibák==
  * ==Szintaktikai hiba==
  * ==Végrehajtás közbeni hiba==
  * ==Program nem áll meg==
  * ==Megáll de nem ad eredményt==
  * ==Hibás eredményt ad==
* ==Hibajavítás: A hibakeresés során megtalált hibák javítása==
  * Ki kell javitani nem csak a tüneteit megszüntni
  * ==Hibajavítás után ujabb tesztelés kell==
  * Hibajavítás visszanyúlhat a fejlesztés előző fázisába

### ==Indukciós, dedukciós módszer==

* ==Indukció: a minta azonos nemű elemeinek egy közös tulajdonságából következtethetünk arra, hogy minden ilyen nemű elem rendelkezik ezzel a tulajdonsággal==
  * A rendelkezésre álló teszteseteket rendszerezni kell aszerint, hogy melyik okozott hibát és melyek nem
  * Fel kell állítani egy feltételezést, hogy milyen tulajdonságú elemek tartoznak az egyik ill. másik csoportba (amennyiben szükséges, további tesztekkel ez megerősíthető)
  * Ha már egyértelmű a hibát okozó tulajdonság, a programon átvezető tesztutak közül ki kell válogatni azokat, amelyek csak az ilyen esetekben futnak le
  * Ezeken a pontokon célszerű keresni a hiba okát
* ==Dedukció: ha minden azonos neműről tudjuk, hogy rendelkezik egy tulajdonsággal, akkor ez az egyedre is igaz==
  * Össze kell gyűjteni az összes feltltelezhető hibaokot
  * A minta elemei alapján ki kell zárni azokat, amelyek nem valósak
  * Az így megtalált okok után a folytatás a hasonló az előzőhözh
* ==Visszalépéses technika==
  * ==A hiba egyik előfordulási helyéből kell kiindulni. Ebből a pontból a programot „visszafelé végrehajtva” kell megkeresni a rendellenességet okozó pontot==
  * Amennyiben a visszafelé görgetett hibás részeredmény helyett egy helyes részeredmény jelenik meg, valószínűleg a hibás sor volt az utolsó lépés
  * Gyakran fejlesztői eszközök segítik ezt a módszert
* ==Teszteléssel segített hibakeresés==
  * ==Úgy kell megválasztani az egymást követő teszteseteket, hogy a teljes programgráfban bejárt út csak egy-egy elemi  változással járjon==
  * Amennyiben egy ilyen elemi lépés távolságban lévő teszteset pár egyik tagja hibás, a másik helyes működést eredményez, szintén lehet következtetni a hiba helyére
* Fejlesztőeszközök által nyújtott funkciók
  * Szintaktikai ellenőrzés
  * Program lépésenkénti futtatása
  * Változók értékeinek nyomon követése
  * Töréspontok elhelyezése a programkódban
  * Adatok értékeire vonatkozó töréspontok
  * Hívási verem megtekintése
  * Változók értékeinek menet közbeni megváltoztatása
  * Vezérlés aktuális pontjának megváltoztatása
  * Programkód futás közbeni módosítása
  * Egyéb megoldások
* Programozási nyelvek által nyújtott hibakezelési lehetőségek
  * Tipikusan a napjainkban elterjedt kivételkezelés

## ==Kivételkezelés==

* Hagyományos módszer hátrányai
  * Nehezen olvasható kód
    * hosszú kódot eredményez
    * hibaforrást gyakran több művelettel lehet kiszűrni
  * Nehezen programozható
    * minden műveletet meg kell vizsgálni
  * Nehezen karbantartható
  * A módszer használatának okai
    * Gyakran hatékonyabb, mint a kivételkezelés

#### c#-os kivételezés

```csharp=
try
<védett blokk>
catch (típus eseményref)
<típus1 kivétel kezelése>
catch (típus2 eseményref)
<típus2 kivétel kezelése>
finally
<mindig lefut>
end
```

* előnyök
  * Programkód könnyebben írható
  * Programkód könnyebben olvasható
  * Programkód könnyebben karbantartható
    * A kivételkezelő blokkok egymástól függetlenek, így az egyik változtatása nem befolyásolja a többit
  * A kivételek általában a hiba típusán túl további adatokat is tárolhatnak
* ==Kivételes esetek központi kezelése==
  * ==Egy blokk minden kivételét egy központi helyen kezeli==
    * ==Akár egyetlen programsor futtatása is okozhat több különböző hibát== 
  * Kivétel típusok szerinti kivételkezelő kiválasztás
    * A gyakorlatban ez lényeges, mivel általában az azonos típusú hibákat azonos módon lehet kezelni
* Egymásba ágyazott kivételkezelők
  * Strukturált programoknál gyakoriak az egymásba ágyazott szerkezetek
* használat
  * ==catch blokkok használata==
    * ==A catch blokkban lehetőség van elérni magát a kivétel objektumot is, ami további információkat hordozhat==
    * ==Ha több blokk is alkalmas egy kivétel kezelésére, általában csak a felülről első megfelelő blokk fut le==
    * ==Általában több catch blokk használatára van lehetőség, különböző kivételtípusok meghatározásával==
  * ==finally blokk használata==
    * ==Függetlenül attól, hogy létrejött-e kivétel, vagy sem, ez a blokk mindig lefut==
    * ==Amennyiben történt kivétel, de nincs hozzá tartozó kivételkezelő, ez a blokk akkor is lefut==
    * Előfordulhat, hogy egy kivételkezelő (vagy finally) blokkban újabb kivétel keletkezik
    * Figyelmet igényel azonban, hogy a finally blokkban csak olyan erőforrásokat szabadítson fel a program, amik biztosan le lettek foglalva


