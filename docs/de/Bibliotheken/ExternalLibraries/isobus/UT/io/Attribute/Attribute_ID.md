# Attribute_ID

* * * * * * * * * *

## Einleitung

`Attribute_ID` ist ein Eingabe-Service-Interface-Funktionsblock für Doppelwort-Eingabedaten (`DWORD`). Er liest ein Attribut eines VT-Objekts (Objekt-ID `u16ObjId`, Attribut-ID `u8AID`) und stellt dessen Wert als `DWORD` am Ausgang `IN` bereit.

![Attribute_ID](Attribute_ID.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Service-Initialisierung
  - Verknüpft mit: `QI`, `PARAMS`, `u16ObjId`, `u8AID`
- **REQ**: Service-Anfrage
  - Verknüpft mit: `QI`

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierungsbestätigung
  - Verknüpft mit: `QO`, `STATUS`
- **CNF**: Bestätigung der angeforderten Service-Anfrage
  - Verknüpft mit: `QO`, `STATUS`, `IN`
- **IND**: Indikation von der Ressource
  - Verknüpft mit: `QO`, `STATUS`, `IN`

### **Daten-Eingänge**

- **QI** (`BOOL`): Ereignis-Eingangsqualifizierer
- **PARAMS** (`STRING`): Service-Parameter
- **u16ObjId** (`UINT`): Objekt-ID, Initialwert `ID_NULL`
- **u8AID** (`USINT`): Attribut-ID des Objekts

### **Daten-Ausgänge**

- **QO** (`BOOL`): Ereignis-Ausgangsqualifizierer
- **STATUS** (`STRING`): Service-Status
- **IN** (`DWORD`): Eingabedaten von der Ressource

### **Adapter**

Keine Adapter-Schnittstellen vorhanden.

## Funktionsweise

Der Funktionsblock ist ein Service-Interface-FB ohne interne Implementierung im Typ (Schnittstellen-Deklaration). `INIT` initialisiert den Dienst mit `QI`, `PARAMS`, `u16ObjId` und `u8AID` und wird mit `INITO` bestätigt. `REQ` stellt eine Anfrage, die mit `CNF` bestätigt wird; `IND` zeigt eine Indikation von der Ressource an. In allen drei Fällen werden `QO`, `STATUS` und (bei `CNF`/`IND`) der Wert `IN` ausgegeben.

## Technische Besonderheiten

- Verwendet den Datentyp `DWORD` (32 Bit) für den Attributwert
- Die zu lesende Attribut-ID wird über den Daten-Eingang `u8AID` angegeben
- Die Objekt-ID ist mit `ID_NULL` (`isobus::UT::Q::const::IDs::ID_NULL`) vorbelegt
- Ursprünglich von fortiss GmbH (Monika Wenger, 2017) als Eingabe-Service-Interface angelegt

## Anwendungsszenarien

- Auslesen von Attributwerten (z. B. Attribut-ID eines Objekts im VT) in ISOBUS-Anwendungen

## ⚖️ Vergleich mit ähnlichen Bausteinen

- [NumericValue_ID](../NumericValue/NumericValue_ID.md) hat dieselbe Service-Schnittstelle, liest aber den Zahlenwert eines Objekts und besitzt keinen `u8AID`-Eingang.

## Fazit

`Attribute_ID` liefert den Attributwert eines VT-Objekts als `DWORD` über eine Service-Interface-Schnittstelle.
