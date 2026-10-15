# Button_IXA_TO_logiBUS_QXA_BG_OPC

![Button_IXA_TO_logiBUS_QXA_BG_OPC_network](./Button_IXA_TO_logiBUS_QXA_BG_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`Button_IXA_TO_logiBUS_QXA_BG_OPC` ist der wiederverwendbare Baustein für **einen einzelnen digitalen Ausgang**, der sowohl über einen VT-Taster als auch über OPC-UA (Web-Client) geschaltet werden kann — inklusive VT-Statusanzeige. Er wird 12-fach parametrisiert in [`InputOutputTesterButton_DIDO_OPC_UA`](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Meins/InputOutputTester/Button_DIDO_OPC_UA/InputOutputTesterButton_DIDO_OPC_UA/) instanziiert.

![Button_IXA_TO_logiBUS_QXA_BG_OPC](Button_IXA_TO_logiBUS_QXA_BG_OPC.svg)

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: Button_IXA_TO_logiBUS_QXA_BG_OPC

- **Typ**: SubAppType
- **Schnittstelle (zusätzlich)**: Ereignis-Eingang `INIT`, Ereignis-Ausgang `INITO` (je `EInit`), Daten-Eingang `QI` (`BOOL`, Vorgabe `TRUE`), Daten-Ausgang `QO` (`BOOL`).
- **Verwendete interne FBs**:
    - **Button_IXA**: `isobus::UT::io::Button::Button_IXA` — VT-Taster, `u16ObjId` identifiziert die VT-Taste. Seit Version 1.2 vorübergehend **non-latching** (tastender Pegel, kein Klick-Toggle mehr).
    - **AX_2_3**: SubApp [`AX_2_3`](./AX_2_3.md) — verknüpft `IN1` (OPC-UA-Subscribe) und `IN2` (VT-Taster) per `AX_OR_2` zu einem gemeinsamen Pegel und verteilt ihn per `AX_SPLIT_3` auf drei Ziele (`OUT1`..`OUT3`).
    - **logiBUS_QXA**: `logiBUS::io::DQ::logiBUS_QXA` — physischer digitaler Ausgang.
    - **GreenWhiteBackground1_AX** (SubApp, `MyLib::sys`): VT-Hintergrundfarbe passend zum Ausgangszustand.
    - **AX_SUBSCRIBE_1** / **AX_PUBLISH_1**: `adapter::net::AX_SUBSCRIBE_1`/`AX_PUBLISH_1` — OPC-UA-Lese-/Schreibzugriff.
    - **AND_QO**: `iec61131::booleanOperators::AND_BOOL_5` — verknüpft die `QO`-Ausgänge von `logiBUS_QXA`, `Button_IXA`, `AX_SUBSCRIBE_1`, `AX_PUBLISH_1` und `GreenWhiteBackground1_AX` zum `QO` der SubApp.
- **Funktionsweise**: Zwei unabhängige Schaltquellen (VT-Taster, OPC-UA-Schreibzugriff) werden in `AX_2_3` ODER-verknüpft; der resultierende Pegel speist gleichzeitig den physischen Ausgang, die VT-Statusfarbe und das OPC-UA-Echo.

## Programmablauf und Verbindungen

1. **VT-Taster-Pfad**: `Button_IXA.IN` (Adapter) → `AX_2_3.IN2`.
2. **OPC-UA-Pfad**: `AX_SUBSCRIBE_1.OUT` (Adapter, Web-Schreibzugriff) → `AX_2_3.IN1`.
3. **Gemeinsamer Zustand**: `AX_2_3.OUT1` → `logiBUS_QXA.OUT` (physischer Ausgang), `AX_2_3.OUT2` → `GreenWhiteBackground1_AX.DI1` (VT-Statusfarbe), `AX_2_3.OUT3` → `AX_PUBLISH_1.IN` (OPC-UA-Echo an den Web-Client).
4. **Initialisierungskette**: `INIT` → `logiBUS_QXA.INIT` → `Button_IXA.INIT` → `AX_SUBSCRIBE_1.INIT`; `AX_SUBSCRIBE_1.INITO` → `AX_PUBLISH_1.INIT` (ausgeblendete Verbindung) — stellt sicher, dass der Publish-Adapter erst initialisiert wird, nachdem der Subscribe-Adapter bereit ist; `AX_PUBLISH_1.INITO` → `GreenWhiteBackground1_AX.INIT`; `GreenWhiteBackground1_AX.INITO` → `INITO` (und → `AND_QO.REQ`).
5. **Parameter**: `u16ObjId` → `Button_IXA.u16ObjId` und `GreenWhiteBackground1_AX.u16ObjId`; `Output` → `logiBUS_QXA.Output`; `ID_READ` → `AX_SUBSCRIBE_1.ID`; `ID_WRITE` → `AX_PUBLISH_1.ID`.
6. **Qualität**: `QI` → `.QI` von `logiBUS_QXA`, `Button_IXA`, `AX_SUBSCRIBE_1`, `AX_PUBLISH_1` und `GreenWhiteBackground1_AX`; deren `QO`-Ausgänge → `AND_QO.IN1`..`IN5`; `AND_QO.OUT` → `QO`. Die Instanzen sind nicht mehr fest auf `QI=TRUE` parametriert, sondern werden über `QI` der SubApp freigegeben.

## Technische Besonderheiten

- **Tastender Pegel statt Klick-Toggle**: Seit Version 1.2 ersetzt ein einfaches `AX_OR_2` (in `AX_2_3`) den früheren SR-Latch (`AX_ASR_RF_TRIG` × 2 + `ASR_MERGE_2` + `ASR_AX_SR`). VT-Taster **und** Remote-Kommando sind dadurch tastend (Pegel); der Ausgang folgt der ODER-Verknüpfung beider Quellen. Die Variante mit Flankenerkennung und Latch ist [`Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING`](./Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING.md).
- **Getrennte OPC-UA-Schlüssel**: `ID_READ` (Subscribe-Key, schreibbar) und `ID_WRITE` (Publish-Key, lesbar) sind getrennte Parameter; der Echo-Pfad (`AX_PUBLISH_1`) schreibt den ODER-Pegel, `AX_SUBSCRIBE_1` liest nur den Schreibzugriff des Web-Clients.
- **`RSP`-Event von `AX_SUBSCRIBE_1` ist wirkungslos**: Quellcode-geprüft in FORTE (`CCommFB::sendData()`, `commfb.cpp`) — für den Service-Typ *Subscriber* ist der `RSP`-Zweig ein kompletter No-Op, das Event-Paar `INIT`/`RSP` existiert nur, weil `CCommFB` die gemeinsame Basisklasse aller "Responder"-artigen Dienst-FBs ist (z. B. `SERVER`, wo `RSP` real etwas auslöst).
- **Boot-Reihenfolge**: `AX_SUBSCRIBE_1.INITO` löst `AX_PUBLISH_1.INIT` aus, um Race Conditions beim Deployment zu vermeiden (vgl. dasselbe Muster im PWM-Beispiel `RampLimitFS_TO_logiBUS_QDA_PWM_OPC`).

## Anwendungsszenarien

- Jeder digitale Ausgang, der sowohl vor Ort (VT-Taster) als auch fernbedient (Web/OPC-UA) im Tastbetrieb schaltbar sein soll, mit VT-Statusfarbe und OPC-UA-Echo.

## Vergleich mit ähnlichen Bausteinen

Gegenüber dem Eingangs-Pendant [`logiBUS_IXA_BG_OPC`](./logiBUS_IXA_BG_OPC.md) kommt hier die zweite Schreibquelle (OPC-UA-Subscribe) hinzu, die per ODER (`AX_2_3`) mit dem VT-Taster zusammengeführt wird. Die rastende Variante mit Flankenerkennung und Latch ist [`Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING`](./Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING.md).

## Zusammenfassung

`Button_IXA_TO_logiBUS_QXA_BG_OPC` verbindet einen digitalen Ausgang bidirektional mit OPC-UA: VT-Taster und Web-Kommando werden in `AX_2_3` ODER-verknüpft und der Pegel an Hardware, VT-Anzeige und OPC-UA-Echo verteilt; `INIT`/`INITO` sind durch die Dienste gekettet, `QO` ist das UND der Dienste.

## 🛠️ Zugehörige Übungen

- [InputOutputTesterButton_DIDO_OPC_UA](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Meins/InputOutputTester/Button_DIDO_OPC_UA/InputOutputTesterButton_DIDO_OPC_UA/)

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
