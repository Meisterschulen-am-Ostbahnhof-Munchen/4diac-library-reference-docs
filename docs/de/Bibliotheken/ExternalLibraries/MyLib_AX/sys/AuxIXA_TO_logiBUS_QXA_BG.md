# AuxIXA_TO_logiBUS_QXA_BG


![AuxIXA_TO_logiBUS_QXA_BG_network](./AuxIXA_TO_logiBUS_QXA_BG_network.svg)

![AuxIXA_TO_logiBUS_QXA_BG](./AuxIXA_TO_logiBUS_QXA_BG.svg)

* * * * * * * * * *
## Einleitung

Der Subapplikationsbaustein **AuxIXA_TO_logiBUS_QXA_BG** realisiert eine AUX-Funktion (Aux_IXA) auf einem digitalen Ausgang (QXA) mit Green-White-Hintergrunddarstellung. Er ist generisch aufgebaut und analog zu dem Baustein `Button_IXA_TO_logiBUS_QXA_BG` implementiert, wobei hier statt eines Buttons eine AUX-Eingangsfunktion verwendet wird. Der Baustein wurde aus einer Übungseinheit ausgelagert, um eine Wiederverwendung in verschiedenen Projekten zu ermöglichen. Er kombiniert eine ISO-bus-konforme AUX-Schnittstelle mit einem logiBUS-Digitalausgang und einer visuellen Statusanzeige.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Der Baustein besitzt keine expliziten Ereignis-Eingänge auf der obersten Ebene. Die Verarbeitung erfolgt rein daten- und adaptergesteuert.

### **Ereignis-Ausgänge**

Es sind keine Ereignis-Ausgänge auf der obersten Ebene vorhanden. Die Ausgabe wird über den Datenausgang und die Adapterverbindungen realisiert.

### **Daten-Eingänge**

| Name | Datentyp | Initialwert | Kommentar |
|------|----------|-------------|-----------|
| `u16ObjIdA` | `UINT` | `ID_NULL` | Object ID der AUX-Funktion (z. B. ISOBUS-Objekt-ID) |
| `Output` | `logiBUS::io::DQ::logiBUS_DO_S` | `logiBUS_DO::Invalid` | Identifiziert den Ausgang (Output_Q1…Q8) |

### **Daten-Ausgänge**

Es sind keine direkten Daten-Ausgänge auf der obersten Ebene definiert. Die Ausgabe erfolgt über den internen Funktionsblock `DigitalOutput_Q1` und die Adapterverbindungen.

### **Adapter**

Der Baustein besitzt über die interne Verdrahtung Adapterverbindungen, die jedoch nicht als öffentliche Schnittstellen nach außen geführt werden. Die internen Adapterverbindungen dienen der Kommunikation zwischen der AUX-Eingangsfunktion, dem Digitalausgang und der Statusanzeige.

## Funktionsweise

Der Baustein nimmt eine AUX-Objekt-ID (`u16ObjIdA`) und eine Ausgangsauswahl (`Output`) entgegen. Intern wird die AUX-Eingangsfunktion `AuxFunction2_X1` (Typ `isobus::UT::io::Auxiliary::IN::Aux_IXA`) mit der übergebenen Objekt-ID versorgt. Deren Ereignis-/Adapterausgang (`IN`) wird über einen Ereignis-Splitter (`AX_SPLIT_2`) auf zwei Pfade aufgeteilt:

1. **Pfad 1** – führt zum Funktionsblock `DigitalOutput_Q1` (Typ `logiBUS::io::DQ::logiBUS_QXA`), der den digitalen Ausgang gemäß der Auswahl `Output` ansteuert.
2. **Pfad 2** – führt zum Subapplikationsbaustein `GreenWhiteBackground2_AX` (Typ `MyLib::sys::GreenWhiteBackground2_AX`), der die grafische Darstellung (grün/weißer Hintergrund) für die Statusanzeige übernimmt.

Die übergebene Objekt-ID `u16ObjIdA` wird ebenfalls an `GreenWhiteBackground2_AX` weitergeleitet, um eine konsistente Darstellung sicherzustellen. Der Ausgang `Output` wird direkt an den Digitalausgangsbaustein übergeben, welcher den tatsächlichen physischen Ausgang (Q1…Q8) des logiBUS-Systems aktiviert.

Durch die Aufteilung des Ereignisses in zwei Zweige kann sowohl die digitale Ausgabe als auch die visuelle Rückmeldung parallel erfolgen, ohne dass eine separate Logik erforderlich ist.

## Technische Besonderheiten

- **Generische Wiederverwendung**: Der Baustein ist modular aufgebaut und kann in verschiedenen Steuerungsanwendungen eingesetzt werden, bei denen eine AUX-Funktion auf einen logiBUS-Digitalausgang abgebildet werden soll.
- **Kombination von ISOBUS und logiBUS**: Er verbindet die ISOBUS-AUX-Schnittstelle (Standard 61499-2) mit der logiBUS-Ausgangssteuerung.
- **Visuelle Statusanzeige**: Durch die Einbindung des Bausteins `GreenWhiteBackground2_AX` wird eine farbliche Rückmeldung (grün/weiß) auf einem Bedienpanel oder einer Visualisierung ermöglicht.
- **Parametrierung**: Der Funktionsblock `DigitalOutput_Q1` wird fest mit `QI = TRUE` betrieben und akzeptiert über eine unsichtbare `PARAMS`-Konfiguration zusätzliche Parameter.
- **Interner Ereignis-Splitter**: Der `AX_SPLIT_2` (adapter::events::unidirectional) verteilt das eingehende Ereignis ohne Verzögerung an zwei unabhängige Zielbausteine.

## Zustandsübersicht

Da der Baustein keine eigenen Zustandsautomaten besitzt, hängt sein Verhalten von den internen Bausteinen ab. Im Wesentlichen gibt es zwei Betriebszustände:

- **Inaktiv**: Keine gültige AUX-Objekt-ID oder kein aktivierter Ausgang – der Digitalausgang ist deaktiviert, die Statusanzeige zeigt den Neutralzustand.
- **Aktiv**: Eine gültige AUX-Objekt-ID liegt an und das Ereignis aktiviert den Ausgang – der Digitalausgang schaltet den entsprechenden Kanal, die Statusanzeige wechselt auf die aktive Farbe (grün).

Der Übergang zwischen den Zuständen erfolgt ereignisgesteuert über die Adapterschnittstelle des `Aux_IXA`-Bausteins.

## Anwendungsszenarien

- **Landmaschinensteuerung**: Abbildung einer AUX-Funktion (z. B. „Hebearm anheben“) auf einen digitalen Ausgang eines logiBUS-Moduls zur Ansteuerung eines Ventils oder Relais.
- **Visualisierung**: Anzeige des Schaltzustands auf einem Terminal mit grün/weißer Hintergrundbeleuchtung für Klarheit und Benutzerfreundlichkeit.
- **Modulare Automatisierung**: Wiederverwendbarkeit in verschiedenen Maschinen, ohne jedes Mal die Logik neu zu programmieren.
- **Test- und Simulationsumgebungen**: Einsatz in Laboraufbauten zur Verifikation von ISOBUS-zu-logiBUS-Verbindungen.

## Vergleich mit ähnlichen Bausteinen

Der Baustein ist konzeptionell identisch mit `Button_IXA_TO_logiBUS_QXA_BG`, unterscheidet sich jedoch in der Quelle des Eingangsereignisses: Hier wird ein AUX-Eingang (`Aux_IXA`) statt eines Button-Eingangs verwendet. Der AUX-Eingang basiert auf ISOBUS-Standard, während ein Button typischerweise ein binäres Signal liefert. Dadurch unterstützt dieser Baustein komplexere, objektorientierte Steuerbefehle (über Objekt-IDs), während der Button-Baustein einfacher aufgebaut ist. Beide teilen sich die Ausgangslogik und die Statusanzeige, was die Wartbarkeit erhöht.

Andere mögliche Bausteine, z. B. direkte Treiber ohne Visualisierung, verzichten auf die GreenWhiteBackground-Subapplikation und sind dementsprechend kompakter, bieten aber keine optische Rückmeldung.

## Fazit

Der Baustein `AuxIXA_TO_logiBUS_QXA_BG` ist ein gut durchdachter, modularer Baustein, der eine ISO-bus-AUX-Funktion nahtlos in ein logiBUS-Ausgangssystem integriert. Durch die Kombination mit einer visuellen Statusanzeige erhöht er die Bedienbarkeit und Transparenz in Steuerungssystemen. Seine generische Struktur ermöglicht eine einfache Wiederverwendung und Anpassung an unterschiedliche Anforderungen. Die saubere Trennung von Eingangslogik, Ausgangstreiber und Anzeige macht ihn wartungsfreundlich und erweiterbar.