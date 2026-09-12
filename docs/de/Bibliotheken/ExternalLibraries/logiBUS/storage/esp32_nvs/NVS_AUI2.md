# NVS_AUI2

![NVS_AUI2](./NVS_AUI2.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **NVS_AUI2** dient dem Laden und Speichern von UINT-Daten im nichtflüchtigen Speicher (NVS) eines ESP32 über einen Adapter. Er kapselt die Initialisierung und den Zugriff auf einen einzelnen NVS-Eintrag, der über einen Schlüssel (KEY) identifiziert wird. Der FB bietet eine initialisierende Ereignisschnittstelle und eine bidirektionale Adapter-Schnittstelle, über die extern auf den gespeicherten Wert zugegriffen werden kann.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Typ | Kommentar | Mitgeführte Variablen |
| -------- | --- | --------- | --------------------- |
| `INIT` | EInit | Service Initialization | QI, KEY, DEFAULT_VALUE |

### **Ereignis-Ausgänge**

| Ereignis | Typ | Kommentar | Mitgeführte Variablen |
| -------- | --- | --------- | --------------------- |
| `INITO` | EInit | Initialization Confirm | QO, STATUS |

### **Daten-Eingänge**

| Name | Typ | Kommentar |
| ---- | --- | --------- |
| `QI` | BOOL | Event Input Qualifier |
| `KEY` | STRING | Schlüsselname für den NVS-Eintrag |
| `DEFAULT_VALUE` | UINT | Standardwert, falls der Schlüssel im NVS nicht existiert (Voreinstellung: 0) |

### **Daten-Ausgänge**

| Name | Typ | Kommentar |
| ---- | --- | --------- |
| `QO` | BOOL | Event Output Qualifier |
| `STATUS` | STRING | Dienststatus |

### **Adapter**

| Adapter | Typ | Kommentar |
| ------- | --- | --------- |
| `VAL` | `adapter::types::bidirectional::AUI2` (Socket) | Wert (UINT) |

## Funktionsweise

1. **Initialisierung**: Ein Ereignis am Eingang `INIT` löst die Initialisierung des internen NVS-Bausteins aus.
2. **Wert auslesen**: Der gelesene Wert wird über den Adapter `VAL` als `DI1` ausgegeben.
3. **Wert speichern**: Ein externer Baustein kann über den Adapter `VAL` ein Ereignis `EO1` senden, um einen neuen Wert (`DO1`) in den NVS zu schreiben.
