# INIT0

![INIT0](./INIT0.svg)

![INIT0_network](./INIT0_network.svg)

* * * * * * * * * *

## Einleitung

`INIT0` ist Stufe 0 der gestuften Initialisierung (Bibliothek `init_stages`, Paket `events::init`). Der Funktionsblock ist das Gegenstück zu einem [`E_TRIG`](../../../../StandardLibraries/events/E_TRIG.md) mit dem Ereignistyp `E0Init`: Der `E_TRIG` löst den unverbundenen Eingang `INIT0` aus, `INIT0O` wird an das `INIT` des Ziels angeschlossen. Sie läuft vor dem Standard-`INIT` (`EInit`). Der Baustein enthält keine Logik.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Kommentar |
| ---- | --- | --------- |
| INIT0 | E0Init | Initialisierungsanforderung |
| REQ | Event | Dienstanforderung |

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
| ---- | --- | --------- |
| INIT0O | E0Init | Initialisierungsbestätigung |
| CNF | Event | Bestätigung der Anforderung |

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

Keine.

## Funktionsweise

Reine Ereignisverbindungen im inneren FBNetzwerk:

- `INIT0` → `INIT0O`
- `REQ` → `CNF`

## Verwendung

1. `INIT0` direkt neben das Ziel setzen, das in dieser Stufe initialisiert werden soll.
2. `INIT0O` mit dem `INIT` des Ziels verbinden. Es muss nichts durch die Hierarchie verdrahtet werden.
3. In `EMB_RES` einen `E_TRIG` mit `EVENTTYPE` `'E0Init'` in die Kette der Stufen einhängen (siehe [`init_stages`](../../index.md)).

## Technische Besonderheiten

- Composite-Funktionsblock ohne Daten und ohne internen Zustand.
- Der Ereignistyp `E0Init` ist eigenständig; nur ein `E_TRIG` mit diesem `EVENTTYPE` löst den Eingang aus.
- Eine Stufe gilt als durch, wenn alle ihre Eingänge angestoßen sind, nicht, wenn die Ziele fertig initialisiert sind.
- Siehe auch [`INIT`](INIT.md) (Standardstufe, `EInit`).

## Fazit

`INIT0` verbindet Initialisierungsstufe 0 mit dem Ziel, ohne Verdrahtung durch die Hierarchie.
