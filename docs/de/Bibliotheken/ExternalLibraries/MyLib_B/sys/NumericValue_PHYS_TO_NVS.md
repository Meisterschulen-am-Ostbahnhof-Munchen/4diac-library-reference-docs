# NumericValue_PHYS_TO_NVS


![NumericValue_PHYS_TO_NVS_network](./NumericValue_PHYS_TO_NVS_network.svg)

![NumericValue_PHYS_TO_NVS](./NumericValue_PHYS_TO_NVS.svg)

* * * * * * * * * *

## Einleitung

Die SubApp `NumericValue_PHYS_TO_NVS` bietet eine generische Lösung zum Einlesen eines numerischen Werts in der physikalischen (PHYS) Variante, der mithilfe einer Skalierung in einen technischen Wert umgerechnet wird. Dieser Wert wird anschließend persistent im NVS (Non-Volatile Storage) eines ESP32 gespeichert. Die SubApp kapselt dabei die vollständige Logik für Speichern, Laden und Umrechnen und stellt eine einfache Schnittstelle zur Verfügung, die sowohl das Schreiben als auch das Lesen des Wertes ermöglicht.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine vorhanden.

### **Ereignis-Ausgänge**

- **IND** (Event): Wird ausgelöst, wenn ein Speichervorgang oder ein Lesevorgang abgeschlossen ist. Konkret wird dieser Ausgang über `NVS.SETO` (nach erfolgreichem Speichern) und über `NVS.GETO` (nach erfolgreichem Laden) aktiviert.

### **Daten-Eingänge**

- **KEY** (STRING): Schlüsselname, unter dem der Wert im NVS abgelegt wird.
- **stObj** (Typ: `logiBUS::utils::conversion::phys::NumericObjectPool_S`): Struktur, die die Parameter für die physikalische Umrechnung enthält:
  - `u16ObjId` (Objektidentifikation, Standard: `ID_NULL`)
  - `r32Scale` (Skalierungsfaktor, Standard: `1.0`)
  - `i32Offset` (Offset, Standard: `0`)
  - `u8Decimals` (Anzahl der Nachkommastellen, Standard: `0`)

### **Daten-Ausgänge**

- **VALUEO** (REAL): Der aktuell geladene oder gespeicherte Wert als `REAL`. Dieser Wert entspricht der intern abgelegten, skalierten Größe.

### **Adapter**

Keine vorhanden.

## Funktionsweise

Die SubApp besteht aus drei internen Funktionsblöcken:

- **`NumericValue_PHYS`** – liest einen physikalischen Wert (z. B. von einem Sensor) und konvertiert ihn in einen skalierten Prozesswert.
- **`NVS`** – verwaltet den nichtflüchtigen Speicher (ESP32 NVS) und ermöglicht das Speichern (SET) und Laden (GET) von Werten.
- **`Q_NumericValue_PHYS`** – dient zur Rückkonvertierung des gespeicherten Wertes in die ursprüngliche physikalische Einheit (wird intern genutzt, aber nicht nach außen geführt).

Der Ablauf ist wie folgt:

1. **Initialisierung**: Beim Start der SubApp wird der NVS-Baustein initialisiert (`INIT`). Nach erfolgreicher Initialisierung (`INITO`) wird automatisch ein `GET` ausgelöst, um den unter `KEY` gespeicherten Wert zu laden.
2. **Laden**: Der geladene Wert (`NVS.VALUEO`) wird direkt an den Ausgang `VALUEO` gelegt und parallel an `Q_NumericValue_PHYS` übergeben, um ggf. eine Rückumrechnung vorzunehmen.
3. **Speichern**: Wird vom Baustein `NumericValue_PHYS` ein neuer Wert erzeugt (Ausgang `IND`), so wird dessen skaliertes Ergebnis (`rPhys`) an `NVS.VALUE` übergeben und mit `SET` im NVS gespeichert. Nach erfolgreichem Speichern signalisiert `SETO` den Abschluss.
4. **Ausgabe**: Sowohl nach dem Laden als auch nach dem Speichern wird der Ausgang `IND` der SubApp aktiviert. Dadurch kann eine übergeordnete Steuerung den Vorgang synchron verfolgen.

Die SubApp unterstützt damit sowohl das Schreiben als auch das Lesen eines persistenten, skalierten Werts.

## Technische Besonderheiten

- **Generische Konfiguration**: Durch das Objekt `stObj` kann die SubApp für verschiedene physikalische Messgrößen (z. B. Temperatur, Druck) konfiguriert werden, ohne die Logik zu ändern.
- **Eingebettete Systeme**: Der Einsatz des NVS-Bausteins ist speziell für ESP32-Mikrocontroller ausgelegt.
- **Skalierung**: Die Werte werden mit `r32Scale` und `i32Offset` linear skaliert; die Anzahl der Nachkommastellen (`u8Decimals`) ist für darstellende Zwecke vorgesehen.
- **Bibliotheksabhängigkeiten**: Die SubApp greift auf die Bibliotheken `isobus` und `logiBUS` zu, die in der CompilerInfo als Imports deklariert sind.

## Zustandsübersicht

Die SubApp selbst besitzt keinen eigenen Zustandsautomaten, sondern delegiert die Zustandslogik an den internen `NVS`-Baustein. Typische Zustände des NVS sind:

- **INIT** – Initialisierung des Speichers
- **IDLE** – Warten auf Anweisungen
- **SET** – Speichern eines Wertes
- **GET** – Lesen eines Wertes

Die SubApp signalisiert den Abschluss dieser Operationen über das Ereignis `IND`.

## Anwendungsszenarien

- **Persistente Sensorwerte**: Ein Temperatursensorwert wird periodisch gelesen und im NVS gespeichert, sodass nach einem Neustart der letzte gültige Wert verfügbar ist.
- **Kalibrierdaten**: Ein Kalibrierfaktor oder Offset wird in NVS abgelegt und bei jedem Start geladen.
- **Parametereinstellungen**: Benutzerdefinierte Einstellungen (z. B. Sollwerte) werden gespeichert und wieder ausgelesen.

## Vergleich mit ähnlichen Bausteinen

Im Vergleich zu einfachen NVS-Bausteinen, die nur Rohdaten speichern, bietet diese SubApp eine integrierte Skalierung und Rückumrechnung. Sie vereint mehrere Funktionen (Lesen, Speichern, Konvertieren) in einem Baustein und reduziert dadurch den Verdrahtungsaufwand. Gegenüber einem direkten Zugriff auf den NVS wird eine höhere Abstraktionsebene erreicht, die Fehlerquellen minimiert.

## Fazit

Die SubApp `NumericValue_PHYS_TO_NVS` ist eine modular aufgebaute, wiederverwendbare Komponente für eingebettete Systeme, die einen skalierten Messwert dauerhaft speichert und bei Bedarf wieder bereitstellt. Sie kombiniert die Vorteile einer flexiblen Konfiguration über das `stObj` mit der einfachen Handhabung einer einheitlichen Schnittstelle und ist somit ideal für Anwendungen, die persistente Wertespeicherung erfordern.
