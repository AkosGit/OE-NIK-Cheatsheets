---
tags: sztf2
---
# SZTF2: 3.Eseménykezelés

## ==Függvénymutatókkal==

* ==A programok kódja ugyan abban a memóriában tárolódik mint  az adatok==
* ==A mutatók nem egy változóra mutatnak hanem egy függvény belépési címére==
  * nincs ellenőrzés futás közben
  * Bonyolult
  * Gyors
  * Nagy a hibalehetőség

## ==Származtatással==

* ==Az eseményeket küldő osztály rendelkezik metódusokkal, amelyeket az esemény bekövetkeztekor mindig meghív== (eseménykezelő)
* ==ezen metódusok implementálásra kerülnek a leszármazott osztályban==
* Előnyei
  * Jól illeszkedik az OOP szemlélethez
* Hátrányai
  * Sok, gyakran minimális kóddal rendelkező osztály definiálását igényli
  * Az esemény forrása és az eseményhez tartozó kezelő nem függetlenek (pl. ugyanaz a kezelő több helyen?)

```csharp
    public abstract class Storage
    {
        string[] array;

        public abstract void ItemInserted(int index);
        public abstract void ItemDeleted(int index);

        public Storage()
        {
            array = new string[0];
        }


        public void AddItem(string item)
        {
            string[] newArray = new string[array.Length + 1];
            for (int i = 0; i < array.Length; i++)
            {
                newArray[i] = array[i];
            }
            newArray[newArray.Length - 1] = item;
           
            ItemInserted(newArray.Length - 1);
            array = newArray;
        }

        public void DeleteItem(string item)
        {
            int i = 0;
            while (i < array.Length && !array[i].Equals(item))
            {
                i++;
            }
            if (i < array.Length)
            {
                string[] newArray = new string[array.Length - 1];
                for (int j = 0; j < i; j++)
                {
                    newArray[j] = array[j];
                }
                //searched item deleted (position = i)
                ItemDeleted(i);
                for (int j = i + 1; j < array.Length; j++)
                {
                    newArray[j-1] = array[j];
                }
                array = newArray;
            }
        }
      
    }
    class MyStorage : Storage
    {
        public override void ItemDeleted(int index)
        {
            Console.WriteLine("New item deleted at position " + index);
        }

        public override void ItemInserted(int index)
        {
            Console.WriteLine("New item insterted at position " + index);
        }

    }
    
```

## ==Interfészekkel==

* Az eseménykezelésben résztvevő típusok
  * ==Esemény támogató interface==
* Esemény kezelés lépes
  * ==Eseménykezelő osztály definiálás==
  * ==Eseménykezelő objektum példányosítása==
  * Eseménykezelő objektum "regisztrálása"
  * ==Esemény bekövetkezte után a forrás meghívja a kezelő metódust==
* Előnyei
  * Az esemény forrása és kezelője egymástól független ez azt jelenti hogy 1 forrásnak sok kezelése is lehet és sok forrásnak is lehet 1 kezelése
  * az interface több metódust is tartalmaz

```csharp
    public interface IStorageDisplay
    {
        void ItemInserted(int index);
        void ItemDeleted(int index);
    }

    public class Storage
    {
        string[] array;
        IStorageDisplay display;
        
        public Storage(IStorageDisplay externalDisplay)
        {
            this.display = externalDisplay;
            array = new string[0];
        }

        public void AddItem(string item)
        {
            string[] newArray = new string[array.Length + 1];
            for (int i = 0; i < array.Length; i++)
            {
                newArray[i] = array[i];
            }
            newArray[newArray.Length - 1] = item;
            //new item inserted (position = newArray.Length - 1)
            if (this.display != null)
            {
                this.display.ItemInserted(newArray.Length - 1);
            }
            array = newArray;
        }

        public void DeleteItem(string item)
        {
            int i = 0;
            while (i < array.Length && !array[i].Equals(item))
            {
                i++;
            }
            if (i < array.Length)
            {
                string[] newArray = new string[array.Length - 1];
                for (int j = 0; j < i; j++)
                {
                    newArray[j] = array[j];
                }
                //searched item deleted (position = i)

                if (this.display != null)
                {
                    this.display.ItemDeleted(i);
                }


                for (int j = i + 1; j < array.Length; j++)
                {
                    newArray[j-1] = array[j];
                }
                array = newArray;
            }
        }
    }
    class MyDisplay : IStorageDisplay
    {
        public void ItemDeleted(int index)
        {
            Console.WriteLine("New item deleted at position " + index);
        }

        public void ItemInserted(int index)
        {
            Console.WriteLine("New item inserted at position " + index);
        }
    }
```

## ==Delegált és Event== Előnyei Külömbségei (Egyéb nyelvi elemekkel)

* Alapelve hasonló a függvénymutatóknál megismert modszerhez
* Külömbségek
  * csak metódus referencia értékadásánál szigorú típusellenbjektumok metódusaira tudunk hivatkozni
  * Metódusreferencia értékadásánál szigorú típusellenőrzésen kell átesnie a metódusnak
  * Az eseménykezelés architektúrája megfelel az OOP alapelveinek
* Előnyei
  * Könnyen olvasható kód,nincs szükség új osztály definiálására az események kezeléséhez
  * Példány és osztály metódusok egyaránt értékül adhatók
  * Eseménykezelés nyelvi szintű támogatásából eredő előnyök

```csharp
    public class Storage
    {
        public delegate void StorageEventHandler(int number);
        
        //delegáltakkal
        public StorageEventHandler ItemInserted;
        public StorageEventHandler ItemDeleted;
        //eventtel
        public event StorageEventHandler ItemInserted;
        public event StorageEventHandler ItemDeleted;
        
        string[] array;

        public Storage()
        {
            array = new string[0];
        }


        public void AddItem(string item)
        {
            string[] newArray = new string[array.Length + 1];
            for (int i = 0; i < array.Length; i++)
            {
                newArray[i] = array[i];
            }
            newArray[newArray.Length - 1] = item;
            //Delegáltal
            ItemInserted(newArray.Length - 1);
            //eventtel
            ItemInserted?.Invoke(newArray.Length - 1);
            array = newArray;
        }

        public void DeleteItem(string item)
        {
            int i = 0;
            while (i < array.Length && !array[i].Equals(item))
            {
                i++;
            }
            if (i < array.Length)
            {
                string[] newArray = new string[array.Length - 1];
                for (int j = 0; j < i; j++)
                {
                    newArray[j] = array[j];
                }
                //Delegáltal
                ItemDeleted(i);
                //eventtel
                ItemDeleted?.Invoke(newArray.Length - 1);
                
                for (int j = i + 1; j < array.Length; j++)
                {
                    newArray[j-1] = array[j];
                }
                array = newArray;
            }
        }
    }
//ez is egy class ba megy
        static void InsertDisplay(int index)
        {
            Console.WriteLine("New item inserted at postion " + index);
        }

        static void DeleteDisplay(int index)
        {
            Console.WriteLine("New item deleted at postion " + index);
        }

        static void Main(string[] args)
        {
            Storage st = new Storage();
            // ez ha sokszor használod
            st.ItemInserted += InsertDisplay;
            // ez ha egyszer
            st.ItemInserted += (index) => Console.WriteLine("Elem hozzáadva ide: " + index);


            st.ItemDeleted += DeleteDisplay;
            st.ItemDeleted += delegate(int index) {
                Console.WriteLine("Elem törölve innen: " + index);
	        };
        }
```


