# Neuro Editor — Visueller Neural Network Builder

> Ein vollständig browser-basierter, interaktiver Editor und Simulator für neuronale Netze — ohne Installation, ohne Backend, ohne Build-Schritt.

**Autor:** Stefan Bauer · DHBW Mosbach 2026  
**Sprache:** Vanilla HTML · CSS · JavaScript (keine Abhängigkeiten)  
**Version:** v33

**[Hier sofort ausprobieren](https://ki42-dhbw.github.io/Neuro_Editor/)**

---

## Überblick

Der **Neuro Editor** ist eine Lehr- und Experimentierplattform für künstliche neuronale Netze. Sie läuft vollständig im Browser und benötigt keine Installation, keinen Server und keine externe Bibliothek (außer Google Fonts). Alle Module — vom grafischen Netz-Editor über den Backpropagation-Trainer bis zum CNN-Faltungssimulator — sind in einer einzigen HTML-Datei enthalten.

---

## Module & Funktionen

### 1. Neural Network Builder (Canvas-Editor)

- Drag & Drop von Neuronen (Input, Hidden, Output, Bias) auf einem unendlichen Canvas
- Verbindungen durch Klick-zu-Klick im Verbindungsmodus
- Gewichte editierbar (Doppelklick), einfrierbar (Weight Freeze)
- Raster-Snap, Zoom (10–400 %), Pan, Lasso-Selektion
- Rechtsklick-Kontextmenü für Neuronen und Hintergrund
- Speichern / Laden als JSON · Python/Keras-Export
- Aktivierungsfunktionen: **ReLU · Sigmoid · Tanh · Softmax · Linear · Leaky ReLU**

### 2. Simulation (Forward Pass)

- Animierte Signalpakete auf Verbindungslinien (cyan = positiv, orange = negativ)
- Eingabewerte direkt auf dem Canvas editierbar
- Steuerung: Geschwindigkeit, Einzelschritte, Loop, Aktivierungsvisualisierung

### 3. Überwachtes Lernen / Backpropagation-Training

- Verschiebbares, andockbares Trainings-Panel
- Trainingsdaten: Tabelleneingabe, Drag & Drop, Vorlagen (XOR, AND, Linear), CSV/JSON-Import
- Auto-Modus und Einzelschritt-Modus (Forward → Backward → Weight Update)
- Echtzeit-Metriken: Loss, Accuracy, Gradienten-Norm, Lernrate
- Charts: Loss-Kurve, Accuracy-Kurve, Gradienten-Norm, LR-Verlauf, Gewichtshistogramm, Konfusionsmatrix
- Optimierer: **SGD · Momentum · Adam · RMSProp · Adagrad**
- Loss-Funktionen: **MSE · MAE · BCE · CCE (Categorical Cross-Entropy)**
- Hyperparameter: Epochen, Batch-Größe (1–32 + Full), L2-Regularisierung, Dropout, Gradient Clipping, LR-Scheduler, Early Stopping

### 4. CNN-Modul — Faltungsmatrizen Visualisierung

- Schwebendes, verschiebbares und größenänderbares Fenster
- **Dreispalten-Layout:** Eingabebild → 3×3-Kernel → Ausgabebild (Feature Map)
- **Eingabebilder:**
  - 7 synthetische Testmuster (32×32 Graustufen): Schachbrett, Kreis, Diagonale, Streifen, Buchstabe A, Kante, Gradient
  - Eigene Fotos laden — werden farbig dargestellt, intern auf Graustufen konvertiert
  - Auflösungs-Dropdown für Fotos: 64 / 96 / 128 / 256 / 512 px oder Originalgröße
- **10 Kernel-Presets:** Identität, Box-Blur, Gauß, Schärfen, Sobel-X/Y, Laplace, Emboss, Verschiebung, Kantenschärfe
- **6 Animationsmodi:**
  - Sliding Window — Kernel gleitet Pixel für Pixel
  - Verbindungslinien — 9 animierte gewichtete Leitungen
  - 3D-Lift — Patch hebt sich perspektivisch aus dem Bild
  - Wellen — konzentrische Wellenpulse vom Ausgabepixel
  - Histogramm — Live-Verteilung der Ausgabewerte
  - Filter-Stapel — alle 10 Kernel gleichzeitig vergleichen
- Parameter: Divisor, Bias, Randbehandlung (Zero-Padding / Erweitern / Abschneiden)
- PNG-Download des Ausgabebilds

### 5. Embedding Lab

- **Tab 1** Token-Zerlegung: Simulierte BERT-Tokenisierung mit Farb-Kodierung
- **Tab 2** 3D-Vektor-Raum: Interaktive 3D-Punktwolke mit PCA-Simulation (semantische Cluster)
- **Tab 3** VectorDB & RAG: Lokale Browser-Vektordatenbank, Cosinus-Ähnlichkeitssuche, ChromaDB-Connector

### 6. Reinforcement Learning

- **GridWorld:** Q-Learning mit Q-Tabelle, editierbare Karte, Heatmap, Pfeilvisualisierung
- **BrickBlaster:** Arkanoid-artiges Spiel als RL-Umgebung (manuell oder KI-gesteuert)
- Lernmodi: klassisches Q-Learning (Q-Tabelle) und Deep RL (DQN mit neuronalem Netz)
- DQN-Viewer zeigt das lernende Netz live; Export in den NN-Builder möglich

### 7. 3D-Ansicht

- Isometrische 3D-Darstellung des Netzwerks
- Schichten räumlich versetzt, Orbit/Pan-Steuerung
- Simulation und Backpropagation-Animation in 3D

### 8. KI-Chat

- Verbindung mit LM Studio (lokaler Sprachmodell-Server)
- Kontext-Chips: aktuelles Netzwerk, Simulation, Embeddings als Kontext senden
- Fragen zum Netz stellen — kein API-Key nötig (lokales LLM)

### 9. Netzwerk-Generator

- Automatisch vollvernetztes Netz erstellen (Architektur per Eingabe, z. B. `2, 4, 3, 1`)

### 10. Info-Drawer (Lehr-Panel)

- Ausklappbares Panel mit Lehrfolien im Pseudo-Browser
- Themen: Neuron & KNN, Einführung in KI, Backpropagation, XOR-Problem, CNN-Erklärung, Hilfe

---

## Interaktive Tutorials

7 animierte Tutorials mit Spotlight-Cursor und optionaler deutscher Sprachausgabe (TTS). Navigation: **▶ Abspielen**, **⏮ Zurück**, **⏭ Schritt**, **⏹ Stopp**.

| Nr. | Tutorial | Inhalt |
|---|---|---|
| **Menüleisten** | | |
| 1 | Die erste Menüleiste | Alle Header-Buttons erklärt |
| 2 | Embedding Lab | Schnelleingabe, Token-Zerlegung, 3D Vektor-Raum, RAG/VectorDB |
| 3 | Reinforcement Learning | Simulator, GridWorld/BrickBlaster, Map-Werkzeuge, RL-Hyperparameter |
| **Netz & Simulation** | | |
| 4 | Einfaches Netz aufbauen | Input, Hidden, Bias, Output per Drag & Drop platzieren und verbinden |
| 5 | Simulation & Forward Pass | Simulations-Panel, Eingabewerte, Datenpakete, Formel-Erklärung |
| **Training** | | |
| 6 | Regression (y = 3·x) | 15 Trainingspaare, alle Hyperparameter, Live-Metriken, gelernte Gewichte |
| 7 | Klassifikation (Softmax & One-Hot) | Fahrzeug-Klassifikation (Fahrrad/Auto/LKW), One-Hot-Encoding, Softmax, CCE-Loss |

---

## Schnellstart

```
1. Repository klonen oder ZIP herunterladen
2. Datei  neural-network-builder_simu_netz_v33.html  im Browser öffnen
3. Fertig — keine Installation, kein Server nötig
```

> Empfohlene Browser: **Chrome** oder **Edge** (aktuell). Firefox funktioniert, Safari eingeschränkt.

---

## Tastaturkürzel

| Taste | Funktion |
|---|---|
| `S` | Auswahl-Modus |
| `C` | Verbinden-Modus |
| `Ctrl`+`A` | Alle auswählen |
| `Del` | Auswahl löschen |
| `Esc` | Abbrechen / Aufheben |
| `+` / `-` | Zoom |

---

## Projektstruktur

```
Neuroedit_webapp/
├── neural-network-builder_simu_netz_v33.html   ← Hauptanwendung (aktuell)
├── index.html                                  ← Kopie von v33 für GitHub Pages
├── info.md                                     ← Technische Infos (Training, Performance, TTS)
├── HTML/
│   ├── hilfe.html                              ← Hilfe-Seite (Info-Drawer)
│   ├── info_1.html … info_4.html               ← Lehrfolien
│   ├── start.html                              ← Startseite Info-Panel
│   └── rl_hilfe.html                           ← RL-Hilfe
└── Bilder/                                     ← Beispielfotos für CNN

```

---

## Technische Details

Ausführliche technische Dokumentation in **[info.md](info.md)**:

- **Training ohne TensorFlow/Keras** — Backpropagation, Optimizer (SGD, Adam, RMSProp, Adagrad) und 4 Loss-Funktionen (MSE, MAE, BCE, CCE) sind komplett in ~500 Zeilen Vanilla-JS implementiert
- **Performance-Grenzen** — Netzwerk-Größenlimits der Browser-Implementierung (problemlos bis ~20 Neuronen, grenzwertig ab ~100) und mögliche Optimierungen
- **Sprachausgabe (TTS)** — Betriebssystem-/Browser-Abhängigkeit, bekannte Bugs und Workarounds, Empfehlungen pro Plattform
- **Deutsch/Englisch-Mischsprache** — Automatische phonetische Aufbereitung englischer Fachbegriffe (Loss, Batch, Softmax etc.) für die deutsche TTS-Engine via `ttsSanitize()`
- **Keras-Export** — Generiert ein Python-Skript mit dem trainierten Netz als `keras.Sequential`-Modell

---

## Speicherformate

**Netzwerk-JSON**
```json
{
  "neurons": [{ "id": "n1", "type": "input", "x": 100, "y": 200, "label": "x₁", "activation": null }],
  "connections": [{ "from": "n1", "to": "n2", "weight": 0.42, "frozen": false }],
  "viewport": { "x": 0, "y": 0, "scale": 1 },
  "regions": []
}
```

**Trainingsdaten-JSON**
```json
{ "inputs": [[0,0],[0,1],[1,0],[1,1]], "outputs": [[0],[1],[1],[0]] }
```

---

## Technologie

| Bereich | Technologie |
|---|---|
| Sprache | Vanilla JavaScript (ES2020+) |
| UI | HTML5 + CSS3 (Custom Properties, Flexbox, Grid) |
| Grafik | Canvas 2D API, requestAnimationFrame |
| Schriften | JetBrains Mono · Syne (Google Fonts CDN) |
| Persistenz | Browser LocalStorage + JSON-Datei-Download/Upload |
| KI-Logik | Vollständig lokal simuliert — kein API-Key erforderlich |
| TTS | Web Speech API (Browser-intern, betriebssystemabhängig) |
| Backend | Keines |
| Build | Keiner |

---

## Lizenz

© Stefan Bauer, DHBW Mosbach 2026 — Alle Rechte vorbehalten.  
Nutzung für Lehr- und Bildungszwecke gestattet.
