# Neural Network Builder

Ein visueller **Editor für neuronale Netze** — vollständig im Browser, keine Installation nötig.
Mit dem Tool lassen sich Netzwerke per **Drag & Drop** erstellen, simulieren, trainieren und exportieren.
Zusätzlich enthält die App ein **Embedding Lab** und einen integrierten **Reinforcement Learning Simulator** mit klassischem Q-Learning und Deep Q-Network (DQN).

Die Anwendung richtet sich besonders an Lernende, Lehrende und alle, die KI-Konzepte **interaktiv verstehen und demonstrieren** möchten.

---

## Aktuelle Version

**`neural-network-builder_simu_netz_v22.html`**

---

## Projektstruktur

Die App besteht aus mehreren Dateien, die im **gleichen Verzeichnis** liegen müssen:

```
📁 Projektordner/
│
├── neural-network-builder_simu_netz_v20.html   ← Hauptdatei (im Browser öffnen)
├── ani_gif_1.gif                                ← Startanimation (optional)
│
├── 📁 HTML/
│   ├── info_1.html          ← Info-Panel: Neuron & KNN
│   ├── info_2.html          ← Info-Panel: Einführung
│   ├── info_3.html          ← Info-Panel: Backpropagation
│   ├── info_4.html          ← Info-Panel: XOR-Problem
│   ├── hilfe.html           ← Info-Panel: Allgemeine Hilfe
│   ├── rl_hilfe.html        ← Info-Panel: RL-Hilfe & Deep RL-Erklärung
│   ├── start.html           ← Info-Panel: Wie starte ich
│   └── 📁 Bilder/ (optional)
│       ├── ArtificialNeuronModel_english.png
│       └── Neuron_(deutsch)-1.svg
```

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
- Eingabefelder der Eingangsneuronen direkt auf dem Canvas — **frei verschiebbar** per Drag, Doppelklick zum Zurücksetzen
- **Überwachtes Lernen** mit eigenen Trainingsdaten (Drag & Drop CSV/JSON)
- Hyperparameter: Lernrate, Epochen, Batch-Größe, Optimizer, Loss-Funktion
- **Trainingsvisualisierung**: Loss, Accuracy, Gradienten-Norm, Lernrate, Konfusionsmatrix, Gewichts-Histogramm

### KI-Assistent
- Verbindung mit lokalem LLM über **LM Studio** (OpenAI-kompatible API)
- Kontextchips: Netzwerktopologie, Simulationswerte, Embeddings, Trainingsmetriken an LLM mitschicken
- **Vorschau** der gesendeten Daten vor dem Absenden (👁-Button)
- Unterstützung für **Thinking-Modelle** (Qwen, DeepSeek-R1): Denkprozess als aufklappbares Panel
- **Max. Tokens** frei einstellbar (256–131 072) mit Schnellauswahl: 1K / 2K / 4K / 8K / 16K / 32K
- Dynamisches Timeout: skaliert automatisch mit der Token-Anzahl
- **Debug-Modus** (🐛): zeigt Antwortzeiten, HTTP-Status, finish_reason, Token-Verbrauch und Fehlerdetails

### Embedding Lab
- Tokenisierung und Vektordarstellung
- **2D/3D-Embedding-Visualisierung**
- Vektordatenbank

### Reinforcement Learning — Q-Learning
- **Interaktiver Gitterwelt-Simulator** — direkt eingebettet, kein Extra-Fenster
- Karteneditor: Start, Ziel, Löcher, freie Felder per Klick setzen
- Rastergröße frei wählbar (2×2 bis 10×10)
- Q-Learning-Engine mit **Bellman-Gleichung**
- **Q-Tabelle live** im rechten Panel:
  - Bester Wert pro Zeile **rot hervorgehoben**
  - Optimaler Pfad (Start → Ziel) **grün markiert**
  - **Flash-Animation** bei jeder Agentenbewegung
- Heatmap und Pfeil-Visualisierung auf dem Spielfeld
- Parameter: Alpha (Lernrate), Gamma (Weitsicht), Epsilon (Zufall), Geschwindigkeit
- Test-Modus: kein Zufall, nur gelerntes Wissen
- Turbo-Training: 100 Episoden auf einmal

### Reinforcement Learning — Deep Q-Network (DQN)
- Umschaltbar per Toggle zwischen **Q-Learning** und **DQN-Modus**
- Eigenes **2→32→4 MLP** (He-Init, ReLU, lineare Ausgabe) direkt in der App
- **Live-Visualisierung** des neuronalen Netzes in der Steuerleiste:
  - Neuronen farbkodiert nach Aktivierungsstärke
  - 4 Ausgabeneuronen mit Richtungspfeil (←↓→↑), Q-Wert und Beschriftung
  - Beste Aktion gold hervorgehoben
  - Zoom (1×/1.5×/2×/3×) per Button oder Mausrad
- **DQN in den NN-Editor kopieren** — Gewichte als visuelles Netzwerk übertragen
- Korrekte Bellman-Gleichung: reines Target `r + γ·max(Q')` ohne doppeltes Alpha
- Test-Modus: kein Zufall, Netz eingefroren (kein Training)
- Steuerleiste stufenlos in der **Breite verschiebbar** (gespeichert im Browser)

### Allgemeines
- **Rechte Info-Seitenleiste** (einklappbar, stufenlos breite verstellbar): Hilfeseiten, RL-Hilfe inkl. Deep RL
- **Seitenleisten-Toggle**: NN-Panel und RL-Panel wechseln automatisch je nach aktivem Modus
- Alle Einstellungen (Schriftgröße, Panel-Breiten) werden im Browser gespeichert

---

## Nutzung

### Starten

Die Hauptdatei direkt im Browser öffnen — kein Server, kein Build-Schritt nötig:

```
neural-network-builder_simu_netz_v20.html
```

> **Wichtig:** Alle Dateien aus der Projektstruktur müssen vorhanden sein, damit Info-Panel und Hilfe funktionieren.

### Drei Arbeitsbereiche

| Menüleiste | Bereich | Aktivierung |
|---|---|---|
| Oben (dunkel) | Neuronales Netz — Editor & Simulation | immer sichtbar |
| Mitte (blau-violett) | Embedding Lab | Button „🔬 Embedding Lab" |
| Unten (dunkelgrün) | Reinforcement Learning | Button „🤖 Simulator" |

### RL-Schnelleinstieg (Q-Learning)

1. **🤖 Simulator** klicken → Spielfeld und RL-Panel erscheinen
2. Karte nach Wunsch bearbeiten (Werkzeuge oben in der RL-Menüleiste)
3. **🚀 Turbo** in der rechten Leiste klicken (mehrfach, bis Epsilon < 0.2)
4. **🛡️ Test-Modus** aktivieren → **▶️ Start** klicken
5. Agent läuft fehlerfrei zum Ziel

### RL-Schnelleinstieg (DQN)

1. **🤖 Simulator** klicken → im RL-Panel **DQN** aktivieren
2. Karte nach Wunsch bearbeiten
3. **🚀 Turbo** mehrfach klicken (500+ Episoden empfohlen)
4. **Test-Modus** aktivieren → **▶️ Start** klicken
5. Optional: **→ In NN-Editor kopieren** um das trainierte Netz zu visualisieren

---

## Technologie

- **Vanilla HTML / CSS / JavaScript** — keine Frameworks, keine Abhängigkeiten
- Läuft vollständig lokal im Browser (offline-fähig)
- Schriftarten via Google Fonts (JetBrains Mono, Syne) — optional, funktioniert auch ohne

---

## Autor

© Stefan Bauer, DHBW Mosbach 2026
