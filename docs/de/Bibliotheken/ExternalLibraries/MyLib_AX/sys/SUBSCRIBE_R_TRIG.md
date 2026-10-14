# SUBSCRIBE_R_TRIG

![SUBSCRIBE_R_TRIG_network](./SUBSCRIBE_R_TRIG_network.svg)

* * * * * * * * * *

## Einleitung

`SUBSCRIBE_R_TRIG` überwacht einen per OPC-UA abonnierten `AX`-Wert (BOOL) auf eine steigende Flanke und gibt diese als einzelnes Ereignis (`EO`) aus. Damit lässt sich ein entfernter, remote gesetzter BOOL-Wert direkt als auslösendes Ereignis verwenden, ohne den Pegel selbst weiterverarbeiten zu müssen.

![SUBSCRIBE_R_TRIG](SUBSCRIBE_R_TRIG.svg)

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: SUBSCRIBE_R_TRIG

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **SUBSCRIBE**: `iec61499::net::SUBSCRIBE_1` — abonniert den entfernten BOOL-Wert (`ID`, z. B. `Anlage_EIN_READ`).
    - **E_R_TRIG**: `iec61499::events::E_R_TRIG` — erkennt die steigende Flanke des abonnierten Werts und feuert `EO`.
- **Funktionsweise**: Seit Version 2.0 sind das rohe `SUBSCRIBE_1` und `E_R_TRIG` direkt verdrahtet (kein doppelter Flankenspeicher mehr): `SUBSCRIBE.IND` und `SUBSCRIBE.RD_1` führen direkt in `E_R_TRIG`, dessen Flankenerkennung das SubApp-Ereignis `EO` auslöst.
- **Schnittstelle**: Ereignis-Eingang `INIT` (`EInit`), Ereignis-Ausgänge `INITO` (`EInit`) und `EO`; Dateneingänge `QI` (`BOOL`, Vorgabe `TRUE`) und `ID` (`WSTRING`); Datenausgang `QO` (`BOOL`).

## Programmablauf und Verbindungen

1. `ID` → `SUBSCRIBE.ID` (Datenverbindung, ausgeblendet).
2. `SUBSCRIBE.IND` → `E_R_TRIG.EI`; `SUBSCRIBE.RD_1` → `E_R_TRIG.QI`.
3. `E_R_TRIG.EO` → `EO` (SubApp-Ereignisausgang).
4. `INIT` → `SUBSCRIBE.INIT`; `SUBSCRIBE.INITO` → `INITO`.
5. `QI` → `SUBSCRIBE.QI`; `SUBSCRIBE.QO` → `QO`.

## Anwendungsszenarien

- Ein entferntes OPC-UA-Signal (z. B. "Anlage EIN") soll ein einmaliges Ereignis auslösen (z. B. Start einer Initialisierung), statt als dauerhafter Pegel weiterverarbeitet zu werden.

## Zusammenfassung

`SUBSCRIBE_R_TRIG` verbindet OPC-UA-Subscribe direkt mit einer Flankenerkennung und liefert ein einzelnes Ereignis bei steigender Flanke des entfernten Werts.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
