---
tags: sztf2
---
# SZTF2: 1.Öröklés

## Osztály öröklődés

* **Redundáns** kódot nem engedünk meg
  * felesleges
  * nehezen variálható
  * spagetti kód lesz belőle :angry:
* Alacsonyabb szinten a kiemelés azt jelenti hogy
  * az ismétlődő értékeket leváltjuk konstansokra
  * és az ismétlődő utasításokat leváltjuk ciklusokra
* **==!!OSZTÁLY SZINTEN A KIEMELÉS AZ ÖRÖKLŐDÉS!!==**
  * **==Öröklődés definíciója: Az öröklődés olyan eszköz amely lehetővé teszi hogy egy vagy több osztályt leszármaztassunk egy másik osztályból, ezek rendelkeznek az előző osztály tulajdonságaival szerkezetével és viselkedésével.==**
  * ==A leszármaztatott osztályokban csak a változtatásokat kell megírni==
  * *~~A leszármazottak bővíthetik vagy szűkíthetik az őstípus állapotterét,működését~~*
* ***~~Többszörös öröklés~~****~~: a leszármaztatott ilyenkor több azonos nevűt is örökölhet~~*
* **Többszörös öröklés problémái**
  * A leszármazottak több ős esetén több azonos nevű metódust  örökölhetnek (ami nem jó)
  * Ha egy osztály több ősének is van egy korábbi közös őse, akkor ennek metódusait a végső leszármazott több féle megvalósítással is megkaphatja
* ==Osztályhierarchia: egymásból leszármazó osztályok hierarchikus rendeszere==
  * ==Ős: a származási fa magasabb szintű eleme==
  * **==Leszármaztatott==**==: az ős alacsonyabb szintű eleme==
  * **==Közvetlen ős==**==:osztályhierarchiában egy szinttel magasabban ős==
* Konstruktor
  * **==Explicit konstruktorhívás:==** ==a leszármaztatottban meghívjuk az ős „egyik” konstruktorát, ezzel inicializáljuk a mezöket==
  * **==Automatikus konstruktorhívás==**==: ha nincs explicit konstruktor, akkor automatikusan az üres konstruktor hívódik meg.==
  * **==Konstruktor hívásisorrend==**==: származási hierarchia esetén minden ős konstruktora lefut a származási sorrend szerint==
* láthatósági szintek
  * **==private==**==: csak a megadott osztály metódusai éri el==
  * **==protected==**==: megadott osztály és leszármazottjainak metódusai érik el==
  * **==public==**==: minden osztály metódusa eléri, a külvilág által látható felület==
  * **==static==** ==példányositás nélkül lehivható==

## Polimorfizmus

* ==Polimorfizmus (többalakúság) azt jelenti, hogy ugyanarra az üzenetre különböző objektumok különbözőképpen reagálhatnak==

  \
* Primitiv típusok def: a változó neve közvetlenül az adatok tárolására szolgáló rekeszre mutat így a deklarált változó típusa megegyezik az eltároltadat típusával Pl.: int típusú számot csak int típusú tömbe tehetünk bele
* referrencia: objektum azonositása memóriában, tiupst is jelent pl. Termek peldany; ennek a refernciája a Termek
* ös osztály refernciájával csak az ösben található metódusok érhetőek el

```csharp
Termek peldany;
peldany= new AkciosPeldany()
peldany.fizetendo()
```

* Melyik fog lefutni termek vagy a akciós termék metódusa?
  * ==Korai kötés==
    * ==forditáskor eldöl hogy az ös metódus fut le==
    * elönyei
      * Egyszerű megvalósítás
      * Nincs teljesítményveszteség
    * Hátrányai
      - Nem alkalmas polimorfizmus megvalósítására
  * ==Késői kötés==
    * ==referencia által hivatkozott objektum valódi típusának megfelelő osztály metódusa hívódik meg, vagyis Az akicósPéldány==
    * **==dinamikus kötés==**==-nek is hivják mivel futásidöben derül ki hogy mi fog lefutni==
    * Előnyei
      * Rugalmasság
      * Polimorfizmus lehetősége
    * Hátrányai
      * Teljesítményveszteség
      * Nagyobb tárigény
      * Bonyolultabb megvalósítás
* A kötési módot nem meghíváskor kell megadnunk, hanem a metódus megvalósításakor
  * ==Nemvirtuális metódus: mindig korai kötésü és nem lehet felüldefiniálni==
  * ==Virtuális metódus: mindig késöi kötést használ és felüldefiniálható==
  * A virtuális metódus a polimorfizmuson keresztül az OOP egyik legfontosabb alapköve
* ==Asszociáció: osztályok közötti tetszőleges viszony==
  * ==asszociációs kapcsolat áll fennt két osztály között ha az egyiknek a saját helyes működéséhez ismernie kell a másikat. pl használja/tartalmazza a másikat.==


* ==Aggregáció: a rész életciklusa nem függ az egésztől==
  * ==pl.: ha a Person megszünik létezni az Adress megmarad mert paraméterként kapta meg a személy a Adress példánynt==

  ```csharp
  public class Address 
  { 
    . . . 
  } 
  public class Person 
  { 
  	private Address address; 
  	public Person(Address address) 
  	{ 
  		this.address = address; 
  	} 
  	. . . 
  }
  public void Main()
  {
      Address ad= new Address();
      Person p =new Person(ad);
  }
  ```
* ==Kompozíció: aggregáció speciális esete: a rész életciklusa függ az egészétől==
  * ==pl.: a motor példánynt a kocsiban hoztuk létre igy ha a kocsi megszünik létezni a motor is==

  ```csharp
  public class Engine 
  { 
  	. . . 
  } 
  public class Car 
  { 
  	Engine e = new Engine(); 
  	... 
  }
  public void Main()
  {
    Car car= new Car();
  }
  ```


