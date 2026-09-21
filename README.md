# IFC & Ökobaudat LCA Webanwendung

Eine webbasierte Benutzeroberfläche zur **Extraktion von IFC-BIM-Modellen** (Schichtaufbauten, Geometrien, Mengen) und zur **Filterung großer Ökobaudat-Datenbanken** mit automatischer GWP-Mittelwertbildung und Toleranzprüfung.

---

## 🌟 Funktionsübersicht

Die Webanwendung vereint zwei zentrale Workflows in einer intuitiven Weboberfläche:

### 1. 📊 Ökobaudat Filter & GWP-Mittelwerte
- **Große CSV-Uploads**: Verarbeitet umfangreiche Ökobaudat-Exportdateien (bis zu 250 MB) direkt über den Browser.
- **Speichereffizientes Streaming**: Schnelle Filterung nach Schlagwörtern/Baustoffen (z. B. `Kalksandstein`, `Beton`, `Dämmung`).
- **Lebenszyklus-Module & GWP-Mittelwert**:
  - Ermittelt Min-, Max- und Mittelwerte für Lebenszyklusphasen wie `A1-A3`, `A4`, `A5`, `C1-C4`, `D`.
- **Toleranzprüfung (15% Standard)**:
  - Berechnet die relative Streuung $\frac{\text{Max} - \text{Min}}{\text{Mittelwert}}$.
  - Markiert Werte optisch als valide ($\le 15\%$) oder außerhalb der Toleranz ($> 15\%$).
- **Exportmöglichkeiten**:
  - Download der berechneten **GWP-Zusammenfassungs-CSV**.
  - Download der **gefilterten Rohdaten-CSV**.

### 2. 🏗️ IFC Material- & Mengenauswertung
- **Drag & Drop Upload**: Unterstützung für IFC2x3- und IFC4-Dateien (z. B. aus Vectorworks, Revit, ArchiCAD).
- **Schichtgenaue Zerlegung**:
  - Analyse von mehrschichtigen Bauteilen (`IfcMaterialLayerSet`, `IfcMaterialConstituentSet`).
  - Berechnung der Schichtdicken, Flächen ($m^2$) und Volumina ($m^3$) über $V = A \cdot d$ oder komplexe Schichtmengen (`IfcPhysicalComplexQuantity`).
- **Übersichtsansichten**:
  - **Detail pro Schicht**: Jede Bauteilschicht mit GUID, Wandtyp, Material, Schichtdicke, Fläche und Volumen.
  - **Materialsummen**: Aggregierte Gesamtmengen pro Baustoff über das gesamte Modell.
- **Export**:
  - Download als strukturierte **Schichtendetail-CSV** (10-Spalten-Standard für LCA-Folgeberechnungen).
  - Download als **Materialsummen-CSV**.

---

## 🚀 Schnellstart & Lokale Ausführung

### Voraussetzungen
- Python 3.9+ (getestet mit Python 3.10–3.14)
- Pip-Paketmanager

### 1. Installation

```bash
git clone <repository-url>
cd ifc-lca-analysis
pip install -e .
```

### 2. Webserver starten

Es gibt verschiedene Möglichkeiten, die Webanwendung zu starten:

#### Option A: Über das Starter-Skript (Empfohlen)
```bash
python3 run_app.py
```

#### Option B: Direkt über das Modul
```bash
python3 -m ifc_lca_analysis.web_app
```

#### Option C: Über den CLI-Befehl
```bash
ifc-lca-web
```

#### Option D: macOS Quick-Start
Doppelklick auf `restart_server.command` im Projekt-Hauptverzeichnis.

### 3. Webanwendung aufrufen
Öffne deinen Webbrowser unter:
👉 **[http://127.0.0.1:5001](http://127.0.0.1:5001)** (oder Port `5000`)

---

## 🖥️ Bedienung der Webanwendung

### Tab 1: Ökobaudat Filter
1. Ziehe deine `Oekobaudat.csv` in die Dropzone.
2. Gib die gewünschten **Suchbegriffe** ein (kommagetrennt, z. B. `Kalksandstein, Schaumglas`).
3. Passe bei Bedarf die **Toleranzgrenze** (Standard: 15%) und die **Module** (z. B. `A1-A3, A4, A5`) an.
4. Klicke auf **„Filtern & Berechnen“**.
5. Prüfe die berechneten Mittelwerte in der Tabelle und exportiere die Ergebnisse als CSV.

### Tab 2: IFC Extraktion
1. Ziehe eine `.ifc`-Datei in den Upload-Bereich.
2. Das Modell wird automatisch geparst und aufbereitet.
3. Wechsle zwischen **„Detail pro Schicht“** und **„Materialsummen“**.
4. Lade die aufbereiteten Daten mit einem Klick auf **„⬇ Schichtendetail CSV“** oder **„⬇ Materialsummen CSV“** herunter.

---

## 🔌 API-Endpunkte

Die Flask-Anwendung stellt für Automatisierungen folgende REST-Endpunkte bereit:

| Endpunkt | Methode | Beschreibung |
|---|---|---|
| `/` | `GET` | Liefert das Web-Frontend aus |
| `/api/extract` | `POST` | Nimmt eine IFC-Datei entgegen und liefert extrahierte Schicht- und Bauteildaten als JSON |
| `/api/export-csv` | `POST` | Erzeugt eine 10-Spalten-Schichtendetail-CSV aus den IFC-Daten |
| `/api/export-summary-csv` | `POST` | Erzeugt eine aggregierte Materialsummen-CSV aus den IFC-Daten |
| `/api/obd/filter` | `POST` | Filtert eine hochgeladene Ökobaudat-CSV nach Suchbegriffen & Modulen und liefert Mittelwerte sowie Einzeldaten als JSON |
| `/api/obd/export-summary` | `POST` | Exportiert die berechneten GWP-Mittelwerte als CSV |
| `/api/obd/export-filtered` | `POST` | Exportiert die gefilterten Rohdatensätze als CSV |

---

## 📦 Verwendete Technologien

- **Backend**: Python, [Flask](https://flask.palletsprojects.com/)
- **IFC-Parsing**: [IfcOpenShell](https://ifcopenshell.org/)
- **Frontend**: Vanilla JavaScript & Responsive CSS (keine zusätzlichen Frameworks/Node-Builds nötig)
- **Architektur**: Speichereffizientes Streaming und In-Memory-Verarbeitung für bis zu 250 MB Dateigröße

---

## 🧪 Tests

```bash
pytest
```
