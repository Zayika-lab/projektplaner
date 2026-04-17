# Planner

Eine einzelne HTML-Datei (`index.html`). Drei Ansichten: **Gantt**, **Woche**, **Kalender**.
Funktioniert sofort offline (localStorage); Firebase ist optional für Geräte-Sync.

---

## 1. Sofort ausprobieren

Doppelklick auf `index.html` — läuft in Safari. Daten werden im Browser gespeichert.

## 2. Deployment auf GitHub Pages

```
1. Neues Repo auf GitHub anlegen, z.B.  planner
2. index.html hochladen (in den Hauptordner)
3. Repo → Settings → Pages
4. Source: "Deploy from a branch"
   Branch: main  /  root
5. Speichern — nach ~1 Min erreichbar unter
   https://<dein-username>.github.io/planner/
```

## 3. Firebase aktivieren (optional — für Sync zwischen Geräten)

```
1. https://console.firebase.google.com → Projekt erstellen
2. Build → Firestore Database → "Create database" → Test mode
3. Project settings (Zahnrad) → "Your apps" → </> Web-App registrieren
4. Die firebaseConfig-Keys (apiKey, authDomain, projectId, ...) kopieren
5. In index.html den Block `const firebaseConfig = { ... }` ganz oben
   im Script ausfüllen (Kommentare entfernen)
6. Firestore → Rules — für Entwicklung zunächst offen:

   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if true;
       }
     }
   }

   ⚠ Für Produktion unbedingt Auth + Regeln einrichten!
```

Wenn Firebase konfiguriert ist, zeigt der Statuspunkt oben rechts **„Firebase"** (grün) statt **„lokal"** (gelb).

---

## Bedienung

### Gantt
- **Ziehen** — Block verschieben (auch zwischen Zeilen → ändert Projekt-Zuordnung)
- **Kanten ziehen** — Dauer anpassen
- **Kleiner Punkt links/rechts am Balken** — herausziehen, auf anderen Balken fallen lassen ⇒ Pfeil-Abhängigkeit
- **Doppelklick** in eine leere Zeile — neuer Balken
- **Klick** auf Balken — bearbeiten / löschen
- **Linke Leiste** — Projekte, Unterprojekte, Unteraufgaben; Klick filtert die Ansicht
- **Rote Linie** — aktuelle Zeit

### Woche
- **Doppelklick** in leere Fläche — neuer Block (Titel, Zeit, wöchentlich oder einmalig)
- **Aufgabe aus Liste ziehen** — in einen Block (Block füllt sich proportional zur Dauer)
- **Aufgabe ziehen auf leere Stelle** — neuer Block mit dieser Aufgabe entsteht
- **Aufgabe ist zu lang** — läuft automatisch in den nächsten Block gleichen Titels über
- **„Übernahme aus vorheriger Woche"** — kopiert die *Nur-diese-Woche*-Blöcke der Vorwoche
- **Template-Blöcke** (dashed) — wiederholen sich wöchentlich (ein Fotografen-Standard-Setup ist vorausgefüllt)

### Kalender
- Monats-Navigation zum schnellen Springen. Klick auf Tag ⇒ öffnet die entsprechende Woche.
