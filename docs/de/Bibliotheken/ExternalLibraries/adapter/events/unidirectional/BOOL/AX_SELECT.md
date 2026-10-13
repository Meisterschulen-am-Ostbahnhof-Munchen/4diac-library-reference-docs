# AX_SELECT

![AX_SELECT_ecc](./AX_SELECT_ecc.svg)

* * * * * * * * * *

## Einleitung

Der AX_SELECT ist ein Funktionsblock, der basierend auf einem booleschen Eingang einen von zwei AX-Adapter-Eingängen auf den Ausgang schaltet.

![AX_SELECT](AX_SELECT.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **EI0**: Eingangsereignis, wird bei G = 0 ausgewählt.
- **EI1**: Eingangsereignis, wird bei G = 1 ausgewählt.

### **Ereignis-Ausgänge**

- **EO**: Ausgangsereignis.

### **Adapter**

**Sockets (Eingänge):**

- **G** (adapter::types::unidirectional::AX): Auswahlsignal. EI0 wird bei G = 0 ausgewählt, EI1 bei G = 1.

## Funktionsweise

Wenn G = 0, wird das Ereignis EI0 als EO ausgegeben.
Wenn G = 1, wird das Ereignis EI1 als EO ausgegeben.

## Technische Besonderheiten

- Verwendet unidirektionale Adapter.

## Zustandsübersicht

Kombinatorisch.

## Anwendungsszenarien

Auswahl von Signalen.

## ⚖️ Vergleich mit ähnlichen Bausteinen

- **E_SELECT**: Standard Ereignis-Select ohne Adapter.

## 🛠️ Zugehörige Übungen

- [Uebung_095_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_095_AX/)

## Fazit

Adapter-basierter Select-Baustein.
