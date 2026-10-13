# AX_SEL

![AX_SEL_network](./AX_SEL_network.svg)

* * * * * * * * * *

## Einleitung

Der AX_SEL Funktionsblock ist ein binärer Selektor, der zur Auswahl zwischen zwei Eingangswerten dient. Er gehört zur Kategorie der Standard-Selektionsfunktionen gemäß IEC 61131-3 und ermöglicht die flexible Auswahl von Daten basierend auf einem Steuersignal.

![AX_SEL](AX_SEL.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **EI0** - Setzt IN0 (Datenzuordnung: `IN0`)
- **EI1** - Setzt IN1 (Datenzuordnung: `IN1`)

### **Ereignis-Ausgänge**

- **CNF** - Bestätigung des angeforderten Dienstes (Confirmation of Requested Service)

### **Daten-Eingänge**

- **IN0** (ANY) - Selektierbare Eingangsvariable
- **IN1** (ANY) - Selektierbare Eingangsvariable

### **Daten-Ausgänge**

- **OUT** (ANY) - Ausgewählter Eingang

### **Adapter**

- **G** (adapter::types::unidirectional::AX) - Unidirektionaler Adapter für Steuerung

## Funktionsweise

Der AX_SEL Funktionsblock wählt zwischen zwei Eingangswerten (IN0 und IN1) aus und gibt den selektierten Wert am Ausgang OUT aus. Die Auswahl wird über den angeschlossenen Adapter G gesteuert: `G.E1` taktet ein internes D-Flipflop (`E_D_FF_G`), das `G.D1` übernimmt und als Auswahl `G` an `F_SEL` liefert. `F_SEL` wird durch das Flipflop-Ereignis (G hat sich geändert) sowie direkt durch `EI0` und `EI1` angestoßen; sein `CNF` wird als `CNF` des Bausteins ausgegeben und der ausgewählte Wert steht dann an OUT bereit. Ein Ausgangsereignis entsteht also nur bei einem Ereignis an `G.E1`, `EI0` oder `EI1`.

## Technische Besonderheiten

- Verwendet den ANY-Datentyp für maximale Flexibilität bei den Ein- und Ausgängen
- Implementiert als unidirektionaler Adapter für einfache Steuerung
- Unterstützt verschiedene Datentypen durch die Verwendung von ANY

## Zustandsübersicht

Der Funktionsblock besitzt einen einfachen Zustand:

- **Bereit**: Wartet auf Steuersignal vom Adapter G
- Bei Empfang eines Ereignisses (`EI0`, `EI1` oder `G.E1`) wird der entsprechende Eingang ausgewählt und das CNF-Ereignis ausgelöst

## Anwendungsszenarien

- Auswahl zwischen zwei verschiedenen Sensoren
- Umschaltung zwischen Betriebsmodi
- Selektion von Datenquellen in Abhängigkeit von Betriebszuständen
- Wechsel zwischen manueller und automatischer Steuerung

## ⚖️ Vergleich mit ähnlichen Bausteinen

Im Vergleich zu anderen Selektionsbausteinen bietet AX_SEL:

- Größere Flexibilität durch ANY-Datentyp
- Einfache Integration durch unidirektionalen Adapter
- Standardisierte Schnittstelle gemäß IEC 61131-3

Vergleich mit [F_SEL](../../../../StandardLibraries/iec61131-3/selection/F_SEL.md)

## Fazit

Der AX_SEL Funktionsblock stellt eine einfache und flexible Lösung für binäre Auswahlaufgaben in Steuerungssystemen dar. Durch die Verwendung des ANY-Datentyps und standardisierter Adapter-Schnittstellen ermöglicht er eine vielseitige Anwendung in verschiedenen Automatisierungsprojekten.
