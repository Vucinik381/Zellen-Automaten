# Simulation der Ausbreitung einer ansteckenden Krankheit mit Zellulären Automaten

## Übersicht

Dieses Projekt simuliert die Ausbreitung einer ansteckenden Krankheit in einer Population mithilfe von **zellulären Automaten**. Es wurde im Rahmen des Kurses "Mathematischer Software" entwickelt und demonstriert, wie mathematische Modelle zur Analyse epidemiologischer Prozesse eingesetzt werden können.

Die Simulation basiert auf einem zweidimensionalen Gitter, bei dem jede Zelle den Gesundheitszustand einer Person repräsentiert. Durch einfache lokale Regeln entstehen komplexe globale Verhaltensmuster, die Einblicke in die Dynamik von Epidemien ermöglichen.

## Motivation

Die COVID-19-Pandemie hat gezeigt, wie wichtig mathematische Modelle für die Einschätzung des Verlaufs einer Epidemie und die Planung geeigneter Maßnahmen sind. Dieses Projekt nutzt **zelluläre Automaten** als Simulationswerkzeug, um:

- Die Verbreitung einer Krankheit in einer räumlich strukturierten Population zu visualisieren
- Den Einfluss verschiedener Parameter (Ansteckungswahrscheinlichkeit, Krankheitsdauer, Impfung, etc.) zu untersuchen
- Ein besseres Verständnis für Infektionsdynamiken zu gewinnen

## Zelluläre Automaten

### Definition

Ein **zellulärer Automat (ZA)** besteht aus:

- **Zellraum (R)**: Ein zweidimensionales Gitter der Größe n×m
- **Nachbarschaftsdefinition (N)**: Von-Neumann-Nachbarschaft (4 direkte Nachbarn: oben, unten, links, rechts)
- **Zellzustände (Q)**: Endliche Menge der möglichen Zustände
  - 0: Gesund und ansteckbar
  - 1: Krank (ansteckend)
  - 2: Gesund und immun
  - -1: Inkubationsphase (nur in erweitertem Modell)
- **Überführungsfunktion (δ)**: Regeln für Zustandsübergänge

### Regeln

Die Simulation folgt deterministischen und stochastischen Regeln:

1. **Ansteckung**: Eine gesunde Zelle wird mit Wahrscheinlichkeit *p* krank, wenn mindestens ein direkter Nachbar krank ist
2. **Genesung**: Eine kranke Zelle wird nach *k* Tagen immun
3. **Immunität**: Eine immune Zelle bleibt dauerhaft immun
4. **Impfung**: Geimpfte Personen werden sofort immun

## Projektstruktur

Das Projekt besteht aus einem Jupyter Notebook (`MS_Final_Code.ipynb`) mit folgenden Hauptkomponenten:

### 1. Grundmodell

Das Basismodell implementiert die Kernfunktionalität:
- Initialisierung eines Gitters mit einer infizierten Person
- Simulation der Krankheitsausbreitung basierend auf Nachbarschaftsregeln
- Visualisierung als animiertes Gitter und Statistikdiagramm

**Hauptparameter:**
- `n, m`: Gittergröße
- `p`: Ansteckungswahrscheinlichkeit (0-1)
- `k`: Krankheitsdauer in Tagen
- `start_pos`: Startposition der ersten infizierten Person

### 2. Erweiterungen

#### Impfung
- Täglich wird eine feste Anzahl gesunder Personen zufällig geimpft
- Geimpfte Personen werden immun und können nicht mehr angesteckt werden
- **Effekt**: Schafft "Brandmauern" und kann die Epidemie verlangsamen oder stoppen

#### Bewegung/Umzug
- Personen tauschen zufällig ihre Plätze im Gitter
- **Effekt**: Führt zu chaotischerer Ausbreitung, entfernte Infektionsherde können entstehen

#### Inkubationszeit
- Neuer Zustand -1 für infizierte, aber noch nicht ansteckende Personen
- Nach *i* Tagen werden inkubierende Personen krank
- **Effekt**: Verzögert die anfängliche Ausbreitung, kann zu plötzlichen Ausbrüchen führen

## Installation und Verwendung

### Voraussetzungen

```bash
pip install numpy matplotlib jupyter
```

### Ausführung

1. Öffnen Sie das Notebook in Jupyter:
```bash
jupyter notebook MS_Final_Code.ipynb
```

2. Führen Sie die Zellen nacheinander aus, um:
   - Das Grundmodell zu simulieren
   - Verschiedene Erweiterungen zu testen
   - Die Animationen anzuzeigen
3. **Alternativ** : Öffnen Sie einfach die Datei MS_Final_Code.html in einem Webbrowser (z.B. Chrome, Firefox). 
                   Die Simulation kann dort direkt gestartet und ausgeführt werden.

### Beispiel

```python
# Grundmodell
animation = run_basic_simulation(
    n=50,           # Gitterhöhe
    m=50,           # Gitterbreite
    start_pos=(25,25),  # Startposition
    p=0.2,          # Ansteckungswahrscheinlichkeit 20%
    k=7,            # Krankheitsdauer 7 Tage
    num_steps=343   # Maximale Simulationsdauer
)
```

## Visualisierung

Die Simulation erzeugt eine Doppelansicht:

1. **Links**: Animiertes Gitter mit Farbcodierung der Zustände
   - Grün: Gesund
   - Rot: Krank
   - Blau: Immun
   - Schwarz: Inkubation (nur im erweiterten Modell)

2. **Rechts**: Statistikdiagramm mit Verlauf der Populationen über die Zeit

## Wichtige Erkenntnisse

### Einfluss der Parameter

- **Ansteckungswahrscheinlichkeit (p)**:
  - Niedrig (z.B. 0.1): Langsame Ausbreitung, Epidemie kann aussterben
  - Hoch (z.B. 0.9): Explosive Ausbreitung, fast vollständige Infektion

- **Krankheitsdauer (k)**:
  - Kurz (z.B. 3 Tage): Kleines Übertragungsfenster, eindämmende Wirkung
  - Lang (z.B. 14 Tage): Viel Zeit zur Ansteckung, größere Gesamtzahl an Infizierten

- **Impfung**:
  - Schafft immune Barrieren
  - Kann Epidemie deutlich verlangsamen oder stoppen
  - Effektivität hängt von Impfrate ab

- **Bewegung**:
  - Erhöht Durchmischung der Population
  - Beschleunigt Ausbreitung
  - Macht Verlauf chaotischer und schwerer vorhersagbar

- **Inkubationszeit**:
  - Verzögert anfängliche Ausbreitung
  - Führt zu "stiller Ausbreitung"
  - Kann zu plötzlichen, exponentiellen Ausbrüchen führen

## Technische Details

### Implementierung

Die Simulation verwendet:
- **NumPy** für effiziente Array-Operationen
- **Matplotlib** für Visualisierung und Animation
- **FuncAnimation** für flüssige Animationen

### Kernfunktionen

- `run_basic_simulation()`: Grundmodell
- `run_simulation_with_vaccination()`: Modell mit Impfung
- `run_simulation_with_movement()`: Modell mit Bewegung
- `run_simulation_with_incubation()`: Modell mit Inkubationszeit
- `create_animation()`: Zentrale Animationsfunktion

## Literatur

- *Zelluläre Automaten*, Johannes Kepler Universität Linz, 2004
  https://ssw.jku.at/General/Staff/HP/SimTech_SS04/Folien/02-ZellulaereAutomaten.pdf

- *Von-Neumann-Nachbarschaft*, Wikipedia
  https://de.wikipedia.org/wiki/Von-Neumann-Nachbarschaft

- *Zellulärer Automat – Eigenschaften*, Wikipedia
  https://de.wikipedia.org/wiki/Zellulärer_Automat#Eigenschaften

## Autor

Projekt erstellt von **Harmony Emadjeu Jontcheu** mit Unterstützung von Prof. Dr. Oliver Rinne im Rahmen des Kurses "Mathematischer Software", Semester 5, Bachelor-Studium.

## Lizenz

Dieses Projekt wurde für Bildungszwecke erstellt.

---

*Letztes Update: November 2025*
