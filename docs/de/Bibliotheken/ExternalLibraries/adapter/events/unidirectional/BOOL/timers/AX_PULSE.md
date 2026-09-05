# AX_PULSE

* * * * * * * * * *

## Einleitung

**Wichtiger Hinweis: Dieser Baustein benötigt nur ein Ereignis (Event) und keine zyklischen Aufrufe. Er besitzt keinen Ausgang ET und zeigt die verstrichene Zeit nicht an.**

Der AX_PULSE ist ein Funktionsblock, der einen Impuls über einen AX-Adapter ausgibt.

![AX_PULSE](AX_PULSE.svg)

## Schnittstellenstruktur

### **Adapter**

**Sockets (Eingänge):**

- **REQ** (adapter::types::unidirectional::AX): Trigger.

**Plugs (Ausgänge):**

- **PULSE** (adapter::types::unidirectional::AX): Impulsausgang.

## Funktionsweise

Bei REQ wird PULSE kurzzeitig aktiv.

## Technische Besonderheiten

- Verwendet unidirektionale Adapter.

## Zustandsübersicht

Impuls.

## Anwendungsszenarien

Signalisierung.

## ⚖️ Vergleich mit ähnlichen Bausteinen

- **E_PULSE**

## 🛠️ Zugehörige Übungen

- [Uebung_020h_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-en/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020h_AX/)
- [Uebung_020i_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-en/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020i_AX/)

## Fazit

Adapter-basierter Impuls-Baustein.
