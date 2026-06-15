# Arbeitsbericht: Regular Expressions

**Klasse:** 3AHITS  
**Name:** Viktor Sandulovic  
**Fach:** ITSE  
**Datum:** 02.06.2026  
**Aufgabenstellung:** Regular Expressions und Validierungs-Skripte



## 1. Übung: Postleitzahlen prüfen

### Aufgabenstellung
Schreibe einen ERE-Ausdruck für österreichische Postleitzahlen.
Regeln:
genau 4 Ziffern
erste Ziffer darf nicht 0 sein
Gültig:
4020
5280
1010
Ungültig:
0123
123
12345
abcd

### Theorie
* `[1-9]` stellt sicher, dass die erste Ziffer keine 0 ist.
* `[0-9]{3}` bedeutet, dass danach genau 3 beliebige Ziffern folgen müssen. Das `{3}` ist eine genaue Längenangabe.

### Lösung

**ERE-Ausdruck:**
```bash
^[1-9][0-9]{3}$
```

---

## 2. Übung: HTML-Überschriften finden

### Aufgabenstellung
Finde alle HTML-Überschriften der Form:
<h1>Text</h1><h2>Hallo</h2><h3>Regex</h3>
Regeln:
nur h1, h2 oder h3
öffnender und schließender Tag müssen zusammenpassen

### Theorie
* `(h[1-3])` gruppiert die Überschriften h1 bis h3 und merkt sie sich .
* `[^<]*` steht für beliebigen Text zwischen den Tags (alles außer dem Zeichen `<`).

### Lösung

**ERE-Ausdruck (z. B. verwendbar mit grep -E):**
```bash
(<h1>[^<]*</h1>|<h2>[^<]*</h2>|<h3>[^<]*</h3>)
```

---

## 3. Übung: MAC-Adressen validieren

### Aufgabenstellung
Schreibe einen ERE-Ausdruck für MAC-Adressen.
Format:
AA:BB:CC:DD:EE:FF
Regeln:
genau 6 Gruppen
jeweils 2 hexadezimale Zeichen
Trennung durch :

### Theorie
* `[0-9A-Fa-f]` ist die Zeichenklasse für hexadezimale Zeichen (Zahlen von 0-9 und Buchstaben von A-F in Groß- oder Kleinschreibung).
* `([0-9A-Fa-f]{2}:){5}` sucht nach genau 5 Gruppen, die aus 2 Hex-Zeichen und einem Doppelpunkt bestehen. Danach folgt noch eine letzte Gruppe aus 2 Hex-Zeichen ohne Doppelpunkt.

### Lösung

**ERE-Ausdruck:**
```bash
^([0-9A-Fa-f]{2}:){5}[0-9A-Fa-f]{2}$
```

---

## 4. Übung: MAC-Adressen validieren – Skript

### Aufgabenstellung
Erstelle ein shell script das eine Mac Adresse als Argument übergeben bekommt und diese mit Hilfe eines ERE-Ausdrucks validiert. Es soll eine passende Ausgabe erzeugt und der exit Status gesetzt werden, so dass man dieses Script aus einem anderen Script heraus aufrufen kann.


### Lösung

**Skript: `mac_validieren.sh`**
```bash
#!/bin/bash

MAC=$1

# Prüfen mit grep -E (ERE) und -q (keine Textausgabe)
if echo "$MAC" | grep -E  '^([0-9A-Fa-f]{2}:){5}[0-9A-Fa-f]{2}$'
then
    echo "Die übergebene MAC-Adresse '$MAC' ist GÜLTIG."
    exit 0
else
    echo "Fehler: Die übergebene MAC-Adresse '$MAC' ist UNGÜLTIG."
    exit 1
fi
```
