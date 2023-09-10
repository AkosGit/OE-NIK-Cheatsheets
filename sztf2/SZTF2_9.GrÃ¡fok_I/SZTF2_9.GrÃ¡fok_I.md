---
tags: sztf2
---
# SZTF2: 9.Gráfok I





## Tárolási módok

### Alapfogalmak

* **==Irányított gráf: csúcsok véges halmaza, illetve egy ezen értelmezett bináris reláció==**
* ==Formálisan:==
  * ==G – gráf==
  * ==V – csúcsok halmaza, pl. { 1, 2, 3, 4, 5, 6, 7 }==
  * ==E – élek halmaza, pl. { (1, 4), (2, 1), (2, 3), … }==
  * ==|V| – csúcsok száma==
  * ==|E| – élek száma==
* Súlyozott gráfok esetén az egyes élekhez egy kiegészítő súly értéket is rendelünk
   ![](Images/SZTF2_9.Gráfok_I_1.png)

### ==Alapvető műveletek==

* A gráfok eltárolására számos klasszikus módszer létezik. Amit mi várunk el, hogy az alkalmas legyen az alábbi műveletek végrehajtására
  * ==Gráfokon értelmezhető alapvető műveletek==
    * ==G.Csúcsok – visszaadja a gráf csúcsait==
    * ==G.Élek – visszaadja a gráf éleit==
  * ==Gráf csúcsain értelmezhető műveletek==
    * ==x.VezetÉl(y) – megadja, hogy vezet-e él a paraméterként átadott y csúcsba?==
    * ==x.Szomszédok – visszaadja a csúcs szomszédjait==
  * ==Súlyozott gráf esetén értelmezhető==
    * ==x.Súly(y) – visszaadja az x-ből y-ba vezető él súlyát==
  * Gráf csúcsok tartalmazhatnak további adatokat is
    * Tart – a csúcshoz kapcsolt tartalom

### ==Szomszédsági lista==

* **==Alapelv: a szomszédsági listás tárolás esetén egy L tömböt használunk, melynek==**
  * ==mérete megegyezik a csúcsok számával==
  * ==az i. eleme egy láncolt lista, amely tárolja az i. csúcs szomszédjait==
* Műveletek megvalósítása
   ![](Images/SZTF2_9.Gráfok_I_2.png)

### Szomszédsági lista kiegészítések

* ==Amennyiben egy csúcsból nem indulnak ki élek, akkor az üres láncolt listákhoz hasonlóan ez a mező közvetlenül null értéket tartalmaz==
* ==A listában szereplő csúcsok sorrendje tetszőleges==
* ==A lista tartalma lehet a csúcs neve/azonosítója, de akár referencia magára a csúcsra==
* ==Súlyozott gráfok esetében a listát célszerű kiegészíteni egy további mezővel, ami a megadott csúcsból a megadott csúcsba vezető él súlyát tárolja==
* ==Irányítatlan gráfok esetében az éleket mindkét csúcs listájában szerepeltetni kell==

### ==Szomszédsági mátrix/csúcsmátrix==

* ==Alapelv: a szomszédsági mátrix alapú tárolás esetén egy CS kétdimenziós tömböt használunk, amely==

## ==sorainak és oszlopainak száma megegyezik a csúcsok számával==
 ![](Images/SZTF2_9.Gráfok_I_3.png)==Mikor mit használjunk==

* ==Tárhely szempontjából==
  * ==Szomszétságit akkor használunk ha kevés az él==
  * ==Csúcsmátrixot akkor használunk ha sok az él==


* ==Műveletek használatának gyakoriságától függően==
  * ==Gyakran van szükség Szomszédok műveletre (vissza adja a szomszédokat irányítottnál akik RE mutat)  akkor Szomszédsági lista is kell==
  * ==Gyakran van szükség VanÉl műveletre (a és b között van e él ami a másikba mutat) akkor Csúcsmátrixosat használunk==

## ==Szélességi bejárás==

* ==bejárás: minden csúcs egyszeri feldolgozása (pl kiirása)==
* Az algoritmusok azonban bonyolultabbak az eddig megismerteknél, ugyanis gondolni kell az alábbiakra
  * ==gráfokban előfordulhatnak körök/ciklusok és végtelelen ciklus elkerülendő==
  * ==gráfok nem feltétlenül összefüggőek,tisztázni kell, hogy hogy a  cél egy komponens bejárása, vagy az összes csúcs elérése==
* ==Egy irányítatlan gráfot **összefüggőnek** nevezünk, ha bármely két csúcsa összeköthető úttal==
* ==Irányított gráfokat akkor nevezzük **erősen összefüggőnek**, ha tetszőleges két csúcs esetén mindegyik elérhető a másikból bármely úton==
* igy beszélehtünk összefüggő, illetve erősen összefüggő komponensekről

### ==Gráf bejárások==

*  ![](Images/SZTF2_9.Gráfok_I_4.png)
* ==Szélességi bejárás (szélességi keresés): egy adott kezdőpontból kiindulva feldolgozza a gráf összes innen elérhető csúcsát==
* Minden lépésben a legkorábban elért, de még teljesen át nem vizsgált csúcs szomszédjai felé haladunk
*  ![](Images/SZTF2_9.Gráfok_I_5.png)
* adatszerkezetek
  * S – egy sor, ami a már elért, de még a feldolgozásra váró csúcsokat tárolja
  * F – egy halmaz, ami a már elért csúcsokat tárolja
* addig fut, amíg elfogynak a feldolgozható csúcsok
* ez nem mindig fogja feldolgozni a gráf minden csúcsát

### Csúcsok állapota a bejárás során

* Már elért, de még fel nem dolgozott csúcsok
  * tehát nem vizsgáltuk meg a belőlök elérhető szomszédokat
  * ezek vannak benne az S sorba
* Már elért és feldolgozott csúcsok
  * összes belőlük kivezető él is fel lett már dolgozva
  * benne vannak az F halmazba viszont S-ben nem
* Még el nem ért csúcsok
  * Nincsenek benne egyik adatszerkezetben sem

### ==Legrövidebb utak hosszát megadó kiegészítés==

*  ![](Images/SZTF2_9.Gráfok_I_6.png)
* d: minden csúcshoz hozzárendelünk egy számot ez jelzi  kiinduló csúcstól való legrövidebb út hosszát
* A kiinduló pont esetében 0 utána csúcsonként egyel növekszik
* A bejárás során el nem ért csúcsok esetén a d üres

### Egy feszítőfát megadó kiegészítés

* Egy π tömb/hasító táblázattal hozzárendelünk egy csúcshoz egy másikat vagy egy ürest jelző értéket, ez mutatja hogy adott csúcsot melyik csúcsból értük el
* A kiinduló pont esetében a π [start] = ürest jelző érték, mivel nincs megelőző elem
* A π értékek alapján megadható az adott csúcsokba a start-ból vezető legrövidebb út (ha van)
* π értékei alapján felépíthető egy feszítőfa (szélességi fa)

### Szélességi keresés gyakorlati alkalmazása

* fordított gráf:
  * ennek csúcsai ugyanazok, élei viszont pont ellenkező irányúak
* Komponensek keresése
  * lefutás után az F halmaz tartalmazza a start-ból elérhető csúcsokat
  * Gyenge összefüggőség vizsgálata:
    * elöbbi algoritmussal
  * Erős összefüggőség vizsgálata:
    * irányított gráf esetén a fordított gráfon végezzük el az algoritmust és ha minden csúcsot megtalál akkor erősen összefüggő
* Útkeresés
  * Amennyiben a gráf egyes csúcsai a lehetséges útelágazásokat reprezentálják, az élek pedig az egyes csomópontok között meglévő utakat
    * szélességi keresés megadja az összes elérhető csomópontot
    * leállási feltétellel megadható, hogy egy megadott cél csúcs elérése után álljon le a program
    * A π értékei alapján egyszerűen megadható, hogy a cél csúcsba melyik úton lehet a legrövidebben eljutni

## ==Mélységi bejárás==

* Mélységi bejárás (mélységi keresés): egy adott kezdőpontból kiindulva feldolgozza a gráf összes innen elérhető csúcsát
* A szélességi kereséssel ellentétben itt mindig az utoljára elért, új kivezető élekkel rendelkező csúcsokat derítjük fel. Ha nincs ilyen, akkor pedig visszalépünk egy már előzőleg vizsgáltra
   ![](Images/SZTF2_9.Gráfok_I_7.png)
  *  ![](Images/SZTF2_9.Gráfok_I_8.png)
* A feldolgozás egy vermen alapul, ezt a rekurzió miatt közvetve használjuk
* Fontos kiemelni, hogy ez nem mindig fogja feldolgozni a gráf minden csúcsát

### Megelőző elemet is tároló változat

* Gyakran szükség van arra, hogy az egyes csúcsokat melyik másik csúcsból értük el
* Az itt látható kiegészítés ezt az adatot gyűjti a π tárolóba
* A kiinduló pont esetében a π [start] = 0, hiszen nincs megelőző csúcs
* Ez alapján megadható egy út a kiinduló start csúcsból a gráf összes többi csúcsába
* A szélességi bejáráshoz hasonlóan a π-ben található adatok alapján egy feszítőfát állíthatunk elő

### Belépési és elhagyási időt is tároló változat

* A gyakorlatban hasznos lehet, ha folyamatában vizsgáljuk a mélységi keresés lépéseit. Ehhez célszerű lehet eltárolni az egyes csúcsokba való be- és kilépési időket
* Ennek megfelelően
* A t változó értéke kezdetben 0, utána pedig az egyes hívások során folyamatosan növekszik
* Az eljárás elején és végén látható a be és ki nevű szerkezetek tárolják az egyes csúcsok elérési és elhagyási idejét

### Topológiai rendezés

* Egy irányított gráf topológiai rendezése a csúcsoknak egy olyan sorba rendezése, amelyre igaz, hogy ha létezik (u,v) él a gráfban, akkor u megelőzi a sorban v-t
* Ha a gráf tartalmaz irányított kört, akkor nincs ilyen sorbarendezés
* Példa: öltözködéskor melyik ruhadarabot kell felvenni egy másik előtt:
   ![](Images/SZTF2_9.Gráfok_I_9.png)

### Topológiai rendezés előállítása

* Jól tudjuk használni a mélységi keresés azon változatát, amely tárolta az egyes csúcsok elérési és elhagyási idejét
* Egy topológiai rendezés előállítható úgy, hogy az egyes csúcsokat az elhagyás fordított sorrendjében soroljuk fel
   ![](Images/SZTF2_9.Gráfok_I_10.png)
* Technikailag ez egyszerűbben is megoldható, pl. minden elhagyáskor a csúcsot szúrjuk egy láncolt lista elejére
* Mivel a gráf nem biztos, hogy erősen összefüggő, így minden csúcsból el kell indítani a rekurzív bejárást

### Mélységi keresés gyakorlati alkalmazása

* Visszalépéses keresés
  * A visszalépéses keresés tulajdonképpen a mélységi keresés alapelvét használja működése során
  * A lehetséges megoldások által kifeszített fában lépeget előre amíg tud, amíg
    * vagy talál meg megoldást
    * vagy ha egy helyen elakad, akkor visszalép és új irányba próbálkozik
* Fa bejárások
  * A pre-, in-, post-order bejárások tulajdonképpen mind egy-egy mélységi keresést mutatnak be
* Útkeresések
  * Hasonlóan a szélességi kereséshez, bár nagy/nem korlátos gráfok esetén problémásabb a használata
* Topológiai rendezés
  * Gyártási folyamat optimalizálása
* Erősen összefüggő komponensek keresése
  * Gráf bejárása minden pontból – kilépési idők megjegyzése
  * Gráf transzponált bejárása


