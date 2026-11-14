# AULI_TO_ATM_IN_NS

![AULI_TO_ATM_IN_NS](AULI_TO_ATM_IN_NS.svg)

![AULI_TO_ATM_IN_NS_network](./AULI_TO_ATM_IN_NS_network.svg)

* * * * * * * * * *

!!! warning "Noch nicht freigegeben"
    Der Baustein ist als Typ in der Bibliothek definiert (Stand 2026-11-13). Die zugehörige Firmware-Seite (C++ und ELF-Bibliotheken) ist noch nicht freigegeben. Bitte noch nicht als lauffähig voraussetzen.

## Einleitung

`AULI_TO_ATM_IN_NS` wandelt einen `ULINT`-Wert (über den Adapter `AULI`) in einen `TIME`-Wert (über den Adapter `ATM`) um: **TIME = IN × 1 NS** (Einheit Nanosekunden). Er ist die Gegenrichtung zu [ATM_IN_NS_TO_AULI](ATM_IN_NS_TO_AULI.md).

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
| Socket | IN | adapter::types::unidirectional::AULI | Zeitwert in NS als ULINT |
| Plug | OUT | adapter::types::unidirectional::ATM | TIME-Wert = IN * 1 NS |

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
- Der Eingangswert ist in Nanosekunden (Typ `ULINT`), das Ergebnis ein `TIME`.

## ⚖️ Vergleich mit ähnlichen Bausteinen

- Gleiche Einheit, anderer Eingangstyp: [ADI_TO_ATM_IN_NS](ADI_TO_ATM_IN_NS.md), [ALI_TO_ATM_IN_NS](ALI_TO_ATM_IN_NS.md), [ALR_TO_ATM_IN_NS](ALR_TO_ATM_IN_NS.md), [AUDI_TO_ATM_IN_NS](AUDI_TO_ATM_IN_NS.md).
- Gleicher Typ, andere Einheit: [AULI_TO_ATM_IN_MS](AULI_TO_ATM_IN_MS.md), [AULI_TO_ATM_IN_S](AULI_TO_ATM_IN_S.md), [AULI_TO_ATM_IN_US](AULI_TO_ATM_IN_US.md).
- Gegenrichtung: [ATM_IN_NS_TO_AULI](ATM_IN_NS_TO_AULI.md).
