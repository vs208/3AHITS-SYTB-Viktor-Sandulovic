# Arbeitsbericht: Regular Expressions

**Klasse:** 3AHITS  
**Name:** Viktor Sandulovic  
**Fach:** ITSE  
**Datum:** 02.06.2026  
**Aufgabenstellung:** Regular Expressions



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


### Lösung

**ERE-Ausdruck:**
```bash
^[1-9][0-9]{3}$
```

---

## 2. Übung: HTML-Überschriften finden

### Aufgabenstellung
Finde alle HTML-Überschriften der Form:
`<h1>Text</h1>`
`<h2>Hallo</h2>`
`<h3>Regex</h3>`
Regeln:
nur h1, h2 oder h3
öffnender und schließender Tag müssen zusammenpassen

### Theorie
* `[^<]*` steht für alles außer <.

### Lösung

**ERE-Ausdruck:**
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
* `[0-9A-Fa-f]` ist hexadezimahl schreibweise.
* `([0-9A-Fa-f]{2}:){5}` sucht nach zwei hexa Zeichen die insgesamt 5 mal vorkommen.

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

if echo "$MAC" | grep -E  '^([0-9A-Fa-f]{2}:){5}[0-9A-Fa-f]{2}$'
then
    echo "Die übergebene MAC-Adresse '$MAC' ist GÜLTIG."
    exit 0
else
    echo "Fehler: Die übergebene MAC-Adresse '$MAC' ist UNGÜLTIG."
    exit 1
fi
```

## 5. Übung: URLs prüfen

### Aufgabenstellung
Schreibe einen ERE-Ausdruck für einfache URLs.
Gültig:
https://example.com
http://test.at
https://sub.domain.org
Ungültig:
ftp://example.com
https:/example.com
example.com
Regeln:
nur http oder https
Domain darf Buchstaben, Zahlen und Punkte enthalten

### Theorie
* `^https?://`Das `?` hinter dem `s` bedeutet dass das `s` optional ist. Danach müssen die Zeichen `://` folgen.
* `[a-zA-Z0-9.]+` ist die Zeichenklasse für die Domain. Das `+` am Ende bedeutet, dass mindestens eines dieser Zeichen vorkommen muss.

### Lösung

**ERE-Ausdruck:**
```bash
^https?://[a-zA-Z0-9.]+
