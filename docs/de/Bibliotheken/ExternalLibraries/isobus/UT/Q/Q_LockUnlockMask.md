# Q_LockUnlockMask

![Q_LockUnlockMask](https://user-images.githubusercontent.com/116869307/214148004-903a6233-7e3e-43eb-a611-03d82d451bf4.png)

* * * * * * * * * *

## Einleitung

Der **Q_LockUnlockMask** ist ein standardkonformer Funktionsbaustein zur Steuerung des Sperrzustands von Masken in Virtual Terminals, entwickelt unter EPL-2.0 Lizenz. Die Version 1.0 implementiert die ISO 11783-6 (Teil 6 - F.46) Spezifikation für VT-Systeme ab Version 4.

![Q_LockUnlockMask](Q_LockUnlockMask.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- `INIT`: Initialisierungsanforderung (mit Masken-Objekt-ID `u16MaskId`)
- `REQ`: Sperr-/Entsperr-Anforderung (mit Sperrkommando und Timeout)

### **Ereignis-Ausgänge**

- `INITO`: Initialisierungsbestätigung
- `CNF`: Betriebsbestätigung

### **Daten-Eingänge**

- `u16MaskId` (UINT): Masken-Objekt-ID (bei `INIT` übergeben)
- `u8LockCmd` (USINT): Sperrbefehl (0=Entsperren, 1=Sperren)
- `u16LockTimeoutMs` (UINT): Timeout in ms (0=kein Timeout)

### **Daten-Ausgänge**

- `STATUS` (STRING): Betriebsstatusmeldung
- `u8OldLockCmd` (USINT): Vorheriger Sperrzustand
- `u16OldLockTimeoutMs` (UINT): Vorheriger Timeout
- `s16result` (INT): ISO-konformer Ergebniscode

## Instanz-Eindeutigkeit (Instance Uniqueness)

Dieser Baustein erfordert Instanz-Eindeutigkeit bezüglich **u16MaskId**. Es darf im gesamten Programm nur eine Instanz von `Q_LockUnlockMask` für dieselbe Masken-ID existieren. `u16MaskId` wird bei `INIT` eingelesen — eine zweite Instanz auf dieselbe Masken-ID wird bei `INIT` deaktiviert (STATUS = "This objID is already in use"). Siehe auch [Instanz-Eindeutigkeit](./INSTANZ_EINDEUTIGKEIT.md).

## Gültige Objekt-IDs

`u16MaskId` adressiert die zu sperrende/entsperrende Maske (F.46). Gültig sind:

**DataMask (1000–1999)** und **WindowMask / User-Layout Data Mask (34000–34999)**.

ID_NULL (65535) ist kein gültiges Kommandoziel — das Kommando wird vom VT mit einem Fehlercode beantwortet (die Maske muss der aktuell sichtbaren Maske entsprechen).

## Funktionsweise

1. **Initialisierung**:
   - `INIT` mit `u16MaskId`
   - `INITO` bestätigt Betriebsbereitschaft

2. **Maskensperrung**:
   - `REQ` mit Sperrkommando und Timeout
   - Steuert die Bildschirmaktualisierung der Maske
   - `CNF` liefert Betriebsstatus und vorherigen Sperrzustand/Timeout

3. **Timeout-Verhalten**:
   - Automatische Entsperrung nach Ablauf
