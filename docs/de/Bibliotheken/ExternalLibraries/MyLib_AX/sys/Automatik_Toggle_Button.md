# Automatik_Toggle_Button

![Automatik_Toggle_Button_network](./Automatik_Toggle_Button_network.svg)

* * * * * * * * * *

## Einleitung

`Automatik_Toggle_Button` ist ein **Automatik/Hand-Umschalter**: Ein VT-Button (bzw. Softkey oder AUX-Funktion) schaltet den Automatik-Zustand um. Dieser wird als AX-Plug (`AUTOMATIK`), als Inverses (`AX_MAN`, z. B. für den `MAN`-Eingang eines PID-Reglers) und als lokaler Publish (`ID_AUTOMATIK_STATE_WRITE`) für VT-Status und Web-Dashboard ausgegeben. Die SubApp ist für jede Automatik-Funktion (Ölkühler, Aufnahme, …) wiederverwendbar und wurde aus `Aufnahme_Automatik.SUB` herausgelöst.

![Automatik_Toggle_Button](Automatik_Toggle_Button.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialization Request
- **FORCE_ON**: Schaltet die Automatik hart EIN (z. B. von einer übergeordneten Automatik oder einem Methodenaufruf). Läuft über `AX_E_PERMIT_2` und ist damit `PERMIT`-gefiltert. Unverdrahtet lassen, wenn nicht gebraucht.
- **FORCE_OFF**: Schaltet die Automatik hart AUS (z. B. durch einen Kollisionsschutz). Unverdrahtet lassen, wenn nicht gebraucht.
- **AUS**: Reset hart auf Hand/AUS von außen als Event (z. B. ein zentral gelesener gemeinsamer Softkey), zusätzlich zum eigenen `u16ObjId_RESET`/`u16ObjIdA_RESET`/`ID_RESET_METHOD`-Pfad. Unverdrahtet lassen, wenn nicht gebraucht.

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialization Confirm
- **AUTOMATIK_SET_FROM_VT**: Feuert nur bei einem Klick (Button/Softkey/Aux, `PERMIT`-gefiltert), der die Automatik von AUS auf EIN schaltet. Ein Klick, der die Automatik ausschaltet, feuert dieses Ereignis nicht. Es ist ein echtes Display-Klick-Ereignis, kein Zustandspegel oder Republish.

### **Daten-Eingänge**

- **QI** (`BOOL`, Vorgabe `TRUE`): Quality Input, TRUE aktiviert den Dienst
- **u16ObjId_Automatik_Toggle** (`UINT`, Vorgabe `ID_NULL`): Object-ID des VT-Softkeys (Automatik/Hand-Umschalter)
- **u16ObjIdA_Automatik_Toggle** (`UINT`, Vorgabe `ID_NULL`): Object-ID der AuxFunction2 (Joystick), gleichberechtigt zum VT-Button; optional
- **u16ObjIdB_Automatik_Toggle** (`UINT`): Object-ID des VT-Buttons (Automatik/Hand-Umschalter)
- **bDefaultAutomatik** (`BOOL`, Vorgabe `FALSE`): Automatik-Zustand beim Start
- **ID_AUTOMATIK_STATE_WRITE** (`WSTRING`): Lokale Publish-Adresse (`BOOL`, `ACTION=WRITE`) für den Automatik-Ein/Aus-Zustand (Status-Text auf der VT, Web-Client/Dashboard)
- **u16ObjId_RESET** (`UINT`, Vorgabe `ID_NULL`): Object-ID des Softkeys „Alles Aus“ (Reset, hart auf Hand/AUS), z. B. `SoftKey_AUS`
- **u16ObjIdA_RESET** (`UINT`, Vorgabe `ID_NULL`): Object-ID der AuxFunction2 (Joystick) für den Reset
- **ID_RESET_METHOD** (`WSTRING`): Lokale Methodenadresse (`ACTION=CREATE_METHOD`) für den argumentlosen Reset-Trigger; wird von Web-Client und OPC-Dashboard aufgerufen

### **Daten-Ausgänge**

- **QO** (`BOOL`): Quality Output, UND-Verknüpfung (`AND_QO`) der `QO`-Ausgänge von `E_T_FF_Automatik`, `AX_PUBLISH_Automatik`, `Background_Automatik`, `Reset` und `Button_Softkey_Aux_Automatik_Toggle`

### **Adapter**

- **PERMIT** (Socket, `adapter::types::unidirectional::AX`): Freigabe. Gatet das gesamte Ein/Aus; beim Übergang 1→0 wird einmalig `RESET` ausgelöst.
- **AUTOMATIK** (Plug, `AX`): Automatik aktiv (roh, z. B. für eine Freigabe-UND-Verknüpfung)
- **AX_MAN** (Plug, `AX`): NICHT Automatik aktiv (für den `MAN`-Eingang eines PID-Reglers)

## Verwendete Funktionsbausteine (FBs)

- **Button_Softkey_Aux_Automatik_Toggle**: SubApp `MyLib::sys::Button_Softkey_Aux_IE` – führt Button, Softkey und Aux zu einem `IND` zusammen
- **Reset**: SubApp `MyLib::sys::Softkey_Aux_Method_IE` – Reset per Softkey, Aux oder Methodenaufruf
- **E_T_FF_Automatik**: [`AX_T_FF_SR_SYM_INIT`](../../adapter/events/unidirectional/BOOL/AX_T_FF_SR_SYM_INIT.md) (`QI` vom Wrapper-Eingang) – speichert den Automatik-Zustand, Startwert `bDefaultAutomatik`
- **AX_E_PERMIT_2**: [`AX_E_PERMIT_2`](../../adapter/events/unidirectional/BOOL/AX_E_PERMIT_2.md) – lässt `FORCE_ON` und den Klick nur bei `PERMIT=TRUE` durch
- **AX_SPLIT_2**: verteilt `PERMIT` auf `AX_E_PERMIT_2` und `E_F_TRIG`
- **E_F_TRIG**: [`AX_F_TRIG`](../../adapter/events/unidirectional/BOOL/AX_F_TRIG.md) – fallende Flanke auf `PERMIT`
- **E_SWITCH**: [`AX_E_SWITCH`](../../adapter/events/unidirectional/BOOL/AX_E_SWITCH.md) – filtert den Klick nach dem Zustand vor dem Klick
- **AX_SPLIT_4_Automatik**: [`AX_SPLIT_5`](../../adapter/events/unidirectional/BOOL/AX_SPLIT_5.md) – verteilt den Zustand auf fünf Ziele
- **AX_NOT_Man**: [`AX_NOT_INIT`](../../adapter/booleanOperators/AX_NOT_INIT.md) – Inverses für `AX_MAN`
- **AX_PUBLISH_Automatik**: [`AX_PUBLISH_1`](../../adapter/net/AX_PUBLISH_1.md) (`QI` vom Wrapper-Eingang) – publiziert den Zustand unter `ID_AUTOMATIK_STATE_WRITE`
- **Background_Automatik**: [`GreenWhiteBackground4_AX`](./GreenWhiteBackground4_AX.md) – färbt Button und AUX grün/weiß
- **AND_QO**: `iec61131::booleanOperators::AND_BOOL_5` – verknüpft die `QO`-Ausgänge der fünf Bausteine `E_T_FF_Automatik`, `AX_PUBLISH_Automatik`, `Background_Automatik`, `Reset` und `Button_Softkey_Aux_Automatik_Toggle` zu `QO`

## Programmablauf und Verbindungen

1. `Button_Softkey_Aux_Automatik_Toggle.IND` → `AX_E_PERMIT_2.EI2`; `FORCE_ON` → `AX_E_PERMIT_2.EI1`.
2. `AX_E_PERMIT_2.EO1` → `E_T_FF_Automatik.S` (Einschalten über `FORCE_ON`).
3. `AX_E_PERMIT_2.EO2` → `E_SWITCH.EI`; `E_SWITCH.G` erhält den Zustand `Q` vor dem Klick (`AX_SPLIT_4_Automatik.OUT5`).
    - `E_SWITCH.EO0` (AUS → EIN) → `AUTOMATIK_SET_FROM_VT` und `E_T_FF_Automatik.S`
    - `E_SWITCH.EO1` (EIN → AUS) → `E_T_FF_Automatik.R`
4. `Reset.IND`, `AUS`, `FORCE_OFF` und `E_F_TRIG.EO` → `E_T_FF_Automatik.R`.
5. `PERMIT` → `AX_SPLIT_2` → `AX_E_PERMIT_2.PERMIT` und `E_F_TRIG.QI`.
6. `E_T_FF_Automatik.Q` → `AX_SPLIT_4_Automatik`: `OUT1` → `AUTOMATIK`, `OUT2` → `AX_NOT_Man` → `AX_MAN`, `OUT3` → `AX_PUBLISH_Automatik`, `OUT4` → `Background_Automatik.DI1`, `OUT5` → `E_SWITCH.G`.
7. Initialisierungskette: `INIT` → `E_T_FF_Automatik.INIT` → `AX_PUBLISH_Automatik.INIT` → `Background_Automatik.INIT` → `Reset.INIT` → `Button_Softkey_Aux_Automatik_Toggle.INIT`; `Button_Softkey_Aux_Automatik_Toggle.INITO` → `INITO` und → `AND_QO.REQ`.
8. Qualität: `QI` → `.QI` von `E_T_FF_Automatik`, `AX_PUBLISH_Automatik`, `Background_Automatik`, `Reset` und `Button_Softkey_Aux_Automatik_Toggle`; deren `QO` → `AND_QO.IN1` bis `AND_QO.IN5` (in dieser Reihenfolge); `AND_QO.OUT` → `QO`.

## Technische Besonderheiten

- **Button statt Softkey**: Der VT-Button muss non-latching (`Latchable=0`) sein; das Feedback der Automatik-Anzeige übernimmt `Background_Automatik`.
- **PERMIT** gatet das gesamte Ein/Aus und setzt beim Übergang 1→0 einmalig zurück – es wirkt dabei wie ein `AX_PERMIT`.
- `FORCE_ON`/`FORCE_OFF`/`AUS` sind echte Ereignisse; Aufrufer mit pegelbasierten Quellen müssen die Flankenerkennung selbst vorschalten.
- `AUTOMATIK_SET_FROM_VT` reagiert nur auf den Display-Klick AUS → EIN, nicht auf `FORCE_ON`-Echos.

## Anwendungsszenarien

- Automatik/Hand-Umschaltung mit Statusanzeige für Automatik-Funktionen (Ölkühler, Aufnahme, Bunker-Ernte, …)
- Handbetrieb-Signal für PID-Regler (`AX_MAN`)

## Fazit

`Automatik_Toggle_Button` kapselt Bedienung, Zustandsspeicher, Freigabe, Reset und Statusausgabe einer Automatik-Funktion in einer wiederverwendbaren SubApp.
