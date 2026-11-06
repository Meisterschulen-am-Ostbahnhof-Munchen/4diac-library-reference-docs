# AI_LAST_2

![AI_LAST_2](AI_LAST_2.svg)

## Einleitung

Der Funktionsblock `AI_LAST_2` führt 2 gleichartige `INT`-Adapter-Signale (**IN1** bis **IN2**) auf einen gemeinsamen Ausgang (**OUT**) zusammen, nach dem Prinzip **Last Writer Wins**: Der Socket, der zuletzt ein Ereignis gesendet hat, gewinnt, und sein Wert wird sofort an OUT durchgereicht. Es ist eine reine Arbitrierung nach zeitlicher Reihenfolge, keine Verknüpfung der Werte.

## Schnittstellenstruktur

### **Adapter**

**Eingangsadapter (Sockets):**

- **IN1**: INT-Adapter (unidirectional)
- **IN2**: INT-Adapter (unidirectional)

**Ausgangsadapter (Plug):**

- **OUT**: INT-Adapter (unidirectional), zusammengeführtes Signal

## Funktionsweise

Der Socket mit dem letzten Ereignis gewinnt. OUT übernimmt sofort dessen Wert und sendet das Adapter-Ereignis, auch wenn der Wert gleich bleibt. Es gibt keine feste Priorität, nur die zeitliche Reihenfolge.

## Technische Besonderheiten

- Generischer Funktionsblock ohne ECC (`GenericClassName` = `GEN_AI_LAST`); das Verhalten ist in der Firmware (FORTE) umgesetzt, die Typbeschreibung enthält keinen Algorithmus. Bis Version 1.0 war `AI_LAST_2` ein Basic-FB mit ECC; Version 1.1 ist generisch, die Schnittstelle bleibt gleich.
- Schnittstelle: Sockets `IN1` und `IN2`, Plug `OUT`.
- Alle Größen `AI_LAST_2` bis `AI_LAST_9` haben dieselbe Funktion und unterscheiden sich nur in der Zahl der Sockets.
- Stand: Laut Entwicklung ist `AD_LAST_3` auf Hardware getestet (Trainer20, ESP32-S31); die übrigen Typen und Größen sind es nicht.

## Anwendungsszenarien

- Zusammenführen gleichwertiger Signalquellen zu einem nachgeschalteten Adapterpfad, zum Beispiel ein manueller und ein automatischer Sollwertgeber.
- Arbitrierung zwischen mehreren Bedienstellen, die denselben Wert schreiben dürfen.
- Vereinfachung von Netzwerken, die sonst `F_SEL`/`E_RS` inline verdrahten müssten.
