# ADI_TO_ATM_IN_US

![ADI_TO_ATM_IN_US](ADI_TO_ATM_IN_US.svg)

![ADI_TO_ATM_IN_US_network](./ADI_TO_ATM_IN_US_network.svg)

* * * * * * * * * *

!!! warning "Noch nicht freigegeben"
    Der Baustein ist als Typ in der Bibliothek definiert (Stand 2026-11-13). Die zugehörige Firmware-Seite (C++ und ELF-Bibliotheken) ist noch nicht freigegeben. Bitte noch nicht als lauffähig voraussetzen.

## Einleitung

`ADI_TO_ATM_IN_US` wandelt einen `DINT`-Wert (über den Adapter `ADI`) in einen `TIME`-Wert (über den Adapter `ATM`) um: **TIME = IN × 1 US** (Einheit Mikrosekunden). Er ist die Gegenrichtung zu [ATM_IN_US_TO_ADI](ATM_IN_US_TO_ADI.md).

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine direkten Ereignisse.

### **Ereignis-Ausgänge**

Keine direkten Ereignisse.

### **Daten-Eingänge**

Keine direkten Daten.

### **Daten-Ausgänge**

Keine direkten Daten.

### **Adapter**

| Rolle | Name | Typ | Beschreibung |
| --- | --- | --- | --- |
| Socket | IN | adapter::types::unidirectional::ADI | Zeitwert in US als DINT |
| Plug | OUT | adapter::types::unidirectional::ATM | TIME-Wert = IN * 1 US |

## Funktionsweise

Intern multipliziert `F_MULTIME` (`iec61131::arithmetic::F_MULTIME`) den Eingangswert mit einer Einheit. Ereignis und Wert laufen über die Adapter, die Verbindungen stehen im Abschnitt „Verbindungen im Baustein“.

## Verbindungen im Baustein

**Interne Bausteine:** `F_MULTIME` (`iec61131::arithmetic::F_MULTIME`)

**Ereignisverbindungen:**

- `IN.E1` → `F_MULTIME.REQ`
- `F_MULTIME.CNF` → `OUT.E1`

**Datenverbindungen:**

- `IN.D1` → `F_MULTIME.IN2`
- `F_MULTIME.OUT` → `OUT.D1`

## Technische Besonderheiten

- Composite mit einem internen Funktionsbaustein (`F_MULTIME`).
- Der Eingangswert ist in Mikrosekunden (Typ `DINT`), das Ergebnis ein `TIME`.

## ⚖️ Vergleich mit ähnlichen Bausteinen

- Gleiche Einheit, anderer Eingangstyp: [ALI_TO_ATM_IN_US](ALI_TO_ATM_IN_US.md), [ALR_TO_ATM_IN_US](ALR_TO_ATM_IN_US.md), [AUDI_TO_ATM_IN_US](AUDI_TO_ATM_IN_US.md), [AULI_TO_ATM_IN_US](AULI_TO_ATM_IN_US.md).
- Gleicher Typ, andere Einheit: [ADI_TO_ATM_IN_MS](ADI_TO_ATM_IN_MS.md), [ADI_TO_ATM_IN_NS](ADI_TO_ATM_IN_NS.md), [ADI_TO_ATM_IN_S](ADI_TO_ATM_IN_S.md).
- Gegenrichtung: [ATM_IN_US_TO_ADI](ATM_IN_US_TO_ADI.md).
