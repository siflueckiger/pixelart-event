# CLAUDE.md — Firmenanlass Pixel-Wimmelbild

## Projektübersicht

An einem Firmenanlass füllen **Mitarbeitende** je ein Blatt aus, das zwei Elemente enthält:
1. Einen **Steckbrief** mit persönlichen Angaben (handschriftlich ausgefüllt)
2. Ein **Pixelart-Selbstporträt** (von Hand in ein vorgedrucktes Raster gezeichnet)

Nach dem Anlass werden alle Blätter eingescannt und automatisiert verarbeitet. Das Endergebnis ist ein **interaktives Wimmelbild** als lokale HTML-Datei, auf dem alle Pixel-Avatare dargestellt und per Klick geöffnet werden können.

---

## Phasen

### Phase 1 — Firmenanlass

- Vorlage wird vorbereitet und ausgedruckt (A4, eine Seite pro Person)
- Mitarbeitende füllen Steckbrief und Pixelart-Raster von Hand aus
- Ca. 180 Blätter total

### Phase 2 — Scan

- Alle ausgefüllten Blätter werden nach dem Anlass eingescannt
- Akzeptierte Formate: **PDF oder Bild (JPG/PNG)**

### Phase 3 — Verarbeitungs-Script (Python)

- Script liest alle Scan-Dateien ein
- Erkennt und verarbeitet Steckbrief-Felder via OCR / Vision-API
- Schneidet Pixelart-Raster aus und speichert als PNG mit transparentem Hintergrund
- Generiert eine JSON-Datei mit allen Steckbrief-Daten

### Phase 4 — Interaktives Wimmelbild

- Lokale HTML-Datei (kein Webserver nötig)
- Zeigt alle Pixel-Avatare in einem gleichmässigen Raster
- Klick auf Avatar öffnet Detailansicht mit grossem Pixelbild und Steckbrief-Infos

---

## Vorlage (Druckblatt)

### Inhalt der Vorlage

- **Kein Logo, kein Titel**
- **Steckbrief oben > zweistufiges Formular-Layout**
	- Zeile 1: drei gleich breite Eingabefelder nebeneinander (Name, Vorname, Ich in einem Wort)
	- Zeile 2 und 3: je ein einzelnes Eingabefeld über fast die gesamte Breite.
    - Jedes Feld hat oben ein Label und unter dem Label ein leeres Rechteck als Eingabebox.

- **Pixelart-Raster unten:** direkt darunter, volle Breite

### Layout der Vorlage
```markdown
Name  [          ]   Vorname  [          ]   Ich in einem Wort  [          ]

Superkraft  [                                                              ]

Fun Fact    [                                                              ]

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
  [ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
```

### Pixelart-Raster

- Grösse: **24 × 48 Kästchen** (konfigurierbar, siehe unten)
- Kästchengrösse auf Papier: **4mm × 4mm**
- Rastergrösse auf Papier: **96mm × 192mm**
- Mitarbeitende malen einzelne Kästchen aus (von Hand)
- Motiv: entweder **nur Kopf** oder **ganzer Körper**
- Wichtig: Raster muss **Eckenmarkierungen / Registrierungsmarker** enthalten für präzise automatische Erkennung im Script

---

## Script-Output

### Pixelart-PNGs

- Format: `PNG` mit transparentem Hintergrund
- Dateinamenschema: `0001.png`, `0002.png`, … `n.png`
- Eindeutige ID pro Mitarbeitenden

### employees.json

```json
[
  {
    "id": "0001",
    "name": "Muster",
    "vorname": "Hans",
    "superkraft": "Kaffee in Code umwandeln",
    "fun_fact": "Hat noch nie eine Deadline verpasst",
    "ich_in_einem_wort": "Neugierig",
    "pixelart": "0001.png"
  }
]
```

---

## Wimmelbild-Frontend

### Technologie

- Reines HTML/CSS/JavaScript (keine Abhängigkeiten, kein Server)
- Läuft lokal im Browser via `index.html`

### Verzeichnisstruktur

```
projekt/
├── index.html
├── employees.json
└── assets/
    ├── 0001.png
    ├── 0002.png
    ├── ...
    └── n.png
```

### Funktionen

- **Rasteransicht:** Alle Pixel-Avatare gleichmässig in einem Grid angeordnet
- **Klick-Interaktion:** Klick auf einen Avatar öffnet eine Detailansicht
- **Detailkarte:** Zeigt grosses Pixelbild + Name + alle Steckbrief-Felder
- **Schliessen:** Detailkarte kann geschlossen werden (zurück zur Rasteransicht)

---

## Offene Entscheidungen / nächste Schritte

- [x] Grösse und Auflösung des Pixelart-Rasters: **24×48 Kästchen, 4mm/Kästchen**
- [x] Layout der Vorlage: **Option B** — Steckbrief oben (horizontal), Pixelart unten
- [x] Steckbrief-Felder: **Feldname + leere Box**
- [x] Kein Logo, kein Titel auf der Vorlage
- [ ] Vorlage (A4 PDF/Word) erstellen und gestalten
	- **ZU KLÄREN BEVOR WEITER MIT ETWAS ANDEREM:**
		- Ich mache die Vorlage in Affinity Publisher.
		- Ist es für die späteren Schritte wichtig, wie gross die Felder zum ausmalen sind? 
		- Auf was muss ich achten, wenn ich die Vorlage erstelle?
		- Wie erkennt das Script die Felder des Steckbriefes resp. den Bereich des Pixelart-Bereichs?
- [ ] OCR-Technologie wählen: Tesseract (lokal/gratis) vs. Google Vision API (genauer, kostenpflichtig)
- [ ] Entscheiden ob ein manuelles Korrektur-Tool für fehlerhafte OCR-Erkennungen sinnvoll ist
- [ ] Python-Script entwickeln (Scan → PNG + JSON)
- [ ] Wimmelbild-Frontend entwickeln (index.html)

---

## Technologie-Stack (geplant)

| Komponente | Technologie |
|---|---|
| Vorlage | PDF / Word |
| Verarbeitungs-Script | Python |
| Bildverarbeitung | OpenCV, Pillow |
| Texterkennung | Tesseract OCR oder Google Vision API |
| Frontend | HTML, CSS, JavaScript (vanilla) |
| Datenaustausch | JSON |
