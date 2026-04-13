# Arbeitsbericht: Shell-Script Übungen

**Klasse:** 3AHITS  
**Name:** Viktor Sandulovic  
**Fach:** ITSE  
**Datum:** 07.04.2026  
 **Aufgabenstellung:** https://www.franzmatejka.at/htl/doc/SYTB_3/05_scripts_ue.html

---

## Argumente: 
(`$1`, `$@`): Wenn man ein Skript aufruft, kann man ihm Werte mitgeben (z.B. `./script.sh hallo welt`). 
`$1`= speichert dabei das erste Wort ("hallo"). `$2` = zweite Wort ("welt").
`$@` ist eine Spezial-Variable und beinhaltet **alle** übergebenen Argumente als eine einzige Liste.
     
## 1. Übung: Directory Struktur

### Aufgabenstellung
Schreibe ein Shell-Script das Unterverzeichnisse und Dateien anlegt, es wird ein Argument an das Script übergeben.
### Theorie
* **Die `for`-Schleife:**  nimmt  jeden Wert aus einer Liste und führt den Code zwischen `do` und `done` damit aus. Bei `for ZAHL in 1 2 3` läuft die Schleife dreimal und die Variable `$ZAHL` ist beim ersten Durchlauf 1, dann 2, dann 3.

### Lösung

**Script: `build_dirs.sh`**
```bash
#!/bin/bash

# Das übergebene Argument (z.B. "abcd") wird in einer Variable gespeichert
ORDNER=$1

mkdir -p "$ORDNER/${ORDNER}_01"
mkdir -p "$ORDNER/${ORDNER}_02"

for ZAHL in {1 2 3}
do
    > "$ORDNER/${ORDNER}_01/${ORDNER}.01.$ZAHL.txt"
    
    > "$ORDNER/${ORDNER}_02/${ORDNER}.02.$ZAHL.txt"
done
```

**Script: `clean_dir.sh`**
```bash
#!/bin/bash

ORDNER=$1

# Der Parameter -r (rekursiv) löscht den Ordner und alle Dateien darin
rm -r "$ORDNER"
```

---

## 2. Übung: Skript Generator

### Aufgabenstellung
Schreibe ein Skript das

eine Skriptdatei erzeugt (Name wird als Argument übergeben),
die She-Bang Zeile einfügt,
einen echo Befehl einfügt und
das eXecution Flag für das Skript setzt

### Lösung

**Script: `makescript.sh`**
```bash
#!/bin/bash

# Speichert den übergebenen Namen und hängt ".sh" an
DATEINAME=$1.sh

# > erstellt die Datei neu und fügt die erste Zeile ein
echo "#!/bin/bash" > "$DATEINAME"

# >> hängt die nächsten Zeilen an die  Datei an
echo "echo \"$1 Skript\"" >> "$DATEINAME"
echo "# write your script here" >> "$DATEINAME"

# Macht das neu erstellte Skript ausführbar
chmod +x "$DATEINAME"
```

---

## 3. Übung: Headline Cat

### Aufgabenstellung
Verwende $@ zur Lösung dieser Aufgabenstellung.

Schreibe ein Skript das eine Art cat zur Verfügung stellt. Als Argumente werden eine beliebige Anzahl von Textdateien übergeben. Das Ergebnis (der Inhalt aller dieser Dateien) wird in die Datei result.txt (fixer Dateiname) geschrieben (ist die Datei vorhanden soll deren Inhalt überschrieben werden). Jedem Datei-Inhalt soll eine Überschrift vorangestellt werden.

### Lösung

**Script: `headline_cat.sh`**
```bash
#!/bin/bash

# Leert die Datei result.txt oder erstellt sie neu, falls sie noch nicht existiert
> result.txt

# Geht jede  Datei einzeln durch
for DATEI in "$@"
do
    echo "== $DATEI ==========================================" >> result.txt
    
    cat "$DATEI" >> result.txt
    
    # Fügt eine leere Zeile hinzu
    echo "" >> result.txt
done
```

---

## 4. Übung: RANDOM

### Aufgabenstellung
Schreibe ein shellscript das eine beliebige Menge von Dateinamen als Parameter akzeptiert. Von jeder dieser Dateien soll eine Kopie im gleichen Verzeichnis angelegt werden. Die Kopie unterscheidet sich vom Original durch eine angefügte Zufallszahl
### Theorie
* **Die Variable `$RANDOM`:** Dies ist eine interne Spezial-Variable der Bash-Shell (funktioniert nicht in der Standard `sh`). Jedes Mal, wenn man sie aufruft, generiert sie automatisch eine neue, zufällige Zahl zwischen 0 und 32767.
Quelle(:https://www.geeksforgeeks.org/linux-unix/random-shell-variable-in-linux-with-examples/)

### Lösung

**Script: `randcp.sh`**
```bash
#!/bin/bash

for DATEI in "$@"
do
    # Der Befehl cp kopiert die Datei. 
    cp "$DATEI" "$DATEI.$RANDOM"
done
```
