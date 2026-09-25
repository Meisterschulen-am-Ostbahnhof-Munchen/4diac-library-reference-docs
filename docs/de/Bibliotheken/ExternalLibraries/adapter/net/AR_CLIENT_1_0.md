# AR_CLIENT_1_0

* * * * * * * * * *

## Einleitung

Der Funktionsblock **AR_CLIENT_1_0** ist ein Composite-Funktionsblock, der den netzwerkbasierten `CLIENT_1_0`-Funktionsblock aus der IEC 61499-Standardbibliothek kapselt und dessen Schnittstelle auf einen unidirektionalen **AR-Adapter** (Datentyp `REAL`) abbildet. Ein am Adapter-Socket `IN` anliegender **REAL**-Wert wird über ein internes `E_D_FF_ANY`-Flipflop gepuffert und anschließend über `CLIENT_1_0` als OPC-UA-**Write** oder Methodenaufruf (`CALL_METHOD`) an den unter `ID` konfigurierten entfernten Knoten gesendet.

Im Unterschied zu **AR_PUBLISH_1** (lokales Publish/Subscribe) schreibt `CLIENT_1_0` aktiv auf einen **entfernten** Server.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (EInit): Initialisierungsereignis, verbunden mit `QI` und `ID`

### **Ereignis-Ausgänge**

- **INITO** (EInit): Bestätigung der Initialisierung, verbunden mit `QO` und `STATUS`
- **CNF** (Event): Bestätigung, dass die Daten gesendet wurden, verbunden mit `QO` und `STATUS`

### **Daten-Eingänge**

- **QI** (BOOL): Qualifier-Eingang, öffnet (TRUE) bzw. schließt (FALSE) die Verbindung zum Server
- **ID** (WSTRING): Identifikator der Verbindung (OPC-UA-Adresse des Zielknotens, z. B. `opc_ua[WRITE;opc.tcp://192.168.1.12:4840#;...]`)

### **Daten-Ausgänge**

- **QO** (BOOL): Qualifier-Ausgang, Verbindungsstatus
- **STATUS** (WSTRING): Statusinformationen als Unicode-String

### **Adapter**

| Adapter | Typ                                | Richtung         | Beschreibung           |
| ------- | ---------------------------------- | ---------------- | ---------------------- |
| IN      | adapter::types::unidirectional::AR | Socket (Eingang) | Zu sendender REAL-Wert |

## Funktionsweise

1. Über das `INIT`-Ereignis wird der interne `CLIENT_1_0`-Block mit `QI` und `ID` initialisiert; er baut die Verbindung zum entfernten Server auf und quittiert bei Erfolg mit `INITO`.
2. Sobald der AR-Socket `IN` ein Ereignis auf `IN.E1` liefert, wird der an `IN.D1` anliegende REAL-Wert in das interne `E_D_FF_ANY`-Flipflop übernommen.
3. Das Flipflop hält den Wert stabil und erzeugt das Ereignis `EO`.
4. `EO` stößt über ein `F_MOVE` (Datentyp `REAL`) die Zuweisung an `CLIENT_1_0.SD_1` an und löst das Sendeereignis `REQ` des `CLIENT_1_0`-Blocks aus.
5. Nach erfolgreichem Senden bestätigt der `CLIENT_1_0`-Block mit `CNF`.

## Technische Besonderheiten

- **Pufferung mit E_D_FF_ANY**: Der zu sendende REAL-Wert wird über ein internes `E_D_FF_ANY` gepuffert, um Störungen durch sich während des Sendevorgangs ändernde Eingangswerte zu verhindern.
- **Remote-Write / Call-Method**: `CLIENT_1_0` adressiert einen entfernten OPC-UA-Server direkt, um analoge Messwerte oder Sollwerte zu übertragen.
- **Kapselung**: Die ursprüngliche Ereignis-/Daten-Schnittstelle von `CLIENT_1_0` wird nach innen verlegt; nach außen ist nur noch die AR-Adapter-Schnittstelle sichtbar.

## Zustandsübersicht

1. **Nicht initialisiert**: Der Block wartet auf das `INIT`-Ereignis.
2. **Initialisiert**: Die Verbindung zum entfernten Server ist aufgebaut, der Block ist bereit zu senden.
3. **Sendeaktiv**: Ein am AR-Socket eintreffendes Ereignis puffert den REAL-Wert und führt den Remote-Write aus.

## Anwendungsszenarien

- **Fernübertragung von Messwerten**: Übertragung von analogen Sensorwerten (z. B. Druck, Temperatur, Position) an eine entfernte Steuerung oder ein HMI via OPC UA.
- **Modulare Steuerungsarchitekturen**: Einbindung von verteilten Analogwerten in Bibliotheken, die durchgängig auf AR-Adapter setzen.

## Vergleich mit ähnlichen Bausteinen

- **AR_CLIENT_1_0_UNGATED**: Verichtet auf das interne `E_D_FF_ANY`-Flipflop (Change-Filter) und löst bei jedem `IN.E1`-Ereignis bedingungslos den Remote-Write bzw. Methodenaufruf aus — ideal für wiederholte Taster-/VT-Eingaben desselben Werts.
- **AX_CLIENT_1_0**: Identischer Aufbau, verarbeitet jedoch BOOL-Werte über einen AX-Adapter.
- **ATM_CLIENT_1_0**: Identischer Aufbau, verarbeitet TIME-Werte über einen ATM-Adapter.
- **AR_PUBLISH_1**: Kapselt `PUBLISH_1` statt `CLIENT_1_0` für lokale Publish/Subscribe-Verbindungen.

## Fazit

**AR_CLIENT_1_0** verbindet die verbindungsorientierte Remote-Write-Kommunikation des Standardbausteins `CLIENT_1_0` mit der Adapter-basierten Real-Wert-Verarbeitung.
