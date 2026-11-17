# AUDI_TO_ATM_IN_S

![AUDI_TO_ATM_IN_S](AUDI_TO_ATM_IN_S.svg)

![AUDI_TO_ATM_IN_S_network](./AUDI_TO_ATM_IN_S_network.svg)

* * * * * * * * * *

!!! warning "Noch nicht freigegeben"
    Der Baustein ist als Typ in der Bibliothek definiert (Stand 2026-11-13). Die zugehörige Firmware-Seite (C++ und ELF-Bibliotheken) ist noch nicht freigegeben. Bitte noch nicht als lauffähig voraussetzen.

## Einleitung

`AUDI_TO_ATM_IN_S` wandelt einen `UDINT`-Wert (über den Adapter `AUDI`) in einen `TIME`-Wert (über den Adapter `ATM`) um: **TIME = IN × 1 S** (Einheit Sekunden). Er ist die Gegenrichtung zu [ATM_IN_S_TO_AUDI](ATM_IN_S_TO_AUDI.md).

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
| Socket | IN | adapter::types::unidirectional::AUDI | Zeitwert in S als UDINT |
| Plug | OUT | adapter::types::unidirectional::ATM | TIME-Wert = IN * 1 S |

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
- Der Eingangswert ist in Sekunden (Typ `UDINT`), das Ergebnis ein `TIME`.

## ⚖️ Vergleich mit ähnlichen Bausteinen

- Gleiche Einheit, anderer Eingangstyp: [ADI_TO_ATM_IN_S](ADI_TO_ATM_IN_S.md), [ALI_TO_ATM_IN_S](ALI_TO_ATM_IN_S.md), [ALR_TO_ATM_IN_S](ALR_TO_ATM_IN_S.md), [AULI_TO_ATM_IN_S](AULI_TO_ATM_IN_S.md).
- Gleicher Typ, andere Einheit: [AUDI_TO_ATM_IN_MS](AUDI_TO_ATM_IN_MS.md), [AUDI_TO_ATM_IN_NS](AUDI_TO_ATM_IN_NS.md), [AUDI_TO_ATM_IN_US](AUDI_TO_ATM_IN_US.md).
- Gegenrichtung: [ATM_IN_S_TO_AUDI](ATM_IN_S_TO_AUDI.md).
