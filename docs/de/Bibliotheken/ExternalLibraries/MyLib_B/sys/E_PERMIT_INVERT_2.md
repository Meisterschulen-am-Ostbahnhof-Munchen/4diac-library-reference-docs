# E_PERMIT_INVERT_2


![E_PERMIT_INVERT_2_network](./E_PERMIT_INVERT_2_network.svg)

![E_PERMIT_INVERT_2](./E_PERMIT_INVERT_2.svg)

* * * * * * * * * *

## Einleitung

Bei `E_PERMIT_INVERT_2` handelt es sich um eine Subapplikation, die ein invertiertes Event-Freigabe-Gate mit zwei unabhängigen Ereigniskanälen realisiert. Der Baustein kombiniert einen IEC-61131-Not-Baustein `F_NOT_BOOL_INIT` mit einem IEC-61499-Event-Gate `E_PERMIT_2`.

Die äußere Freigabebedingung `PERMIT` wird vor der eigentlichen Ereignisweitergabe negiert. Dadurch ist der Baustein für Low-aktive Freigabesignale geeignet: Liegt `PERMIT` auf `FALSE`, werden Ereignisse durchgeschaltet; liegt `PERMIT` auf `TRUE`, werden Ereignisse blockiert.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Kommentar |
|---|---|---|
| `EI1` | Event | Ereignis-Eingangskanal 1 |
| `EI2` | Event | Ereignis-Eingangskanal 2 |

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
|---|---|---|
| `EO1` | Event | Ereignis-Ausgangskanal 1 |
| `EO2` | Event | Ereignis-Ausgangskanal 2 |

### **Daten-Eingänge**

| Name | Typ | Kommentar |
|---|---|---|
| `PERMIT` | BOOL | Invertierte Freigabebedingung |

### **Daten-Ausgänge**

Es sind keine Daten-Ausgänge vorhanden.

### **Adapter**

Es sind keine Adapter-Schnittstellen vorhanden.

## Funktionsweise

Die Subapplikation besitzt zwei Ereignispfade:

- `EI1` → `EO1`
- `EI2` → `EO2`

Beide Pfade werden durch das interne Gate `E_PERMIT_2` gesteuert. Der externe Eingang `PERMIT` wird zunächst durch den Baustein `F_NOT_BOOL_INIT` invertiert. Das Ergebnis dieser Negation wird an den Freigabeeingang `PERMIT` von `E_PERMIT_2` übergeben.

Dadurch gilt:

- Externer `PERMIT` = `FALSE`  
  Interner Freigabewert = `TRUE`  
  Ereignisse werden von `EI1` nach `EO1` und von `EI2` nach `EO2` weitergeleitet.

- Externer `PERMIT` = `TRUE`  
  Interner Freigabewert = `FALSE`  
  Ereignisse werden blockiert und es wird kein Ereignisausgang aktiviert.

## Technische Besonderheiten

- Es handelt sich um eine Subapplikation, nicht um einen direkt implementierten Funktionsblock.
- Der Baustein verwendet `F_NOT_BOOL_INIT` aus der IEC-61131-Bibliothek und `E_PERMIT_2` aus der IEC-61499-Ereignisbibliothek.
- Die beiden Ereigniskanäle sind getrennt geführt, werden aber gemeinsam über ein einziges Freigabesignal gesteuert.
- Der Baustein besitzt keine Datenausgänge und kein eigenes internes Zustandsgedächtnis.
- Durch die Integration der Negation in die Subapplikation wird eine extern sichtbare Freigabelogik mit invertierter Bedeutung erreicht.

## Zustandsübersicht

Da die Subapplikation kein eigenes Speicherverhalten besitzt, ist die folgende Tabelle als logische Wirkungskette zu verstehen:

| Externer `PERMIT` | Internes Gate `PERMIT` | Ereignisverhalten |
|---|---|---|
| `FALSE` | `TRUE` | `EI1` → `EO1`, `EI2` → `EO2` |
| `TRUE` | `FALSE` | Keine Ereignisweitergabe |

## Anwendungsszenarien

- Einsatz in Steuerungen, bei denen die Freigabe durch ein Low-aktives Signal erfolgt, zum Beispiel Sicherheitskontakte oder Schutztüren.
- Zweikanalige Ereignisweiterleitung mit einer gemeinsamen Sperrbedingung.
- Ersatz für eine externe Verdrahtung von `F_NOT_BOOL_INIT` und `E_PERMIT_2`, wodurch die Applikation übersichtlicher wird.

## Vergleich mit ähnlichen Bausteinen

Im Gegensatz zum Standardbaustein `E_PERMIT_2`, bei dem ein Ereignis bei `PERMIT = TRUE` durchgeschaltet wird, arbeitet `E_PERMIT_INVERT_2` mit negierter Freigabe. Die Funktion entspricht einer fest verdrahteten Kombination aus `F_NOT_BOOL_INIT` und `E_PERMIT_2`.

Vorteil der Subapplikation ist die Kapselung der invertierten Logik. Dadurch wird eine versehentliche falsche Verdrahtung vermieden und die Wiederverwendbarkeit erhöht.

## Fazit

`E_PERMIT_INVERT_2` ist ein kompakter, wiederverwendbarer Baustein für zweikanalige Ereignis-Freigaben mit invertierter Logik. Er kombiniert Standardbausteine zu einer klaren Schnittstelle und ermöglicht eine einfache Integration Low-aktiver Freigabesignale in IEC-61499-Anwendungen.
