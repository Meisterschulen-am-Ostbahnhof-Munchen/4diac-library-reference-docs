# ILOCK_BLOCK_PROTECT_A2X


![ILOCK_BLOCK_PROTECT_A2X_ecc](./ILOCK_BLOCK_PROTECT_A2X_ecc.svg)

![ILOCK_BLOCK_PROTECT_A2X](./ILOCK_BLOCK_PROTECT_A2X.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock `ILOCK_BLOCK_PROTECT_A2X` realisiert eine **Interlock‑Logik mit Schutzzeit**. Er priorisiert den ersten aktiven Eingang (Vorwärts/Rückwärts bzw. Auf/Ab) und verhindert durch eine einstellbare Totzeit ein unmittelbares Umschalten zwischen den beiden Richtungen. Die Richtungssignale werden über einen bidirektionalen A2X‑Adapter bereitgestellt, die Schutzzeit wird über einen Timeout‑Adapter gesteuert. Dieser Baustein eignet sich besonders für sicherheitsrelevante Antriebssteuerungen, bei denen ein sofortiger Richtungswechsel unerwünscht ist.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Beschreibung |
|----------|--------------|
| `UPDATE` | Aktualisiert den Parameter `DT_PROTECT` (wird im aktuellen Zustand verarbeitet) |

### **Ereignis-Ausgänge**

*Keine direkten Ereignis‑Ausgänge vorhanden.*  
Alle Ereignis‑Ausgänge werden über den **Adapter `OUT`** (A2X‑Typ) bereitgestellt: `OUT.E_UP` und `OUT.E_DOWN`.

### **Daten-Eingänge**

| Daten | Typ | Beschreibung |
|-------|-----|--------------|
| `DT_PROTECT` | `TIME` | Schutzzeit (Totzeit) für das Umschalten zwischen Auf/Ab bzw. Vorwärts/Rückwärts. Initialwert: `T#50ms` |

### **Daten-Ausgänge**

*Keine direkten Daten‑Ausgänge.*  
Die Ausgangsdaten `OUT.UP` und `OUT.DOWN` (BOOL) werden über den **Adapter `OUT`** bereitgestellt.

### **Adapter**

| Adapter | Richtung | Typ | Beschreibung |
|---------|----------|-----|--------------|
| `IN` | Socket (Eingang) | `adapter::types::unidirectional::A2X` | Eingang für die Richtungssignale (Vorwärts/Up bzw. Rückwärts/Down) und deren zugehörige Ereignisse (`E_UP`, `E_DOWN`) |
| `OUT` | Plug (Ausgang) | `adapter::types::unidirectional::A2X` | Ausgang für die priorisierten Richtungssignale (`UP`, `DOWN`) und die zugehörigen Ereignisse (`E_UP`, `E_DOWN`) |
| `timeOut` | Plug (Ausgang) | `iec61499::events::ATimeOut` | Timer‑Adapter zur Realisierung der Schutzzeit; erhält die Dauer `DT_PROTECT` und liefert das Ereignis `TimeOut` |

## Funktionsweise

Der Baustein arbeitet als endlicher Automat (ECC) und folgt einer klaren Prioritätslogik:

1. **STOP** – Warten auf ein gültiges Ereignis:
   - Bei `IN.E_UP` mit `IN.UP = TRUE` → Wechsel in Zustand **UP**.
   - Bei `IN.E_DOWN` mit `IN.DOWN = TRUE` → Wechsel in Zustand **DOWN**.

2. **UP** – Aktiver Zustand „Aufwärts“:
   - Setzt `OUT.UP = TRUE` und `OUT.DOWN = FALSE`.
   - Bei `IN.E_UP` mit `IN.UP = FALSE` (d.h. das Aufwärts‑Signal fällt ab) → Wechsel in Zustand **UP_STOP**.

3. **DOWN** – Aktiver Zustand „Abwärts“:
   - Setzt `OUT.UP = FALSE` und `OUT.DOWN = TRUE`.
   - Bei `IN.E_DOWN` mit `IN.DOWN = FALSE` → Wechsel in Zustand **DOWN_STOP**.

4. **UP_STOP / DOWN_STOP** – Schutzphase:
   - Entsprechendes Ausgangssignal wird auf `FALSE` gesetzt (`OUT.UP` bzw. `OUT.DOWN`).
   - Der Timer `timeOut` wird mit `DT_PROTECT` gestartet.
   - Nach Ablauf des Timers (`timeOut.TimeOut`) erfolgt der Wechsel in den Zustand **EVAL**.

5. **EVAL** – Auswertung der aktuellen Eingänge nach Ablauf der Schutzzeit:
   - Ist `IN.UP = TRUE` und `IN.DOWN = FALSE` → Wechsel in **UP**.
   - Ist `IN.DOWN = TRUE` und `IN.UP = FALSE` → Wechsel in **DOWN**.
   - Sind beide Eingänge aktiv oder keiner aktiv → Wechsel zurück in **STOP**.

Während jedes Zustands kann das Ereignis `UPDATE` eintreten, das lediglich den Parameter `DT_PROTECT` aktualisiert und den aktuellen Zustand beibehält.

Die Zustandsübergänge und die zugehörigen Algorithmen (`UP`, `DOWN`, `STOP`) gewährleisten, dass immer nur eine Richtung aktiv ist und ein Richtungswechsel erst nach Ablauf der Schutzzeit möglich ist.

## Technische Besonderheiten

- **Prioritätslogik mit Schutzzeit:** Das erste aktive Eingangssignal wird übernommen und erst nach Ablauf der einstellbaren Totzeit darf ein Richtungswechsel stattfinden. Dadurch werden ungewollte Schaltspitzen und mechanische Belastungen vermieden.
- **Verwendung des A2X‑Adapters:** Die Richtungssignale werden über bidirektionale A2X‑Adapter ein- und ausgegeben. Dies erlaubt eine konsistente Übertragung von Zustand und Ereignissen für Auf/Ab‑ bzw. Vorwärts/Rückwärts‑Bewegungen.
- **Timer‑Adapter:** Die Schutzzeit wird über den standardisierten `ATimeOut`‑Adapter realisiert. Dadurch ist die Verzögerung exakt steuerbar und kann zur Laufzeit über `DT_PROTECT` angepasst werden.
- **Endlicher Automat:** Die klare ECC‑Struktur erleichtert die formale Verifikation und Wartung des Bausteins.
- **Keine direkten Ausgangs‑Events:** Alle Ausgangsereignisse werden über den OUT‑Adapter bereitgestellt, was die Wiederverwendbarkeit erhöht.

## Zustandsübersicht

| Zustand | Bedeutung | Ausgang (OUT.UP / OUT.DOWN) | Timer‑Status |
|---------|-----------|------------------------------|--------------|
| `STOP` | Warten auf ein gültiges Eingangssignal | `FALSE` / `FALSE` | inaktiv |
| `UP` | Aufwärtsbewegung aktiv | `TRUE` / `FALSE` | inaktiv |
| `DOWN` | Abwärtsbewegung aktiv | `FALSE` / `TRUE` | inaktiv |
| `UP_STOP` | Schutzphase nach Ende der Aufwärtsbewegung | `FALSE` / `FALSE` | läuft |
| `DOWN_STOP` | Schutzphase nach Ende der Abwärtsbewegung | `FALSE` / `FALSE` | läuft |
| `EVAL` | Auswertung der Eingänge nach Ablauf der Schutzzeit | `FALSE` / `FALSE` (temporär) | inaktiv |

| Übergang | Bedingung | Aktion |
|----------|-----------|--------|
| `STOP → UP` | `IN.E_UP` mit `IN.UP = TRUE` | – |
| `STOP → DOWN` | `IN.E_DOWN` mit `IN.DOWN = TRUE` | – |
| `UP → UP_STOP` | `IN.E_UP` mit `IN.UP = FALSE` | `STOP`‑Algorithmus, Start Timer |
| `DOWN → DOWN_STOP` | `IN.E_DOWN` mit `IN.DOWN = FALSE` | `STOP`‑Algorithmus, Start Timer |
| `UP_STOP → EVAL` | `timeOut.TimeOut` | – |
| `DOWN_STOP → EVAL` | `timeOut.TimeOut` | – |
| `EVAL → UP` | `IN.UP = TRUE` und `IN.DOWN = FALSE` | – |
| `EVAL → DOWN` | `IN.DOWN = TRUE` und `IN.UP = FALSE` | – |
| `EVAL → STOP` | beide Eingänge aktiv oder beide inaktiv | – |
| `* → *` (Selbstschleife) | `UPDATE` | Parameter `DT_PROTECT` wird aktualisiert |

## Anwendungsszenarien

- **Antriebssteuerungen:** Schutz vor zu schnellem Richtungswechsel bei Förderbändern, Hubwerken oder Verfahrwerken, um mechanische Belastungen zu reduzieren.
- **Sicherheitsinterlocks:** In Maschinensteuerungen, bei denen ein sofortiges Umschalten zwischen zwei Bewegungsrichtungen verboten ist.
- **Automatisierte Prozesse:** Wenn ein definiertes Zeitfenster zwischen zwei gegenläufigen Aktionen erforderlich ist (z. B. bei Ventil‑ oder Klappensteuerungen).
- **Integration in bestehende 4diac‑Applikationen:** Dank der A2X‑Schnittstellen können die Signale direkt mit anderen A2X‑fähigen Bausteinen verbunden werden.

## Vergleich mit ähnlichen Bausteinen

Im Vergleich zu einem einfachen Interlock‑Baustein (der nur eine Prioritätslogik ohne Zeitverzögerung bietet) zeichnet sich `ILOCK_BLOCK_PROTECT_A2X` durch die integrierte Schutzzeit aus. Dadurch wird ein unkontrolliertes Oszillieren zwischen den Zuständen verhindert. Gegenüber Bausteinen, die separate Timer‑Funktionen verwenden, ist hier die Zeitsteuerung direkt in die Zustandsmaschine integriert, was die Anwendung vereinfacht. Der Einsatz des standardisierten A2X‑Adapters stellt sicher, dass die Daten- und Ereignisschnittstellen einheitlich und wiederverwendbar sind.

## Fazit

Der FB `ILOCK_BLOCK_PROTECT_A2X` bietet eine robuste und flexible Lösung für Interlock‑Anforderungen mit Schutzzeit. Die klare Zustandsmaschine und die Verwendung standardisierter Adapter machen ihn einfach in industriellen Steuerungssystemen einsetzbar. Durch die einstellbare Parameter `DT_PROTECT` kann die Schaltverzögerung an die jeweilige Anwendung angepasst werden. Somit ist dieser Baustein eine zuverlässige Komponente für sicherheitsbewusste Ansteuerungen von Antrieben und Aktoren.
