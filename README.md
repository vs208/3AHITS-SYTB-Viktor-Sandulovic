# Arbeitsbericht: Schleifen Übungen I

**Klasse:** 3AHITS  
**Name:** Viktor Sandulovic  
**Fach:** ITSE  
**Datum:** 21.06.2026  
**Aufgabenstellung:** Bash Scripting – Schleifen, Parameterübergabe und Standardeingabe (stdin)

---

## 1. Übung: Even/Odd

### Aufgabenstellung
Create a simple script which will print the numbers 1–42 (each on a separate line) and whether they are even or odd.

### Theorie
* `for i in {1..42}`: Eine Sequence-Expression, die eine Schleife von 1 bis 42 durchläuft.
* `(( i % 2 == 0 ))`: Der Modulo-Operator `%` gibt den Rest einer Division zurück. Ist der Rest bei einer Division durch 2 gleich 0, ist die Zahl gerade (even).

### Lösung

**Skript: `even_odd.sh`**
```bash
#!/bin/bash

for i in {1..42}; do
    if (( i % 2 == 0 )); then
        echo "$i is even"
    else
        echo "$i is odd"
    fi
done
