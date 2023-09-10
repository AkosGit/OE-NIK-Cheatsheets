---
tags: sztf2
---
# SZTF2: 8.Bináris keresőfa

## Rekurzív típusok, fa adatszerkezet

### Fa definíciója

* ==Fa: csomópontok halmaza, amelyeket élek kötnek össze, és teljesülnek az alábbi feltételek==
  * ==létezik egy kitüntetett csomópont: a gyökér==
  * ==a gyökértől különböző minden más c csomópont egy éllel van összekötve a szülőjéhez==
  * ==a fa összefüggő: bármely nem-gyökér csomóponttól kiindulva a szülőkön keresztül a gyökérhez eljutunk==
* ==Gráfelmélet alapján fa definíciója: olyan gráf, amelyik összefüggő és nem tartalmaz kört==

### Alapfogalmak

 ![](Images/SZTF2_8.Bináris_keresőfa_1.png)

### A fa további jellemzői

* ==Csomópont mélysége: a gyökértől a csomóponthoz vezető út hossza==
* ==Csomópont magassága: a kiinduló pontból a levelekig vezető leghosszabb út==
* ==Fa magassága: a gyökér magassága==
* ==Csomópont szintje: a fa magassága minusz a csomópont mélysége==

### Rekurzív típusok

* Az adatszerkezetek egy része jól leírható egy rekurzív szerkezettel is, ami fizikailag hasonló tárolást, de egészen más algoritmusokat eredményezhet
* ==TLista: A lista rekurzív értelmezése szerint minden eleme felfogható egy tartalom résszel, és egy másik listával rendelkező szerkezetként==
* ==TFa:Egy fa egy eleme leírható egy tartalom résszel, és további részfákkal rendelkező struktúrával==
* ==TBinárisFa: A bináris fa egy olyan fa, amely legfeljebb két másik fát tartalmazhat==

### Bináris fa eleme

* Bináris fa egy elemének a definíciója
  * **TBinFaElem = Struktúra(tartalom, balgyerek, jobbgyerek)**
* Tartalom mező
  * mező neve
  * típpusa
  * ez a típus lehet a láncolt listához hasonlóan
    * egyszerű
    * összetett
    * objektum referencia
* Gyerek hivatkozások
  * mezők neve
  * típusa
  * ez a tipus implementációfüggő
  * üres
* Egy gyökér nevű külső változó hivatkozik a fa első elemére
* A csomóponton belül egyéb mezők is lehetnek, pl. gyakran tároljuk a szülő címét is

### Példa bináris fára

* Aritmetikai kifejezés fa:
   ![](Images/SZTF2_8.Bináris_keresőfa_2.png)
* Értelmezése
  * Levelek: operandusok
  * Csomópontok: operátorok
  * Fenti kifejezés: (x+(y-10))*2*(6/z)

### Bináris keresőfa eleme

* ==Bináris keresfőfa definíciója: **Egy elem tartalmaz egy Struktúrát amiben benne van kulcs,tartalom,balgyerek,jobbgyerek**==
* Kulcs tulajdonság
  * tulajdonság neve: kulcs
  * típusa: k
  * ez a tulajdonság a láncolt listához hasonlóan lehet
    * külön mező
    * a tartalom egy része
    * maga a tartalom
* ==Kulcs alapján rendezett: **A fa minden r csomópontjára igaz hogy , hogy a baloldali részfája kisebb nála a jobbaldali meg nagyobb**==
   ![](Images/SZTF2_8.Bináris_keresőfa_3.png)

## Bejárás és keresés

### ==Bináris fa bejárások==

* ==Bejárás: elemek feldolgozása egyesével==
* mivel több irányba el lehet indulni ezért több fajta bejárás létezik
* feldolgozás alapján ezek a bejárások léteznek
  * ==Preorder bejárás: tartalom, bal, jobb==
  * ==Inorder bejárás: bal, tartalom, jobb==
  * ==Postorder bejárás: bal, jobb, tartalom==
* az algorimusok közötti különbség az elem elérésének ideje
* a rendezetséget nem használjuk ki ezért a kulcsnak nincs szerepe
* Preorder bejárás
  *  ![](Images/SZTF2_8.Bináris_keresőfa_4.png)
  * Bejárás indítása
  *  ![](Images/SZTF2_8.Bináris_keresőfa_5.png)
  * Gyakorlati alkalmazás: fa elmentése
* Inorder bejárás
  *  ![](Images/SZTF2_8.Bináris_keresőfa_6.png)
  * Bejárás indítása
  *  ![](Images/SZTF2_8.Bináris_keresőfa_7.png)
  * Gyakorlati alkalmazás: elemek elérése rendezés szerinti sorrendben(növekvő vagy csökkenő)
* Postorder bejárás
  *  ![](Images/SZTF2_8.Bináris_keresőfa_8.png)
  * Bejárás indítása
  *  ![](Images/SZTF2_8.Bináris_keresőfa_9.png)
  * Gyakorlati alkalmazás: fa felszabadítása
* Keresés bináris keresőfában
  * ==ki tudjuk használni a fa rendezettségét: a fa gyökérelemének kulcsa vagy egyenlő vagy meghatározza hogy melyik ágban található az elem==
* A rekurzív keresés menete (p gyökerű (rész)fában keressük az x-et)
  * ==triviális megoldás==
    * ==ha p üres, akkor x nincs a fában==
    * ==ha p kulcsa x, akkor megtaláltuk==
  * ==indukció==
    * ==ha p kulcsa kisebb mint x, akkor x-et a jobb részfában,==
    * ==ha p kulcsa nagyobb mint x, akkor a bal részfában keressük.==
* Keresés algoritmusa
  * Kulcs alapján keresés
  *  ![](Images/SZTF2_8.Bináris_keresőfa_10.png)
  * Átlagos lépésszám (kiegyensúlyozott fában): O(log2n\

## Beszúrás művelete

* a beszurásnál a beláncoláson kivül ügyelnünk kell a keresőfa tulajdonságainak megtartására
* az elemeket többféleképpen is elhelyezkedhetnek igy több stratégia létezik
  * minél kisebb erőforrásigényű legyen a beszúrás
  * minél kiegyensúlyozottabb legyen a fa a beszúrás(ok) után
* Mi olyan beszúrást használunk, ahol nem kell átmozgatni a már meglévő elemeket az új elem felvételekor
* ==A rekurzív beszúrás menete (p gyökerű (rész)fába szúrjuk az x-et)==
  * ==triviális esetek==
    * ==ha p üres, akkor új x kulcsú csomópontot veszünk fel, ami a gyökér lesz==
    * ==ha p nem üres, és a kulcsa x, akkor hibát jelzünk==
  * ==indukció==
    * ==ha p kulcsa kisebb mint x, akkor x-et a jobb részfába,==
    * ==ha p kulcsa nagyobb mint x, akkor a bal részfába szúrjuk be.==
* Új elem beszúrása
*  ![](Images/SZTF2_8.Bináris_keresőfa_11.png)

## Törlés művelete

### Törlés bináris keresőfából

* ==A törlés során az elem kiláncolásán kívül ügyelnünk kell a keresőfa fenntartására is==
* Törlés során az alábbi problémák merülhetnek fel
  * ==két gyerekkel rendelkező elem mindkét gyerekét nem tudjuk az ő szülőjének egy mutatójára rákapcsolni==
  * gyökérelem tökéletes
* A rekurzív törlés menete (p gyökerű (rész)fából töröljük az x ez)
  * ==triviűlis esetek==
    * ==ha *p* üres akkor hibát jelzünk==
    * ==ha *p* nem üres és a kulcsa x akkor töröljük és helyre állítjuk a fát==
  * ==indukció==
    * ==ha p kulcsa kisebbm mint x akkor x-et a jobb részfából töröljük==
    * ==ha p kulcsa nagyobb mint x akkor a bal részfából töröljü==k
* ==A bináris keresőfa helyreállítási menete attól függ hogy hány gyereke van a p csomónak==
  * egysincs
  * egy gyerek
  * két gyerek

### ==Ha egy sincs:==

* ==Itt elhagyjuk a törlendő kulcsot tartalmazó elemet a szülő megfelelő mutatóját pedig null ra állítjuk==

### ==Ha egy gyerek van==

* ==Ilyenkor a láncolt listákhoz hasonlóan ki tudjuk láncolni és az egy gyereket az egy őshöz kapcsoljuk==

### ==Két gyerek van==

* ==Megkeressük a baloldali részfa legjobboldalibb elemét==
* ==Ennek tartalmát és kulcsát felmásoljuk a törlendő elembe==
* ==Majd ezt az elemet töröljük==
* ==Ezt megtehetjük, hiszen ez==
  * ==biztosan nagyobb, mint a baloldali elemek és==
  * ==biztosan kisebb, mint a jobboldali elemek==

### Törlés algoritmusa

* Megadott kulcsú elem törlése
*  ![](Images/SZTF2_8.Bináris_keresőfa_12.png)
* két gyerek kezelése
*  ![](Images/SZTF2_8.Bináris_keresőfa_13.png)

### Mire használjuk

* ==A bináris fa előnyei==
  * ==rendezett==
  * ==ebből adódóan gyors keresés==
  * ==relatív gyors elem felvétel==
  * ==relatív gyors elem törlés,== legrosszabb esetben sincs szükség a fa nagy részének átalakítására
  * ==a csomópontok tartalma gyakran egy más adatszerkezet elemére mutat: ideális indexelésre kiegészítő adatszerkezetként==
* A bináris fa hátrányai
  * bonyolult, lásd bejárások
  * rekurzióval a műveletek költségesek, illetve maga a rekurzió az OOP nyelvekben gyakran életidegen módon jelenik meg
  * elemek nem érhetőek el közvetlenül (sőt, maga az indexelés sem egyértelmű, csak a bejárás módjával együtt)
  * a gyors keresés nem garantált, csak kiegyensúlyozott fákban valósul meg

a két gyerek címe miatt nagy lehet egy elem helyfoglalá

## ___________________VIZSGÁRA INNNENTŐL NEM KELL_____________________

### Kiegyensúlyozatlan fák problémája

* adattartalom szempontjából azonos, de szerkezetileg más fák
*  ![](Images/SZTF2_8.Bináris_keresőfa_14.png)
* A jobboldali (elfajult) fa keresési szempontból nem ideális, láncolt listához hasonló lépésszámot igénye

### Kiegyensúlyozottság

* Kiegyensúlyozott fa: legfeljebb egy szintnyi (mélység) eltérés van az egyes (azonos szinten található) részfái között
* Teljesen kiegyensúlyozott fa: minden csúcsából leágazó bal- és jobboldali részfa csúcsainak száma legfeljebb egyel különbözik egymáshoz képest
* Teljes fa: Minden csúcsból leágazó bal- és jobboldali részfa csúcsainak száma azonos
* Célunk: a módosító algoritmusok kiegészítése, hogy minél inkább kiegyensúlyozottabban épüljenek fel

### Forgatás

* **Forgatás: olyan lokális művelet, amely megváltoztatja a fa szerkezetét, de megőrzi a rendezettséget**
   ![](Images/SZTF2_8.Bináris_keresőfa_15.png)

### Piros-fekete fa

* Piros-fekete fa: Olyan bináris keresőfa, amely rendelkezik az alábbi tulajdonságokkal:
  * minden csúcs színe piros vagy fekete
  * minden levél színe fekete
  * minden piros csúcsnak mindkét gyereke fekete
  * bármely két, azonos csúcsból kiinduló és levélig vezető úton ugyanannyi fekete csúcs van (fekete magasság)
* Megvalósítással kapcsolatos megjegyzések:
  * a BST struktúrát kiegészítjük egy új attribútummal, ami jelzi a színt (értéke csak piros vagy fekete lehet)
  * az egyszerűbb algoritmusok miatt tároljuk a csomópontok szüleit is (a gyökérpont esetén ezt -el jelöljük)
  * az egyszerűbb algoritmusok miatt a tartalommal külön nem foglalkozunk, feltételezzük, hogy amennyiben a kulcsot átmásoljuk, az mindig a kulccsal együtt „mozog”

### Piros-fekete fa példa

 ![](Images/SZTF2_8.Bináris_keresőfa_16.png)

* Megjegyzés: levélnek a nem ábrázolt  értékeket tekintjük (ezek kötelezően feketék)
* Megközelítőleg kiegyensúlyozott: biztosítható, hogy bármely, a gyökértől levélig vezető út hossza nem nagyobb, mint a legrövidebb ilyen út hosszának a kétszerese


