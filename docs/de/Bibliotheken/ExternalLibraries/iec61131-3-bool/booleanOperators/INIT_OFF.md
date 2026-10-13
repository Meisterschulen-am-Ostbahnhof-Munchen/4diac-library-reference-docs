# INIT_OFF

![INIT_OFF_network](./INIT_OFF_network.svg)

* * * * * * * * * *

## Einleitung

`INIT_OFF` ist das Gegenstück zu `E_TRIG`, erweitert um einen eigenständigen `INIT_OFF`/`INITO_OFF`-Pass-Through (Ereignistyp `EInitOff`). Der Baustein enthält keine Logik; jedes Eingangsereignis wird direkt an den zugehörigen Ausgang weitergereicht.

![INIT_OFF](INIT_OFF.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Kommentar |
| --- | --- | --- |
| INIT | EInit | Initialisierungsanforderung |
| INIT_OFF | EInitOff | Initialisierungsanforderung, eigener Kanal unabhängig von `INIT` |
| REQ | Event | Dienstanforderung |

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
| --- | --- | --- |
| INITO | EInit | Initialisierungsbestätigung |
| INITO_OFF | EInitOff | Initialisierungsbestätigung, eigener Kanal unabhängig von `INITO` |
| CNF | Event | Bestätigung der angeforderten Dienstanforderung |

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

Keine.

## Funktionsweise

Der Baustein besteht nur aus Ereignisverbindungen:

- `INIT` → `INITO`
- `REQ` → `CNF`
- `INIT_OFF` → `INITO_OFF`

## Technische Besonderheiten

- Reine Durchleitung ohne Logik und ohne Daten
- Der `INIT_OFF`-Kanal ist unabhängig vom `INIT`-Kanal

## Anwendungsszenarien

- Weiterreichen von Initialisierungs- und Abschaltereignissen (`EInitOff`) in einer Kette von Bausteinen

## ⚖️ Vergleich mit ähnlichen Bausteinen

- [INIT](./INIT.md) enthält nur `INIT`/`REQ` ohne den `INIT_OFF`-Kanal
- [INIT_ON_OFF](./INIT_ON_OFF.md) bietet zusätzlich einen `INIT_ON`/`INITO_ON`-Kanal

## Fazit

`INIT_OFF` reicht `INIT`, `INIT_OFF` und `REQ` unverändert als `INITO`, `INITO_OFF` und `CNF` weiter.
