# E_PERMIT_1

![E_PERMIT_1](E_PERMIT_1.svg)

* * * * * * * * * *

## Einleitung

Der `E_PERMIT_1` ist ein Funktionsbaustein zur **bedingten Weiterleitung eines Ereigniskanals** (generischer Baustein). Das Ereignis `EI1` wird nur dann an `EO1` weitergegeben, wenn die Freigabebedingung `PERMIT` erfüllt ist. Er ist die Variante mit einem Kanal der generischen `E_PERMIT`-Familie.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **EI1**: Ereigniseingang, Kanal 1. Verbundene Daten: `PERMIT`

### **Ereignis-Ausgänge**

- **EO1**: Ereignisausgang, Kanal 1

### **Daten-Eingänge**

- **PERMIT** (`BOOL`): Freigabebedingung

### **Daten-Ausgänge**

*Keine Daten-Ausgänge vorhanden*

### **Adapter**

Dieser Funktionsblock verwendet keine Adapter.

## Funktionsweise

Bei jedem `EI1`-Ereignis wird `PERMIT` ausgewertet. Ist `PERMIT = TRUE`, wird `EO1` ausgelöst; andernfalls wird das Ereignis unterdrückt.

## Technische Besonderheiten

- Generischer Baustein, der native Implementierung `GEN_E_PERMIT` (FORTE) benötigt.
- Reiner Ereignisfilter: Es werden keine Daten gespeichert oder ausgegeben.
- `PERMIT` ist mit `EI1` verbunden und wird daher bei jedem Ereignis neu gelesen.
- Siehe auch [`E_PERMIT`](E_PERMIT.md) (Standardvariante).

## Anwendungsszenarien

- Ereignisse nur bei aktiver Freigabe weiterleiten, z. B. Tastenereignisse nur im Automatikbetrieb
- Sperren von Ereignispfaden durch ein Statussignal

## Fazit

`E_PERMIT_1` leitet ein einzelnes Ereignis nur bei erfüllter `PERMIT`-Bedingung weiter.
