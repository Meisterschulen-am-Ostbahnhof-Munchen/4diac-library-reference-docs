# Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC

![Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC_network](./Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC` liest einen VT-SoftKey ODER eine AUX-Zuweisung (Joystick) ODER einen lokalen Web-Override und schreibt das Ergebnis per Remote-OPC-UA-Write an ein Zielmodul — zusätzlich wird die VT-Hintergrundfarbe (SoftKey UND AUX) aus einem Remote-Subscribe des Zielmoduls gespeist. Gedacht für Funktionen ohne lokalen physischen Ausgang, bei denen die Bedienung auf einem Modul (z. B. STG1) sitzt, der Aktor aber auf einem anderen Modul ohne eigene VT-Anbindung.

Seit Version 1.2 ist der Baustein ein dünnwandiger Wrapper, der zwei eigenständig wiederverwendbare Teilbausteine nebeneinanderstellt: [`Softkey_Aux_IXA_TO_Remote_WRITE`](./Softkey_Aux_IXA_TO_Remote_WRITE.md) (Kommando) und [`AX_SUBSCRIBE_BG3_WEB_OPC`](./AX_SUBSCRIBE_BG3_WEB_OPC.md) (Status). Die externe Schnittstelle blieb dabei unverändert.

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **Command** (SubApp, `MyLib::sys::Softkey_Aux_IXA_TO_Remote_WRITE`): SoftKey ODER Aux ODER lokaler Web-Override → Remote-Write an das Zielmodul.
    - **Status** (SubApp, `MyLib::sys::AX_SUBSCRIBE_BG3_WEB_OPC`): Remote-Subscribe → Hintergrundfarbe auf SoftKey UND Aux + lokaler Web-Republish.
- **Funktionsweise**: Beide Teilbausteine laufen unabhängig nebeneinander; sie teilen sich lediglich dieselben `u16ObjId`/`u16ObjIdA`-Werte, damit Kommando und Statusanzeige auf demselben VT-Objekt wirken.

## Programmablauf und Verbindungen

1. `u16ObjId` → `Command.u16ObjId` und `Status.u16ObjId`; `u16ObjIdA` → `Command.u16ObjIdA` und `Status.u16ObjIdA` (Datenverbindungen, ausgeblendet).
2. `ID_WRITE_REMOTE` → `Command.ID_WRITE_REMOTE`; `ID_WEB_READ` → `Command.ID_WEB_READ`.
3. `ID_SUBSCRIBE` → `Status.ID_SUBSCRIBE`; `ID_STATUS_WEB` → `Status.ID_STATUS_WEB`.
4. Es gibt keine direkte SubApp-Verbindung zwischen `Command` und `Status` — beide SubApps sind vollständig eigenständig und kommunizieren nur indirekt über das Zielmodul (Command schreibt dorthin, Status abonniert von dort). Lokal können ihre Pfade dennoch verknüpft sein, wenn `ID_WEB_READ` und `ID_STATUS_WEB` versehentlich auf denselben OPC-UA-Knoten konfiguriert werden (siehe Feedback-Loop-Hinweis unten).

## Technische Besonderheiten

- **Reiner Wrapper seit Version 1.2**: Die Zerlegung in `Command`/`Status` ändert nichts am Verhalten, erlaubt aber die unabhängige Wiederverwendung beider Hälften (z. B. `AX_SUBSCRIBE_BG3_WEB_OPC` allein für reine Statusanzeigen).
- **Kein lokaler physischer Ausgang**: Der Aktor sitzt auf einem anderen Modul; dieser Baustein bildet nur Bedienung und Statusrückmeldung auf dem Bedienmodul ab.
- **Getrennte lokale Knoten für Web-Override und Web-Status**: `ID_WRITE_REMOTE` (Schreiben) und `ID_SUBSCRIBE` (Lesen) zeigen beide auf das entfernte Zielmodul und dürfen dort durchaus denselben Remote-Knoten referenzieren — Write und Subscribe sind unterschiedliche OPC-UA-Operationen, kein Rückkopplungsrisiko auf diesem Modul. Die tatsächliche Feedback-Loop-Gefahr liegt bei den beiden *lokalen* Knoten dieses Moduls: `ID_WEB_READ` (von `Command`, per `AX_SUBSCRIBE_1` gelesen und ins Kommando-ODER gemischt) und `ID_STATUS_WEB` (von `Status`, per `AX_PUBLISH_1` mit dem vom Zielmodul zurückgemeldeten Zustand beschrieben) müssen unterschiedliche lokale OPC-UA-Knoten sein — sonst liest `Command` den von `Status` republizierten Echo-Zustand als vermeintlichen Web-Override und schreibt ihn als neues Kommando zurück ans Zielmodul.

## Anwendungsszenarien

- Bedienstellen ohne eigenen physischen Ausgang, bei denen sowohl SoftKey als auch AUX-Joystick-Zuweisung einen entfernten Aktor schalten sollen und der Bedienbildschirm zusätzlich dessen Status farblich anzeigen muss.

## Vergleich mit ähnlichen Bausteinen

Wird nur ein SoftKey ohne Aux benötigt, ist `Softkey_IXA_TO_Remote_WRITE_BG_OPC` zu verwenden. Für Kommando bzw. Status allein (ohne die jeweils andere Hälfte) sind die Teilbausteine [`Softkey_Aux_IXA_TO_Remote_WRITE`](./Softkey_Aux_IXA_TO_Remote_WRITE.md) bzw. [`AX_SUBSCRIBE_BG3_WEB_OPC`](./AX_SUBSCRIBE_BG3_WEB_OPC.md) direkt einsetzbar.

## Zusammenfassung

`Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC` bündelt Kommando- und Status-Seite eines Remote-Kanals mit SoftKey- und Aux-Bedienung hinter einer stabilen, bekannten Schnittstelle — seit Version 1.2 als reiner Wrapper um zwei eigenständige Teilbausteine.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
