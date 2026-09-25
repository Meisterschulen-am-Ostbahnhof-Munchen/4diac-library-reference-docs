# ATM_CLIENT_1_0

* * * * * * * * * *

## Einleitung

Der Funktionsblock **ATM_CLIENT_1_0** ist ein Composite-Funktionsblock, der den netzwerkbasierten `CLIENT_1_0`-Funktionsblock aus der IEC 61499-Standardbibliothek kapselt und dessen Schnittstelle auf einen unidirektionalen **ATM-Adapter** (Datentyp `TIME`) abbildet. Ein am Adapter-Socket `IN` anliegender **TIME**-Wert wird über ein internes `E_D_FF_ANY`-Flipflop gepuffert und anschließend über `CLIENT_1_0` als OPC-UA-**Write** oder Methodenaufruf (`CALL_METHOD`) an den unter `ID` konfigurierten entfernten Knoten gesendet.

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

| Adapter | Typ                                 | Richtung         | Beschreibung            |
| ------- | ----------------------------------- | ---------------- | ----------------------- |
| IN      | adapter::types::unidirectional::ATM | Socket (Eingang) | Zu sendender TIME-Wert |

## Funktionsweise

1. Über das `INIT`-Ereignis wird der interne `CLIENT_1_0`-Block mit `QI` und `ID` initialisiert; er baut die Verbindung zum entfernten Server auf und quittiert mit `INITO`.
2. Sobald der ATM-Socket `IN` ein Ereignis auf `IN.E1` liefert, wird der an `IN.D1` anliegende TIME-Wert in das interne `E_D_FF_ANY`-Flipflop übernommen.
3. Das Flipflop hält den Wert stabil und erzeugt das Ereignis `EO`.
4. `EO` stößt über ein `F_MOVE` (Datentyp `TIME`) die Zuweisung an `CLIENT_1_0.SD_1` an und löst das Sendeereignis `REQ` des `CLIENT_1_0`-Blocks aus.
5. Nach erfolgreichem Senden bestätigt der `CLIENT_1_0`-Block mit `CNF`.

## Technische Besonderheiten

- **Pufferung mit E_D_FF_ANY**: Der zu sendende TIME-Wert wird über ein internes `E_D_FF_ANY` gepuffert, um Störungen während des Sendevorgangs zu vermeiden.
- **Remote-Write / Call-Method**: `CLIENT_1_0` adressiert einen entfernten OPC-UA-Server direkt, um Zeitwerte (z. B. Timer-Sollwerte, Verzögerungszeiten) zu übertragen.
- **Kapselung**: Die ursprüngliche Ereignis-/Daten-Schnittstelle von `CLIENT_1_0` wird nach innen verlegt.

## Zustandsübersicht

1. **Nicht initialisiert**: Der Block wartet auf das `INIT`-Ereignis.
2. **Initialisiert**: Die Verbindung zum entfernten Server steht, der Block ist bereit zu senden.
3. **Sendeaktiv**: Ein am ATM-Socket eintreffendes Ereignis puffert den TIME-Wert und führt den Remote-Write aus.

## Anwendungsszenarien

- **Fernübertragung von Zeitparametern**: Übertragung von Zeitdauer-Werten (z. B. Ventilschaltzeiten, Spülzeiten) an ein entferntes Steuerungsmodul via OPC UA.
- **Modulare Steuerungsarchitekturen**: Einbindung von verteilten Zeitwerten in Bibliotheken, die auf ATM-Adapter setzen.

## Vergleich mit ähnlichen Bausteinen

- **AX_CLIENT_1_0**: Verarbeitet BOOL-Werte über einen AX-Adapter.
- **AR_CLIENT_1_0**: Verarbeitet REAL-Werte über einen AR-Adapter.
- **ATM_PUBLISH_1**: Kapselt `PUBLISH_1` für lokale Publish/Subscribe-Verbindungen.

## Fazit

**ATM_CLIENT_1_0** ermöglicht das direkte Schreiben von TIME-Werten auf einen entfernten OPC-UA-Server über eine saubere ATM-Adapter-Schnittstelle.
