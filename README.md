# Arbeitsbericht: Shell-Script Übungen (If-Statements)

**Klasse:** 3AHITS  
**Name:** Viktor Sandulovic  
**Fach:** ITSE  
**Datum:** 05.05.2026   
**Aufgabenstellung:** If-Statements und Bedingungen


## 1. Übung: Maximum

### Aufgabenstellung
Schreibe ein bash Skript das 2 Zahlen als Argumente aus der Kommandozeile übernimmt. Die größere der beiden Zahlen soll ausgegeben werden.

### Lösung

**Script: `maximum.sh`**
```bash
#!/bin/bash

ZAHL1=$1
ZAHL2=$2

if (( ZAHL1 > ZAHL2 ))
then
    echo "$ZAHL1"
else
    echo "$ZAHL2"
fi
```

---

## 2. Übung: Un-/Gerade

### Aufgabenstellung
Schreibe ein Script das von einer als Argument übergebenen Zahl prüft ob sie gerade oder ungerade ist.

### Lösung

**Script: `ungerade.sh`**
```bash
#!/bin/bash

ZAHL=$1

if (( ZAHL % 2 == 0 ))
then
    echo "Die Zahl $ZAHL ist gerade."
else
    echo "Die Zahl $ZAHL ist ungerade."
fi
```

---

## 3. Übung: Directory

### Aufgabenstellung
Schreibe ein Skript `makedir.sh` das mit einem Argument aufgerufen wird.

```bash
$ ./makedir.sh xyz
```

Ein Directory mit dem Namen `xyz` soll angelegt werden falls es nicht existiert. Im Directory lege eine Datei mit dem Namen `xyz.txt` und Inhalt `xyz` an.

Existiert das Directory bereits so soll gefragt werden ob das Directory gelöscht werden darf. Bei dieser Abfrage soll angegeben werden wie viele Files sich im Directory befinden.

```text
Soll das Directory "xyz" (mit 5 Files) gelöscht werden? [j|n]: 
```

Auswahl `j`: Das Directory wird gelöscht und wieder wie oben angelegt.  
Auswahl `n`: Das Skript wird beendet.

### Lösung

**Script: `makedir.sh`**
```bash
#!/bin/bash

DIR=$1

if [ -d "$DIR" ]
then
    ANZAHL=$(ls -1 "$DIR" | wc -l)
    
    echo "Soll das Directory $DIR (mit $ANZAHL Files) gelöscht werden? [j|n]: "
    read ANTWORT
    
    if [ "$ANTWORT" = "j" ]
    then
        rm -r "$DIR"
        mkdir "$DIR"
        echo "$DIR" > "$DIR/$DIR.txt"
        echo "Directory wurde angelegt."
    else
        exit 0
    fi
else
    mkdir "$DIR"
    echo "$DIR" > "$DIR/$DIR.txt"
    echo "Directory wurde angelegt."
fi
```

---

## 4. Übung: number lines script

### Aufgabenstellung
Schreibe ein Skript `number.sh` das von der Kommandozeile aus aufgerufen werden kann.

Angenommen es gibt die Datei `test.txt` mit Inhalt:
```text
bbb eins
ccc zwei
aaa drei
```

Grundsätzlich kann das Skript mit einer Datei als Argument ausgeführt werden:
```bash
$./number.sh test.txt$ cat test.txt
     1	bbb eins
     2	ccc zwei
     3	aaa drei
```
Es werden die einzelnen Zeilen nummeriert (dafür kann das Tool `nl` verwendet werden). **Achtung:** das Skript soll die Datei verändern, nicht nur ausgeben.

Sollte die Datei nicht existieren, nicht lesbar sein oder leer sein, soll ein entsprechender Fehlertext ausgegeben werden.

Es soll weiters auch möglich sein, dem Tool Daten über `stdin` zu übergeben (wenn keine Datei angegeben wurde):
```bash
$ sort test.txt | ./number.sh
     1	aaa drei
     2	bbb eins
     3	ccc zwei
```

Die Verwendung der Option `-?` soll dann noch zur Ausgabe eines Hilfetexts führen:
```text
number.sh: number lines of input
usage: number.sh [FILE]
FILE ... path to readable file of nonzero length
if FILE is omitted data is read from standard input
```

### Theorie
* `-z` prüft, ob eine Variable leer ist.
* `-r` prüft auf Lesbarkeit, `-s` prüft ob die Datei größer als 0 Bytes ist.

### Lösung

**Script: `number.sh`**
```bash
#!/bin/bash

FILE=$1

if [ "$FILE" = "-?" ]
then
    echo "number.sh: number lines of input"
    echo "usage: number.sh [FILE]"
    echo "FILE ... path to readable file of nonzero length"
    echo "if FILE is omitted data is read from standard input"
    exit 0
fi

if [ -z "$FILE" ]
then
   cat -n /dev/stdin
else
    if [ -r "$FILE" ] && [ -s "$FILE" ]
    then
        cat -n "$FILE" > temp_file.txt
        cat temp_file.txt > "$FILE"
        rm temp_file.txt
        echo "Datei wurde nummeriert."
    else
        echo "Fehler: Datei existiert nicht, ist nicht lesbar oder ist leer."
        exit 1
    fi
fi
```

---

## 5. Übung: Stundenplan V2

### Aufgabenstellung
Schreibe ein Skript das aufgrund der aktuellen Zeit das aktuell Fach laut Stundenplan ausgibt. Pausen sind vereinfacht einem Fach zuzurechnen. Berücksichtige nur einen Wochentag, d.h. das Skript funktioniert nur an einem Donnerstag.

### Lösung

**Script: `splan2.sh`**
```bash
#!/bin/bash

TAG=$(date +%A)
STUNDE=$(date +%H)
ZEIT=$(date +%H:%M)

if [ "$TAG" != "Thursday" ] && [ "$TAG" != "Donnerstag" ]
then
    echo "Heute ist nicht Donnerstag!"
    exit 0
fi

if (( STUNDE == 8 ))
then
    echo "Donnerstag $ZEIT PH"
elif (( STUNDE == 9 ))
then
    echo "Donnerstag $ZEIT AM"
elif (( STUNDE == 10 ))
then
    echo "Donnerstag $ZEIT SYTE"
elif (( STUNDE == 11 ))
then
    echo "Donnerstag $ZEIT ITPM"
elif (( STUNDE == 12 ))
then
    echo "Donnerstag $ZEIT BESP"
elif (( STUNDE == 13 ))
then
    echo "Donnerstag $ZEIT D"
else
    echo "Donnerstag $ZEIT Frei"
fi
```
