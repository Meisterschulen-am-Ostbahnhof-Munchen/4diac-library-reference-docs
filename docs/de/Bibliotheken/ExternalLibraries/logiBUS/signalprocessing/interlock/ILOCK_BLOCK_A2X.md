# ILOCK_BLOCK_A2X


![ILOCK_BLOCK_A2X_ecc](./ILOCK_BLOCK_A2X_ecc.svg)

![ILOCK_BLOCK_A2X](./ILOCK_BLOCK_A2X.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **ILOCK_BLOCK_A2X** realisiert eine Richtungsverriegelung für Geräte, die über eine A2X-Schnittstelle angesteuert werden. Er priorisiert die erste aktive Eingabe und stellt sicher, dass zu jedem Zeitpunkt nur eine Richtung (Aufwärts/Abwärts) aktiv ist. Die Implementierung erfolgt als Basic-Funktionsblock mit einem ECC (Execution Control Chart) und verwendet zwei A2X-Adapter: einen Eingangsadapter (IN) und einen Ausgangsadapter (OUT).

## Schnittstellenstruktur

### **Ereignis-Eingänge**
- Es sind keine direkten Ereignis-Eingänge vorhanden.  
- Ereignisse werden ausschließlich über die Adapter-Ereignisse `IN.E_UP` und `IN.E_DOWN` vom angeschlossenen A2X-Adapter empfangen.

### **Ereignis-Ausgänge**
- Es gibt keine expliziten Ereignis-Ausgänge.  
- Die Ausgabe von Ereignissen erfolgt über die Adapter-Ausgänge `OUT.E_UP` und `OUT.E_DOWN` sowie die zugehörigen Datenwerte (`OUT.UP`, `OUT.DOWN`).

### **Daten-Eingänge**
- Es sind keine direkten Dateneingänge definiert.  
- Die Eingangsdaten (`UP`, `DOWN`) werden über den A2X-Adapter `IN` bereitgestellt.

### **Daten-Ausgänge**
- Es sind keine direkten Datenausgänge definiert.  
- Die Ausgangsdaten (`UP`, `DOWN`) werden über den A2X-Adapter `OUT` bereitgestellt.

### **Adapter**
Der FB besitzt zwei Adapter-Schnittstellen vom Typ `adapter::types::unidirectional::A2X`:

- **IN** (Socket) – Eingang für die Richtungsbefehle.  
  Liefert die Ereignisse `E_UP` und `E_DOWN` sowie die zugehörigen booleschen Daten `UP` und `DOWN`.  
- **OUT** (Plug) – Ausgang für die geschalteten Richtungen.  
  Stellt die Ereignisse `E_UP` und `E_DOWN` sowie die Datenwerte `UP` und `DOWN` bereit.

Der A2X-Adapter ist ein unidirektionaler Adaptertyp, der die beiden booleschen Variablen `UP` und `DOWN` transportiert und jeweils zwei Ereignisse (für Aufwärts- und Abwärtsbewegung) einführt.

## Funktionsweise

Der FB implementiert eine Zustandsmaschine mit fünf Zuständen: `STOP`, `UP`, `DOWN`, `UP_STOP` und `DOWN_STOP`. Die Logik reagiert auf die Ereignisse `IN.E_UP` und `IN.E_DOWN` und prüft dabei die zugehörigen Datenwerte (`IN.UP` bzw. `IN.DOWN`).

- **Zustand STOP**: Initialer Ruhezustand.  
  - Wenn `IN.E_UP` eintrifft und `IN.UP` = TRUE, wechselt der FB in den Zustand `UP` und setzt `OUT.UP` auf TRUE, `OUT.DOWN` auf FALSE.  
  - Analog wechselt er bei `IN.E_DOWN` und `IN.DOWN` = TRUE in den Zustand `DOWN` und setzt `OUT.DOWN` auf TRUE, `OUT.UP` auf FALSE.  
- **Zustand UP**: Richtung „Aufwärts“ ist aktiv.  
  - Wenn `IN.E_UP` eintrifft und `IN.UP` = FALSE (Befehl wird zurückgenommen), wechselt der FB in den Zustand `UP_STOP`.  
  - Im `UP_STOP`-Zustand wird der Ausgang `OUT.E_UP` ausgelöst und der Algorithmus `STOP` setzt `OUT.UP` und `OUT.DOWN` auf FALSE.  
  - Danach kehrt der FB automatisch in den Zustand `STOP` zurück (Transition mit Bedingung `1`).  
- **Zustand DOWN**: Richtung „Abwärts“ ist aktiv.  
  - Analog zum `UP`-Zustand wechselt er bei `IN.E_DOWN` und `IN.DOWN` = FALSE in den Zustand `DOWN_STOP`, setzt die Ausgänge zurück und kehrt anschließend zu `STOP` zurück.

Die Priorisierung ergibt sich aus der Reihenfolge der Transitionen im ECC: Wenn im Zustand `STOP` sowohl `IN.E_UP` als auch `IN.E_DOWN` anstehen, hat die zuerst definierte Transition (hier die nach `DOWN`) Vorrang. Somit gewinnt die erste aktive Eingabe.

## Technische Besonderheiten

- Implementierung als **Basic FB** mit ECC und strukturierten Algorithmen in ST (Structured Text).  
- Verwendung des Adaptertyps `A2X` (unidirektional) für die Signalein-/ausgabe.  
- Keine herkömmlichen Ein-/Ausgänge (Ereignis oder Daten) – die Kommunikation erfolgt ausschließlich über Adapter.  
- Der FB gehört zum Paket `logiBUS::signalprocessing::interlock`.  
- Lizenz: Eclipse Public License 2.0 (SPDX-License-Identifier: EPL-2.0).

## Zustandsübersicht

| Zustand     | Beschreibung                                                                 |
|-------------|------------------------------------------------------------------------------|
| `STOP`      | Keine Richtung aktiv. Wartet auf Befehl.                                     |
| `UP`        | Richtung „Aufwärts“ aktiv. `OUT.UP` = TRUE, `OUT.DOWN` = FALSE.              |
| `DOWN`      | Richtung „Abwärts“ aktiv. `OUT.DOWN` = TRUE, `OUT.UP` = FALSE.               |
| `UP_STOP`   | Übergang vom `UP`-Zustand: Richtung wird gestoppt, danach Rückkehr zu `STOP`.|
| `DOWN_STOP` | Übergang vom `DOWN`-Zustand: Richtung wird gestoppt, danach Rückkehr zu `STOP`.|

## Anwendungsszenarien

- Steuerung von Motoren oder Antrieben, die nur eine Richtung gleichzeitig zulassen (z. B. Hubwerke, Schiebetore, Förderbänder).  
- Verriegelung von Bewegungsrichtungen in sicherheitsrelevanten Anwendungen.  
- Übernahme von Richtungskommandos über eine serielle Schnittstelle, die über den A2X-Adapter angebunden ist.

## Vergleich mit ähnlichen Bausteinen

Ähnliche Bausteine wie ein SR-Latch oder ein allgemeiner Interlock-Baustein steuern ebenfalls sich gegenseitig ausschließende Ausgänge. Der ILOCK_BLOCK_A2X unterscheidet sich durch seine adapterbasierte Schnittstelle (`A2X`), die Ereignis-/Datenstruktur und die explizite Priorisierungslogik innerhalb der Zustandsmaschine. Dadurch ist er besonders für modulare Systeme geeignet, die eine einheitliche Adapterkommunikation verwenden.

## Fazit

Der ILOCK_BLOCK_A2X ist ein kompakter, auf Adapterbasis arbeitender Funktionsblock für die sichere Richtungsverriegelung. Durch die klare Zustandsmaschine und die definierten Übergänge bietet er eine zuverlässige und einfache Lösung für Anwendungen, die eine gegenseitige Ausschließung von Auf-/Abwärtsbewegungen erfordern. Die Abwesenheit herkömmlicher Ein-/Ausgänge macht ihn besonders geeignet für modulare Systeme, die auf Adapter-Kommunikation aufbauen.