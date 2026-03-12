# Neural Network Builder

Ein visueller **Editor für neuronale Netze** als einzelne HTML-Datei.  
Mit dem Tool lassen sich Netzwerke per **Drag & Drop** erstellen, verbinden, simulieren, trainieren und als **Keras/TensorFlow-Modell** exportieren.

Die Anwendung richtet sich besonders an Lernende, Lehrende und alle, die neuronale Netze **interaktiv verstehen und demonstrieren** möchten.

## Features

- Visueller Aufbau von neuronalen Netzen per Drag & Drop
- Unterstützung für:
  - Input-Neuronen
  - Hidden-Neuronen
  - Output-Neuronen
  - Bias-Neuronen
- Aktivierungsfunktionen direkt am Neuron auswählbar:
  - ReLU
  - Sigmoid
  - Tanh
  - Softmax
  - Linear
  - Leaky ReLU
- Verbindungen zwischen Neuronen manuell erstellen
- Gewichte bearbeiten, anzeigen und einfrieren
- Lasso-Auswahl und Mehrfachselektion
- Gespeicherte Bereiche wiederverwenden
- Zoom, Raster und anpassbare Schriftgröße
- 2D- und 3D-Ansicht
- Simulationsmodus für Forward Pass
- Überwachtes Lernen mit Trainingsdaten und Hyperparametern
- Trainingsvisualisierung mit:
  - Loss
  - Accuracy
  - Gradienten-Norm
  - Lernrate
  - Konfusionsmatrix
  - Gewichts-Histogramm
- Export des Netzwerks als JSON
- Import eines gespeicherten Netzwerks
- Export als Python-Skript für Keras/TensorFlow

## Projektziel

Der **Neural Network Builder** ist ein interaktives Lehr- und Visualisierungstool für neuronale Netze.  
Im Fokus stehen:

- verständliche Darstellung von Netzstrukturen
- visuelles Lernen von Forward Pass und Backpropagation
- schneller Aufbau kleiner Testnetzwerke
- Demonstration von Trainingsprozessen im Unterricht oder Selbststudium

## Datei

Das Projekt besteht aktuell aus einer zentralen Datei:

- `neural-network-builder_simu_netz_v14.html`

Diese Datei kann direkt im Browser geöffnet werden.

## Nutzung

### Starten

Einfach die HTML-Datei lokal im Browser öffnen:

```bash
neural-network-builder_simu_netz_v14.html
