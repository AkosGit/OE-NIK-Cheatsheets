---
tags: opre
---
# Opre: Linux fájlrendszer és jogosultságok

- 3 partició: 
	- A gyökérpont: / -> ext4 alapból
	- Felhasználói mappák: /home -> ext4
	- Swap(memória kitoldása): swap tipusu

## Mappastruktúra
- /bin - felhasználói bináris fájlok: parancsok (pl: grep, ping, stb.) 
- /dev – eszközök fájljai: terminálok, hardverelemek (pl: lemezek, cpu) 
- /etc – konfigurációs fájlok (pl hálózat, gépnév, szolgáltatások) 
- /home – felhasználók saját mappái 
- /mnt – ideiglenes eszközök általános csatolási pontja
- /root – root (rendszergazda) felhasználó saját mappája 
- /var/log – a rendszer és a telepített szolgáltatások naplófájljai

## Mappa és fájlműveletek
- *ls* Mappa tartalmának kiírása
- *cd* Mappaváltás, vissza: cd ..
- *mkdir* Mappa létrehozása
- *rmdir* mappa törlése
- *cat* fájl kiirás
- *touch* fájl létrehozás 
- *mv* átnevezés mozgatás
- *rm* egy file törlése, rm -r rukerziv fájl és almappák
- *cp* másolás

## Háttértárak kezelése
- A fizikai háttértárak és részletes tulajdonságainak listázása: 
	- lshw –class disk
 - A hátértárinformációk és partíciók listázása:
	 - fdisk –l
	 - parted –l
- Partíciók listázása: lsblk
- Partíciók létrehozása, törlése: fdisk /dev/<disk név>
	- A parancs kiadása után betűk segítségével navigálhatunk a partíciók konfigurációja során
		- d - törlés
		- n - új létrehozása
		- p – elsődleges partíció meghatározása
		- w – partíciók változásainak mentése
- Fájlrendszer létrehozása: mkfs –t fájlrendszer(pl. ntfs) /dev/<disk név>

## Felhasználókezelés
- A mai operációs rendszerekre jellemzően többfelhasználósak. Ez nem csak azt jelenti, hogy több felhasználható is létrehozható benne, hanem, hogy konkurensen akár többen is lehetnek bejelentkezve. Ez alól a Linux sem kivétel.
- Linux-ban is van egy kiemelt felhasználó, ami sokkal több jogosultsággal rendelkezik már a létrehozásnál: root
- A root-ot megszemélyesíthetik azon a felhasználók, az /etc/sudoers konfigurációban felvett felhasználók.
- Fontos, hogy a Linux bár használ csoportneveket, a belépéshez pedig felhasználóneveket, de ez csak az könnyebb felhasználás érdekében van így, a rendszerben a felhasználókat és csoportokat általa generált ID-k alapján azonosítja.
### helyi fiókok menedzselése 
- A felhasználói fiókoknál megkülönböztetünk helyi és tartományi fiókokat. Az előbbi csak az adott számítógépen használható, még utóbbi több olyan számítógépen is, ami egy tartományba tartozik.
- felhasználok konfigurációs fájlja: **/etc/passwd**
- A rendszer a jelszavakat a **/etc/shadow** fájlban tárolja hash-elt alakban
- új felhasználó felvétele: **useradd**
- az igy felvett felhasználók home mappája a /home-ba található
- useradd paraméterei:
	- u – egyedi felhasználói azonosító
	- g – megadható a felhasználó elsődleges csoportja (név vagy ID is)
	- d – home mappa beállítása
### helyi fiókok jelszóváltoztatása, módosítása, törlése
- A felhasználók alapesetben jelszó nélkül jönnek létre, de nem „üres” jelszóval. A jelszó létrehozáshoz vagy váltáshoz a **passwd** parancsra van szükség
- Már létező felhasználó tulajdonságait (home mappa, fiók érvényességi idő, zárolás, elsődleges csoport, stb) meg lehet változtatni a **usermod** nevű parancs segítségével.
- sudo userdel  - felhasználó törlése
- sudo userdel –r  - felhasználó és home mappájának törlése
### helyi csoport kezelése, fiókok csoporthoz rendelése
- A csoportok szintén egy konfigurációs fájlban találhatók: **/etc/group** A létrehozáskor, módosításkor és törléskor ez a fájl módosul.
-  Csoport létrehozása: sudo groupadd <csoportónév>
-  Csoport törlése: sudo groupdel <csoportónév>
-  Felhasználó csoporthoz rendelése: 
	-  felhasználó elsődleges csoportjának módosítása: sudo usermod -g <csoportnév> <felhasználónév>
	-  felhasználó egy vagy több csoporthoz rendelése: sudo usermod -G <csoport vagy csoportok\> <felhasználónév>
## Partitions 
- view partitions
```bash 
sudo fdisk -l
```
- view unpartitioned disk space
```bash 
sudo sfdisk -F
```
- create partitions
	- sub commands
		1. **n** - for creation dilalog
		2. **p**: primary e:extended volume
		3. partition number
		4. first and last sector(default)
		5. **p**: check new information
		6. **w**: finalizing
```bash 
sudo fdisk <partiton> 
```
- delete partitions(fdisk)
	- sub commands
		1. **d**: start deletion dialog
		2. partition number
		3. **w**: finalizing
- format partitions 
	- mkfs.ext4 -> ext4
	- mkfs.ext3 → ext3 
	- mkfs.ntfs → NTFS
```bash 
sudo mkfs.* <partiton> 
```
- mount partition
```bash 
sudo mount <partiton> <folder>
```
- unmount partition
```bash 
sudo umount <folder>
```
# Fileand folder permissions
- 3 * 3 + 3 bit: deny or enable
- file permissions
	- Read
	- Write
	- eXecute
- folder permissions
	- **r** - list folder contents
	- **w**  - create,delete,rename file in folder
	- **x** - enter directory
- last 3 bit
	- SUID (Set User ID),SGID (Set Group ID): file inherits permissions from running user
	- Sticky mode: if permission set on folder contents can only be changed by owner
- show permissions
	- first character: 
		- **d**: folder
		- **-**: file
		- **l**: system link
	- 9 permission bit -> file,folder categorizes users
		- User
		- Group
		- Other: everyone else
```bash 
ls -l <file path>
stat <file or folder path>
```
- set permissions
	- target classifier
		- **nothing**: owner
		- **g**:group
		- **o**: other
	- allow(+) or deny(-)
```bash 
chmod <target><+/-><rwx> <path>
 ```
 - another version
	 - sum of permissions
		 - Read=4
		 - Write=2
		 - execute=1
```bash 
chmod <sum of owner><group><other>
 ```
 - change file/folder ownership
 ```bash 
chown <user> <path>
 ```
  - change file/folder group
 ```bash 
chown :<group> <path>
 ```
 - change file/folder ownership and group
  ```bash 
chown <user>:<group> <path>
 ```