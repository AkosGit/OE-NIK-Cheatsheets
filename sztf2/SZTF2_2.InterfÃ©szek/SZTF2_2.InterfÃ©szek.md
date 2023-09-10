---
tags: sztf2
---
# SZTF2: 2.Interfészek

* ==Az interfész== meghatározza egy osztály felületét
  * Másként megfogalmazva: ==egy szerződés, ami kikényszeríti, hogy az interfészt megvalósító osztályok rendelkezzenek bizonyos műveletekkel==
* ==Interfészek== néhány tipikus ==alkalmazása==
  * ==Osztályok felruházása valamilyen műveletekkel (nevet rendel metódus szignatúrák egy csoportjához)==
  * Többszörös öröklődés egyszerűsített megvalósítása (polimorfizmus lehetőségeinek elérésére)
* Tartalom
  * ==Interfész== tipikusan (nyelvtől függően) ==tartalmazhat==
    * ==Metódus szignatúrát==
      * szignatúra: ==metódus neve + visszatérési értéke + paraméterek és az interfész metódus mindig virtuális és absztrakt==
    * ==Konstans mezőt (az amit elé oda írjuk hogy “const” ezek NEM módosíthatók beállítás után)==
    * ==Tulajdonságot== (amennyiben a nyelv támogatja)
  * ==Interfész NEM tartalmazhat==
    * ==Konkrét metódus==, tulajdonság implementációt
    * ==Példányszintű mezőt (Az amit az osztályba deklarálunk és nem a metódusban)==
    * Példányosításhoz kapcsolódó ==konstruktort/destruktort==
* Interfész megvalósítása
  * **==Interfészek önmagukban nem példányosíthatók, csak az őket megvalósító osztályokon keresztül érhetők el a műveleteik==**
  * ==Egy osztály egyszerre bármennyi interfészt meg valósíthat==
  * ==Az interface-t megvalósitó osztálynak kötelező megvalósítania az interface metódusait==
  * Ez nem vonatkozik az absztrakt osztályokra, akik ezt a leszármazottakra hárítják
* Interfész típusú referencia
  * ==Az interfészek tulajdonképpen típusok, ezért lehetséges ilyen típusú változók deklarációjára is==
  * ==Egy ilyen változóval hivatkozhatunk bármilyen objektumra, amely megvalósítja az adott interfészt==
  * Az interfész típusú referenciák az osztály típusú referenciákhoz hasonló módon működnek
* Interfész hierarchia
  * Az osztályokhoz hasonlóan az interfészek között is fel lehet építeni egy öröklődési hierarchiát
  * Az osztályok és az interfészek hierarchiája egymástól független
  * Az osztályokhoz hasonlóan a polimorfizmus előnyeit az interfészek között is alkalmazhatjuk (minden interfész használható bármelyik őse helyén)
* Interfész – absztrakt osztályok kapcsolata
  * Más a cél
    * ==Az absztrakt osztályok egy részben elkészült osztálynak tekinthetők==
    * ==Az interfészek nem adnak impelementációt==
  * ==Osztály→interface nem működik==
  * ==A kapcsolat csak ott jelenik meg, amikor egy osztály megvalósít egy interfészt==
* Többszörös öröklés korlátai
  * ==Osztályok között nincs többszörös öröklésre lehetőség==
  * ==Egy interfész több interfész leszármazottja is lehet, és egy osztály megvalósíthat több interfészt is==
* Implicit/explicit interfész megvalósítás
  * Több interfész is tartalmazhat ugyanolyan szignatúrájú metódusokat
  * Implicit megvalósítás
    * **==Az osztály metódusának a szignatúrája megegyezik az interfész(ek)ben megadott szignatúrával Bármelyik interfésszel hivatkozunk az osztályra, mindig ugyanaz a metódus fut le==**
  * Explicit megvalósítás:
    * **==Az osztályban a metódus neve mellett megadjuk az általa megvalósított interfész nevét is. Attól függően, hogy melyik interfésszel hivatkozunk az osztályra, mindig a megfelelő metódus fut le==**

    ```csharp
    public interface Message2
    {
        public void Write();
    }
    public interface Message1
    {
        public void Write();
    }
    public class Implicit : Message1,Message2
    {
        public void Write()
        {
            Console.WriteLine("implicit");
        } 
    }
    public class Explicit : Message1,Message2
    {
        void Message1.Write()
        {
            Console.WriteLine("explicit1");
        }
        void Message2.Write()
        {
            Console.WriteLine("explicit2");
        }
    }
    class Program {
        static public void Main(String[] args)
        {
            Implicit im = new Implicit();
            im.Write();
            
            Explicit ex = new Explicit();
            ((Message1)ex).Write();
            ((Message2)ex).Write();
    
        }
    }
    ```
* Ősre vonatkozó követelmény kiküszöbölése
  * Szeretnénk szétválasztani az objektumok létrehozását azok használatától
  * Egy objektum gyár létrehoz (ha szükséges) és visszaad új objektumokat, esetenként valamilyen paraméter alapján
* Modulok összekapcsolása
  * Az interfészek erre jó lehetőséget adnak
    * Egy magasabb absztrakciós szintet nyújt, ami leegyszerűsíti az egész rendszer áttekintését
    * Elrejti a konkrét megvalósítást, így a használója biztos nem fog a megvalósítástól függő kódot írni


