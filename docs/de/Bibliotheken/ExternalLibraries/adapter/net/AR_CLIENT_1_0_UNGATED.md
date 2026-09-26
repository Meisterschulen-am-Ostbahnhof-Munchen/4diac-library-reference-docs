# AR_CLIENT_1_0_UNGATED

* * * * * * * * * *

## Einleitung

Der Funktionsblock **AR_CLIENT_1_0_UNGATED** ist ein Composite-Funktionsblock aus dem Paket `adapter::net`. Er kapselt den netzwerkbasierten `CLIENT_1_0`-Funktionsblock aus der IEC 61499-Standardbibliothek und stellt dessen Schnittstelle auf einen unidirektionalen **AR-Adapter** (`IN`, Datentyp `REAL`) bereit.

Im Gegensatz zu **AR_CLIENT_1_0** enthält dieser Baustein **keinen** internen `E_D_FF_ANY`-Wert-Filter (Send-on-Change). Jedes eintreffende Ereignis an `IN.E1` löst **ungefiltert und bedingungslos** ein Sendeereignis an `CLIENT_1_0.REQ` aus.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (EInit): Initialisierungsereignis, verbunden mit `QI` und `ID`

### **Ereignis-Ausgänge**

- **INITO** (EInit): Bestätigung der Initialisierung, verbunden mit `QO` und `STATUS`
- **CNF** (Event): Bestätigung, dass die Daten gesendet wurden, verbunden mit `QO` und `STATUS`

### **Daten-Eingänge**

- **QI** (BOOL): Qualifier-Eingang, öffnet (TRUE) bzw. schließt (FALSE) die Verbindung zum Server
- **ID** (WSTRING): Identifikator der Verbindung (OPC-UA-Adresse des Zielknotens, z. B. `opc_ua[CALL_METHOD;opc.tcp://192.168.1.12:4840#;...]`)

### **Daten-Ausgänge**

- **QO** (BOOL): Qualifier-Ausgang, Verbindungsstatus
- **STATUS** (WSTRING): Statusinformationen als Unicode-String

### **Adapter**

| Adapter | Typ                                | Richtung         | Beschreibung                                              |
| ------- | ---------------------------------- | ---------------- | --------------------------------------------------------- |
| IN      | adapter::types::unidirectional::AR | Socket (Eingang) | Zu sendender REAL-Wert (wird bei jedem `IN.E1` gesendet) |

## Funktionsweise

1. Über das `INIT`-Ereignis wird der interne `CLIENT_1_0`-Block mit `QI` und `ID` initialisiert und quittiert bei Erfolg mit `INITO`.
2. Sobald der AR-Socket `IN` ein Ereignis auf `IN.E1` liefert, wird das Ereignis **direkt** an `F_MOVE.REQ` weitergeleitet.
3. `F_MOVE` (`REAL`) übernimmt den an `IN.D1` anliegenden REAL-Wert, leitet ihn an `CLIENT_1_0.SD_1` weiter und löst `CLIENT_1_0.REQ` aus.
4. Der `CLIENT_1_0`-Block führt den OPC-UA-Remote-Write bzw. den Methodenaufruf aus und bestätigt mit `CNF`.

## Technische Besonderheiten

- **Ungefilterte Ereignisauslösung (Ungated)**: Da kein `E_D_FF_ANY`-Flipflop vorgeschaltet ist, wird kein Change-Filter angewendet. Selbst wenn der Wert `IN.D1` identisch zum zuletzt gesendeten Wert ist, führt ein erneutes `IN.E1`-Ereignis zur Ausführung.
- **Speziell für OPC-UA CALL_METHOD / Remote-Input**: Unverzichtbar für Bedienkomponenten (z. B. VT-Eingabefelder oder Web-HMIs), bei denen die wiederholte Eingabe desselben Zahlenwerts zuverlässig einen erneuten Remote-Methodenaufruf auslösen muss.
- **Entkopplung vom Change-Filter**: Während `AR_CLIENT_1_0` für kontinuierliche Sende-bei-Änderung-Anwendungsfälle optimiert ist, dient `AR_CLIENT_1_0_UNGATED` der ereignisgesteuerten Aufrufausführung.

## Zustandsübersicht

1. **Nicht initialisiert**: Der Block wartet auf das `INIT`-Ereignis.
2. **Initialisiert**: Verbindung zum Server steht; bereit für Sendeereignisse.
3. **Sendeaktiv**: Jedes `IN.E1`-Ereignis führt unmittelbar zur Übertragung via `CLIENT_1_0`.

## Anwendungsszenarien

- **Remote-Methodenaufrufe über VT/HMI**: Z. B. `NumericValue_TO_CLIENT_1_0_OPC.SUB` — Wenn der Bediener einen Zahlenwert am VT bestätigt, muss die OPC-UA-Methode auch dann aufgerufen werden, wenn der Wert unverändert geblieben ist.
- **Ereignisgesteuerte Befehlsübermittlung**: Übertragung von REAL-Parametern, deren Sende-Zeitpunkt als Ereignis eine eigenständige Bedeutung hat.

## Vergleich mit ähnlichen Bausteinen

- **AR_CLIENT_1_0**: Enthält ein internes `E_D_FF_ANY`-Flipflop, das Werteänderungen filtert (Send-on-Change). `AR_CLIENT_1_0_UNGATED` verzichtet bewusst auf diesen Filter.
- **AX_CLIENT_1_0**: Verarbeitet BOOL-Werte mit Change-Filter.
- **ATM_CLIENT_1_0**: Verarbeitet TIME-Werte mit Change-Filter.

## Fazit

**AR_CLIENT_1_0_UNGATED** stellt sicher, dass jedes Trigger-Ereignis an der AR-Adapter-Schnittstelle zu einem OPC-UA-Sendevorgang führt — ideal für Remote-Methodenaufrufe in HMI- und VT-Anwendungen.
