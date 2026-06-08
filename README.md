# Arbeitsbericht: Regular Expressions

**Klasse:** 3AHITS  
**Name:** Viktor Sandulovic  
**Fach:** ITSE  
**Datum:** 25.05.2026  
**Aufgabenstellung:** Regular Expressions und Textmanipulation mit grep und sed

---

## Theorie: Regular Expressions, grep und sed

### Regular Expressions (Reguläre Ausdrücke)
Regular Expressions (Regex oder RE) sind Muster, mit denen man Texte durchsuchen und verändern kann. Sie bestehen aus normalen Zeichen und Metazeichen.
* `^` steht für den Anfang einer Zeile.
* `$` steht für das Ende einer Zeile.
* `.` steht für ein beliebiges Zeichen.
* `*` bedeutet: Das Zeichen davor darf 0-mal oder beliebig oft vorkommen.
* `+` bedeutet: Das Zeichen davor muss mindestens 1-mal oder öfter vorkommen.
* `[ ]` definiert eine Zeichenklasse (z. B. `[a-z]` für alle Kleinbuchstaben).

### grep
Das Tool `grep` sucht in Dateien nach bestimmten Textmustern. Mit der Option `-E` aktiviert man **ERE** (Extended Regular Expressions). Das ist wichtig, damit Zeichen wie `+` oder `?` ohne Backslash richtig erkannt werden.

### sed (Stream Editor)
`sed` bearbeitet Textströme Zeile für Zeile. Der wichtigste Befehl ist das Suchen und Ersetzen:  
`sed 's/Suchen/Ersetzen/g'`  
Das `g` am Ende sorgt dafür, dass alle Vorkommen in einer Zeile ersetzt werden (global).

---

## 1. Übung: RegexONE

### Aufgabenstellung
Arbeite dich in RegexONE von Lesson 1 – 14.

### Lösung
Die Lektionen 1 bis 14 auf RegexONE wurden erfolgreich durchgearbeitet. Dabei wurden die Grundlagen von regulären Ausdrücken wie Zeichenklassen, Quantifier (`*`, `+`), Whitespaces und Wildcards erlernt und interaktiv getestet.

---

## 2. Übung: Subdir Count

### Aufgabenstellung
Schreibe einen shell Einzeiler mit dem man die Anzahl der Unterverzeichnisse im aktuellen Verzeichnisse zählt.  
Tipp: Directories haben in der ls -l Ausgabe ganz am Beginn ein d.

### Theorie
* `ls -l` listet den Inhalt des Verzeichnisses im Langformat auf.
* Verzeichnisse beginnen in dieser Liste immer mit dem Buchstaben `d`.
* `grep "^d"` filtert alle Zeilen heraus, die mit einem `d` beginnen.
* `wc -l` zählt die Zeilen, die von grep weitergegeben werden.

### Lösung

**Einzeiler-Befehl:**
```bash
ls -l | grep "^d" | wc -l
```

---

## 3. Übung: REs

### Aufgabenstellung
Finde 5 substantiell unterschiedliche Strings die durch folgende RE gematcht werden:  
`^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9.-]+$`  
Verwende zum test grep.js.org  
Achtung: ERE daher -E Option notwendig.

### Theorie
Die RE beschreibt ein typisches Muster für E-Mail-Adressen:
1. `^[a-zA-Z0-9_.+-]+` : Der Name vor dem `@` darf Buchstaben, Zahlen, Punkte, Unterstriche, Plus und Minus enthalten.
2. `@` : Das Trennzeichen.
3. `[a-zA-Z0-9-]+` : Der Providername nach dem `@` darf Buchstaben, Zahlen und Bindestriche enthalten.
4. `\.` : Ein echter Punkt vor der Top-Level-Domain.
5. `[a-zA-Z0-9.-]+$` : Die Endung darf Buchstaben, Zahlen, Punkte und Bindestriche enthalten.

### Lösung

**5 unterschiedliche Strings:**
1. `test@example.com` (Standard-E-Mail)
2. `viktor.sandulovic+itse@htl-braunau.at` (Mit Plus-Zeichen und Subdomain)
3. `123_456@sub-domain.net` (Reine Zahlen mit Unterstrich und Bindestrich in Domain)
4. `a@b.c` (Minimalistische E-Mail mit nur jeweils einem Zeichen)
5. `my-email.test_group@company.co.uk` (Komplexe Struktur mit mehreren Punkten in der Endung)

**Test mit grep:**
```bash
echo "test@example.com" | grep -E '^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9.-]+$'
```

---

## 4. Übung: sed

### Aufgabenstellung
Löse mit sed:
* Entferne alle # die sich am Ende der Zeile befinden
* Entferne alle # die sich am Anfang der Zeile befinden
* Füge === am Beginn jeder Zeile ein
* Füge () rund um jedes Wort ein. Ein Wort ist definiert als mindestens ein nicht-Leerzeichen.

### Theorie
* `\1` in der Ersetzung greift auf die erste gefundene Gruppe `( )` im Suchmuster zu.
* `[^[:space:]]+` sucht nach einer Kette von Zeichen, die keine Leerzeichen sind (ein Wort).

### Lösung

**Befehle:**
```bash
# 1. Entferne alle # am Ende der Zeile
sed 's/#$//' datei.txt

# 2. Entferne alle # am Anfang der Zeile
sed 's/^#//' datei.txt

# 3. Füge === am Beginn jeder Zeile ein
sed 's/^/===/' datei.txt

# 4. Füge () rund um jedes Wort ein
sed -E 's/([^[:space:]]+)/(\1)/g' datei.txt
```

---

## 5. Übung: Datum

### Aufgabenstellung
Mit sed. Datum re-formatieren von YYYY-MM-TT auf TT.MM.YYYY. 01/12/2020-> 12.01.2020. Das Datum kann sich an beliebiger Position in der Zeile befinden.

### Theorie
Das Muster sucht nach 4 Zahlen für das Jahr, 2 Zahlen für den Monat und 2 Zahlen für den Tag. Die Gruppen werden mit `( )` eingeklammert und in der Reihenfolge `\3.\2.\1` wieder zusammengesetzt.

### Lösung

**Befehl:**
```bash
sed -E 's/([0-9]{4})-([0-9]{2})-([0-9]{2})/\3.\2.\1/g' datei.txt
```

---

## 6. Übung: Logfile

### Aufgabenstellung
Lege eine Textdatei mit folgendem Inhalt an (Ausschnitt aus einem Logfile).  
Aufgabenstellung:
* Verwende grep um nur jene Zeilen auszugeben die configure enthalten. Jene Zeilen die half-configured enthalten sollen nicht ausgegeben werden.
* Verwende grep um nur jene Zeilen auszugeben die libsombok oder libposix enthalten.
* Verwende sed um die Zeilen ohne die Uhrzeit auszugeben, d.h. ersetzte durch einen leeren String.
* Verwende sed um die Zeilen ohne das Datum auszugeben.
* Verwende sed um das Datum umzuformatieren von YYYY-MM-TT auf TT.MM.YYYY. 2021-01-16-> 16.01.2021.

### Theorie
* `grep -v` kehrt die Suche um. Es gibt nur Zeilen aus, auf die das Muster **nicht** zutrifft.
* `|` (Pipe) wird genutzt, um die Ausgabe eines Befehls als Eingabe für den nächsten zu verwenden.

### Lösung

**Datei anlegen (ohne touch):**
```bash
cat << 'EOF' > logfile.txt
2021-01-16 23:38:01 status unpacked libarchive-zip-perl:all 1.60-1ubuntu0.1
2021-01-16 23:38:01 status half-configured libarchive-zip-perl:all 1.60-1ubuntu0.1
2021-01-16 23:38:01 status installed libarchive-zip-perl:all 1.60-1ubuntu0.1
2021-01-16 23:38:01 configure libmime-charset-perl:all 1.012.2-1 <none>
2021-01-16 23:38:01 status unpacked libmime-charset-perl:all 1.012.2-1
2021-01-16 23:38:01 status half-configured libmime-charset-perl:all 1.012.2-1
2021-01-16 23:38:01 status installed libmime-charset-perl:all 1.012.2-1
2021-01-16 23:38:01 configure libimage-exiftool-perl:all 10.80-1 <none>
2021-01-16 23:38:01 status unpacked libimage-exiftool-perl:all 10.80-1
2021-01-16 23:38:01 status half-configured libimage-exiftool-perl:all 10.80-1
2021-01-16 23:38:01 status installed libimage-exiftool-perl:all 10.80-1
2021-01-16 23:38:01 trigproc man-db:amd64 2.8.3-2 <none>
2021-01-16 23:38:01 status half-configured man-db:amd64 2.8.3-2
2021-01-16 23:38:21 status installed man-db:amd64 2.8.3-2
2021-01-16 23:38:21 configure libsombok3:amd64 2.4.0-1 <none>
2021-01-16 23:38:21 status unpacked libsombok3:amd64 2.4.0-1
2021-01-16 23:38:21 status half-configured libsombok3:amd64 2.4.0-1
2021-01-16 23:38:21 status installed libsombok3:amd64 2.4.0-1
2021-01-16 23:38:21 status triggers-pending libc-bin:amd64 2.27-3ubuntu1
2021-01-16 23:38:21 configure libposix-strptime-perl:amd64 0.13-1build3 <none>
2021-01-16 23:38:21 status unpacked libposix-strptime-perl:amd64 0.13-1build3
2021-01-16 23:38:21 status half-configured libposix-strptime-perl:amd64 0.13-1build3
2021-01-17 23:38:21 status installed libposix-strptime-perl:amd64 0.13-1build3
2021-01-17 23:38:21 configure libunicode-linebreak-perl:amd64 0.0.20160702-1build2 <none>
2021-01-17 23:38:21 status unpacked libunicode-linebreak-perl:amd64 0.0.20160702-1build2
2021-01-18 23:38:21 status half-configured libunicode-linebreak-perl:amd64 0.0.20160702-1build2
2021-01-20 23:38:21 status installed libunicode-linebreak-perl:amd64 0.0.20160702-1build2
2021-02-01 23:38:21 trigproc libc-bin:amd64 2.27-3ubuntu1 <none>
2021-03-03 23:38:21 status half-configured libc-bin:amd64 2.27-3ubuntu1
2021-03-04 23:38:23 status installed libc-bin:amd64 2.27-3ubuntu1
EOF
```

**Befehle zur Bearbeitung:**

```bash
# 1. Zeilen mit 'configure', aber ohne 'half-configured'
grep "configure" logfile.txt | grep -v "half-configured"

# 2. Zeilen die 'libsombok' oder 'libposix' enthalten
grep -E "libsombok|libposix" logfile.txt

# 3. Zeilen ohne die Uhrzeit ausgeben (Uhrzeit löschen)
sed -E 's/[0-9]{2}:[0-9]{2}:[0-9]{2} //' logfile.txt

# 4. Zeilen ohne das Datum ausgeben (Datum löschen)
sed -E 's/[0-9]{4}-[0-9]{2}-[0-9]{2} //' logfile.txt

# 5. Datum umformatieren von YYYY-MM-TT auf TT.MM.YYYY
sed -E 's/([0-9]{4})-([0-9]{2})-([0-9]{2})/\3.\2.\1/g' logfile.txt
```
