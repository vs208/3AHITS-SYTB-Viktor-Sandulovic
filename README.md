# Arbeitsbericht: Shell-Script Übungen (Command Substitution)

**Klasse:** 3AHITS  
**Name:** Viktor Sandulovic  
**Fach:** ITSE  
**Datum:** 20.04.2026  
**Aufgabenstellung:** https://www.franzmatejka.at/htl/doc/SYTB_3/06_cmdsubst.html

---

## Command Substitution (Befehlsersetzung)
**Theorie:** Command Substitution bedeutet, dass das Ergebnis (die Ausgabe) eines Befehls an einer anderen Stelle wiederverwendet oder in einer Variable gespeichert wird. 
* Die Schreibweise ist `$(Befehl)`. 
* Berechnungen können mit `$(( Formel ))` direkt durchgeführt werden.

Beispiel: `HEUTE=$(date)` führt den Befehl `date` aus und speichert das Ergebnis in der Variable `HEUTE`.

---

## 1. Übung: Anzahl Einträge in einem Verzeichnis

### Aufgabenstellung
Schreibe ein shellscript das als Argument einen Pfad auf ein Verzeichnis erhält. Das Script soll die Anzahl der Einträge in diesem Verzeichnis als Zahl ausgeben. Dies soll aus der ls Ausgabe mit wc ermittelt werden.
### Theorie
* **`wc -l`:** Zählt die Zeilen einer Ausgabe.
* **`ls -1`:** Listet Dateien auf.

### Lösung

**Script: `nbrentries.sh`**
```bash
#!/bin/bash

# Speichert das übergebene Verzeichnis
VERZEICHNIS=$1

# Führt ls aus und zählt die Zeilen mit wc -l
ANZAHL=$(ls -l "$VERZEICHNIS" | wc -l)

echo "Es sind $ANZAHL Einträge im dir $VERZEICHNIS"
```

**Dokumentation der Ausgaben-Unterschiede (`ls` vs `ls | cat` vs `echo "$(ls)"`):**
* **`ls`**: Wenn man `ls` normal im Terminal eingibt,  zeigt er den Inhalt eines Verzeichnisses (Dateien und Unterordner) an.
* **`ls | cat`**: Gibt es als eine Datei pro Zeile aus.
* **`echo "$(ls)"`**: Durch das `$()` wird der `ls`-Befehl zuerst im Hintergrund ausgeführt. Die Ausgabe  wird abgefangen und dann als ein einziger, langer Textblock vom `echo`-Befehl auf dem Bildschirm ausgegeben. Die Anführungszeichen `""` sorgen dafür, dass die Zeilenumbrüche nicht verloren gehen.

---

## 2. Übung: Tage bis zum Ball

### Aufgabenstellung
Schreibe ein Shell-Skript das die Anzahl der Tage bis zum HTL Ball ermittelt. Die Ausgabe soll in der folgenden Form sein.


### Theorie
* **`date +%s`:** Gibt die aktuellen Sekunden seit dem 1.1.1970 aus.
* **`date -d "YYYY-MM-DD" +%s`:** Berechnet die Sekunden für ein Datum in der Zukunft.
* Ein Tag hat 86400 Sekunden (60 Sekunden * 60 Minuten * 24 Stunden).

### Lösung

**Script: `days_to_ball.sh`**
```bash
#!/bin/bash

BALL_DATUM="2027-01-16"

# Aktuelle Zeit in Sekunden
JETZT=$(date +%s)

# Zeit des Balls in Sekunden
ZIEL=$(date -d "$BALL_DATUM" +%s)

# Differenz berechnen
DIFF_SEKUNDEN=$(( ZIEL - JETZT ))

# Sekunden in Tage umrechnen (durch 86400 teilen)
TAGE=$(( DIFF_SEKUNDEN / 86400 ))

echo "Es sind noch $TAGE Tage bis zum HTL Ball ($BALL_DATUM)"
```

---

## 3. Übung: Zufälliger Satz

### Aufgabenstellung
Erstelle ein Skript das einen Satz aus 5 zufälligen Wörtern bildet.
Benutze diese Wörterliste: https://www.franzmatejka.at/htl/doc/SYTB_3/testdata/wortliste1000.txt

### Theorie
* **`sed -n 5p datei.txt`:** Gibt exakt die 5. Zeile einer Textdatei aus. Das `p` steht für print.
* Um dynamisch eine Zufallszahl von 1 bis X zu bekommen, nutzt man: `$(( RANDOM % MAX_ZEILEN ))`

### Lösung

**Script: `random_sentence.sh`**
```bash
#!/bin/bash

WORTLISTE="wortliste.txt"

MAX_ZEILEN=$(wc -l < "$WORTLISTE")

SATZ=""

for i in {1..5}
do
    # Generiert eine Zufallszahl zwischen 1 und der Zeilenanzahl
    ZUFALL=$(( RANDOM % MAX_ZEILEN))
    
    # Holt das spezifische Wort aus der Datei
    WORT=$(sed -n "${ZUFALL}p" "$WORTLISTE")
    
    # Hängt das Wort an den Satz an
    SATZ="$SATZ $WORT"
done

echo "$SATZ"
```

---

## 4. Übung: dated copy V1

### Aufgabenstellung
Erstellen Sie ein Skript, das einen Dateinamen als erstes Argument entgegennimmt und eine datierte Kopie der Datei erstellt. Beispiel: Wenn unsere Datei den Namen „.txt“ trägt file1.txtund heute der 29.10.2021 ist, soll eine Kopie wie (z.B. `2021-10-29_file1.txt`).

### Theorie
* **`date +%Y-%m-%d`:** Formatiert das Datum in Jahr-Monat-Tag 

### Lösung

**Script: `dated_copy_v1.sh`**
```bash
#!/bin/bash

DATEINAME=$1
HEUTE=$(date +%Y-%m-%d)

cp "$DATEINAME" "${HEUTE}_${DATEINAME}"
```


*(Hinweis: Damit dieses Skript funktioniert, muss `dated_copy_v2.sh` im selben Ordner liegen und mit `chmod +x` ausführbar gemacht worden sein).*
