# Q_BackgroundColour_EnableOk_A

![Q_BackgroundColour_EnableOk_A_network](./Q_BackgroundColour_EnableOk_A_network.svg)

* * * * * * * * * *

## Einleitung

`Q_BackgroundColour_EnableOk_A` leitet aus zwei Booleschen Werten (Kanal aktiviert und Hardware-`QO`) eine 3-Farben-VT-Hintergrundfarbe ab: **Weiß** = deaktiviert, **Grün** = aktiviert und `QO` TRUE, **Rot** = aktiviert und `QO` FALSE (Störung). Generisch für jeden Kanal mit Enable-Schalter und Status-LED (PWM, PI, …).

![Q_BackgroundColour_EnableOk_A](Q_BackgroundColour_EnableOk_A.svg)

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: Q_BackgroundColour_EnableOk_A

- **Typ**: SubAppType
- **Verwendete interne Bausteine**:
    - **Select_EnableOk_Colour**: SubApp [`MyLib::sys::Select_EnableOk_a`](./Select_EnableOk_a.md) mit `valDisabled=COLOR_WHITE`, `valOk=COLOR_GREEN`, `valFault=COLOR_RED` – wählt anhand von `bEnable` und `bOk` eine der drei Farben
    - **Q_BackgroundColour**: `isobus::UT::Q::Q_BackgroundColour` – schreibt die gewählte Farbe auf das VT-Objekt `u16ObjId`
- **Importe**: `COLOR_GREEN`, `COLOR_RED`, `COLOR_WHITE`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialisierungsanforderung (an `Q_BackgroundColour.INIT`)
- **REQ**: Neu auswerten – vom Aufrufer einmal pro möglicher Änderung von `bEnable` oder `bOk` gefeuert (Schalter getoggelt oder frisches `QO` eingetroffen); beide Werte werden zusammen mitgegeben

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierungsbestätigung (von `Q_BackgroundColour.INITO`)
- **CNF**: Farbe gesendet (Durchreichung von `Q_BackgroundColour.CNF`)

### **Daten-Eingänge**

- **QI** (`BOOL`, Vorgabe `TRUE`): Quality Input - TRUE aktiviert den Dienst
- **u16ObjId** (`UINT`): Object-ID der Status-Anzeige (`CButton`, nur Hintergrundfarbe, kein Text) – einmalig bei der Instanziierung per Parameter gesetzt
- **bEnable** (`BOOL`): Kanal aktiviert (z. B. Enable-Flipflop `Q`)
- **bOk** (`BOOL`): Hardware bestätigt aktiv (z. B. `logiBUS_..._IDA.QO`) – nur relevant, wenn `bEnable=TRUE`

### **Daten-Ausgänge**

- **QO** (`BOOL`): Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde
- **STATUS** (`STRING`): Dienststatus (Durchreichung von `Q_BackgroundColour.STATUS`)
- **s16result** (`INT`): Rückgabewert (Durchreichung von `Q_BackgroundColour.s16result`)

## Programmablauf und Verbindungen

- `REQ` → `Select_EnableOk_Colour.REQ`; `Select_EnableOk_Colour.CNF` → `Q_BackgroundColour.REQ`; `Q_BackgroundColour.CNF` → `CNF`
- `bEnable` → `Select_EnableOk_Colour.bEnable`; `bOk` → `Select_EnableOk_Colour.bOk`
- `Select_EnableOk_Colour.value` → `Q_BackgroundColour.u8Colour`
- `u16ObjId` → `Q_BackgroundColour.u16ObjId`
- `Q_BackgroundColour.STATUS` → `STATUS`; `Q_BackgroundColour.s16result` → `s16result`
- `INIT` → `Q_BackgroundColour.INIT`; `Q_BackgroundColour.INITO` → `INITO`
- `QI` → `Q_BackgroundColour.QI`; `Q_BackgroundColour.QO` → `QO`

## Technische Besonderheiten

- Die reine Logik „2 Bool → 1 von 3 Werten" ist in `MyLib::sys::Select_EnableOk` ausgelagert (generisch, nicht auf Farben beschränkt); dieser Baustein instanziiert sie nur mit den drei Farben als Parameter
- Variante mit AX-Adaptern: [`Q_BackgroundColour_EnableOk_AX`](./Q_BackgroundColour_EnableOk_AX.md)
- Version 1.1 (2026-08-31)

## Zustandsübersicht

| `bEnable` | `bOk` | Farbe |
| :--- | :--- | :--- |
| FALSE | beliebig | Weiß (`COLOR_WHITE`) |
| TRUE | TRUE | Grün (`COLOR_GREEN`) |
| TRUE | FALSE | Rot (`COLOR_RED`) |

## Anwendungsszenarien

- Status-Anzeige eines Kanals mit Enable-Schalter und Hardware-Rückmeldung (PWM, PI, …)

## Fazit

`Q_BackgroundColour_EnableOk_A` setzt die Hintergrundfarbe eines VT-Objekts nach dem Schema Weiß/Grün/Rot aus `bEnable` und `bOk`.
