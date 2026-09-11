# FT_DERIV_AR

![FT_DERIV_AR](./FT_DERIV_AR.svg)

* * * * * * * * * *

## Einleitung

`FT_DERIV_AR` ist ein Adapter-Wrapper um den OSCAT-Baustein `FT_DERIV`.  
Er kapselt die Berechnung der zeitlichen Ableitung eines Eingangssignals hinter einer rein adapterbasierten Schnittstelle. Die Werte `K` (Ableitungsfaktor) und `run` (Berechnung aktiv) werden als gewöhnliche Input-Variablen durchgereicht und sind nicht Teil des Adapter-Datenflusses.

Der Baustein besitzt keine Klartext-Datenausgänge. Stattdessen werden alle von `FT_DERIV` erzeugten Werte über eigene Adapter-Plugs bereitgestellt:

- `AR_OUT` – die berechnete Ableitung
- `AR_DELTA_IN` – die Differenz des Eingangssignals
- `AUDI_DELTA_T` – die Zeitdifferenz in Mikrosekunden

Damit kann der Baustein in übergeordneten SubApp-Netzwerken über normale `AdapterConnections` mit anderen AR-/AUDI-Bausteinen verbunden werden, ohne dass auf interne `.E1`-/`.D1`-Signale fremdinstanziierter Bausteine zugegriffen werden muss.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| `INIT` | `EInit` | Service-Initialisierung, wird an `FT_DERIV.EINIT` durchgereicht |
| `RST`  | `Event` | Setzt die Ableitungshistorie von `FT_DERIV` zurück |

### **Ereignis-Ausgänge**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| `INITO` | `EInit` | Initialisierungsbestätigung von `FT_DERIV` |

### **Daten-Eingänge**

| Name | Typ | Initialwert | Beschreibung |
|------|-----|-------------|--------------|
| `K`   | `REAL` | `1.0` | Ableitungsfaktor, wird an `FT_DERIV.K` durchgereicht |
| `run` | `BOOL` | `TRUE` | Schaltet die Berechnung aktiv, wird an `FT_DERIV.run` durchgereicht |

### **Daten-Ausgänge**

Keine. Alle Ergebniswerte werden über die Adapter-Plugs ausgegeben.

### **Adapter**

| Schnittstelle | Richtung | Adaptertyp | Datentyp | Beschreibung |
|----------------|----------|------------|----------|--------------|
| `AR_IN`        | Socket   | `AR`       | `REAL`   | Eingangssignal, dessen Ableitung berechnet werden soll |
| `AR_OUT`       | Plug     | `AR`       | `REAL`   | Berechnete Ableitung (`FT_DERIV.out`) |
| `AR_DELTA_IN`  | Plug     | `AR`       | `REAL`   | Differenz des Eingangssignals (`FT_DERIV.delta_in`) |
| `AUDI_DELTA_T` | Plug     | `AUDI`     | `UDINT`  | Zeitdifferenz des letzten Berechnungsschritts in Mikrosekunden (`FT_DERIV.delta_t`) |

## Funktionsweise

Der Baustein ist intern als FBNetzwerk aufgebaut und verwendet zwei zentrale Komponenten:

1. `FT_DERIV` aus der OSCAT-Bibliothek für die eigentliche Ableitungsberechnung.
2. `E_D_FF_ANY_3`, ein dreikanaliges D-Flipflop, das die berechneten Werte synchron übernimmt und an die Adapterausgänge weitergibt.

Ablauf:

- Ein Ereignis auf `AR_IN.E1` löst `FT_DERIV.REQ` aus.
- Gleichzeitig wird `AR_IN.D1` als Eingangssignal an `FT_DERIV.in` übergeben.
- `K` und `run` werden direkt als Parameter an `FT_DERIV` durchgereicht.
- Nach Abschluss der Berechnung signalisiert `FT_DERIV.CNF` das Ereignis `CLK` an das interne Flipflop.
- Das Flipflop übernimmt die Werte `out`, `delta_t` und `delta_in` und gibt sie über `Q1`, `Q2` und `Q3` aus.
- Mit dem Folgeereignis `EO` werden alle drei Adapter-Plugs `AR_OUT`, `AR_DELTA_IN` und `AUDI_DELTA_T` parallel bedient.

Der eigentliche Differenzierer arbeitet mit der Differenz des Eingangssignals und der verstrichenen Zeit. Der Faktor `K` skaliert das Ergebnis. Bei `K = 1.0` liefert der Baustein die Ableitung direkt, beispielsweise in Hz, wenn das Eingangssignal ein Impuls- oder Zählerstand ist.

`INIT` und `RST` werden unverändert an die entsprechenden Eingänge von `FT_DERIV` durchgereicht:

- `INIT` initialisiert den Baustein.
- `RST` setzt die interne Ableitungshistorie zurück.

## Technische Besonderheiten

- **Reine Adapter-Grenze:**  
  Kein Klartext-Event und kein Klartext-Datum verlässt den Baustein. Alle Ergebnisse werden ausschließlich über Adapter-Plugs bereitgestellt.

- **Keine Abhängigkeit zwischen `adapter` und `OSCAT`:**  
  Der Wrapper liegt in einem eigenen Brückenprojekt und kapselt die Verbindung zwischen der generischen Adapter-Bibliothek und der OSCAT-Bibliothek.

- **Durchreichung von `K` und `run`:**  
  Beide Werte sind pro Berechnungsaufruf konstant. Deshalb werden sie als normale `InputVars` geführt und können bei der Instanziierung über Parameter gesetzt werden.

- **Synchronisation über `E_D_FF_ANY_3`:**  
  Das interne Flipflop stellt sicher, dass die Datenausgänge erst nach Abschluss der Berechnung aktualisiert werden und ein gemeinsames Ausgangsereignis erzeugt wird.

- **Initialisierung:**  
  Wenn `INIT` nicht verdrahtet ist, feuert das `EInit`-Ereignis beim Deployment automatisch einmalig. Damit wird `FT_DERIV` auch ohne explizite Initialisierung korrekt gestartet.

## Zustandsübersicht

Da `FT_DERIV_AR` ein zusammengesetzter Funktionsbaustein ist, besitzt er keinen eigenen internen Zustandsautomaten. Die Zustandsübersicht ergibt sich aus den Ereignispfaden:

| Zustand / Phase | Auslöser | Aktion |
|-----------------|----------|--------|
| Initialisierung | `INIT` | `FT_DERIV.EINIT` wird ausgeführt, Bestätigung über `INITO` |
| Warten auf Messwert | `AR_IN.E1` | Eingangsdaten werden eingelesen, `FT_DERIV.REQ` wird ausgelöst |
| Berechnung | `FT_DERIV.CNF` | Ableitung, Zeitdifferenz und Eingangsdifferenz werden übernommen |
| Ausgabe | `EO` des Flipflops | `AR_OUT`, `AR_DELTA_IN` und `AUDI_DELTA_T` werden aktualisiert |
| Reset | `RST` | Ableitungshistorie von `FT_DERIV` wird zurückgesetzt |

## Anwendungsszenarien

- **Frequenz- und Drehzahlmessung:**  
  Zählerstände oder Impulse werden über `AR_IN` zugeführt. Mit `K = 1.0` kann die Ableitung als Frequenz in Hertz interpretiert werden.

- **Signalglättung und Regelungstechnik:**  
  Der Baustein kann als Differenzierer für Prozessgrößen eingesetzt werden. Über `run` lässt sich die Berechnung bei Bedarf deaktivieren.

- **Diagnose und Überwachung:**  
  Über die zusätzlichen Adapterausgänge `AR_DELTA_IN` und `AUDI_DELTA_T` stehen die internen Differenzwerte zur weiteren Verarbeitung bereit, etwa für Plausibilitätsprüfungen.

- **Adapterbasierte SubApp-Integration:**  
  `FT_DERIV_AR` kann in übergeordneten Netzwerken direkt mit anderen AR-/AUDI-Bausteinen über `AdapterConnections` verbunden werden. Eine Klartext-Konvertierung ist nur dann nötig, wenn ein nachgelagerter Baustein keinen Adapter unterstützt. Dafür kann beispielsweise `AR_R_TO_REAL` verwendet werden.

## Vergleich mit ähnlichen Bausteinen

- **`FT_DERIV` (OSCAT):**  
  Der ursprüngliche Baustein besitzt keine Adapter-Schnittstelle. Er arbeitet mit einfachen Events und Datenports. `FT_DERIV_AR` ergänzt diese fehlende Adapteranbindung.

- **`Q_NumericValue_AUDI` / `logiBUS_PI_IDA`:**  
  Ähnliche Wrapper-Bausteine, die OSCAT-Funktionalität hinter einer Adaptergrenze kapseln. Sie folgen demselben Muster: Ein einfacher OSCAT-Baustein wird in ein eigenes FBType eingebettet, um die Anbindung an adapterbasierte Netzwerke zu ermöglichen.

- **`AR_R_TO_REAL`:**  
  Kein Differenzierer, sondern ein Konvertierungsbaustein. Er wandelt einen `AR`-Adapterwert in einen Klartext-`REAL`-Wert um. Er kann in Kombination mit `FT_DERIV_AR` verwendet werden, wenn ein Klartextwert für nachgelagerte Bausteine benötigt wird.

## Fazit

`FT_DERIV_AR` ist eine saubere und praktische Kapselung des OSCAT-Differenzierers `FT_DERIV` für adapterbasierte IEC-61499-Anwendungen. Durch die vollständig adapterbasierte Außengrenze bleibt der Baustein flexibel einsetzbar und vermeidet direkte Abhängigkeiten zwischen der `adapter`- und der `OSCAT`-Bibliothek. Die Durchreichung von `K` und `run` als einfache Parameter hält die Schnittstelle kompakt, während die zusätzlichen Ausgänge für `delta_in` und `delta_t` hilfreiche Diagnose- und Überwachungsmöglichkeiten bieten.