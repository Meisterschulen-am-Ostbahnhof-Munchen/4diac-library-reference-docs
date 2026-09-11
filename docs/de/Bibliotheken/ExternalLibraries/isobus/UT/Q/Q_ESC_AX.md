# Q_ESC_AX

![Q_ESC_AX](./Q_ESC_AX.svg)

* * * * * * * * * *

## Einleitung

Der **Q_ESC_AX** ist ein AX‑Adapter‑Wrapper für den Baustein **Q_ESC** (ISO 11783‑6, Teil 6 – F.8). Er ermöglicht das Auslösen des ESC-Befehls (Abbruch der aktuellen Benutzereingabe auf dem VT) über eine unidirektionale AX‑Ereignisschnittstelle.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Kommentar |
|---|---|---|
| `INIT` | `EInit` | Service‑Initialisierung |

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
|---|---|---|
| `INITO` | `EInit` | Bestätigung der Initialisierung |
| `CNF` | `Event` | Bestätigung des angeforderten Dienstes |

### **Daten-Eingänge**

Keine Dateneingänge erforderlich – die Auslösung erfolgt über den AX-Adapter-Socket.

### **Daten-Ausgänge**

| Name | Typ | Kommentar |
|---|---|---|
| `STATUS` | `STRING` | Dienststatus |
| `s16result` | `INT` | Rückgabewert des Befehls |

### **Adapter**

| Typ | Name | Richtung | Kommentar |
|---|---|---|---|
| `adapter::types::unidirectional::AX` | `xReq` | Socket (Eingang) | Auslöser für ESC-Befehl |

## Funktionsweise

Der Baustein verbindet den inneren FB `Q_ESC` (*isobus::UT::Q::Q_ESC*) mit dem `AX`-Adapter `xReq`.
Beim Eintreffen eines Ereignisses am Socket `xReq.E1` wird das `REQ`‑Ereignis des internen FBs ausgelöst und der ESC-Befehl an das Terminal gesendet. Der Ausführungsstatus wird über `CNF`, `STATUS` und `s16result` zurückgemeldet.

## Technische Besonderheiten

- **Ereignisbasierter Abbruch:** Erlaubt das Abbrechen von VT-Eingabedialogen per AX-Adapter-Ereignis.
- **ISOBUS-Konformität:** Gemäß ISO 11783-6 F.8.

## Zustandsübersicht

Delegiert alle Aktionen an den inneren FB `Q_ESC`.

## Anwendungsszenarien

- Abbruch von Eingabedialogen über Taster, Fehlerzustände oder Ablaufsteuerungen per AX-Adapter.

## Vergleich mit ähnlichen Bausteinen

Erweitert `Q_ESC` um eine `AX`-Adapter-Schnittstelle zur direkten Anbindung an adapterbasierte Logik.

## Fazit

**Q_ESC_AX** ist ein kompakter AX-Adapter-Wrapper zur Ausführung des ISOBUS ESC-Befehls.
