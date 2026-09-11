# BargraphSplitFS

![BargraphSplitFS](./BargraphSplitFS.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **BargraphSplitFS** visualisiert einen vorzeichenbehafteten REAL-Wert als zwei nebeneinanderliegende lineare Balkendiagramme (Linear Bar Graphs). Die beiden Balken treffen sich an einem gemeinsamen Nullpunkt, wobei positive Werte auf der rechten Seite und negative Werte auf der linken Seite dargestellt werden. Der Baustein kapselt zwei Instanzen des ISO 11783-6 Bausteins `Q_NumericValue_PHYS` und übernimmt die Aufbereitung des Eingangswertes sowie die Begrenzung auf den zulässigen Magnitudenbereich.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `INIT` | EInit | Service Initialization; übernimmt den Objekt-Pool `stObj` per F_MOVE-Snapshot |
| `REQ` | Event | Anforderung zur Anzeige eines neuen vorzeichenbehafteten Wertes |

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `INITO` | EInit | Bestätigung der Initialisierung |
| `CNF` | Event | Bestätigung der Verarbeitung; liefert Status und Ergebnis der beiden internen `Q_NumericValue_PHYS`-Instanzen sowie Überlauf-Flags |

### **Daten-Eingänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `stObj` | `isobus::utils::bargraph::BargraphSplit_S` | Objekt-Pool mit links/rechts Bargraph-Referenzen und gemeinsamen Magnituden-Grenzen; wird bei INIT übernommen |
| `rValue` | REAL | Vorzeichenbehafteter physikalischer Wert zur Anzeige (beliebige Quelle) |

### **Daten-Ausgänge**

| Name | Typ | Kommentar |
|------|-----|-----------|
| `STATUSRight` | STRING | Statusmeldung der rechten Bargraph-Instanz (Passthrough) |
| `s16resultRight` | INT | Rückgabewert der rechten Bargraph-Instanz (Passthrough) |
| `xOverRight` | BOOL | Wird `TRUE`, wenn `rValue` den Maximum-Magnitudenwert überschreitet und die rechte Seite begrenzt wurde |
| `STATUSLeft` | STRING | Statusmeldung der linken Bargraph-Instanz (Passthrough) |
| `s16resultLeft` | INT | Rückgabewert der linken Bargraph-Instanz (Passthrough) |
| `xOverLeft` | BOOL | Wird `TRUE`, wenn der negierte Wert den Maximum-Magnitudenwert überschreitet und die linke Seite begrenzt wurde |

### **Adapter**

Keine Adapter vorhanden.

## Funktionsweise

Der Baustein arbeitet nach folgendem Prinzip:

1. **Initialisierung (INIT)**: Der eingehende `stObj` wird über einen `F_MOVE`-Baustein in eine interne, stabile Kopie übernommen. Dies entspricht dem in verwandten Bausteinen (z.B. ScrollFS, PositionMarkerFS) verwendeten Muster, um spätere Änderungen am Objektpool während des Betriebs zu verhindern.

2. **Wertaufbereitung (REQ)**:
   - **Positive Werte** (`rValue > 0`): Der Wert wird direkt an einen `F_ClampReal`-Baustein übergeben, der ihn auf den Bereich `[0, r32MaxMagnitude]` begrenzt. Das begrenzte Ergebnis wird als physikalischer Wert an die rechte `Q_NumericValue_PHYS`-Instanz übergeben. Die linke Instanz erhält den Wert 0 (kein Ausschlag).
   - **Negative Werte** (`rValue < 0`): Der Wert wird zunächst durch einen `F_MUL`-Baustein mit `-1.0` multipliziert (Negation). Das positive Ergebnis wird dann durch einen zweiten `F_ClampReal` begrenzt und an die linke `Q_NumericValue_PHYS`-Instanz übergeben. Die rechte Seite erhält den Wert 0.
   - **Wert = 0**: Beide Seiten erhalten den Wert 0 und zeigen keine Magnitude an.

3. **Überwachung**: Tritt eine Überschreitung des Maximalwerts (`r32MaxMagnitude`) auf, setzt der jeweilige `F_ClampReal` das Flag `xOver` und das entsprechende `xOverRight`/`xOverLeft` wird ausgegeben. Es gibt bewusst keine `xUnder`-Ausgänge, da ein Unterschreiten des Minimalwerts (0) auf der inaktiven Seite den normalen Ruhezustand darstellt und keine Diagnose erfordert.

4. **Ausgabe**: Nach Verarbeitung eines `REQ`-Ereignisses wird `CNF` ausgelöst und die Status- sowie Ausgabewerte der beiden internen `Q_NumericValue_PHYS`-Instanzen durchgereicht.

## Technische Besonderheiten

- **Snapshot-Mechanik**: Der `stObj` wird nur einmalig bei `INIT` übernommen. Dies verhindert, dass Änderungen an der übergebenen Struktur während des laufenden Betriebs die Anzeige unkontrolliert beeinflussen.
- **Zwei unabhängige Bargraph-Instanzen**: Die Verwendung von zwei separaten `Q_NumericValue_PHYS`-Bausteinen ermöglicht eine getrennte Ansteuerung der linken und rechten Balkengrafik gemäß ISO 11783-6.
- **Keine zusätzliche Skalierung**: Der Baustein akzeptiert jeden REAL-Wert, solange der physikalische Bereich der `Q_NumericValue_PHYS`-Instanzen durch die Begrenzung eingehalten wird. Eine externe Skalierung ist nicht erforderlich.
- **Kaskadierte Verarbeitung**: Die Ereigniskette verarbeitet rechts und links nacheinander, sodass eine klare Reihenfolge der Aktualisierung gewährleistet ist.

## Zustandsübersicht

Der Baustein besitzt keine explizite Zustandsmaschine, sondern folgt einem ereignisgesteuerten Ablauf:

- **Initial** (nach Abschluss von `INIT`): Alle internen Instanzen sind initialisiert, die Ausgänge sind gültig.
- **Bereit** (wartend auf `REQ`): Der Baustein wartet auf neue Werte.
- **Verarbeitung** (während `REQ`): Die Eingangswerte werden verarbeitet, die Ausgänge werden aktualisiert und `CNF` wird ausgelöst.

Fehlersituationen (z.B. ungültiger `stObj`) werden nicht speziell behandelt; sie würden sich in den Status-Ausgängen der `Q_NumericValue_PHYS`-Instanzen widerspiegeln.

## Anwendungsszenarien

- **Maschinensteuerung (ISO 11783)**: Anzeige von Betriebsparametern, die sowohl positive als auch negative Werte annehmen können, z.B. Differenzdrehzahl, Druckdifferenz oder Temperaturabweichung von einem Sollwert.
- **Instrumententafeln**: Darstellung von Füllständen oder Messwerten mit bilateralem Bereich, bei dem positive und negative Abweichungen visuell getrennt dargestellt werden.
- **Diagnose und Monitoring**: Visualisierung von Regelabweichungen in Steuerungssystemen, wobei die getrennte Anzeige eine schnelle Erfassung der Richtung ermöglicht.

## Vergleich mit ähnlichen Bausteinen

Im Gegensatz zu einem einfachen `Q_NumericValue_PHYS`, das nur einen einzelnen Bargraph ansteuert, bietet `BargraphSplitFS` eine integrierte Aufteilung in zwei Bereiche. Dies spart externe Logik und vereinfacht die Anbindung an ein Anzeigesystem mit gemeinsamer Nullpunktbasis. Ähnliche Bausteine (z.B. ein einzelner `Bargraph` mit negativem Wertebereich) würden entweder eine separate Vorzeichenbehandlung oder eine aufwändigere Konfiguration erfordern. `BargraphSplitFS` kapselt diese Komplexität und bietet eine klare Schnittstelle für den Anwendungsentwickler.

## Fazit

Der Baustein `BargraphSplitFS` stellt eine robuste und effiziente Lösung zur Visualisierung vorzeichenbehafteter Messwerte auf zwei getrennten Balkendiagrammen dar. Durch die Kapselung der Aufbereitungslogik und die Verwendung bewährter ISO-11783-Bausteine wird eine hohe Wiederverwendbarkeit erreicht. Die klare Schnittstelle mit separaten Status- und Überlaufmeldungen erleichtert die Integration in übergeordnete Systeme und ermöglicht eine präzise Diagnose. Ohne unnötige Komplexität erfüllt er die Anforderungen typischer Anzeigeszenarien in der Automatisierungstechnik.
