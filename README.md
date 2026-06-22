# Arbeitsbericht: Schleifen Übungen I

**Klasse:** 3AHITS  
**Name:** Viktor Sandulovic  
**Fach:** ITSE  
**Datum:** 21.06.2026  
**Aufgabenstellung:** Schleifen 

---

### 1. while-Schleife 
```bash
#!/bin/bash

counter=1

while [ $counter -le 10 ] 
do
    echo $counter
    ((counter++))         
done
```

### 2.  for-Schleife mit Wortlisten
```bash
# Liste = durch whitespaces getrennter String
data="1 2 3 4 5 6 7"
for d in $data
do 
    echo $d
done
```

### 3. Kommandozeilenargumente mit $@
Die Variable `$@` enthält alle Argumente, die dem Skript beim Aufruf beim Start übergeben wurden.

```bash
echo "---- Kommandozeilenargumente ----"
for arg in $@
do 
    echo $arg
done
```

### 4. Brace Expression
```bash
# brace expression {}
begin=20
end=25
for value in {20..25}
do 
    echo $value
done
```

### 5. Internal Field Separator (IFS)
Die Variable `IFS` bestimmt welches Zeichen Bash Strings für Schleifen oder Arrays auftrennt. 

```bash
echo "--- field separator ----"
mylist=" hallo welt, hello world, guten tag; hi hao"
IFS=",;" # Trenne nun bei Komma und Semikolon statt bei Leerzeichen

for el in $mylist
do 
    echo $el
done
```

---

## 1. Übung: Even/Odd

### Aufgabenstellung
Erstelle ein einfaches Skript, das die Zahlen 1–42 (jeweils auf einer separaten Zeile) ausdruckt und ob sie gerade oder ungerade sind.

### Lösung
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/a03b0aa5-6c26-4e17-b577-72af81166db6" />

**Skript: `odd.sh`**
```bash
#!/bin/bash

for i in {1..42}
do
    if (( i % 2 == 0 ))
    then
        echo "$i ist even"
    else
        echo "$i ist odd"
    fi
done
```

---

## 2. Übung: Summe und Maximum

### Aufgabenstellung
Schreibe ein bash Skript das eine beliebige Menge von positiven Zahlen als Argumente aus der Kommandozeile übernimmt. Es soll die Summe und die größte Zahl ausgegeben werden. Hinweis: Verwende `$@`.

### Lösung
<img width="1919" height="680" alt="image" src="https://github.com/user-attachments/assets/03a9785d-b517-457f-b831-1e8703f5111b" />

**Skript: `SumAndMax.sh`**
```bash
#!/bin/bash
max=0;
sum=0;
for arg in $@
do
    if (( arg > max ))
    then
        (( max = arg ))
    fi
    (( sum += arg ))
done
echo "Maximum: $max "
echo "Summe: $sum "
```

---

## 3. Übung: Min

### Aufgabenstellung
Schreibe ein bash Skript das den Namen einer Datei als Argument akzeptiert (`$ ./min.sh input.txt`). Die Input Datei enthält positive Zahlen die durch white spaces getrennt sind. Das Skript soll die kleinste Zahl ermitteln und ausgeben.

### Lösung

**Skript: `min.sh`**
```bash
#!/bin/bash

DATEI=$1
MIN=$((2**63-1))

for num in $(cat "$DATEI")
 do
    if (( num < MIN ))
     then
        MIN=$num
    fi
done

echo "Minimum: $MIN"
```

---

## 4. Übung: Durchschnitt

### Aufgabenstellung
Schreibe ein bash Skript das eine beliebige Anzahl von Zahlen von stdin akzeptiert und den (ganzzahligen) Durchschnitt der Zahlen ausgibt.

`$ echo 3 5 7 2 4 | ./avg.sh`

Wie Daten von stdin gelesen werden können ist zu recherchieren.

Teste das Skript und verwende dieses dann um den Durchschnitts-Wert für die Spalte Stück in d

### Lösung

**Skript: `avg.sh`**
```bash
#!/bin/bash

sum=0
count=0

for num in $(cat)
 do
    sum=$((sum + num))
    count=$((count + 1))
done

    avg=$((sum / count))
    echo "Durchschnitt: $avg"
```


```bash
tail -n +2 lager.csv | cut -d';' -f3 | ./avg.sh
```
