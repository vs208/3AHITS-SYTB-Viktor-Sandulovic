# Arbeitsbericht: Shell-Script Übungen (Exit Status und test Kommando)

**Klasse:** 3AHITS  
**Name:** Viktor Sandulovic  
**Fach:** ITSE  
**Datum:** 28.04.2026   
**Aufgabenstellung:** Exit Status und test Kommando Übungen

---

## Theorie: Exit Status und Bedingungen
Jedes Programm in Linux gibt beim Beenden den  **Exit Status** zurück.
* Dieser wird in der Variable `$?` gespeichert.
* Ein Status von `0` bedeutet: **(true)**.
* Jeder andere Wert bedeutet: **(false)**.

Das Kommando `test` (bzw. `[ ]`) wird verwendet um Vergleiche zu machen. 
Bei den eckigen Klammern `[ ]` muss immer ein Leerzeichen nach der ersten und vor der letzten Klammer stehen!

Außerdem kann man Befehle verknüpfen mit :
* `&&`
* `||`

---

## 1. Übung: Datei zu groß

### Aufgabenstellung
Schreibe ein Skript mit einer Datei als Argument, es soll der Text Datei ist zu groß ausgegeben werden wenn die Datei mehr als 100 Bytes hat. Die Dateigröße kann mit ls -l und anschließendes cut ermittelt werden.
### Lösung

**Script: `check_size.sh`**
```bash
#!/bin/bash

DATEI=$1

INFO=$(ls -l "$DATEI")

INFO_CLEAN=$(echo "$INFO")

GROESSE=$(echo "$INFO_CLEAN" | cut -d' ' -f5)

[ "$GROESSE" -gt 100 ] && echo "Datei ist zu groß" || echo "Größe OK"
```

---

## 2. Übung: Dir Creator

### Aufgabenstellung
Schreibe ein Skript das den Namen eines Verzeichnisses übergeben bekommt. Das Verzeichnis soll angelegt werden wenn dieses noch nicht existiert. Wenn es das Verzeichnis schon gibt soll eine Warnung und der Inhalt des Verzeichnis angezeigt werden.
### Theorie
* Der Parameter `-d` prüft ob ein Ordner  bereits existiert.

### Lösung

**Script: `create_dir.sh`**
```bash
#!/bin/bash
VERZEICHNIS=$1
 
[ -d "$VERZEICHNIS" ] && (echo "Verzeichnis $VERZEICHNIS existiert bereits" && ls "$VERZEICHNIS") || (mkdir "$VERZEICHNIS" && echo "Das VERZEICHNIS $VERZEICHNIS wurde erstellt")
```


## 3. Übung: Stundenplan

### Aufgabenstellung
Schreibe ein Bash Script das für den aktuellen Wochentag eine Kurzform des Stundenplans ausgibt:


### Theorie
* `date +%A` gibt den aktuellen Wochentag aus (z.B. "Montag", "Donnerstag").

### Lösung

**Script: `splan.sh`**
```bash
#!/bin/bash

HEUTE=$(date +%A)

echo "Es ist $HEUTE"

# Prüft welcher Tag ist in einzelnen Zeilen ohne if
[ "$HEUTE" = "Monday" ] && echo "  GGP SEW SEW SEW INSY INSY"
[ "$HEUTE" = "Tuesday" ] && echo "  SYTB SYTB ITPM E ITSE SYTB"
[ "$HEUTE" = "Wednesday" ] && echo "  D AM NWT RK E SYTB GGP AM"
[ "$HEUTE" = "Thursday" ] && echo "  PH AM SYTE ITPM BESP BESP D"
[ "$HEUTE" = "Friday" ] && echo "  MEDT ITP2A RK ITSE SYTE"
```

---

## 4. Übung: HTML Generator

### Aufgabenstellung
Erstelle ein Skript das ein Markdown Dokument nach HTML konvertiert:
### Theorie
* `-e` prüft, ob eine Datei existiert.
* `-nt` steht für "newer than"  und vergleicht das Änderungsdatum von zwei Dateien.

### Lösung

**Script: `md2html.sh`**
```bash
#!/bin/bash

QUELLE=$1
ZIEL=$2

[ ! -e "$ZIEL" ] || [ "$QUELLE" -nt "$ZIEL" ] && { pandoc "$QUELLE" } || { "$ZIEL"; echo "Die Datei wurde konvertiert."; } || echo "Konvertieren nicht notwendig."
```

---

## 5. Übung: C-Programm

### Aufgabenstellung
Schreibe und kompiliere ein C-Programm das 2 Argumente über die Kommandozeile erhält. Das Programm soll die Differenz ausgeben und im exit Status anzeigen ob die erste Zahl größer als die zweite Zahl war. Teste den exit Status mit &&. Lass dir dabei von genAI helfen.

Die Aufgabe habe ich zeitlich nicht mehr geschafft.
