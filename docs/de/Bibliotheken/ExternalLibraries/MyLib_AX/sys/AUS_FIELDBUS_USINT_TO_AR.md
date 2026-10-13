# AUS_FIELDBUS_USINT_TO_AR

![AUS_FIELDBUS_USINT_TO_AR_network](./AUS_FIELDBUS_USINT_TO_AR_network.svg)

* * * * * * * * * *

## Einleitung

`AUS_FIELDBUS_USINT_TO_AR` wandelt einen **AUS-Rohwert** (TECU-`USINT`) in einen `AR`-Wert um. Die Werte `NOT_AVAILABLE`/`ERROR_INDI` werden dabei gefiltert (`VALID_SIGNAL_W`/`ERROR_INDI_W`/`NOT_AVAILABLE_W`, `FIELDBUS_SIGNAL.gcf`); die Skalierung ist fest auf `SCALE=1.0` und `OFFSET=0` eingestellt, der Wert bleibt also unskaliert.

Die SubApp wurde aus `AUS_FIELDBUS_USINT_TO_SIGNAL_SCALED` herausgelöst, da in `TECU_FHS`/`TECU_RHS` `SCALE=1.0`/`OFFSET=0` bisher jedes Mal einzeln parametriert wurde; sie liegt in `MyLib_AX::sys`, weil sie generisch und nicht TECU-spezifisch ist.

![AUS_FIELDBUS_USINT_TO_AR](AUS_FIELDBUS_USINT_TO_AR.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

*Keine direkten Ereignis-Eingänge vorhanden – Ereignisse kommen über die Adapter-Sockets/-Plugs*

### **Ereignis-Ausgänge**

*Keine direkten Ereignis-Ausgänge vorhanden*

### **Daten-Eingänge**

*Keine Daten-Eingänge vorhanden*

### **Daten-Ausgänge**

*Keine Daten-Ausgänge vorhanden*

### **Adapter**

- **IN** (Socket): Unidirektionaler Adapter vom Typ `adapter::types::unidirectional::AUS` – TECU-Rohwert
- **OUT** (Plug): Unidirektionaler Adapter vom Typ `adapter::types::unidirectional::AR` – `0.0` bei `NOT_AVAILABLE`/`ERROR_INDI`, sonst `IN` unskaliert

## Verwendete Funktionsbausteine (FBs)

- **SCALE_1**: [`logiBUS::signalprocessing::fieldbus::AUS_FIELDBUS_USINT_TO_SIGNAL_SCALED`](../../logiBUS/signalprocessing/fieldbus/AUS_FIELDBUS_USINT_TO_SIGNAL_SCALED.md)
    - Parameter: `SCALE=1.0`, `OFFSET=0`

## Programmablauf und Verbindungen

- `IN` → `SCALE_1.IN`
- `SCALE_1.OUT` → `OUT`

Der `VALID`-Plug des inneren Bausteins wird nicht nach außen geführt.

## Anwendungsszenarien

- Übernahme von TECU-Werten (`USINT`) als gefilterter, unskalierter `AR`-Wert

## Fazit

`AUS_FIELDBUS_USINT_TO_AR` ist eine kleine Wrapper-SubApp, die `AUS_FIELDBUS_USINT_TO_SIGNAL_SCALED` mit festen Parametern `SCALE=1.0`/`OFFSET=0` bereitstellt.
