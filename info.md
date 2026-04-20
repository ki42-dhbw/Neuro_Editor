# Neuro Editor — Technische Infos

## Training ohne TensorFlow/Keras

Das Training ist komplett in Vanilla-JavaScript implementiert — keine externen Bibliotheken, kein Backend, alles läuft im Browser. Die relevanten Code-Blöcke in der Hauptdatei:

| Komponente | Funktion | Beschreibung |
|---|---|---|
| Loss-Funktionen | `calcLoss()`, `lossGrad()` | MSE, MAE, BCE, CCE (Categorical Cross-Entropy) |
| Aktivierungen | `applyAct()` | ReLU, Sigmoid, Tanh, Softmax, LeakyReLU, Linear |
| Forward Pass | `forwardTrain()` | Topologisch sortiert, Softmax-Gruppen-Normalisierung |
| Ableitungen | `actDeriv()` | Analytische Ableitungen aller Aktivierungsfunktionen |
| Backpropagation | `backprop()` | Gradienten rückwärts durch das Netz propagieren |
| Optimizer-Update | `optimUpdate()` | SGD, Momentum, Adam, RMSProp, Adagrad |
| Training-Loop | `startTraining()` | Epochen, Batches, Shuffle, Metriken, Early Stopping |

### Ablauf pro Trainingsschritt

```
forwardTrain(inputs)              ← Eingabe → Aktivierungen berechnen
calcLoss(preds, targets)          ← Fehler messen
backprop(vals, preAct, targets)   ← Gradienten rückwärts propagieren
optimUpdate(grads)                ← Gewichte anpassen (Adam etc.)
```

Das ist im Prinzip das, was TensorFlow/Keras intern auch tut — nur hier in ~500 Zeilen JS statt in einer C++/CUDA-Bibliothek.

### Keras-Export

Die App nutzt TensorFlow/Keras nicht selbst, bietet aber eine **Export-Funktion** (`exportKeras()`): Sie generiert ein Python-Skript (`export_keras_model.py`) zum Download, das:
- TensorFlow automatisch installiert (`pip install tensorflow`)
- Das im Browser gebaute Netz als `keras.Sequential`-Modell nachbaut
- Alle trainierten Gewichte setzt
- Das Modell als `.keras`-Datei speichert

---

## Performance-Grenzen der Browser-Implementierung

### Engpässe

1. **`neurons.find()` und `neurons.filter()` in jeder Berechnung** — O(n) Suche statt Lookup-Tabelle
2. **`connections.filter()` in Backprop** — für jedes Neuron alle Verbindungen durchsuchen
3. **`topoSort()` wird bei jedem Forward Pass neu berechnet**
4. **Kein Batch-Parallelismus** — Samples werden einzeln sequentiell durch `backprop()` geschickt
5. **Alles Single-Thread** — kein WebWorker, kein WASM, kein GPU

### Grobe Netzwerk-Größenlimits

| Netzgröße | Neuronen | Verbindungen | Training |
|---|---|---|---|
| **Problemlos** | ~5–20 | ~20–100 | Flüssig, auch mit Animation |
| **Geht noch** | ~20–50 | ~100–500 | Spürbare Verzögerung pro Epoche, aber machbar |
| **Grenzwertig** | ~50–100 | ~500–2000 | Mehrere Sekunden pro Epoche, UI ruckelt |
| **Unpraktisch** | >100 | >2000 | Browser hängt, Tabs frieren ein |

### Beispiele

- Vollvernetztes `2, 4, 3, 1`-Netz (10 Neuronen, 23 Verbindungen): Training quasi instant
- Vollvernetztes `10, 64, 32, 10`-Netz (116 Neuronen, ~3000 Verbindungen): Browser kämpft spürbar

### Warum ist das so?

Der eigentliche Flaschenhals ist weniger die reine Rechenzeit, sondern die **O(n²)-Suchen** per `filter`/`find` auf den Arrays bei jedem einzelnen Sample. Ein Framework wie TensorFlow rechnet das in Matrixoperationen auf der GPU — hier ist alles Neuron-für-Neuron in einer Schleife.

Für den **Lehr-Zweck** der App ist das aber perfekt — Netze mit 5–20 Neuronen sind genau die Größe, die man auf dem Canvas noch visuell verstehen kann.

### Mögliche Optimierungen (falls größere Netze nötig)

- **Lookup-Maps** statt `neurons.find()` → O(1) statt O(n)
- **Adjacency-Listen** statt `connections.filter()` → O(Grad) statt O(Verbindungen)
- **Topo-Sort cachen** → nur bei Strukturänderung neu berechnen
- **WebWorker** für Training → UI bleibt responsiv
- **Matrixform** statt Neuron-für-Neuron → SIMD-freundlicher

---

## Sprachausgabe (Text-to-Speech)

### Technologie

Die Tutorial-Sprachausgabe nutzt die **Web Speech API** (`window.speechSynthesis`), die direkt im Browser eingebaut ist. Es wird keine externe Bibliothek, kein Cloud-Dienst und kein API-Key benötigt. Die Sprachsynthese läuft vollständig lokal auf dem Rechner des Nutzers.

### Implementierung

| Funktion | Beschreibung |
|---|---|
| `tutTtsLoadVoices()` | Lädt alle verfügbaren Stimmen vom Betriebssystem, sortiert deutsche Stimmen nach vorne |
| `tutTtsToggle()` | Schaltet Sprachausgabe ein/aus (Button 🔇/🔊) |
| `tutTtsSetVoice(idx)` | Wählt eine Stimme aus der Dropdown-Liste |
| `tutTtsSpeak(text)` | Spricht Text, gibt Promise zurück (resolved nach Ende der Ausgabe) |
| `tutTtsStop()` | Bricht laufende Sprachausgabe sofort ab |

Die Sprechgeschwindigkeit wird an die Tutorial-Geschwindigkeit gekoppelt:
```
utt.rate = Math.min(1.4, 0.8 + (TUT.speed - 1) * 0.15)
```
Bei Geschwindigkeit 1 (🐢): Rate 0.80 · Bei Geschwindigkeit 5 (🐇): Rate 1.40.

### Rechner- und Betriebssystem-Abhängigkeit

Die Sprachausgabe ist **stark vom Betriebssystem und Browser abhängig**, weil die Web Speech API nur ein Interface bereitstellt — die eigentliche Synthese übernimmt das Betriebssystem:

#### Windows 10/11
- **Stimmen:** Microsoft liefert für jede Systemsprache mindestens eine Stimme mit. Deutsch: typisch „Microsoft Katja" (Desktop) oder „Microsoft Stefan" (neuere Versionen). Zusätzliche Stimmen über Einstellungen → Zeit & Sprache → Sprachausgabe installierbar.
- **Qualität:** Ältere SAPI-Stimmen klingen robotisch. Windows 11 hat natürlichere „Neural Voices", diese stehen aber nicht allen Browsern über die Web Speech API zur Verfügung.
- **Chrome:** Bringt zusätzlich Google-eigene Stimmen mit (z.B. „Google Deutsch"), die über Netzwerk-Synthese laufen und natürlicher klingen, aber eine Internetverbindung benötigen können.
- **Edge:** Nutzt die Windows-Stimmen direkt, hat aber Zugriff auf die hochwertigen Microsoft-Edge-Stimmen.
- **Firefox:** Nutzt ausschließlich die Windows-SAPI-Stimmen. Weniger Auswahl, aber zuverlässig.

#### macOS
- **Stimmen:** Apple liefert hochwertige Stimmen mit. Deutsch: z.B. „Anna" (kompakt) oder „Petra" (erweitert, muss nachgeladen werden über Systemeinstellungen → Bedienungshilfen → Gesprochene Inhalte).
- **Qualität:** Sehr gut, besonders die erweiterten Stimmen. Apple hat einige der besten Offline-TTS-Stimmen.
- **Safari:** Beste Integration, alle System-Stimmen verfügbar.
- **Chrome/Firefox:** Zugriff auf die System-Stimmen, gelegentlich leicht verzögert.

#### Linux
- **Stimmen:** Abhängig von der Distribution. Häufig ist **eSpeak-NG** vorinstalliert — funktional, aber sehr robotisch. Bessere Alternative: **Festival** oder **MBROLA**-Stimmen, müssen aber manuell installiert werden.
- **Deutsche Stimmen:** Oft **nicht vorinstalliert**. eSpeak-NG hat einen deutschen Synthesizer, der verständlich aber synthetisch klingt. Für natürlichere Stimmen muss der Nutzer `mbrola-de1` oder ähnliche Pakete installieren.
- **Chrome:** Bringt eigene Google-Stimmen mit (Netzwerk), funktioniert daher oft besser als Firefox.
- **Firefox:** Nutzt nur System-Stimmen (speech-dispatcher + eSpeak/Festival). Wenn keine deutschen Stimmen installiert sind, ist die Dropdown-Liste leer oder enthält nur englische Stimmen.
- **Verfügbarkeit:** Am unzuverlässigsten unter allen Plattformen. `speechSynthesis.getVoices()` kann ein leeres Array zurückgeben.

#### Android (Mobile Browser)
- **Chrome:** Nutzt die Android-TTS-Engine (standardmäßig Google TTS). Deutsche Stimmen meist vorinstalliert und in guter Qualität.
- **Firefox:** Unterstützt die Web Speech API auf Android **nicht** oder nur eingeschränkt.
- **Samsung Internet:** Nutzt Samsung TTS, deutsche Stimmen abhängig vom Gerät.

#### iOS / iPadOS
- **Safari:** Funktioniert, nutzt die hochwertigen Apple-Stimmen. Einschränkung: `speechSynthesis.speak()` muss durch eine Benutzeraktion (Klick/Touch) ausgelöst werden — automatisches Abspielen ist blockiert.
- **Chrome/Firefox auf iOS:** Sind unter der Haube alle WebKit/Safari, verhalten sich identisch.

### Bekannte Probleme und Workarounds

| Problem | Betrifft | Workaround im Code |
|---|---|---|
| **Chrome-Bug:** Nach `speechSynthesis.cancel()` funktioniert der nächste `speak()`-Aufruf nicht sofort | Chrome (alle Plattformen) | 50ms `setTimeout` nach `cancel()` bevor neues `speak()` (Zeile 16346) |
| **Leere Stimmliste:** `getVoices()` gibt beim ersten Aufruf ein leeres Array zurück | Chrome, Edge | Event `onvoiceschanged` registriert — Stimmen werden nachgeladen (Zeile 16316) |
| **Keine deutschen Stimmen:** Dropdown zeigt nur englische oder keine Stimmen | Linux, ältere Android-Geräte | Sortierung bevorzugt deutsche Stimmen, fällt aber auf die erste verfügbare Stimme zurück |
| **Sprache wird abgeschnitten:** Sehr lange Texte (>300 Zeichen) werden nach ~15 Sekunden gestoppt | Chrome (bekannter Bug) | Tutorial-Texte sind auf kurze Absätze aufgeteilt |
| **Autoplay-Blockade:** Browser blockiert Sprachausgabe ohne vorherige Nutzerinteraktion | iOS Safari, manche Android-Browser | Sprachausgabe startet erst nach Klick auf „▶ Abspielen" oder „⏭ Schritt" |
| **Promise hängt:** `onend`-Event wird nicht gefeuert wenn der Tab im Hintergrund ist | Alle Browser | `onerror`-Handler als Fallback, `_ttsResolve`-Mechanismus löst vorheriges Promise bei neuem `speak()` auf |

### Empfehlung für Nutzer

| Plattform | Empfohlener Browser | Sprachqualität |
|---|---|---|
| **Windows 10/11** | Chrome oder Edge | ★★★★☆ Gut (Google/Microsoft-Stimmen) |
| **macOS** | Safari oder Chrome | ★★★★★ Sehr gut (Apple-Stimmen) |
| **Linux** | Chrome | ★★☆☆☆ Mäßig (eSpeak) bis ★★★★☆ (mit MBROLA) |
| **Android** | Chrome | ★★★★☆ Gut (Google TTS) |
| **iOS** | Safari | ★★★★★ Sehr gut (Apple-Stimmen, Autoplay-Einschränkung) |

### Fallback-Verhalten

Wenn keine Sprachausgabe verfügbar ist, funktioniert die App und das Tutorial-System trotzdem vollständig:
- Der 🔇 Sprache-Button bleibt deaktiviert
- Die Stimmen-Dropdown-Liste bleibt leer
- Alle Tutorials laufen normal mit visuellen Tooltips und Spotlight-Cursor
- Die `tutTtsSpeak()`-Funktion resolved das Promise sofort (`if (!TTS.enabled || !text) { resolve(); return; }`)
- Kein Fehler, keine Warnung — die Sprachausgabe ist ein optionales Feature

### Deutsch/Englisch-Mischsprache: `ttsSanitize()`

Da die Tutorial-Texte deutsche Sätze mit englischen ML-Fachbegriffen mischen (z.B. „Der Cross-Entropy-Loss sinkt"), die deutsche TTS-Engine aber alle Wörter nach deutschen Regeln ausspricht, gibt es eine Bereinigungsfunktion `ttsSanitize(text)`. Sie wird automatisch in `tutTtsSpeak()` aufgerufen, bevor der Text an die `SpeechSynthesisUtterance` übergeben wird.

**Was sie tut:**

1. **Phonetische Ersetzung englischer Fachbegriffe** — über 80 Begriffe werden in deutsche Lautschrift umgewandelt, z.B.:
   - „Loss" → „Lohss" (statt deutsches „Loß")
   - „Batch Size" → „Bätsch-Seis"
   - „Backpropagation" → „Bäckproppagäischen"
   - „Softmax" → „Ssoftmäx"
   - „DQN" → „Dee-Kjuh-Enn"
   - „ReLU" → „Reh-Luh"

2. **Formeln und Sonderzeichen sprechbar machen** — mathematische Notation wird in gesprochene Sprache übersetzt:
   - `ŷ` → „y-Dach"
   - `w₁` → „w eins"
   - `∂L/∂wᵢ` → „Ableitung von L nach w i"
   - `η` → „Eta", `ε` → „Epsilon"
   - `√` → „Wurzel aus", `≈` → „ungefähr"

3. **Gedankenstriche → Sprechpausen** — die Web Speech API unterstützt kein SSML, reagiert aber auf Satzzeichen. Gedankenstriche (— / –) werden durch Punkte ersetzt, die eine natürliche Pause erzeugen.

**Wichtig:** Die Funktion verändert nur den an die TTS-Engine übergebenen Text. Die sichtbaren Tooltips und Beschreibungen bleiben unverändert. Alle bestehenden und zukünftigen Tutorials profitieren automatisch, ohne dass Texte manuell angepasst werden müssen.
