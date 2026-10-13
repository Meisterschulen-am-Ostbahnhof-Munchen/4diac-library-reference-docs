# SoftKey_Kurz_Lang

![SoftKey_Kurz_Lang_network](./SoftKey_Kurz_Lang_network.svg)

* * * * * * * * * *

## Einleitung

`SoftKey_Kurz_Lang` unterscheidet einen **kurzen** Tastendruck (Loslassen vor Ablauf von `PT`) von einem **langen** Tastendruck (`PT` gehalten) auf einem `AX`-Adapter-Eingang (z. B. `Softkey_IXA.IN`). `LANG` feuert sofort beim Erreichen von `PT` (nicht erst beim Loslassen); `KURZ` feuert beim Loslassen, wenn `PT` noch nicht erreicht war. `PT` ist konfigurierbar (Default 5 s).

![SoftKey_Kurz_Lang](SoftKey_Kurz_Lang.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

- **KURZ**: kurzer Druck bestätigt (Loslassen vor Ablauf `PT`)
- **LANG**: langer Druck bestätigt (`PT` erreicht, feuert sofort, nicht erst beim Loslassen)

### **Daten-Eingänge**

- **PT** (`TIME`): Haltezeit für "lang", Default `T#5s`

### **Daten-Ausgänge**

Keine.

### **Adapter**

- **IN** (Socket, `adapter::types::unidirectional::AX`): Rohzustand der Taste (z. B. `Softkey_IXA.IN`)

## Verwendete Funktionsbausteine (FBs)

- **`SPLIT_IN`** (`adapter::events::unidirectional::AX_SPLIT_3`): verteilt `IN` auf drei Ziele
- **`E_TON`** (`adapter::events::unidirectional::timers::AX_TON`): Einschaltverzögerung mit `PT`
- **`R_TRIG_LONG`** (`adapter::events::unidirectional::AX_R_TRIG`): steigende Flanke von `E_TON.Q`
- **`R_TRIG_PRESS`** (`adapter::events::unidirectional::AX_R_TRIG`): steigende Flanke von `IN` (Tastendruck-Beginn)
- **`LANG_LATCH`** (`adapter::events::unidirectional::AX_SR`): merkt, dass `LANG` in diesem Zyklus schon gefeuert hat
- **`F_TRIG_RELEASE`** (`adapter::events::unidirectional::AX_F_TRIG`): fallende Flanke von `IN` (Loslassen)
- **`PERMIT_KURZ`** (SubApp `MyLib::sys::AX_E_PERMIT_INVERT_1`, [AX_E_PERMIT_INVERT_1](./AX_E_PERMIT_INVERT_1.md)): lässt das Loslass-Ereignis nur durch, wenn die Freigabe FALSE ist

## Funktionsweise

- `IN` → `SPLIT_IN.IN`; `SPLIT_IN.OUT1` → `E_TON.IN`; `PT` → `E_TON.PT`
- `SPLIT_IN.OUT2` → `R_TRIG_PRESS.QI`; `SPLIT_IN.OUT3` → `F_TRIG_RELEASE.QI`
- `E_TON.Q` → `R_TRIG_LONG.QI`
- `R_TRIG_LONG.EO` → `LANG` und `LANG_LATCH.S`
- `R_TRIG_PRESS.EO` → `LANG_LATCH.R`
- `F_TRIG_RELEASE.EO` → `PERMIT_KURZ.EI`; `LANG_LATCH.Q` → `PERMIT_KURZ.PERMIT`; `PERMIT_KURZ.EO` → `KURZ`

`E_TON` läuft, solange `IN` gehalten wird. Die steigende Flanke von `E_TON.Q` (Zeitpunkt, an dem `PT` erreicht ist, während die Taste noch gehalten wird) löst `LANG` sofort aus und setzt `LANG_LATCH`. Beim Loslassen erkennt `F_TRIG_RELEASE` die fallende Flanke; `PERMIT_KURZ` lässt sie nur durch, wenn `LANG_LATCH.Q` FALSE ist, `LANG` also in diesem Druckzyklus noch nicht gefeuert wurde. Beim nächsten Tastendruck-Beginn (`R_TRIG_PRESS`) wird `LANG_LATCH` zurückgesetzt.

## Technische Besonderheiten

- **Bugfix gegenüber Version 1.0:** Die Prüfung darf nicht direkt auf `E_TON.Q` erfolgen. `E_TON.Q` wird durch denselben `IN`-Flankenwechsel (Loslassen), der auch `F_TRIG_RELEASE` auslöst, auf FALSE zurückgesetzt (`AX_TON`: kein `IN` → kein `Q`). Da beide Ziele vom selben `IN`-Ereignis gespeist werden, ist die Ausführungsreihenfolge zwischen ihnen nicht garantiert; `E_TON.Q` wäre bei der Prüfung praktisch immer schon FALSE, und `KURZ` hätte auch nach bereits gefeuertem `LANG` zusätzlich gefeuert.
- `LANG_LATCH` ist vom Reset-Timing komplett entkoppelt (gesetzt während des Haltens, zurückgesetzt erst beim nächsten Tastendruck) und macht die Prüfung robust.
- `PT` ist als Parameter konfigurierbar.

## Anwendungsszenarien

- Taste mit Doppelbelegung: kurzer Druck für eine Funktion, langer Druck (z. B. 5 s) für eine andere, zusammen mit [`Softkey_Web_IXA`](./Softkey_Web_IXA.md) für Tasten, die auch per Web-Client bedient werden sollen

## Zusammenfassung

`SoftKey_Kurz_Lang` liefert aus einem `AX`-Tastenzustand die getrennten Ereignisse `KURZ` und `LANG` mit konfigurierbarer Haltezeit.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
