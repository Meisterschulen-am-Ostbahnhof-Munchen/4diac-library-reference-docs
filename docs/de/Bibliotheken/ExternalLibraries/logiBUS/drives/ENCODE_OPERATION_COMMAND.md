# ENCODE_OPERATION_COMMAND

![ENCODE_OPERATION_COMMAND](./ENCODE_OPERATION_COMMAND.svg)

* * * * * * * * * *

## Einleitung

`ENCODE_OPERATION_COMMAND` erzeugt das Operation-Command-Wort für einen Frequenzumrichter/Antrieb (VFD) aus `RUN`/`DIRECTION` bzw. einem expliziten Fault-Reset-Ereignis. Der Baustein ist generisch für jeden Antrieb gedacht, dessen Steuerwort dem verbreiteten Run/Direction/Stop-plus-Fault-Reset-Muster folgt (Modbus, CANopen PZD1, Profibus PPO usw.) — alle konkreten Befehlscodes werden über Parameter vom Aufrufer vorgegeben, der Baustein selbst kennt keine herstellerspezifischen Werte, nur die Kodierlogik.

Die Bezeichnung `Operation Command` (statt `Control Word`) ist bewusst gewählt: `Control Word` ist CANopen-spezifische Terminologie, während `Operation Command` der herstellerneutrale Begriff ist, wie ihn z. B. das DELIXI-H300-Handbuch verwendet.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **`INIT`**: Initialisierung — setzt `OPERATION_COMMAND` einmalig auf `OC_NO_COMMAND`.
- **`REQ`**: `RUN`/`DIRECTION` neu auswerten und `OPERATION_COMMAND` entsprechend kodieren.
- **`FAULT_RESET`**: Fault-Reset-Befehlscode anfordern, unabhängig vom aktuellen `RUN`/`DIRECTION`-Zustand.

### **Ereignis-Ausgänge**

- **`INITO`**: Initialisierungsbestätigung.
- **`CNF`**: `OPERATION_COMMAND` wurde aktualisiert.

### **Daten-Eingänge**

- **`RUN`** (BOOL): Laufbefehl.
- **`DIRECTION`** (BOOL): `FALSE`=Vorwärts, `TRUE`=Rückwärts.
- **`OC_NO_COMMAND`** (INT): Befehlscode für „kein Befehl“/sicherer Grundzustand (antriebsspezifisch, per Parameter).
- **`OC_FORWARD`** (INT): Befehlscode für Vorwärtslauf (antriebsspezifisch, per Parameter).
- **`OC_REVERSE`** (INT): Befehlscode für Rückwärtslauf (antriebsspezifisch, per Parameter).
- **`OC_DECEL_STOP`** (INT): Befehlscode für kontrollierten Stopp (antriebsspezifisch, per Parameter).
- **`OC_FAULT_RESET`** (INT): Befehlscode für Fault Reset (antriebsspezifisch, per Parameter).

### **Daten-Ausgänge**

- **`OPERATION_COMMAND`** (INT): Kodiertes Operation-Command-Wort (Inhalt des Zielregisters).

## Funktionsweise

Der Baustein hat keine echte Zustandsmaschine (kein `<ECC>` mit Übergangsbedingungen) — jedes Ereignis löst unabhängig genau einen Algorithmus aus:

1. **`INIT`**: Setzt `OPERATION_COMMAND := OC_NO_COMMAND`, bevor der erste `REQ`/`FAULT_RESET` eintrifft.
2. **`REQ`**: Ist `RUN = TRUE`, wird abhängig von `DIRECTION` `OC_FORWARD` oder `OC_REVERSE` kodiert; ist `RUN = FALSE`, wird `OC_DECEL_STOP` kodiert.
3. **`FAULT_RESET`**: Überschreibt `OPERATION_COMMAND` einmalig mit `OC_FAULT_RESET`, unabhängig vom aktuellen `RUN`/`DIRECTION`-Zustand — der nächste `REQ` kodiert danach wieder normal.

## Technische Besonderheiten

- **Alle Befehlscodes sind Parameter, kein festverdrahtetes Wissen.** Der Baustein selbst kennt keine antriebsspezifischen Zahlenwerte; `OC_*` werden vom Aufrufer über `Parameter` vorgegeben.
- **Umbenennung in Version 2.0**: Vormals `ENCODE_CONTROLWORD`/`CW_*`/`CONTROL_WORD`, da „Control Word“ CANopen-spezifische Terminologie ist. Semantik unverändert, nur Namen generalisiert.
- **Verallgemeinerung aus `H300_ENCODE_CONTROLWORD`** (Version 1.0): Ursprünglich für einen konkreten Antriebstyp geschrieben, dann auf das generische Run/Direction/Stop-plus-Fault-Reset-Muster verallgemeinert.

## Anwendungsszenarien

- Ansteuerung eines Frequenzumrichters/Antriebs über ein Feldbus-Steuerwort (Modbus-Holding-Register, CANopen PZD1, Profibus PPO), bei dem Run/Direction/Stop und ein expliziter Fault-Reset in einem einzigen Wort kodiert werden.
- Generischer Baustein für mehrere Antriebstypen: nur die `OC_*`-Parameter müssen pro Antrieb angepasst werden, die Kodierlogik bleibt gleich.

## Fazit

`ENCODE_OPERATION_COMMAND` kapselt die verbreitete Run/Direction/Stop-plus-Fault-Reset-Kodierung für VFD-Steuerworte in einem einzigen, herstellerneutralen Baustein — alle konkreten Befehlscodes kommen von außen, der Baustein selbst trägt kein antriebsspezifisches Wissen.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
