# PositionMarkerFS

![PositionMarkerFS](./PositionMarkerFS.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **PositionMarkerFS** dient zur Positionierung eines Markierobjekts (z. B. eines Dreiecks) innerhalb eines Virtual-Terminal-Displays gemäß ISO 11783-6. Er akzeptiert einen beliebigen physikalischen REAL-Wert (z. B. von einem Sensor oder einer Eingabe), versetzt diesen um einen vorgegebenen Mittenoffset, begrenzt ihn auf den zulässigen Bewegungsbereich und überträgt die resultierende Integer-Position an eine interne Instanz von `Q_ChildPosition`. Dadurch kann ein Markerobjekt kontinuierlich den aktuellen Messwert visualisieren, ohne dass der Anwender sich um die zugrunde liegende Skalierung oder Begrenzung kümmern muss.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Datentyp | Kommentar |
|----------|----------|-----------|
| `INIT` | `EInit` | Service Initialization – übernimmt die Objekteigenschaften und das Skalierungs-Flag. |
| `REQ` | `Event` | Fordert die Bewegung des Markers auf Basis eines neuen Wertes an. |

### **Ereignis-Ausgänge**

| Ereignis | Datentyp | Kommentar |
|----------|----------|-----------|
| `INITO` | `EInit` | Initialisierungsbestätigung. |
| `CNF` | `Event` | Bestätigung des angeforderten Dienstes; liefert Status, Ergebnis und Über-/Unterbereichsflags. |

### **Daten-Eingänge**

| Name | Datentyp | Kommentar |
|------|----------|-----------|
| `stObj` | `isobus::utils::childposition::PositionMarker_S` | Struktur mit Markerobjekt-Eigenschaften (Kind‑/Parent‑ID, Bereichsgrenzen, Mittenoffset, Y‑Position). Wird bei `INIT` einmalig gespeichert. |
| `xScale` | `BOOL` | Skalierungsflag: `FALSE` (Standard) sendet die Positionen unverändert, `TRUE` skaliert gemäß dem Faktor der Parent‑ID. Wird direkt an die interne `Q_ChildPosition` durchgereicht. |
| `rValue` | `REAL` | Physikalischer Wert, der angezeigt werden soll – aus beliebiger Quelle (z. B. NumericValue_PHYS, Sensor). |

### **Daten-Ausgänge**

| Name | Datentyp | Kommentar |
|------|----------|-----------|
| `STATUS` | `STRING` | Dienststatus – direkt von der internen `Q_ChildPosition` übernommen. |
| `s16result` | `INT` | Rückgabewert der internen `Q_ChildPosition` (siehe dort). |
| `xOver` | `BOOL` | Wird `TRUE`, wenn der (um `r32Center` versetzte) Wert `r32MaxPos` überschritten und begrenzt wurde. |
| `xUnder` | `BOOL` | Wird `TRUE`, wenn der (um `r32Center` versetzte) Wert unter `r32MinPos` liegt und begrenzt wurde. |

### **Adapter**

Keine Adapter vorhanden.

## Funktionsweise

Der FB arbeitet in zwei Phasen:

1. **Initialisierung (`INIT`)**: Das übergebene `stObj` – bestehend aus Kind‑/Parent‑ID, den Bewegungsgrenzen `r32MinPos`/`r32MaxPos`, dem Mittenoffset `r32Center` und der festen Y‑Position – wird über einen internen `F_MOVE`-Baustein in einem Zwischenspeicher abgelegt. Anschließend wird die interne `Q_ChildPosition` mit diesen Eigenschaften initialisiert. Das Flag `xScale` wird unverändert übergeben.

2. **Positionierung (`REQ`)**: Beim Eintreffen eines neuen `REQ` wird der Eingangswert `rValue` zunächst um `r32Center` addiert (mittels `F_ADD`). Danach wird der resultierende Wert mithilfe des Bausteins `F_ClampReal` auf den Bereich `[r32MinPos, r32MaxPos]` begrenzt. Überschreitet der Wert den oberen Grenzwert, wird `xOver` gesetzt; unterschreitet er den unteren Grenzwert, wird `xUnder` gesetzt. Der begrenzte REAL-Wert wird anschließend über `F_REAL_TO_INT` in einen Integer konvertiert und als `s16Xposition` an die `Q_ChildPosition` übergeben. Die zuvor gespeicherte Y‑Position (`s16YPosition`) wird unverändert durchgereicht. Nach Abschluss der Bewegung wird das Ereignis `CNF` mit den entsprechenden Ausgabedaten ausgelöst.

## Technische Besonderheiten

- **Snapshot-Verhalten**: Die Objektstruktur `stObj` wird nur bei `INIT` einmalig kopiert. Änderungen an den Parametern nach der Initialisierung haben keinen Einfluss auf die laufende Positionierung.
- **Automatische Bereichsbegrenzung**: Der Eingangswert wird vor der Konvertierung immer auf den gültigen Bereich geclamped. Die Flags `xOver`/`xUnder` signalisieren, ob der Ursprungswert außerhalb des Bereichs lag.
- **Durchreichen von Skalierungs- und Statusinformationen**: Das Flag `xScale` sowie die Ausgaben `STATUS` und `s16result` werden direkt von der internen `Q_ChildPosition` übernommen, ohne weitere Verarbeitung.
- **Keine Y‑Berechnung**: Die Y‑Position wird nicht dynamisch verändert; sie wird als fester Wert aus der Struktur übernommen.

## Zustandsübersicht

Der Funktionsblock besitzt keine expliziten Zustandsautomaten, sondern arbeitet ereignisgesteuert. Der Ablauf lässt sich jedoch wie folgt beschreiben:

- **Idle-Zustand**: Nach erfolgreicher Initialisierung (`INITO`) wartet der FB auf ein `REQ`-Ereignis.
- **Verarbeitungsschritt**: Bei `REQ` werden nacheinander die Bausteine `F_ADD`, `F_ClampReal`, `F_REAL_TO_INT` und `Q_ChildPosition` durchlaufen.
- **Bestätigungsphase**: Nach Abschluss der internen Verarbeitung wird `CNF` mit den aktuellen Werten von `STATUS`, `s16result`, `xOver` und `xUnder` ausgegeben.

## Anwendungsszenarien

- **Anzeige von Sensormesswerten** auf einem ISOBUS‑VT, z. B. Füllstand, Temperatur oder Druck, wobei der Wert direkt als REAL ansteht.
- **Steuerung von Markerpositionen** in grafischen Benutzeroberflächen, bei denen der Wertebereich des Messwerts vom Anzeigebereich abweicht.
- **Einbindung in komplexere Visualisierungslogik**, bei der mehrere Markerobjekte verschiedene Werte parallel darstellen, ohne dass für jedes Objekt separate Clamping‑Logik erforderlich ist.

## Vergleich mit ähnlichen Bausteinen

| Baustein | Unterschied zu `PositionMarkerFS` |
|----------|-----------------------------------|
| `Q_NumericValue_PHYS` | Stellt eine numerische Anzeige dar, verwendet aber typischerweise eine feste Skalierung und keinen Offset; `PositionMarkerFS` integriert Offset, Clamping und Konvertierung in einem Baustein. |
| `ScrollFS` | Ähnliches Muster (Snapshot bei INIT), aber fokussiert auf Scroll‑Operationen; `PositionMarkerFS` spezialisiert sich auf die Positionierung eines Markers mit Wertebereichsanpassung. |
| Direkte Verwendung von `Q_ChildPosition` | Erfordert manuelles Umrechnen, Clamping und Integer‑Konvertierung durch den Anwender; `PositionMarkerFS` kapselt diese Schritte. |

## Fazit

Der Funktionsblock `PositionMarkerFS` bietet eine kompakte und robuste Lösung, um einen VT‑Marker basierend auf einem beliebigen REAL‑Wert präzise zu positionieren. Durch die Kombination von Offsetsetzung, Bereichsbegrenzung und Integer‑Konvertierung entfällt für den Anwender die Notwendigkeit, eigene Umrechnungs‑ und Begrenzungslogik zu implementieren. Die klare Trennung zwischen Initialisierung und Betrieb erleichtert die Integration in bestehende Steuerungsanwendungen und erhöht die Wiederverwendbarkeit.
