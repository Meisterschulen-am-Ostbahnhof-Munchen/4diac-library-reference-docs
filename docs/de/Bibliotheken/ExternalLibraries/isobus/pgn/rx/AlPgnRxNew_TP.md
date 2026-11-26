# AlPgnRxNew_TP

![AlPgnRxNew_TP](./AlPgnRxNew_TP.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: Receive a PGN of any length (also via transport protocol) into a byte array

Paket: `isobus::pgn::rx`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **INIT** (Typ: `EInit`): Service Initialization
-   **install** (Typ: `Event`): installs a Receive PGN (RX) (mit Daten: `u32Pgn`, `NmSource`, `u8Priority`, `Data`)
-   **CHG** (Typ: `Event`): Change source to NmSource.s16Handle (mit Daten: `NmSource`)

### **Ereignis-Ausgänge**

-   **INITO** (Typ: `EInit`): Initialization Confirm
-   **installO** (Typ: `Event`): install Done (mit Daten: `PGN_handle`)
-   **IND** (Typ: `Event`): Data was received (mit Daten: `s32TimeStamp`, `u16DaSize`, `u8RxPriority`, `OVERFLOW`, `Data`)
-   **dataERR** (Typ: `Event`) (mit Daten: `dataERRC`)
-   **pgnERR** (Typ: `Event`) (mit Daten: `pgnERRC`)

### **Daten-Eingänge**

-   **u32Pgn** (Typ: `UDINT`): PGN - Range: 0 - 0x3FFFF
-   **NmSource** (Typ: `isobus::pgn::ISONETEVENT_T`): Define the Communication Partner
-   **u8Priority** (Typ: `USINT`): Priority given to the stack (0..7), not used as receive filter; received priority see u8RxPriority (Vorgabewert: `7`)

### **Daten-Ausgänge**

-   **PGN_handle** (Typ: `INT`): PGN-handle on success, HANDLE_UNVALID in case of an error
-   **dataERRC** (Typ: `INT`): dataERR error Code
-   **pgnERRC** (Typ: `INT`): pgnERR error Code
-   **s32TimeStamp** (Typ: `DINT`): Time stamp of the message in ms (Vorgabewert: `-1`)
-   **u16DaSize** (Typ: `UINT`): Received data length in bytes (0..65535)
-   **u8RxPriority** (Typ: `USINT`): Priority of the received PGN
-   **OVERFLOW** (Typ: `BOOL`): TRUE if more bytes were received than Data holds (Data is cut)

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

Keine.

## Extended Transport Protocol (ETP)

!!! warning "Stand: noch nicht getestet"
    Die folgenden Angaben stammen von der Firmware-Entwicklung (LOGIBUS-Commit `24534469`, forte-Commit `8d02d14f7`). Beide Commits waren zum Zeitpunkt der Doku nicht gepusht und ich konnte sie nicht prüfen. Auf Hardware ist nichts getestet.

- **Größe:** Der Baustein übernimmt Nachrichten bis 65535 Byte (vorher 1785). `u16DaSize` gilt von 0 bis 65535. `OVERFLOW` zeigt an, dass `Data` (zum Beispiel ein `PROVIDE_ARR_*`) zu klein war.
- **ETP nur adressiert:** ETP gibt es nur an ein bestimmtes Ziel, eine Rundsendung ist nicht möglich.
- **LOGIBUS:** Bisher verwarf der Standard-TP-Filter des CCI-Treibers jede ETP. LOGIBUS setzt jetzt einen eigenen Filter, der TP und ETP bis 128 KiB annimmt (`LOGIBUS_ETP_IN_SIZE_MAX`). Größere ETPs bricht der Stack mit „System resources“ ab. Der Empfangspuffer kommt aus dem PSRAM und wird nach der Verteilung der Daten freigegeben, auch nach einem Abbruch.
- **PGN-Bereich:** PGNs von `0x1F000` bis `0x1FFFF` mit mehr als 8 Byte gehen als NMEA Fast Packet (höchstens 223 Byte), nicht als ETP. Für ETP eignen sich Proprietary A2 (`0x1EF00`) oder Proprietary A (`0xEF00`).
- Siehe auch [`AlPgnTxNew_TP`](../tx/AlPgnTxNew_TP.md), [`DlPgnSend`](../tx/DlPgnSend.md) und die Seite NMEA 2000 Fast Packet.

Hinweis: Diese Seite wurde aus der Typdefinition (`AlPgnRxNew_TP.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.
