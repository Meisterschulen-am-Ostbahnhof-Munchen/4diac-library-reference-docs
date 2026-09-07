# SystemTickSender_ISO

![SystemTickSender_ISO_network](./SystemTickSender_ISO_network.svg)

* * * * * * * * * *

## Einleitung

`SystemTickSender_ISO` zeigt den [`System_Tick`](./System_Tick.md)-Heartbeat rein lokal auf einem VT-Zahlenausgabefeld an, ohne OPC-UA. Der laufende Tick-Zähler wird direkt in ein VT-Zahlenfeld geschrieben, sodass am Bildschirm auf einen Blick erkennbar ist, ob die Steuerung noch lebt (der Wert muss sich alle 200 ms ändern).

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: SystemTickSender_ISO

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **System_Tick** (SubApp, `MyLib::sys`): liefert den Zählerstand als `ADI`-Adapter (DINT).
    - **ADI_TO_AUDI**: `adapter::conversion::unidirectional::ADI_TO_AUDI` — wandelt den DINT-Wert in `AUDI` (UDINT) um, weil `Q_NumericValue_AUDI` einen vorzeichenlosen Wert erwartet.
    - **Q_NumericValue_AUDI**: `isobus::UT::Q::Q_NumericValue_AUDI` — VT-Kommando "Change numeric value" (Part 6 – F.22), schreibt den Wert in das Zahlenausgabefeld `u16ObjId`.
- **Funktionsweise**: `System_Tick` liefert den Zählerstand, `ADI_TO_AUDI` wandelt ihn in den von `Q_NumericValue_AUDI` erwarteten Datentyp um.

## Programmablauf und Verbindungen

1. `u16ObjId` → `Q_NumericValue_AUDI.u16ObjId` (Datenverbindung, ausgeblendet).
2. `System_Tick.ADI_OUT` → `ADI_TO_AUDI.ADI_IN` → `ADI_TO_AUDI.AUDI_OUT` → `Q_NumericValue_AUDI.u32NewValue`.

## Technische Besonderheiten

- **Rein lokal, kein OPC-UA**: Im Gegensatz zu `SystemTickSender_ISO_OPC` besitzt dieser Baustein keinen `ADI_PUBLISH_1`-Adapter.

## Anwendungsszenarien

- Module mit eigener VT-Anbindung, die den Heartbeat nur lokal sichtbar machen müssen, ohne Remote-Überwachung.

## Vergleich mit ähnlichen Bausteinen

Für zusätzliche Remote-Verteilung ist [`SystemTickSender_ISO_OPC`](./SystemTickSender_ISO_OPC.md) zu verwenden; für ein Modul ohne eigene VT-Anbindung (nur OPC-UA) [`SystemTickSender_OPC`](./SystemTickSender_OPC.md). Gegenüber [`SystemTickSender`](./SystemTickSender.md) (VT + OPC-UA, älteres Muster mit `ADI_SPLIT_2`) ist dieser Baustein die reine VT-only-Variante ohne Split/Publish.

## Zusammenfassung

`SystemTickSender_ISO` zeigt den SystemTick-Heartbeat ausschließlich auf einem lokalen VT-Zahlenfeld an — die schlankste Variante für Module mit eigener VT-Anbindung ohne Remote-Überwachung.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
