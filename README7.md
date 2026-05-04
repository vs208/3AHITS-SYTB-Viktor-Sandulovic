# Arbeitsbericht: Shell-Script Übungen (Arithmetik)

**Klasse:** 3AHITS  
**Name:** Viktor Sandulovic  
**Fach:** ITSE  
**Datum:** 21.04.2026   
**Aufgabenstellung:** Shell Arithmetik Übungen

---

## Theorie: Rechnen in Bash (Arithmetik)
Für Berechnungen in Bash-Skripten gibt es 3 verschiedene Möglichkeiten:

1. **`let`**: Das Ergebnis wird direkt einer Variablen zugewiesen. Leerzeichen sind hier nicht erlaubt, außer man verwendet Anführungszeichen.
   * *Beispiel:* `let a=5+4` oder `let "a = 5 + 4"`
2. **`expr`**: Gibt das Ergebnis direkt auf dem Bildschirm aus anstatt es zu speichern. Hier müssen Leerzeichen zwischen den Zahlen und Zeichen stehen. Ein Mal-Zeichen (`*`) muss escaped werden (`\*`).
   * *Beispiel:* `expr 5 + 4` oder `expr 2 \* 3`
3. **BASH Doppelklammer-Arithmetik `$(( ))`**: Hier sind Leerzeichen egal und Variablen brauchen darin kein `$`-Zeichen.
   * *Beispiel:* `a=$(( 4 + 5 ))`

---

## 1. Übung: Multiplizierer

### Aufgabenstellung
Erstelle ein Skript das 2 Zahlen als Kommandozeilenargumente übernimmt. Multipliziere diese Zahlen und gib das Ergebnis aus, verwende jede der 3 besprochenen Methoden.

### Lösung

**Script: `multi.sh`**
```bash
#!/bin/bash

ZAHL1=$1
ZAHL2=$2

# Methode 1: let 
let "RES1 = ZAHL1 * ZAHL2"
echo "Mit let: $RES1"

# Methode 2: expr 
RES2=$(expr $ZAHL1 \* $ZAHL2)
echo "Mit expr: $RES2"

# Methode 3: BASH Arithmetik 
RES3=$(( ZAHL1 * ZAHL2 ))
echo "Mit BASH Arithmetik: $RES3"
```

---

## 2. Übung: Werkstatt Summe

### Aufgabenstellung
Schreibe ein shell Script das die Summe aller Beträge in klassenkassa.csv mit dem Text Werkstatt in ermittelt.

### Theorie
* **`paste -s -d+`**: Nimmt viele einzelne Zeilen und fügt sie zu einer einzigen langen Zeile zusammen. Das `-d+` sagt, dass als Trennzeichen ein Plus verwendet werden soll.

### Lösung

**Script: `werkstatt_summe.sh`**
```bash
#!/bin/bash

ZEILEN=$(grep "Werkstatt" klassenkassa.csv)

BETRAEGE=$(echo "$ZEILEN" | cut -d',' -f3)

GANZZAHLEN=$(echo "$BETRAEGE" | cut -d'.' -f1)

RECHNUNG=$(echo "$GANZZAHLEN" | paste -s -d+)

SUMME=$(( RECHNUNG ))

echo "Die Gesamtsumme für die Werkstatt ist: $SUMME"
---
````
## 3. Übung: Zeitmessung

### Aufgabenstellung
Schreibe 2 Skripts: time_start.sh und time_stop.sh. Bei Aufruf von time_stop.sh wird die Anzahl der Sekunden ausgegeben die seit dem letzten Aufruf von time_start.sh vergangen sind.

### Theorie
* **`mktemp`**: Erstellt eine leere temporäre Datei mit einem zufälligen Namen.

### Lösung

**Script 1: `time_start.sh`**
```bash
#!/bin/bash
TEMP=$(mktemp)

echo "$TEMP" > /timer_pfad.txt

date +%s > "$TEMP"

echo "Timer wurde gestartet."
```

**Script 2: `time_stop.sh`**
```bash
#!/bin/bash
TEMP=$(cat /timer_pfad.txt)

STARTZEIT=$(cat "$TEMP")

JETZT=$(date +%s)

DAUER=$(( JETZT - STARTZEIT ))

echo "Es sind $DAUER Sekunden vergangen."
```

---

## 4. Übung: Random Number

### Aufgabenstellung
$RANDOM erzeugt Zahlen zwischen 0 und 32767. Das ist nicht immer hilfreich. Schreibe ein Skript random.sh wo über 2 Grenzen (Argumente des Skripts) der gewünschte Wertebereich vorgegeben werden kann. Zum Beispiel generiert ./random.sh 10 45 eine zufällige Zahl im Bereich von 10 bis 45 (jeweils inklusive).

### Theorie
Zuffalszahl in einem bestimmten Berreich: `$(( RANDOM % BEREICH + MINIMUM ))`. 

### Lösung

**Script: `random.sh`**
```bash
#!/bin/bash

MIN=$1
MAX=$2

# Zuerst den möglichen Bereich ausrechnen
BEREICH=$(( MAX - MIN ))

# Zufallszahl berechnen
ZUFALL=$(( RANDOM % BEREICH + MIN ))

echo "Zufallszahl zwischen $MIN und $MAX: $ZUFALL"
```
