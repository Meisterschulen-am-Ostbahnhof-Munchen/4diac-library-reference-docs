# AR_SERVER_0_1

* * * * * * * * * *

## Einleitung

Der Funktionsblock **AR_SERVER_0_1** ist ein Composite-Funktionsblock aus dem Paket `adapter::net`. Er kapselt den netzwerkbasierten Server-Baustein `iec61499::net::SERVER_0_1` und stellt empfangene OPC-UA-Methodenaufrufe (`CALL_METHOD`) für **REAL**-Argumente als unidirektionalen **AR-Adapter** (`OUT`) bereit. Er bildet das Empfangs-Gegenstück zu **AR_CLIENT_1_0** (bzw. `CLIENT_1_0`).

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (EInit): Initialisierungsereignis, verbunden mit `QI` und `ID` (leitet an `SERVER_0_1.INIT` weiter)

### **Ereignis-Ausgänge**

- **INITO** (EInit): Bestätigung der Initialisierung (`SERVER_0_1.INITO`), verbunden mit `QO` und `STATUS`

### **Daten-Eingänge**

- **QI** (BOOL): Qualifier-Eingang, schaltet den Server aktiv (TRUE) bzw. inaktiv (FALSE)
- **ID** (WSTRING): Server-Identifikator (OPC-UA-Serveradresse und Methodenspezifikation)

### **Daten-Ausgänge**

- **QO** (BOOL): Qualifier-Ausgang, Server-Betriebsstatus
- **STATUS** (WSTRING): Statusinformationen als Unicode-String

### **Adapter**

| Adapter | Typ                                | Richtung      | Beschreibung                                     |
| ------- | ---------------------------------- | ------------- | ------------------------------------------------ |
| OUT     | adapter::types::unidirectional::AR | Plug (Ausgang)| Empfangener REAL-Wert bei jedem Methodenaufruf |

## Funktionsweise

1. Ein `INIT`-Ereignis an `AR_SERVER_0_1` wird an den internen `SERVER_0_1.INIT`-Eingang weitergeleitet. Der Server startet den OPC-UA-Endpunkt und quittiert bei Erfolg mit `INITO`.
2. Bei einem eintreffenden Methodenaufruf von einem entfernten Client (`CLIENT_1_0`/`AR_CLIENT_1_0`) erzeugt `SERVER_0_1` das Indikations-Ereignis `IND` und stellt den empfangenen REAL-Wert an `RD_1` bereit.
3. Zur Vermeidung von Antwort-Latenz-Sperren (RSP-Latenz-Bug) ist `SERVER_0_1.IND` im internen Netzwerk direkt auf `SERVER_0_1.RSP` zurückverdrahtet. Der Server quittiert die Methode dadurch sofort ohne Verzögerung.
4. Gleichzeitig leitet `IND` den empfangenen Wert über `F_MOVE` (Datentyp `REAL`) an den AR-Plug `OUT.D1` weiter und löst das Ausgangs-Ereignis `OUT.E1` aus.

## Technische Besonderheiten

- **Direkte RSP-Rückverdrahtung**: Um 4-Sekunden-Server-Sperren durch verzögerte Antworten zu vermeiden, wird `SERVER_0_1.IND` intern unmittelbar auf `SERVER_0_1.RSP` zurückgekoppelt.
- **Nahtlose Adapter-Integration**: Der empfangene Rohwert `RD_1` von `SERVER_0_1` wird über ein `F_MOVE` (REAL) direkt auf die AR-Adapter-Struktur (`OUT.D1` / `OUT.E1`) abgebildet.
- **Empfangs-Gegenstück zu `AR_CLIENT_1_0`**: In IEC 61499 pairt ein `CLIENT_1_0`-Baustein ("Connect to a SERVER_0_1 Block") mit einem `SERVER_0_1`-Baustein.

## Zustandsübersicht

1. **Nicht initialisiert**: Der Server ist inaktiv und wartet auf das `INIT`-Ereignis.
2. **Bereit (Listening)**: Der OPC-UA-Server ist aktiv und wartet auf Methodenaufrufe von entfernten Clients.
3. **Empfang (Indikation)**: Ein Methodenaufruf trifft ein; der REAL-Wert wird an den `OUT`-Adapter übergeben und die Quittierung sofort gesendet.

## Anwendungsszenarien

- **Empfang von Fernsteuerungs-Sollwerten**: Empfangen von analogen Einstellwerten (z. B. Soll-Druck, Soll-Drehzahl) von einem entfernten Steuerungsmodul via OPC UA `CALL_METHOD`.
- **Modulare Server-Architekturen**: Bereitstellung von OPC-UA-Methoden-Endpunkten in durchgängig adapterbasierten Anwendungen.

## Vergleich mit ähnlichen Bausteinen

- **AX_SERVER_0_1**: Empfängt BOOL-Werte über einen AX-Adapter.
- **ATM_SERVER_0_1**: Empfängt TIME-Werte über einen ATM-Adapter.
- **AR_SUBSCRIBE_1**: Kapselt `SUBSCRIBE_1` für lokales Publish/Subscribe statt verbindungsorientierter Methodenaufrufe.

## Fazit

**AR_SERVER_0_1** stellt empfangene OPC-UA-Methodenaufrufe für REAL-Werte als sauberen AR-Adapter bereit und verhindert durch direkte RSP-Rückkopplung Server-Lockups.
