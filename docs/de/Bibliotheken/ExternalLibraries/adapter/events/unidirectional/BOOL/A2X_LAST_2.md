# A2X_LAST_2

![A2X_LAST_2](A2X_LAST_2.svg)

## Einleitung

Der A2X_LAST_2 Funktionsblock führt zwei A2X-Adapter-Signale (**IN1**, **IN2**) auf einen gemeinsamen Ausgang (**OUT**) zusammen. Anders als beim einkanaligen [`AX_LAST_2`](AX_LAST_2.md) trägt A2X zwei unabhängige Kanäle (**UP** und **DOWN**), die deshalb auch unabhängig voneinander nach Last-Writer-Wins gemergt werden: welcher Socket zuletzt auf UP geschrieben hat, gewinnt für UP; welcher zuletzt auf DOWN geschrieben hat, gewinnt für DOWN - unabhängig davon, was auf dem jeweils anderen Kanal passiert.

## Schnittstellenstruktur

### **Adapter**

**Eingangsadapter (Sockets):**

- **IN1**: A2X-Adapter (unidirectional) - erste Signalquelle (UP + DOWN)
- **IN2**: A2X-Adapter (unidirectional) - zweite Signalquelle (UP + DOWN)

**Ausgangsadapter (Plug):**

- **OUT**: A2X-Adapter (unidirectional) - zusammengeführtes Signal (UP + DOWN)

## Funktionsweise

Der Baustein ist als Basic-FB mit einer 5-Zustands-ECC umgesetzt (START, PASS1_UP, PASS2_UP, PASS1_DOWN, PASS2_DOWN), die beide Kanäle über dieselbe ECC bedient - da pro FB-Aufruf immer nur ein Ereignis verarbeitet wird, kollidieren UP- und DOWN-Verarbeitung nie miteinander. Trifft an IN1.UP oder IN2.UP ein Ereignis ein, wird der jeweilige UP-Wert nach OUT.UP kopiert und OUT.E_UP gesendet. Symmetrisch dazu wird bei IN1.DOWN/IN2.DOWN der DOWN-Wert nach OUT.DOWN kopiert und OUT.E_DOWN gesendet. Die beiden Kanäle haben also unabhängige "Gewinner" - IN1 kann z. B. gerade den UP-Kanal dominieren, während IN2 den DOWN-Kanal dominiert.

## Technische Besonderheiten

- Zwei unabhängige Last-Writer-Wins-Merges (UP und DOWN) in einem Baustein
- Erweiterung des generischen einkanaligen LAST_2-Musters auf den 2-Kanal-A2X-Adaptertyp
- Eine gemeinsame ECC für beide Kanäle, da immer nur ein Ereignis pro Aufruf verarbeitet wird - kein Zustandskonflikt möglich
- Keine Signalverzögerung: der Wert wird im selben Zyklus durchgereicht, in dem das auslösende Ereignis eintrifft

## Anwendungsszenarien

- Zusammenführen zweier gleichwertiger A2X-Signalquellen (z. B. zwei Bedienstellen für eine Auf/Ab-Steuerung) zu einem einzigen nachgeschalteten Adapterpfad
- Arbitrierung zwischen zwei unabhängigen Auf/Ab-Gebern, bei denen UP und DOWN aus unterschiedlichen Quellen kommen dürfen

## ⚖️ Vergleich mit ähnlichen Bausteinen

A2X_LAST_2 ist die zweikanalige Variante des generischen [`AX_LAST_2`](AX_LAST_2.md)-Musters, angewendet auf den A2X-Adaptertyp mit seinen zwei unabhängigen UP/DOWN-Kanälen. Für Mischbetrieb zwischen einem Daten- und einem reinen Ereignisadapter ist [`AX_AE_MERGE`](AX_AE_MERGE.md) gedacht.
