# Arbeitsbericht: Netzwerk-Requests mit cURL

**Klasse:** 3AHITS  
**Name:** Viktor Sandulovic  
**Fach:** ITSI  
**Datum:** 23.03.2026  

---


## 1. Browser Developer Tools nutzen
* Mit der Taste **F12** bzw. **fn und f12** werden die Entwicklertools im Browser geöffnet.
* Dann zum **Network** Tab gehen.
* Lädt man eine Webseite neu, sieht man hier alle Dateien, die der Browser im Hintergrund anfordert.
* Mit einem Rechtsklick auf eine Datei (z.B. eine CSS-Datei) und **Copy » Copy as cURL** kann man den exakten Ladebefehl für die Kommandozeile kopieren.

## 2. API-Request im Notenmanagement nachstellen
In dieser Übung wenden wir das Prinzip beim HTL Notenmanagement an:

1. Wir loggen uns ein und rufen die Ergebnisse eines Testes auf.
2. Im Network-Tab suchen wir den genauen Request, der die Noten vom Server lädt. Diese Daten werden im **JSON-Format** übertragen.

<img width="1447" height="924" alt="image" src="https://github.com/user-attachments/assets/7c0cfb69-5625-4fa5-b8e3-dc6cc42b8386" />
 
*Bild 1: Die Developer Tools zeigen den Request und die vom Server empfangenen Noten (JSON).*

3. Nun kopieren wir diesen Request wieder per **Copy as cURL**.
4. Wenn wir diesen kopierten Befehl in das Linux-Terminal (hier Kali Linux) einfügen und ausführen, macht `curl` exakt dieselbe Anfrage wie der Browser. 

Da alle Login-Cookies und Tokens im Befehl enthalten sind, akzeptiert der Server die Anfrage und liefert uns die JSON-Daten direkt im Terminal zurück.

<img width="734" height="831" alt="image" src="https://github.com/user-attachments/assets/e6f7d75f-ca75-4213-a145-c55f5dd08ec0" />
*Bild 2: Das Terminal zeigt den  cURL-Befehl und die exakt gleichen Noten als Ausgabe.*

