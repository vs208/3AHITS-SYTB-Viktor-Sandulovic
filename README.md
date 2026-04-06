# Arbeitsbericht: Variablen und Shell Scripts

**Klasse:** 3AHITS  
**Name:** Viktor Sandulovic  
**Fach:** SYTB  
**Datum:** 24.03.2026  

---

### Übung (Variablen)

```bash
DIR1="ordner1"
DIR2="ordner2"
FNAME="datei"
EXT="txt"

mkdir -p "$DIR1/$DIR2"

echo "Hallo Welt" > "$DIR1/$DIR2/${FNAME}.${EXT}"

cat "$DIR1/$DIR2/${FNAME}.${EXT}"

#Übung (Begrüßung)
echo "Name?:"
read NAME
echo "Hallo $NAME"
echo "Schön, dass du da bist."

#Übung (Zeilenumbruch)
MSG="Name: Viktor\nKlasse: 3AHITS\nRaum: ..."
echo -e "$MSG"

#Übung (admin)
USER="admin"
echo "${USER}_backup"
echo "${USER}_2026"

#Übung (admin)
DIR="/var/log/"
echo "${DIR}nginx"
echo "${DIR}apache"

#Übung (backup)
BASE="backup"
DATE="2026-01-16"
echo "${BASE}_${DATE}.tar"
echo "${BASE}_${DATE}.tar.gz"

