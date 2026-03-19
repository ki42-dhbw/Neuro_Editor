# Neural Network Builder

Ein visueller **Editor für neuronale Netze** — vollständig im Browser, keine Installation nötig.
Mit dem Tool lassen sich Netzwerke per **Drag & Drop** erstellen, simulieren, trainieren und exportieren.
Zusätzlich enthält die App ein **Embedding Lab** und einen integrierten **Reinforcement Learning Simulator**.

Die Anwendung richtet sich besonders an Lernende, Lehrende und alle, die KI-Konzepte **interaktiv verstehen und demonstrieren** möchten.

---

## Aktuelle Version

**`neural-network-builder_simu_netz_v19.html`**

---

## Projektstruktur

Die App besteht aus mehreren Dateien, die im **gleichen Verzeichnis** liegen müssen:

```
📁 Projektordner/
│
├── neural-network-builder_simu_netz_v19.html   ← Hauptdatei (im Browser öffnen)
├── ani_gif_1.gif                                ← Startanimation
│
├── 📁 HTML/
│   ├── info_1.html          ← Info-Panel: Neuron & KNN
│   ├── info_2.html          ← Info-Panel: Einführung
│   ├── info_3.html          ← Info-Panel: Backpropagation
│   ├── info_4.html          ← Info-Panel: XOR-Problem
│   ├── hilfe.html           ← Info-Panel: Allgemeine Hilfe
│   ├── rl_hilfe.html        ← Info-Panel: RL-Hilfe
│   └── 📁 Bilder/
│       ├── ArtificialNeuronModel_english.png
│       └── Neuron_(deutsch)-1.svg
```

> Das `RL/`-Verzeichnis wird von v19 nicht mehr benötigt — der Q-Learning-Simulator ist vollständig eingebettet.

---

## Features

### Neuronales Netz — Editor
- Visueller Aufbau per **Drag & Drop** auf einem unendlichen Canvas
- Neurontypen: Input, Hidden, Output, Bias
- **Aktivierungsfunktionen** direkt am Neuron: ReLU, Sigmoid, Tanh, Softmax, Linear, Leaky ReLU
- Verbindungen manuell erstellen, Gewichte bearbeiten und einfrieren
- **Lasso-Auswahl** und Mehrfachselektion
- **Gespeicherte Bereiche** (Regionen) zum Wiederverwenden
- Zoom, Pan, Raster
- **Anpassbare UI-Schriftgröße** (70 %–150 %, gespeichert im Browser)
- Netzwerk als **JSON** exportieren / importieren
- Export als **Python-Skript** (Keras/TensorFlow)

### Neuronales Netz — Simulation & Training
- **Forward-Pass-Simulation** mit animierten Datenpaketen
- **Überwachtes Lernen** mit eigenen Trainingsdaten (Drag & Drop CSV/JSON)
- Hyperparameter: Lernrate, Epochen, Batch-Größe, Optimizer, Loss-Funktion
- **Trainingsvisualisierung**: Loss, Accuracy, Gradienten-Norm, Lernrate, Konfusionsmatrix, Gewichts-Histogramm
- **KI-Chat** (verbindet sich mit lokalem LLM über LM Studio)

### Embedding Lab
- Tokenisierung und Vektordarstellung
- **2D/3D-Embedding-Visualisierung**
- Vektordatenbank

### Reinforcement Learning (Q-Learning)
- **Interaktiver Gitterwelt-Simulator** — direkt eingebettet, kein Extra-Fenster
- Karteneditor: Start, Ziel, Löcher, freie Felder per Klick setzen
- Rastergröße frei wählbar (2×2 bis 10×10)
- Q-Learning-Engine mit **Bellman-Gleichung**
- **Q-Tabelle live** im rechten Panel:
  - Bester Wert pro Zeile **rot hervorgehoben**
  - Optimaler Pfad (Start → Ziel) **grün markiert**
  - **Flash-Animation** bei jeder Agentenbewegung
- Heatmap und Pfeil-Visualisierung auf dem Spielfeld
- Weiße Felder / schwarze Löcher für maximalen Kontrast
- Parameter: Alpha (Lernrate), Gamma (Weitsicht), Epsilon (Zufall), Geschwindigkeit
- Test-Modus: kein Zufall, nur gelerntes Wissen
- Turbo-Training: 100 Episoden auf einmal

### Allgemeines
- **Rechte Info-Seitenleiste** (einklappbar, stufenlos breite verstellbar): Hilfeseiten, RL-Hilfe
- **Seitenleisten-Toggle**: NN-Panel und RL-Panel wechseln automatisch je nach aktivem Modus
- Alle Einstellungen (Schriftgröße, Panel-Breite) werden im Browser gespeichert

---

## Nutzung

### Starten

Die Hauptdatei direkt im Browser öffnen — kein Server, kein Build-Schritt nötig:

```
neural-network-builder_simu_netz_v19.html
```

> **Wichtig:** Alle Dateien aus der Projektstruktur müssen vorhanden sein, damit Info-Panel und Hilfe funktionieren.

### Drei Arbeitsbereiche

| Menüleiste | Bereich | Aktivierung |
|---|---|---|
| Oben (dunkel) | Neuronales Netz — Editor & Simulation | immer sichtbar |
| Mitte (blau-violett) | Embedding Lab | Button „🔬 Embedding Lab" |
| Unten (dunkelgrün) | Reinforcement Learning | Button „🤖 Simulator" |

### RL-Schnelleinstieg

1. **🤖 Simulator** klicken → Spielfeld und RL-Panel erscheinen
2. Karte nach Wunsch bearbeiten (Werkzeuge oben in der RL-Menüleiste)
3. **🚀 Turbo** in der rechten Leiste klicken (mehrfach, bis Epsilon < 0.2)
4. **🛡️ Test-Modus** aktivieren → **▶️ Start** klicken
5. Agent läuft fehlerfrei zum Ziel

---

## Technologie

- **Vanilla HTML / CSS / JavaScript** — keine Frameworks, keine Abhängigkeiten
- Läuft vollständig lokal im Browser (offline-fähig)
- Schriftarten via Google Fonts (JetBrains Mono, Syne) — optional, funktioniert auch ohne

---

## Autor

© Stefan Bauer, DHBW Mosbach 2026
