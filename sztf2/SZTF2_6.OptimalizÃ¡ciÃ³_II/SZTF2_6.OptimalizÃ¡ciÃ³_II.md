---
tags: sztf2
---
# SZTF2: 6.Optimalizáció II

## Feladat

* N darab részeredményt keresünk (E1, E2... EN)
* Mindegyikhez ismerjük a véges értékkészletet (pl. E1 hez ennekmérete M1, elemei: R1,1, R1,2, ... R1,M1)
*  ![](Images/SZTF2_6.Optimalizáció_II_1.png)

## Visszalépéses keresés

* A visszalépéses keresés olyan feladat típusoknál hatékonyan, ahol
  * A megoldandó feladat több, egymástól csak közvetve függő részfeladatokból ál
  * Már a részfeladatok egy részéből is megállapitható hogy nem fog teljes megoldáshoz jutni

### paraméterei

* Keresés bemenete:
  * N – megoldandó részfeladatok száma
  * Mszint – szint-edik részfeladat lehetséges részmegoldásainak a száma
  * Rszint,i – szint-edik részfeladat i. lehetséges részmegoldása
* Keresés kimenete:
  * van – van-e teljes megoldás?
  * E – részmegoldásokat tartalmazó vektor (Ei – az i. részmegoldás értéke)
* feladattól függően két fügvénnyel oldjuk meg
  * Ft(szint, s) – Egy függvény, ami azt határozza meg, hogy a szint-edik részfeladat esetében lehetséges megoldás-e az s?
  * Fk(szint, s, E) – Azt határozza meg, hogy a szint-edik részfeladat esetében választhatjuk-e az s rész megoldást az elöző megoldások fényében (E vektor)
* Rekurzív visszalépéses keresés
  *  ![](Images/SZTF2_6.Optimalizáció_II_2.png)

```csharp
static void Main(string[] args) {
	string[, ] R = new string[6, 3]; // ez reprezentálja magát a teljes "táblázatot"
	// FELADATKÖR 1
	R[0, 0] = "Miklós";
	R[0, 1] = "Klaudia";

	// FELADATKÖR 2
	R[1, 0] = "Zsolt";
	R[1, 1] = "Miklós";

	// FELADATKÖR 3
	R[2, 0] = "András";

	// FELADATKÖR 4
	R[3, 0] = "András";
	R[3, 1] = "Pál";
	R[3, 2] = "Zsolt";

	// FELADATKÖR 5
	R[4, 0] = "András";
	R[4, 1] = "Géza";

	// FELADATKÖR 6
	R[5, 0] = "Géza";
	R[5, 1] = "Miklós";

	// Mszint - egyes szinthez (feladatkörhöz) tartozó részmegoldások száma
	// megjegyzés: az indexelés miatt, itt eggyel kevesebb a számok értéke
	int[] M = {
		1,
		1,
		0,
		2,
		1,
		1
	};
	// Eredmények tömb (mérete megegyezik a szintek számával, hiszen minden szinthez találhatunk 1 megoldást)
	string[] E = new string[6];
	bool van = false;
	BTS(0, ref E, ref van, M, R);
	for (int i = 0; i < E.Length; i++) {
		Console.WriteLine(E[i]);
	}
	Console.ReadLine();
}
static bool Ft(int szint, string xEmber) {
	// A mi esetünkben mindig igazat ad vissza, mondván: X ember alkalmas Y feladatra
	return true;
}

static bool Fk(string[] eredmenyek, string ember, int szint) {
	for (int i = 0; i < szint; i++) // szint fontos, hogy csak eddig nézzük az eredményeket is
	if (eredmenyek[i] == ember){
		return false;
	}
	return true;
}

static void BTS(int szint, ref string[] E, ref bool van, int[] M, string[, ] R) {
	int i = -1;
	while (!van && i < M[szint]) {
	    i++;
		if (Ft(szint, R[szint, i])) // adott szinten i. ember alkalmas-e a szerepre
	    {
			if (Fk(E, R[szint, i], szint)) // adott ember foglalt-e már
			{
				E[szint] = R[szint, i];
				//elértük az utolsó szintet
				if (szint == (R.GetLength(0) - 1)){ 
					van = true;
				}
			    else BTS(szint + 1, ref E, ref van, M, R);
		    }
	    }
	}
}
```

*  ![](Images/SZTF2_6.Optimalizáció_II_3.png)
* Amennyiben az egyes részeredmények páronként kizáróak
  * Ezért a Fk-t kibővitjük E[k]-val és végig nézzük minden szinten hogy k szintedik megoldás és R[szint,i] együtt nem kizáróak-e.
  *  ![](Images/SZTF2_6.Optimalizáció_II_4.png)

### Visszalépéses kereséssel kezelhető problémák osztályozása

* Részfeladatok egymást kizárják
  * igen (ilyenkor használható az általános algoritmus)
  * nem (erre láttuk a második algoritmust)
* Részmegoldások száma
  * logikai változók – minden részfeladatnál két lehetőség közül választhatunk
  * M darab – több előre rögzített feltétel közül választhatunk
  * előzményfüggő – egy adott helyen a lehetséges választások száma attól függ, hogy az előző szinteken milyen értékeket választottunk (ilyenkor úgy kell az Ft függvényt módosítani, hogy az is hozzáférjen az eddigi részeredményekhez)
*  ![](Images/SZTF2_6.Optimalizáció_II_5.png)

## Más problémák

* 8 királynő a sakktáblán
  * Klasszikus feladat: helyezzünk el úgy 8 királynőt a sakktáblán, hogy azok ne üssék egymást
  * A visszalépéses keresés jól használható, mivel ha bármelyik két már lerakott királynő üti egymást, akkor nem is kell vizsgálni a többieket
  * Problématér átalakítása: minden oszlopba pontosan egy királynőt kell elhelyeznünk, így valójában 8 darab 1..8 közötti számot keresünk, ez már egyszerű egydimenziós probléma és ezt a kizáró algoritmussal meg lehet oldani
  *  ![](Images/SZTF2_6.Optimalizáció_II_6.png)
* Huszár útja a sakktáblán
  * Klasszikus feladat: egy fix kezdőpontból be lehet járni egy huszárral az egész táblát úgy, hogy minden mezőt pontosan egyszer érintünk?
  * Visszalépéses keresés jól használható, mivel egy n-edik rossz lépés után nem kell foglalkozni az utána következő (63-n) darabbal
  * Problématér átalakítása: tudjuk, hogy 63 lépésünk lesz, így a feladat valójában a 63 megfelelő irány megtalálása (minden lépésnél 8 lehetőség közül választhatunk)
* Sudoku feladat megoldó
  * Egy 9x9-es táblázat tartalmaz előre beírt és üres mezőket. Az n darab üres mezőt kell kitölteni az alábbi szabályok szerint
    * minden üres helyre egy szám írható 1..9 között
    * egy sorban, illetve egy oszlopban nem szerepelhet kétszer ugyanaz a szám
    * a tábla 3x3-as blokkokra oszlik, egy blokkon belül nem lehet kétszer ugyanaz
  * Visszalépéses keresés jól használható, mivel ha két mezőbe beírt érték kizárja egymást, akkor a többi mezőt nem is kell vizsgálni
  * Problématér átalakítása: maga a tábla kétdimenziós, azonban egy listába felsorolhatjuk az üres mezőket. Így megfelelően n darab részfeladatunk lesz, amelyekbe 1..9 közötti értéket keresünk

## Visszalépéses kereséssel optimalizálás

### Minden megoldás kiválogatása

* Az első megoldás után nem állunk meg, keressük a többit
   ![](Images/SZTF2_6.Optimalizáció_II_7.png)
   ![](Images/SZTF2_6.Optimalizáció_II_8.png)

### Optimális megoldás keresése

* Keresés helyett tulajdonképpen minimumkiválasztás
   ![](Images/SZTF2_6.Optimalizáció_II_9.png)
   ![](Images/SZTF2_6.Optimalizáció_II_10.png)

### Hátizsák probléma

* 0-1 hátizsák probléma
  * bemenete
    * Wmax:hátizsák mérete
    * N:rendelkezésre álló tárgyak száma
    * wi: i tárgy súlya
    * pi: i tárgy értéke
  * Kimenete
    * Pmax egy optimális pakolás értéke
* Pakolás: tárgyanként meghatározzuk, hogy bekerül-e a zsákba vagy sem
* Pakolás összsúlya/összértéke: a pakolás által a hátizsákba választott tárgyak súlyának/értékének összege
* Érvényes pakolás: olyan pakolás, amelynek összsúlya nem nagyobb mint a hátizsák mérete
* Optimális pakolás: olyan érvényes pakolás, amelyiknél nincs nagyobb összértékű érvényes pakolás

### Hátizsákba pakolás

* A visszalépéses keresés használható: ha néhány tárgy együtt nem fér a hátizsákba, akkor az ezen az úton nem kell tovább vizsgálódni
* Az algoritmus előzményfüggő változata használható:
  * N: tárgyak száma
  * Mi:
* Az összehasonlíthatóság érdekében egyszerűsített algoritmust írunk
  * Nincs külön Mi és Ri, hiszen minden szinten két részmegoldás van csak
  * Az Ft-t mindig igaznak tekintjük így el is hagyhatjuk (ha egy tárgy mégse férnebe a zsákba, akkor az Fk úgyis hamis lesz)
  * A VAN változóra sincs szükség, mivel a kiinduló állapot (semmit se rakunk a zsákba) egy biztosan érvényes pakolást tartalmaz. Megállási feltételre pedig nincs szükség az optimalizáció miatt.

### 0-1 hátizsák probléma megoldása „visszalépéses keresés” segítségével

 ![](Images/SZTF2_6.Optimalizáció_II_11.png)

### A „visszalépéses keresés” módszer értékelése

* Előnyei
  * Jól áttekinthető, általánosan használható módszer
  * Felesleges részfeladatok vizsgálatát elkerüli
* Hátrányai
  * A rekurzív hívások miatt az erőforrásigénye nagy lehet (persze megírható a backtrack iteratív formában is)
  * Az átfedő részfeladatok esetén elképzelhető, hogy többször megoldja ugyanazt a részfeladatot
  * Bizonyos feladatokra találhatunk jobb megoldást is

## Szétválasztás és korlátozás (branch and bound)

* Bizonyos feladatoknál a keresés közben nem csak azt tudjuk megállapítani, hogy egy úton lehet-e megoldás, hanem azt is, hogy ez a megoldás lehet-e jobb mint az eddig talált legjobb
* Ilyenkor használható jól a „szétválasztás és korlátozás” technika,amely során az alábbi két függvényre van szükség:
  * Szétválasztási függvény: ennek szerepe, hogy egy bonyolult feladatot szétbontson egyszerűbb részfeladatokra
  * Korlátozó függvény: ez próbálja megmondani, hogy egy részfeladat megoldásának érdemes-e nekiállni.
  * \
* A hátizsákpakolásnál pl. megnézhetjük, hogy ha az összes hátralévőtárgyat bele tudjuk rakni a megmaradt szabad helyre, akkor jobbmegoldást kapunk-e mint az eddigi legjobb
  * ez egy felső becslés, ha már ez se igaz, akkor nem is érdemes vizsgálódni erre
  * ha nem, akkor folytatni kell a rekurziót, hogy mi a tényleges részmegoldás erre
*  ![](Images/SZTF2_6.Optimalizáció_II_12.png)
* Előnyei
  * sok felesleges lépést tud meg sporólni
  * nem csak a rögzitett részeredményeken alapul hanem nem feldolgozott problémákat is figyelembe veszi
* Hátrányai
  * Csak megfelelően kiválasztott szétválasztó és korlátozó függvények esetében működik
  * \


