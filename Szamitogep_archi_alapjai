## Konzol kezelés, stringek, pointerek
### Kurzor, és tisztitás
- cursor pozicionálás => koordináták: 24 sor 79 oszlop
	- TECH Topics >> INTs & BIOS Services >> 10H Video services >> 02H set cursor position
	- kezdeti pozició 0,0 ball felsö sarok: 24 sor és 79 oszlop
    ```nasm
	mov ah,2 ; cursor position
	MOV DH,13 ; 13.sor
	MOV DL,41 ;41. oszlop
	INT 10h	
    ``` 
- tisztitás és karakter megtartása
	- tisztitás: dosbox 3-as video mode
	- TECH Topics >> INTs & BIOS Services >> 10H Video services >> 00H set video mode
	```nasm
	mov ax, 03; tisztitás => AH=0 AL=3
	int 10h
	```

### Beolvasás
- Karakter bekérés: AL-be kerül
	- TECH Topics >> INTs & BIOS Services >> 16H Keyboard I/O >> 00H Read Keyboard 
	```nasm
	mov ah, 0
	int 16h
	```
- Nem megálitó karakter bekérés: AL-be kerül
	```nasm
	mov ah, 01h
	int 16h	
	```
	- karakter bekérés: nem vár => if-re jó
	- Vizsgálja hogy a key bufferben vannak e karakterek, ha igen a zero flag 1 re áll
	```nasm
	mov cl, ' '				; Kezdeti karakter egy space, eleinte ezt fogja csak kiírni
	Ciklus:
		mov ah, 02h			;\
		mov dl, cl			; | Elmentett karakter kiíratása
		int 21h	;/
		
		mov ah, 01h			;\
		int 16h				; | Vizsgálja hogy a key bufferben vannak e karakterek, ha igen a zero flag 1 re áll
		
		jnz KarakterMent	;/
		jmp Ciklus
	
	KarakterMent:
		mov ah, 00h			;\ A bufferből kivesszük a karaktert, belekerül az "al" regiszterbe
		int 16h				;/
	```
### Kiirás 
- Betü kiirás
	- TECH Topics >> DOS Functions Quick Ref >> 02H Display Char
	- 5 féle módon
		1. decimálisan megadva: mov dl, 48
		2. hexadecimálisan: mov dl, 30h
		3. binárisan: mov dl, 00110000b
		4. karakter megadásával:
		5. mov dl, '0'
```nasm
	mov AH, 02H
	mov DL, 'A'
	int 21h
```
- sztring változó
	```nasm
	EGYENLOS db "EGYENLO$" ; Code alján
	```
- kiirás változóval
	- TECH Topics >> DOS Functions Quick Ref >> 09H Display Text
	- OFFSET: cim lekérése
	- mov dx, offset uzenet ;ekkor a "dx" az "uzenet" címére fog mutatni
	- ekkor a "dx" értékét ha lekérjük akkor az a "v"-re fog mutatni az uzenetben
	- Ezt az értéket a "\[]" operátorral lehet lekérni.
	```nasm
	mov ah,9 ;dos api
	mov dx, OFFSET EGYENLOS
	int 21h ;megszakitás
	```
### Pointerek
- mov di, offset ertek ;"di" eddig az a ra mutatott
	```nasm
	inc di ;"di" mostmár a b re mutat
	ertek db "ab$"
	```
- Helyi string mint változó
	```nasm
	mov di, offset valtozo 
	mov al, [di] ; betöltés AL-be 
	; művelet az AL regiszterrel 
	mov [di], al ; visszamentés AL-be 
	
	valtozo: db "0$"
	```
	- ![[Pasted image 20221020141340.png]]
- Különbség "mov ax, 10" és "mov \[ax], 10" között.
	- mov ax, 10: Ebben az esetben a 10 az "ax" regiszterbe kerül.
	- mov \[ax], 10: Ebben az esetben a 10 oda kerül ahova az "ax" pointer mutat. Itt az "ax" csak egy mutató egy cím felé.
- Használható szövegek bejárására is. Például olyan feladatokhoz hogy egy adott szövegben mennyi "x" betű van.
	```nasm
	mov cx, 0 ;"di" regiszterben fogjuk eltárolni hogy éppen hanyadik betűnél járunk
	Vizsg:
		mov di, offset uzenet ;Beállítjuk a "di" regisztert hogy az a "uzenet" kezdetére mutasson 
		add di, cx ;"hozzáadjuk a mutatóhoz hogy hanyadik betűnél jártunk"
		mov al, 'e'	; \
		cmp [di], al;  > Megvizsgáljuk hogy az éppen kijelölt betű 'e'-e, ha igen ugrunk a "Novel" kódrészhez
		jz Novel	; /
		mov al, '$'		;\
		cmp [di], al	; > Megvizsgáljuk hogy a szöveg végéhez értünk e. Ha igen akkor elugrunk a program végéhez.
		jz Program_Vege	;/
		inc cx ;Növeljük a "cx" regiszter érétkét, ezzel egyel tovább haladunk a betűvizsgálatban
		jmp Vizsg ;Ha se nem 'e' se nem '$' akkor folytatjuk tovább a vizsgálatot
	
	Novel:
		mov di, offset ertek	;\
		mov al, [di]			; \ Növeljük a számláló értékét 
		inc al					; /
		mov [di], al			;/
		inc cx ;Növeljük a betűszámláló értékét
		jmp Vizsg ;Visszaugrunk a vizsgálatba és folytatódik tovább a program
	
	```
- számlálás
	- ha egy szöveg a következő "0" ha ezt megnöveljük akkor "1" lesz
	```nasm
	mov di, offset ;{NÖVELENDŐ SZÖVEG}
	mov al, [di]
	inc al ;ha pl 2-vel szeretnénk növelni a karaktert akkor ennek a sor helyére kerülhet pl hogy "add al, 2"
	mov [di], al
	```
- Példa számlálás 9-nél nagyobb számokkal
	```nasm
	ertek db "00$"
	mov cx, 90; ;Loop beállítása
	Novel:
		mov dx, offset ertek ; \
		mov ah, 09			 ;  \
		int 21h				 ;   \
		mov ah, 02			 ;    > Kiíratás
		mov dl, ' '			 ;   /
		int 21h				 ;  /
	
		mov di, offset ertek	; \
		inc di					;  \
		mov al, [di]			;   > Érték második karakterjének a léptetése
		inc al					;  /
		mov [di], al			; /
	
		mov ah, '9'				;\ Ha a második karakter 9 és növelni szeretnénk akkor az első karakter értéke növelődik 1-el
		cmp ah, al				;/ 
		js Leptet
		loop Novel				;Loop vissza a "Novel" kódrész kezdetéhez
		jmp Program_Vege		;Loop után vége a programnak
		
	Leptet:
		mov di, offset ertek	;\
		mov al, [di]			; > Első karakter megnövelése
		inc al					;/
		
		mov [di], al			;\
		inc di					; \
		mov al, [di]			;  > Második karakterből kivonunk 10-et, ezzel visszaállítva annak az értékét '0'-ra
		sub al, 10				; /
		mov [di], al			;/
		
		jmp Novel				; Visszaugrás a "Novel" kódrészhez
	```
- memória foglalás: dw/dd/db
- tömb foglalás
	```nasm
	tomb dd 1,5,32768
	```
- kódszegmensben adat:
	-   végére kell rakni
	-  string tárolás: vneve db(egy bájt) “string$”
## Adatstruktúrák
### Verem
-  *push/pop*: Ezzel a két művelettel lehet regiszterek értékét a verembe(stack)-be mozgatni, illetve kiszedni azokat onnan egy megadott regiszterbe.
- A verembe csak teljes regisztereket lehet mozgatni, nem lehet külön az "xl" vagy a "xh" regisztereket
- A verem LIFO elven működik, ez azt jelenti hogy azt pop oljuk amit utoljára pusholtunk
- Ha pop olunk akkor a popolt érték felülírja a megadott regiszter tartalmát
- Akkor hasznos ha pl. valamilyen utasítást akarunk elvégezni de a számunkra fontos eredmény értéke abban a regiszterben található ahová az utasítás paramétereit kell mozgatni.
- Pl.: Stringet megforditjuk veremmel. Ha a karaktereket sorrendben rakjuk be és közben számoljuk a karakterek számát akkor ha egy loopal addig megyünk ahány karakter van és a loopban kiveszünk egy karaktert a veremböl az már forditott sorrendben lesz
```nasm
Start:
	mov ax, Code
	mov DS, AX

	MOV DI,OFFSET SZTRING
	MOV DX,DI
	MOV AH,9
	INT 21h
	
	XOR CX,CX
ELTAROL:	
	MOV AL,[DI]
	CMP AL,'$'
	JE TOVABB
	PUSH AX
	INC CX		;MEGSZAMOLJUK CX-BEN A KARAKTEREK SZAMAT
	INC DI
	JMP ELTAROL
	
TOVABB:
	MOV DI,OFFSET SZTRING
VISSZATESZ:
	POP AX
	MOV [DI],AL
	INC DI
	LOOP VISSZATESZ

	MOV AH,2
	MOV DL,10
	INT 21h
	MOV DL,13
	INT 21h

	MOV DX,OFFSET SZTRING
	MOV AH,9
	INT 21h
```
## Müveletek
- mov:
	- Egyik memória területről a másikba való másolás
	- mov {HOVA}, {MIT}
### Elágazások és aritmetikai müveletek
- szorzás => mul {register}
	- ax vagy al-t szorozza össze a mul utánival és ax-t érdemes használni hogy beleférjen
	- Ha 2 16-bites szám szorzása az alábbi példa alapján nagyobb számot eredményezne akkor a szám also 16 bitje az "ax" regiszterben lesz megtalálható a felső része a "dx" regiszterben
	- 16 bit szorzás
	```nasm
	mov ax, 5
	mov dx, 10
	mul dx ; ax = ax * dx -> ax = 50

	mov cx, 2

	mul cx ; ax = ax * cx -> cx = 100
	```
	- 8 bit szorzás
	```nasm
	mov al, 2
	mov dl, 2
	mul dl ; al = al * dl	
	```
- osztás => div
	 - 8-bites szám osztása 8-bites számmal, maradékkal 
	 ```nasm
	mov al, 16
	mov dl, 7
	div dl ; al = (al / dl) -> al = 2, ah = 2
	```
	- 16-bites szám osztása 8-bites számmal ("al" regiszterben fog eltárolódni az osztás egész értéke, "ah" reg.-ben pedig az osztás maradéka)
	```nasm
	mov dx, di	
	mov ax, 16
	mov dl, 8
	div dl ; al = ax / dl -> al = 2
	```
	- 16-bites szám osztása 16-bites számmal, maradékkal 
	 ```nasm
	mov ax, 16
	mov bx, 7
	div bx ; ax = (ax / bx) -> ax = 2, dx = 2
	```
-   összeadás: add {MIHEZ}, {MIT}
	- add bl, 2 ;-> bl += 2
-   kivonás: sub {MIBŐL}, {MIT}
	- sub bl, 2 ;-> bl -= 2
	- használható ASCII szám konvertáláshoz: sub {ASCII szám}, 48
- Register inkrementálás
	- inc {MIT}
- Register dekrementálás
	- dec {MIT}
-   befolyásolt flagek (státusz): A, C, O, P, S, Z
- C: carry flag => ha van maradék
	- this flag is set to **1** when there is an **unsigned overflow**. For example when you add bytes **255 + 1** (result is not in range 0...255). When there is no overflow this flag is set to **0**.
- P: parity flag => 0 az értéke ha páros, 1 ha páratlan
- Z: zero flag => ha kivonás eredménye 0 csak ez lesz aktiv
- S: sign flag => az eredmény negativ akkor 1
- O: overflow: => egy matematikai művelet eredménye meghaladja a kiszabott tartományt, akkor 1-re áll
	- set to **1** when there is a **signed overflow**. For example, when you add bytes **100 + 50** (result is not in range -128...127).
- CMP AX, BX (AX - BX)

|         | **C**arry flag | **Z**ero flag | **S**ign flag |
| ------- | -------------- | ------------- | ------------- |
| AX = BX | 0              | 1             | 0             |
| AX > BX | 0              | 0             | 0             |
| AX < BX | 1              | 0             | 1             |
- Elválasztás implementálása
	- ugrálás a kódban cmp feltétellel
	- cmp cél, forrás: kivonja a célból a forrást
	- jump flagek állit be: JZ,JNZ, JC, JNC
		- JZ,E: ZF=1 => egyenlö
		- JC,JB(below): CF =1 => kissebb
		- JL : jump if lesser
		- JG: ZF=0 és SF=0 => nagyobb
		- JNZ: megfelel a "!="-nek
		- JNC: CF=0
		- JE/JNE = Equal, ez kb ugyan az mint a JZ/JNZ
		- JO/JNO = Overflow
		- JS/JNS = Signed
	- feltételes ugrás végrehajtása – J(feltétel) – feltétel lehet pl. a jelzőbitek
	- ugrási távolság -128 -+ 127 bájt => megoldás jump pontok létrehozása
	- if végére: JMP rögtöni ugrás a végére => hogy túl ugorjuk a feltételeket
- ha minden ág sztring kiirás ki lehet hagyni  
### Bitenkénti müveletek 
- müveletek: AND,OR,XOR, SHL, SHR, ROR, ROL
- Maszkolás: OR, AND, XOR
	- or,and helyiértékenként dolgozik
	- OR 1 maszk => 1-re állitani biteket
	- AND: 0 maszk => 0-ra állitás 
	- XOR: kinulázás
	```nasm
	    00110011b
	    01010101b
	XOR 01100110b ;A XOR A = 0 
	OR  01110111b ;A OR 0 = A, A OR 1 = 1 használat: 1 maszk => 1-re állitani biteket
	AND 00010001b ;A AND 0 = 0, A AND 1 = A
	```
- XOR	
	- csak akkor 1, ha különböznek a bit értékei
	- komplemens képzésre lehet használni
-   SHL: bitléptetés balra, szorozva 2-vel, belépő bit 0
	- ![[Pasted image 20220920163805.png]]
	- ![[Pasted image 20220920163822.png]]
-   SHR: bitléptetés jobbra, osztva 2-vel, belépő bit 0
	```nasm
	shr ax, 1 ; AX=AX/2
	```
	- ![[Pasted image 20220920164040.png]]
- ROL: Rotate left => belépő bit a kilépő bit
	- ![[Pasted image 20220920164132.png]]
- ROR: rotate right => kilépö bit a belépö bit
	- ![[Pasted image 20220920164151.png]]
- RCL:
	- Az LSB bitet(balról a legelsö) A Carry Flagbe kerül át a többi bitek meg balra kerülnek egyel és a jobb oldali üres helyre a Carry flagbe tárolt bit kerül  
	- ![[Pasted image 20221118141617.jpg]]
- RCR:
	- Az MSB bitet(jobbról a legelsö) A Carry Flagbe kerül át a többi bitek meg jobra kerülnek egyel és a ball oldali üres helyre a Carry flagbe tárolt bit kerül  
	- ![[Pasted image 20221118141652.jpg]]
### Ciklusok
- loop
	```nasm
	    mov CX, 5 ; hányszor fog lefutni
	CIKLUS:
		mov AH, 2 ;csillag kiirás
		mov DL, '*'
		int 21h
		loop CIKLUS
		mov CX, 6 ;következö ciklus, ciklus változója
	
	```
	- loop röviditése:
	    - dec cx 
	    - jnz cimke
- végtelen ciklus ameddig esc-re kilépés
    - ha nem ASCII karakter akkor scan kódja van
    ```nasm
    BEKER: ;ciklus	
		XOR AH,AH ; ah kinullázás
		int 16h ; bill bekérés, ASCCI kod AL-bekérés
		CMP AL, 27 ; ha esc(27) akkor kilép
		JE Program_Vege
		JMP BEKER
    ``` 
- Változó növelése AL-való áttöltéssel
	```asm
	mov si,OFFSET VALTOZO ; változó betöltés 
	mov AL, [SI] ; beirjuk AL-be
	INC AL ;megnöveljük 
	MOV [SI], AL ;visszairjuk
	```
## Időzítők
- Óra beolvasás
	- TECH Topics >> INTs & BIOS Services >> 1aH Time I/0 >> 00H read the clock (tics)
	- A gép órájának az értékét tick-ben beletölti a "CX:DX" regiszterekbe, DX-be
	- 1 tick ≈ 0,0549 mp
	- 1 mp ≈ 1	cmp cx, 58.2 tick (18)
	- 1 perc ≈ 1092 tick
	- 1 óra ≈ 65543
	- 1 nap ≈ 1573040
	```nasm
	xor ah, ah ;\ 
	int 1ah    ;/ Óra bekerül a "CX:DX" regiszterbe
	```
- eltelt idő számítása
	- Eltelt időt 2 időpont különbségével tudjuk számolni
	```nasm
	xor ah, ah
	int 1ah
	mov bx, dx

	xor ah, ah
	int 1ah
	
	sub dx, bx ; "dx" regiszter eredménye az eltelt tickek száma
	```
## Grafikus műveletek
- Grafikus módban a munkaterünk 320x200-as. Az indexelés a bal felső saroktól kezdődik, ennek az értéke  \[0;0\], első sor utolsó értéke 319.
- ![[Pasted image 20221110174539.png]]
- Az egyes pixelek helye, színe az ES:DI regiszterekben vannak eltárolva.
- 256 különböző szín lehetséges.
- A pixelek értékei folyamatosan vannak eltárolval, ez azt jelenti hogy a 320. érték a második sor első pixelét jelenti már.
- Kezdő pixel értékét így lehet megadni: $y*320+x$ 
- Ha az X koordinátát szeretnénk módosítani akkor a "di" regiszter értékét vagy csökkentjük, vagy növeljük 1-el.
- Ha az Y koordinátát szeretnénk módosítani akkor a "di" regiszter értékét vagy csökkentjük, vagy növeljük 320-al.
- vissza kell váltani text mode müveletek müködéséhez (pl. képernyö tisztitás) int 16h-al és utána vissza grafikusba
- grafikus módba váltás
	- Ha grafikus módba váltunk akkor utána mielőtt vége lesz a programunknak vissza kell lépni karakteres módba
	- TECH Topics >> INTs & BIOS Services >> 10H Video services >> AH: 0, AL: 13
- pixel színe
	```nasm
	mov al, 129 ; "al"-regiszter értékével lehet megadni a színeket
	```
- pixel rajzolása
	```nasm
	mov ax, 13h ;grafikus módba váltás
	int 10h 
	mov ax, 0a000h ;szegmens váltáshoz szükséges, fontos hogy kis "a"
	mov es, ax ;berakjuk a szegmens regiszterbe 
	
	mov ax, 100 ; y=100 
	
	mov bx, 320 ; szorzáshoz kell 
	mul bx ; ax*bx → y*320 eredmény: DX és AX ilyen sorrendben (itt elég az ax, mert "belefér") 

	;kezdö pixel számolás x=160
	add ax, 160 ; → y*320 + x 
	mov di, ax ; di = 320*y + x
	
	mov al, 128 ;színválasztás 
	mov es: [di], al ; szegmensben elmentjük a változásokat 
	
	Program_vege: ; billentyű leütésre várakozás, hogy lássunk is valamit 
		xor ax, ax 
		int 16h ; visszaváltás karakteres üzemmódba: (fontos!) 
		mov ax, 3 
		int 10h
	```
	-  ![[Pasted image 20221020133930.png]]
	- ![[Pasted image 20221020134058.png]]
- téglalap 2 egymásba ágyazott looppal
	```nasm
		MOV AX,13H
		INT 10H		;atmegyunk grafikus modba
	
		MOV AX,0A000h
		MOV ES,AX
	
		MOV BL,100	;Y
		MOV BH,2	;SZIN
	
		MOV CX,30
	KULSOCIKLUS:
		MOV AX,160	;X
	
		PUSH CX ;30 mentése
		MOV CX,50
	CIKLUS:
		CALL PUTPIXEL	;PUSH IP, JMP PUTPIXEL
		INC AX
		LOOP CIKLUS ;50-szer x vagyis oszlop növelés
		POP CX
		INC BL			;30-szor KOVETKEZO SOR
		LOOP KULSOCIKLUS
	PUTPIXEL:
		PUSH BX; állapot mentés
		PUSH DX
		PUSH DI
		PUSH AX
	
		MOV DI, BX ;ELMENTJUK A SZINT
	
		MOV AX,320
		XOR BH,BH	;BX = Y -> y bl-ben van ezért ha kinullázzuk bh akkor bx-ben lesz
  MUL BX		;AX = 320 * Y
		POP BX		;BX = X
		PUSH BX		;
		ADD AX,BX	; AX = 320* Y +X
	
		MOV BX,DI	;BX-BE VISSZARAKJUK A SZINT
		MOV DI,AX	;DI-VE A KISZAMOLT MEMORIACIM
	
		MOV ES:[DI],BH
	
		POP AX
		POP DI
		POP DX
		POP BX
		RET				;POP IP
  ```
- grafikus módban billentyű leütések észlelése
	- Pontosan ugyan úgy történik mint karakteres módban, viszont figyelni kell rá hogy egyes karakterek észeléséhez azoknak a "scancode"-ját kell használnunk, ezek is ugyan úgy megtalálhatóak a helper programban.
## Függvények
- Hasonlóak mint a ":"-al ellátott kódrészek, viszont itt vissza is ugrunk oda ahol eddig a kód tartott.
- Értékeket átadni, regiszterek segítségével lehet legegyszerűbben.
- Függvény meghívása, és deklarálása:
```nasm
	call Fuggveny
	;következő utasítás ami a föggvény lefutása után fog lefutni. A program halad tovább innentől.
;ad az "ax"-regiszterhez 2-t, majd kiírja az értékét
Fuggveny:
	push dx ;"dx"-regiszter érzékét stackbe elmentjük mivel nem szeretnénk hogy ennek az értéke módosuljon függvény lefutásakor.
	add ax, 2
	push ax
	mov ah, 02h
	mov dx, ax
	int 21h

	pop ax
	pop dx
	ret ;visszatér a "call Fuggveny" sor utáni utasításra és folytatódik tovább a program
```
## Átalakitások (kiirás egy bizonyos számrendszerbe)
- Bináris decimális átalakitás RCL utasitással (nem kell osztani) 
	```nasm
	mov si,47994
	mov cx,16 ;16 bites szám
	Binaris:
		rcl si,1 ;Baloldali bit Carry flagbe
		jc Egyes ;jump ha van carry vagy ha CF=1
		mov dl,"0"
		mov ah,02h
		int 21h
		loop Binaris
		jmp Tovabb
	Egyes:
		mov dl,"1"
		mov ah,02h
		int 21h
		dec cx ;jump miatt nem ér el a loop utasitáshoz
		jmp Binaris
	Tovabb:
	```
- Hexa decimális átalakitás
```nasm
HexaPre:
	xor ax,ax
	int 16h
	
	mov ax,03h
	int 10h
Hexa:
	mov si,47985
	
	mov bx,si
	and bx,1111000000000000b
	mov cl,12 ;12 db 0 az jobbról ballra 
	shr bx,cl ;bitlépés jobbra 12-t
	
	call Kiir
	
	mov bx,si
	and bx,0000111100000000b
	mov cl,8 ;8 db 0 az jobbról ballra
	shr bx,cl ;bitlépés jobbra 8-t
	
	call Kiir
	
	mov bx,si
	and bx,0000000011110000b
	mov cl,4
	shr bx,cl
	
	call Kiir
	
	mov bx,si
	and bx,0000000000001111b
	
	call Kiir
Kiir:
	cmp bl,9
	jg KiirBetu
	
	add bl,48 ;szám karakterek 48-nál kezdödnek
	mov ah,02h
	mov dl,bl
	int 21h
	ret
KiirBetu:
	add bl,55
	mov ah,02h
	mov dl,bl
	int 21h
```
- Okta átalakitás
```nasm
Okta:
	xor ax,ax
	int 16h
	
	mov ax,03h
	int 10h
	
	mov si,58642
	
	mov bx,si
	and bx, 1000000000000000b
	mov cl,15
	shr bx,cl
	
	call Kiir2
	
	mov bx,si
	and bx, 0111000000000000b
	mov cl,12
	shr bx,cl
	
	call Kiir2


	mov bx,si
	and bx, 0000111000000000b
	mov cl,9
	shr bx,cl
	
	call Kiir2
	

	mov bx,si
	and bx, 0000000111000000b
	mov cl,6
	shr bx,cl
	
	call Kiir2
	

	mov bx,si
	and bx, 0000000000111000b
	mov cl,3
	shr bx,cl
	
	call Kiir2
	

	mov bx,si
	and bx, 0000000000000111b
	
	call Kiir2
Kiir2:
	add bl,48 ;szám karakterek 48-nál kezdödnek
	mov dl,bl
	mov ah,02h
	int 21h
	ret
```
- Kiirás decimális formában
	- helyiértékenként való kiirás, a helyiértékeket osztással választjuk le
	- aztán az egyeseket pedig a 10-essel való osztás maradékából olvassuk ki
```nasm
Decimalis:
	xor ax,ax
	xor dx,dx
	
	mov si,56781
	
	mov ax,si 
	mov bx,10000 ;helyiérték
	div bx ;elosztás helyiértékkel ax-ben érték 
	
	push dx 
	call Kiir3
	pop dx
	
	mov ax,dx
	xor dx,dx
	mov bx,1000
	div bx
	
	push dx
	call Kiir3
	pop dx
	
	mov ax,dx
	xor dx,dx
	mov bx,100
	div bx
	
	push dx
	call Kiir3
	pop dx
	
	mov ax,dx
	xor dx,dx
	mov bx,10
	div bx
	
	push dx
	call Kiir3
	pop dx
	
	mov ax,dx ;10-es helyérték maradéka dx-ben ami az 1-es helyiértéket jelenti
	
	call Kiir3
Kiir3:
	add al,48 ;szám karakterek 48-nál kezdödnek
	mov dl,al ;ax-en belül al-be kerül a szám
	mov ah,02h
	int 21h
	xor dx,dx
	ret
```
## Egyéb műveletek
- paraméterek kezelése
	- PSP argument beadás 0dh-val (13) végzödik
		- “/”-jellel kell kezdeni
		- alaból ds-be kerül a Code segmens igy nem fog müködni mert ds-be kerül az args
		- mov ax, Code, mov Bx, AX helyett: mov DI,81h
		- ha végeztünk vissza kell álliani hogy tudjunk változókat használni
		- DS-be nem lehet konstanst tölteni
	- Paraméterek azok a dolgok amelyek program elindítása után a futtatható tartomány után írunk.
	- Figyelni kell hogy a "start" részébe a programnak "mov di, 81h"-t kell írni. majd ha végeztünk a paraméter vizsgálattal vissza kell ezt állítani az alapértelmezettre.
	- pl: program.exe {VALAMILYEN PARAMÉTER}
	- A beolvasott paraméterek a "di" regiszterben tárolódnak el.
	- A ennek a karaktersorozatnak az utolsó karaktere egy "0Dh", ezt lehet arra használni hogy kiderítsük hogy a beovlasott paraméterek végére értünk-e.
	- Paraméterek vizsgálata, pl: '/' jel után megadott karakter kiolvasása:
	- VISZONT a 
		- mov ax, Code, mov	DS, AX visszálitja a programmot
	```nasm
	Code	Segment
	assume CS:Code, DS:Data, SS:Stack

	Start:
			mov di, 81h ;Paraméterkeresés indítása
		Keres:
			mov dl, [di]
			cmp dl, '/'
			jz ParamKezel
			cmp dl, 0Dh
			jz NincsParam
			inc di
			jmp Keres
		
		ParamKezel:
			;dl eltárolta a paraméter értékét
			inc di
			mov dl, [di]
			mov ah, 02h
			int 21h
			jmp Init
		
		NincsParam:
			mov ax, Code
			mov DS, AX
			mov dx, offset nincs
			mov ah, 09h
			int 21h
			jmp Program_Vege
		
		Init:
			mov	ax, Code ;Alapértelmezett beállítások visszaállítása paraméter kezelés után
			mov	DS, AX
			nincs db "Nincs parameter!$"
	```
- bináris szám decimálissá alakítása
	- Osztáson alapul a folyamat.
	- Osztásnál az "ax"-regisztert osztjuk a megadott másik regiszterrel.
	- Az eredmény egész része az "al"-ban fog eltárolódni, az osztás maradéka pedig az "ah"-ba kerül.
	- Majd kiíratjuk/eltároljuk az "al"-t és tovább dolgozunk a maradékkal. 
	```nasm
	mov ax, 153 ; alap értéked amit ki szeretnénk iratni
	mov dl, 100 ; 100-al fogjuk elosztani ax-t
	div dl ;ax -> [1|53]

	push ax

	mov dl, al ; kiíratjuk az 1-et
	add dl, '0'
	mov ah, 02h
	int 21h

	pop ax

	mov al, ah ;\ ax értékét a maradékra állítjuk
	xor ah, ah ;/

	mov dl, 10
	div dl ; ax -> [5|3]

	;kiíratás
	push ax
	mov ah, 02h
	mov dl, al
	add dl, '0'
	int 21h

	pop ax
	mov dl, ah
	mov ah, 02h
	add dl, '0'
	int 21h

	; működése:
	; ax értéke 153
	; ax/100 -> 1 | 53 maradék
	; eredmény kiíratása
	; maradék / 10 -> 5 | 3 maradék
	; eredmény kiíratása
	; maradék kiíratása
	```
