# Arbeitsbericht
 
**Klasse:** 3AHITS  
**Name:** Viktor Sandulovic  
**Fach:** SYTB  
**Datum:** 03.03.2026  

---


## 1. Installation von Visual Studio Code (Linux)
Visuel Studio Code herunterladen: [VsCode](https://code.visualstudio.com/docs/setup/linux)
**Schritte zur Installation**
```bash
sudo apt install ./<file>.deb
```

## 2. GitHub Repository anlegen

1. Anmeldung auf [GitHub](https://github.com/).
2. Oben rechts auf das **"+"** Symbol klicken und **"New repository"** auswählen.
3. Namen vergeben 
4. Das Repository auf **"Public"** (öffentlich) setzen, damit es später über GitHub Pages erreichbar ist.
5. Ein Häkchen bei **"Add a README file"** setzen.
6. Auf **"Create repository"** klicken.

## 3. Lokale Git-Einrichtung und Klonen
Damit lokal gearbeitet werden kann, muss das Repository auf den Linux-Rechner geklont werden.

**Git konfigurieren (falls noch nicht geschehen):**
```bash
git config --global user.name "Viktor Sandulovic"
git config --global user.email "Viktor.Sandulovic@htl-braunau.at"
```



## 4. Arbeiten in VS Code und Git Workflow
Nachdem das Repository geklont wurde, wird der Ordner in VS Code geöffnet.

**Änderungen in Git erfassen und hochladen (Commit & Push):**
Um die neue Datei in das GitHub-Repository hochzuladen, auf commit changes drücken.

## 5. Bereitstellung des abgabereifen HTML-Links
Um nicht nur den Quellcode, sondern die gerenderte Webseite abzugeben, wird **GitHub Pages** genutzt.

1. Auf der GitHub-Seite des Repositories auf den Tab **"Settings"** (Einstellungen) klicken.
2. Im linken Menü unter "Code and automation" auf **"Pages"** klicken.
3. Unter **"Build and deployment"** bei der Source "Deploy from a branch" auswählen.
4. Bei **"Branch"** den Hauptbranch (`main`) auswählen und den Ordner auf `/(root)` belassen. Auf **"Save"** klicken.
5. *Wartezeit:* Es dauert etwa 1–2 Minuten, bis GitHub die Seite gebaut hat.
6. Die fertige, abgabereife URL erscheint oben in den Pages-Einstellungen 
