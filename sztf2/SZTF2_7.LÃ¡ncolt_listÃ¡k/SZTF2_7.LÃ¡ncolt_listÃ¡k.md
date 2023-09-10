---
tags: sztf2
---
# SZTF2: 7.Láncolt listák

## Adatszerkezetek

* ==Adatszerkezet: Az adatelemek egy olyan véges halmaza, amelyben az adatelemek között szerkezeti összefüggések vannak==
* Műveletek szempontjából
  * Egyszerű adattárolás
    * ==Új elem felvétele==
    * ==Elem törlése/módosítása==
    * ==Keresés tetszőleges feltétel szerint==
  * Rendezett adattárolás
    * ==Keresés kulcs szerint==
  * Sor
  * Verem

## Láncolt listák elvi felépítése

* ==Láncolt lista: A láncolt lista olyan adatszerkezet, amelynek minden eleme tartalmaz egy hivatkozást egy másik, ugyanolyan típusú elemre.==
* Tárgyalt altípusok:
  * ==Láncolás szempontjából==
    * Egyszeresen láncolt
    * Többszörösen láncolt
  * ==Rendezés szempontjábó==l
    * Rendezetlen (egyszerű)
    * Rendezett
  * Egyéb szempontok szerint
    * Strázsa elemek használata
    * Speciális listák (ciklikus, kétirányú, stb.)

### Láncolt lista eleme

* ==A láncolt lista egy elemének a felépítése: TLáncElem = Struktúra(tartalom, következő)==
  * ==Tartalom bármi lehet==, T típusú:
    * egyszerű típus
    * összetett típus
    * objektum referencia
  * ==Következő: hivatkozás a láncolt lista következő elemére, M típusú:==
    * tömb index
    * objektum referencia
* Egyirányú láncolt lista jellemzői
  *  ![](Images/SZTF2_7.Láncolt_listák_1.png)
  * ==A lánc minden eleme tartalmaz egy hivatkozást a következő elemre==
  * ==A lánc első elemének címét egy külső változó, a listafej tartalmazza==
  * ==a listafej csak cimet tartalmaz==
  * ==a lánc végét egy spaciális érték jelzi==

## Tömb és dinamikus adatszerkezetek összehasonlítása

* ==A tömb adatszerkezet hátrányai==
  * ==Méret nem változtatható==
  * ==Összefüggő memóriaterületet igényel==
  * ==Beszúrás nehézkes==
  * ==Törlés nehézkes==
* ==Dinamikus adatszerkezetek hátrányai==
  * ==Bonyolult==
  * ==Elemek nem érhetők el közvetlenül indexeléssel==
  * ==csak a lineáris keresés használható==
  * ==A következő elemre való hivatkozás eltárolása miatt nagyobb egy elem helyfoglalása==

## Egyirányú egyszerű láncolt lista

* ==Bejárás: Az adatszerkezet valamennyi elemének egyszeri elérése==
* A tartalmak feldolgozása egymástól független, így kb. megfelel a sorozatszámítás programozási tételnek
* Bejárás algoritmusa
   ![](Images/SZTF2_7.Láncolt_listák_2.png)

### ==Keresés listában==

* ==Keresés: A lista fej ismeretében egy megadott feltételnek megfelelő tartalom megkeresése==
  * ==eldönteni, hogy van-e ilyen tartalmú elem==
  * ==ha nincs, akkor a visszatérési érték hamis==
* Keresés algoritmusa
  

### ==Inicializálás és új elem felvétele (elejére)==

* ==Lista inicializálása: a lista alaphelyzetbe állítása==
   ![](Images/SZTF2_7.Láncolt_listák_3.png)
* ==Lista elejére új elem beszúrása==
   ![](Images/SZTF2_7.Láncolt_listák_4.png)

### ==Új elem felvétele (végére)==

* ==Lista inicializálása==
* ==Létrehozás==
* ==Utolsó elem megkeresése== 


* ==Lista végére beszúrás==
   ![](Images/SZTF2_7.Láncolt_listák_5.png)

### ==Új elem felvétele (megadott helyre)==

* ==Megadott helyre (n. pozíció) beszúrás==
* 2 fontos szekciója van
* 1 egy nagy if ami megvizsgálja hogy üres e a lista
* ha üres vagy első helyre kell meilleszteni akkor ez megtörténik
* ha nem üres vagy nem első helyre kell be illeszteni akkor a p lesz a fej és i pedig 2 mer pszeudo és első helyre nem akarunk (arra van kész feltétel)
* 2 egy ciklus végig megy a listán és  p be beleteszi p köv-öt és növeli i-t
* ha ennek vége akkor beállítjuk az új lista hivatkozását a kövi listára
* a p kövije pedig a mi listánk lesz (az uj)
*  ![](Images/SZTF2_7.Láncolt_listák_6.png)

### ==Elem törlése==

* ==Megadott tartalmú elem törlése (ha van ilyen)==
* p be beletesszük a fejet ezzel átadva a metódusnak a listát
* e be nullt mert később használjuk
* ciklus addig fut amig NEM érünk a végére vagy NEM találtunk törlendőt
* Ha találtunk törlnedőt akkor p!=null lesz és megvizsgáljuk hogy e=Null (ha e null akkor nem léptünk be a ciklusba tehát tudjuk hogy az első elem amit törölni kell és töröljük)
* Ha e!=Null akkor meg az e.köv mutatóját át tesszük az őt követő mutatójára ezzel törölve az elemet
*  ![](Images/SZTF2_7.Láncolt_listák_7.png)

### ==Teljes lista törlése==

* ==Az összes elem törlése==
   ![](Images/SZTF2_7.Láncolt_listák_8.png)

## ==Egyirányú rendezett láncolt lista==

* ==A rendezés érdekében kiegészítjük a láncolt lista elemeket egy új, kulcs nevű tulajdonságga==
* ==Ennek típusa mindig összehasonlítható==
* ==Ez a kulcs lehet==
  * ==egy tartalomtól független mező==
  * ==a tartalom egy része==
  * ==maga a tartalom==
* Az utólagos rendezés nehézkes a felépités miatt
* Kulcs szerinti keresés
  *  ![](Images/SZTF2_7.Láncolt_listák_9.png)
* beszúrás
  *  ![](Images/SZTF2_7.Láncolt_listák_10.png)
* beszurás azonos ágak összevonása után
  *  ![](Images/SZTF2_7.Láncolt_listák_11.png)


### Néhány optimalizálási ötlet

* ==A fej mutató használata nélkül, csak egy belső lista elemre való hivatkozással (p) is tudunk beszúrni, illetve törölni==
* Mutatott elem mögé beszúrás
   ![](Images/SZTF2_7.Láncolt_listák_12.png)
* Mutatott elem elé beszúrás (a fej nem ismert)
   ![](Images/SZTF2_7.Láncolt_listák_13.png)

## ==Láncolt lista strázsa elemekkel==

### Strázsa technika

* ==Strázsa elemek: A lista elejére és a végére felvett kiegészítő elemek. Értékes tartalmat nem tárolnak, csak technikai szerepük van, hogy kiküszöböljék a kivételes eseteket==
* Elérhető előnyök
  * egyszerűbb beszúrás/törlés
  * gyorsabb beszúrás/törlés
* Hátrány
  * helyfoglalás
  * bejárás körülményesebb
* Példa strázsa elemek használatára
   ![](Images/SZTF2_7.Láncolt_listák_14.png)

### Technikai megvalósítás

* Lista inicializálása
   ![](Images/SZTF2_7.Láncolt_listák_15.png)
   ![](Images/SZTF2_7.Láncolt_listák_16.png)

## ==Speciális láncolt listák==

### ==Kétirányú láncolt lista felépítése==

* **==A lánc minden eleme tartalmaz egy hivatkozást a sorban rá következő, illetve a sorban őt megelőző elemre is==**
* ==A lánc végét az utolsó elem következő részének egy kitüntetett értéke jelzi==
* ==A lánc elejét az első elem előző részének egy kitüntetett értéke jelzi==
* **==A lánc első elemének címét a listafej tartalmazza==**
* **==A lánc utolsó elemének címét is célszerű tárolni egy külső változóban==**

### Kétirányú láncolt lista értékelése

* Előnyei az egyirányú listához képest
  * Keresés: ha van információnk az elem listabeli elhelyezkedéséről (pl. tudjuk, hogy a vége felé helyezkedik el) akkor előnyös lehet
  * Törlés: előnyös, hiszen azonnal elérhetjük a szomszédos elemeket
  * Beszúrás: szintén kihasználható a beláncolás során, hogy közvetlenül elérhető minden elem őt megelőző eleme
* Hátrányok az egyirányú listához képest
  * Nagyobb elemenkénti helyfoglalás
  * Módosításkor bonyolultabb algoritmusra van szükség, mivel az előző hivatkozást is mindig aktualizálni kell

### Többszörösen láncolt lista felépítése és értékelése

 ![](Images/SZTF2_7.Láncolt_listák_17.png)

* ==A lánc minden eleme tartalmazza n darab következő elem címet==
* ==A lánc végét az utolsó elem megfelelő következő részének egy kitüntetett értéke jelzi==
* ==A lánc tartalmaz n darab listafejet==
* ==Műveletei gyakorlatilag megfelelnek az egyszerű láncolt listánál megismertekkel, felfogható n darab független láncolt listaként==
* A tartalmi rész azonban csak egyszer szerepel, emiatt:
  * kisebb helyfoglalás módosításkor minden láncban módosul a tartalmi rész

### ==Ciklikusan láncolt lista felépítése és értékelése==

 ![](Images/SZTF2_7.Láncolt_listák_18.png)

* ==A lánc minden eleme tartalmazza a következő elem címét, az utolsó elem pedig a elsőre mutat vissza==
* ==A lánc végét külön nem jelöljük, a bejáró algoritmus felelőssége, hogy észrevegye, ha már== ==feldolgozott minden elemet==
* ==A láncba akár több belépési pont is tárolható==
* ==A lista lehet egy- illetve kétirányú is==
* Előnyei az egyszerű láncolt listához képest:
  * speciális feladatoknál hasznos
  * törléskor nincsenek kivételes első és utolsó elemek
  * beszúráskor nincsenek kivételes első és utolsó elemek

## Implementációs lehetőségek

### Statikus megvalósítás

* a láncolt lista elemeit egy tömbe tárolja
* felépités
  * tartalom
  * index a következő elemre
* jelezni kell az törölt elemek helyét valamivel pl -1
* méret nem változhat de beszúrás és törlés szempontjából előnyösebb lehet
   ![](Images/SZTF2_7.Láncolt_listák_19.png)
* Műveletek implementációja
  * FELSZABADÍT(p : Egész)
    * töröltnek jelöli az elemet helyben vagy egy másik tömben
  * LÉTREHOZ : Egész
    * keres egy szabad helyet és annak indexét adja vissza

### Dinamikus megvalósítás

* A láncolt lista elemeit dinamikus memóriakezelés segítségével hozzuk létre
* felépités
  * tartalom
  * mutató a következő elem memória cimére
*  ![](Images/SZTF2_7.Láncolt_listák_20.png)
* törölt elemek helyét bármilyen nyelvi elem jelölheti: 0,null...
* Műveletek implementációja:
  * FELSZABADÍT(p : Mutató)
    * felszabadítja a p által hivatkozott memóriacímen található listaelem méretű memóriaterületet
  * LÉTREHOZ : Mutató
    * dinamikus memóriakezelés segítségével lefoglal egy listaelem méretű memóriaterületet és vissza adja a ezt a cimet

### Objektum-orientált megvalósítás

* technikai megvalósítás tekintetében tulajdonképpen megegyezik a
* dinamikus megoldással:
  * TListaElem struktúra → TListaElem osztály
  * Mutató → Objektum referencia
  * Memória foglalás → Konstruktor hívása
  * Memória felszabadítás → Destruktor hívása
* Célszerűen az egész láncolt lista önmagában is lehet egy objektum, így a paraméterként minden esetben átadott fej mutató lehet ennek egy belső mezője (ez jelentősen egyszerűsíti az algoritmusokat)


