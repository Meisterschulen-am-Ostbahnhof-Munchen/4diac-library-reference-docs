# AOption_T_2

![AOption_T_2](./AOption_T_2.svg)

![AOption_T_2_network](./AOption_T_2_network.svg)

* * * * * * * * * *

## Einleitung

`AOption_T_2` ist ein Composite-FB, der als sequenzielles **T-Stück** für den Options-Adapter [`AOption`](../AOption.md) dient: Er nimmt über den Socket `IN` eine einzelne Optionsanforderung entgegen und aktiviert daraus zwei nachgeschaltete Options-Verbraucher (Plugs `OUT2` und `OUT1`) **nacheinander**, nicht parallel. Erst wird `OUT2` angesprochen, und erst nach dessen `INIT_CNF` folgt `OUT1`. `IN` wird erst bestätigt, wenn beide Ausgänge bestätigt haben.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine vorhanden (die Ereignisse laufen über die Adapter).

### **Ereignis-Ausgänge**

Keine vorhanden.

### **Daten-Eingänge**

Keine vorhanden.

### **Daten-Ausgänge**

Keine vorhanden.

### **Adapter**

| Adapter | Typ | Richtung | Kommentar |
|---------|-----|----------|-----------|
| `IN` | `Option::AOption` | Socket | Von einem AOption-Ausgang (z. B. `DEMUX.OUTn`) gespeist. |
| `OUT2` | `Option::AOption` | Plug | Wird zuerst aktiviert (`INIT_REQ` mit `QI_OUT=TRUE`) bzw. zuletzt deaktiviert (`QI_OUT=FALSE`). |
| `OUT1` | `Option::AOption` | Plug | Wird erst nach `OUT2.INIT_CNF` aktiviert bzw. zuerst deaktiviert; kann zum Kaskadieren an den Socket `IN` eines weiteren `AOption_T_2` angeschlossen werden. |

## Funktionsweise

Das interne Netzwerk besteht nur aus Verbindungen (keine weiteren FBs, keine Algorithmen, kein ECC):

Ereignisse:

- `IN.INIT_REQ` -> `OUT2.INIT_REQ`
- `OUT2.INIT_CNF` -> `OUT1.INIT_REQ`
- `OUT1.INIT_CNF` -> `IN.INIT_CNF`

Daten:

- `IN.QI_OUT` -> `OUT2.QI_OUT`
- `IN.QI_OUT` -> `OUT1.QI_OUT`

Eine Anforderung an `IN` löst also die Kette `IN.INIT_REQ` -> `OUT2` -> (wartet auf `OUT2.INIT_CNF`) -> `OUT1` -> (wartet auf `OUT1.INIT_CNF`) -> `IN.INIT_CNF` aus. Die Deaktivierung läuft über dieselbe Verdrahtung in derselben Reihenfolge; `QI_OUT` trägt dann `FALSE` statt `TRUE`. Die Ereigniskette selbst unterscheidet nicht zwischen Aktivierung und Deaktivierung.

## Technische Besonderheiten

- **Sequenziell statt parallel:** Die `*_SPLIT_2`-Bausteine (z. B. `AX_SPLIT_2`, `AR_SPLIT_2`) geben ein Ereignis bzw. einen Wert parallel an mehrere Ausgänge weiter. `AOption_T_2` serialisiert die Aktivierung bewusst, damit eine Gruppe mit Abhängigkeit zu einer vorher aktivierten Gruppe erst startet, wenn diese tatsächlich fertig ist.
- **Kaskadierbar:** `OUT1` kann an den `IN`-Socket eines weiteren `AOption_T_2` angeschlossen werden, so lassen sich beliebig viele Gruppen streng nacheinander hochfahren, ohne dass die einzelne Optionsgruppe selbst einen Weiterleitungs-Plug mitbringen muss (vgl. den Daisy-Chain-Stil in [`OptionalGroup_001a`](../../MyLib_AX_Option/sys/OptionalGroup_001a.md)).
- **Composite statt ECC-BasicFB:** Eine eigene Zustandslogik ist nicht nötig, die reine Verkettung der Ereignisse erzwingt bereits die Reihenfolge.

## Anwendungsszenarien

- Eine Optionsanforderung (z. B. aus einem `AOption`-Demultiplexer) soll zwei Optionsgruppen nacheinander aktivieren, wobei die zweite erst nach Abschluss der ersten starten darf.
- Mehrere kaskadierte `AOption_T_2` für längere Ketten von Optionsgruppen.

## Zusammenfassung

`AOption_T_2` verteilt eine `AOption`-Anforderung ohne eigene Logik sequenziell auf zwei Ausgänge (`OUT2`, dann `OUT1`) und bestätigt `IN` erst nach beiden Bestätigungen.

## 🛠️ Zugehörige Übungen

- [Uebung_094b3d_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094b3d_AX/)

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
