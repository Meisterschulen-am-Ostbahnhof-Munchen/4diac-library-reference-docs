# INIT2

![INIT2](./INIT2.svg)

![INIT2_network](./INIT2_network.svg)

* * * * * * * * * *

## Einleitung

`INIT2` ist Stufe 2 der gestuften Initialisierung (Bibliothek `init_stages`, Paket `events::init`). Der Funktionsblock ist das Gegenstück zu einem [`E_TRIG`](../../../../StandardLibraries/events/E_TRIG.md) mit dem Ereignistyp `E2Init`: Der `E_TRIG` löst den unverbundenen Eingang `INIT2` aus, `INIT2O` wird an das `INIT` des Ziels angeschlossen. Sie läuft nach dem Standard-`INIT` (`EInit`). Der Baustein enthält keine Logik.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Kommentar |
| ---- | --- | --------- |
| INIT2 | E2Init | Initialisierungsanforderung |
| REQ | Event | Dienstanforderung |

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
| ---- | --- | --------- |
| INIT2O | E2Init | Initialisierungsbestätigung |
| CNF | Event | Bestätigung der Anforderung |

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

Keine.

## Funktionsweise

Reine Ereignisverbindungen im inneren FBNetzwerk:

- `INIT2` → `INIT2O`
- `REQ` → `CNF`

## Verwendung

1. `INIT2` direkt neben das Ziel setzen, das in dieser Stufe initialisiert werden soll.
2. `INIT2O` mit dem `INIT` des Ziels verbinden. Es muss nichts durch die Hierarchie verdrahtet werden.
3. In `EMB_RES` einen `E_TRIG` mit `EVENTTYPE` `'E2Init'` in die Kette der Stufen einhängen (siehe [`init_stages`](../../index.md)).

## Technische Besonderheiten

- Composite-Funktionsblock ohne Daten und ohne internen Zustand.
- Der Ereignistyp `E2Init` ist eigenständig; nur ein `E_TRIG` mit diesem `EVENTTYPE` löst den Eingang aus.
- Eine Stufe gilt als durch, wenn alle ihre Eingänge angestoßen sind, nicht, wenn die Ziele fertig initialisiert sind.
- Siehe auch [`INIT`](INIT.md) (Standardstufe, `EInit`).

## Fazit

`INIT2` verbindet Initialisierungsstufe 2 mit dem Ziel, ohne Verdrahtung durch die Hierarchie.
