# AS2_SINT_TO_S

![AS2_SINT_TO_S_network](./AS2_SINT_TO_S_network.svg)

* * * * * * * * * *

## Einleitung

`AS2_SINT_TO_S` ist ein Composite-Funktionsblock, der zwischen klassischen Ereignis-/Datenanschlüssen vom Typ `SINT` und einem **bidirektionalen** `AS2`-Adapter vermittelt. Der Eingang `OUT` wird bei `REQ` auf die ausgehende Seite des Adapter-Plugs `AS2_OUT` geschrieben; der über die Rückrichtung des Adapters gemeldete Wert erscheint am Ausgang `IN` und wird mit `CNF` bestätigt.

![AS2_SINT_TO_S](AS2_SINT_TO_S.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **REQ**: Anforderung an den Adapter. Verbundene Daten: `OUT`

### **Ereignis-Ausgänge**

- **CNF**: Bestätigung vom Adapter. Verbundene Daten: `IN`

### **Daten-Eingänge**

- **OUT** (`SINT`): Daten, die an den Adapter gesendet werden

### **Daten-Ausgänge**

- **IN** (`SINT`): Vom Adapter empfangene Daten

### **Adapter**

- **AS2_OUT** (Plug): Bidirektionaler Adapter vom Typ `adapter::types::bidirectional::AS2` (Adapter-Ausgang)

## Funktionsweise

1. `REQ` wird an `AS2_OUT.EO1` weitergeleitet, `OUT` an `AS2_OUT.DO1`.
2. `AS2_OUT.EI1` wird an `CNF` weitergeleitet, `AS2_OUT.DI1` an `IN`.

## Technische Besonderheiten

- Reine Ereignis-/Datenverbindungen (`FBNetwork`), keine eigene Logik und kein interner Zustand
- Teil der Konvertierungsfamilie (analog zu `AR2`/`AB2`/`AE2`)

## Anwendungsszenarien

- Anbindung von `SINT`-Werten aus klassischen Funktionsbaustein-Netzen an einen bidirektionalen `AS2`-Adapter

## Fazit

`AS2_SINT_TO_S` überführt `SINT`-Ereignis-/Datenanschlüsse 1:1 in die beiden Richtungen eines `AS2`-Adapters.
