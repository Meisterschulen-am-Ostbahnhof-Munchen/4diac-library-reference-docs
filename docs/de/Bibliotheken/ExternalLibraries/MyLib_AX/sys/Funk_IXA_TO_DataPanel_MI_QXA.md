# Funk_IXA_TO_DataPanel_MI_QXA


![Funk_IXA_TO_DataPanel_MI_QXA_network](./Funk_IXA_TO_DataPanel_MI_QXA_network.svg)

![Funk_IXA_TO_DataPanel_MI_QXA](./Funk_IXA_TO_DataPanel_MI_QXA.svg)

* * * * * * * * * *

## Einleitung

Die SubApp `Funk_IXA_TO_DataPanel_MI_QXA` kapselt eine adapter-basierte Verbindung zwischen einem Funk-Eingangsmodul und einem DataPanel-Ausgangsmodul. Sie ermöglicht die nahtlose Übertragung von empfangenen Funkdaten an ein Anzeige- oder Steuergerät, wobei die spezifischen Parameter für die Subnetzadresse und den Ausgangskanal extern konfigurierbar sind. Die SubApp ist generisch aufgebaut und kann in verschiedenen Kontexten wiederverwendet werden.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

- **Input** (Typ: `Funk::io::DI::Funk_DI_S`)  
  Identifiziert den digitalen Eingang, z. B. `DigitalInput_Key_01` etc. Initialwert: `Funk_DI::Invalid`.
- **u8SAMember** (Typ: `USINT`)  
  Node-SA (Subnetzadresse) im Bereich 224..239. Initialwert: `MI::MI_00`.
- **Output** (Typ: `DataPanel::io::MI::DQ::DataPanel_MI_DO_S`)  
  Identifiziert den digitalen Ausgang, z. B. `DigitalOutput_1A..8B` und `Input_Power_Port_5..8`. Initialwert: `Invalid`.

### **Daten-Ausgänge**

Keine.

### **Adapter**

Keine externen Adapter vorhanden. Die Adapterverbindung wird intern zwischen den enthaltenen Funktionsblöcken genutzt.

## Funktionsweise

Die SubApp enthält zwei Funktionsbausteine:

- **IXA** (Typ: `Funk::io::DI::Funk_IXA`) – verarbeitet den eingehenden Funkdatenstrom.
- **QXA** (Typ: `DataPanel::io::MI::DQ::DataPanel_MI_QXA`) – steuert die Ausgabe auf einem DataPanel.

Der Datenfluss erfolgt wie folgt:

1. Der externe Daten-Eingang `Input` wird an den Daten-Eingang `IXA.Input` weitergeleitet.
2. Der Funktionsblock `IXA` verarbeitet die Funkdaten und stellt das Ergebnis über seinen Adapter-Ausgang `IN` bereit.
3. Über eine interne Adapterverbindung werden diese Daten an den Adapter-Eingang `OUT` des Funktionsblocks `QXA` übertragen.
4. Die externen Eingänge `u8SAMember` und `Output` werden direkt an die gleichnamigen Daten-Eingänge des Blocks `QXA` angeschlossen, um die Zieladresse und den Ausgangskanal festzulegen.

Beide Funktionsblöcke sind mit `QI = TRUE` parametrisiert, sodass sie im aktivierten Zustand betrieben werden. Der Parameter `PARAMS` des Blocks `IXA` ist leer und nur intern sichtbar.

## Technische Besonderheiten

- **Adapter-basierte Kopplung**: Die Kommunikation zwischen den beiden Funktionsblöcken erfolgt über eine standardisierte Adapterschnittstelle, was eine flexible und erweiterbare Verbindung ermöglicht.
- **Generische Parametrierung**: Die SubApp kann durch die externen Eingänge `u8SAMember` und `Output` für verschiedene Zieladressen und Ausgangskanäle konfiguriert werden, ohne Änderungen an der internen Struktur.
- **Wiederverwendbarkeit**: Die Kapselung der Verbindung in einer SubApp erlaubt eine einfache Integration in übergeordnete Systeme und fördert die Modularität.
- **Keine eigenen Zustände**: Als reine Datenfluss- und Adapterverbindung besitzt die SubApp keinen internen Zustandsautomaten.

## Zustandsübersicht

Die SubApp selbst besitzt keine internen Zustände, da sie ausschließlich eine statische Verbindung zwischen den enthaltenen Funktionsblöcken herstellt. Das Zustandsverhalten wird vollständig durch die zugrunde liegenden Funktionsblöcke (z. B. `Funk_IXA` und `DataPanel_MI_QXA`) definiert. Auf Ebene der SubApp besteht keine aktive Logik oder Zustandshaltung.

## Anwendungsszenarien

- **Funkempfang und Anzeige**: Anbindung eines Funkempfängers an ein DataPanel, um empfangene Werte oder Statusmeldungen anzuzeigen.
- **Drahtlose Steuerung**: Verwendung in Systemen, in denen über Funk empfangene Signale direkt eine Ausgabe auf einem Panel steuern sollen.
- **Adressierbare Ausgänge**: Einsetzbar in Mehrpunkt-Konfigurationen, bei denen über die `u8SAMember`-Adresse unterschiedliche Panels oder Ausgänge selektiert werden.
- **Prototypen und Schulungen**: Als Vorlage für ähnliche Verbindungen in Lern- und Übungsumgebungen (z. B. im Rahmen von 4diac-Projekten).

## Vergleich mit ähnlichen Bausteinen

Im Vergleich zu einer direkten Verbindung zwischen `Funk_IXA` und `DataPanel_MI_QXA` bietet diese SubApp eine **gekapselte, wiederverwendbare Einheit**. Sie reduziert die Komplexität im übergeordneten Netzwerk, da die Adapter- und Datenverbindungen bereits fest verdrahtet sind. Ein ähnlicher Baustein könnte eine separate Lösung sein, die nicht adapterbasiert arbeitet, sondern direkte Datenverbindungen nutzt – hier wird jedoch die standardisierte Adapter-Kommunikation bevorzugt, die eine spätere Erweiterung (z. B. Austausch der Funk- oder Ausgangsmodule) erleichtert.

## Fazit

Die SubApp `Funk_IXA_TO_DataPanel_MI_QXA` stellt eine praktische, generische Lösung zur Kopplung von Funkempfang und DataPanel-Ausgabe dar. Durch die Kombination von Daten- und Adapterverbindungen wird eine klare Schnittstelle bereitgestellt, die einfach in größere Systeme integriert werden kann. Ihre modulare Struktur und Parametrierbarkeit machen sie für vielfältige Einsatzszenarien geeignet.
