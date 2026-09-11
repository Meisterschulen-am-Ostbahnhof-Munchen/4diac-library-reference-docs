# LinksRechts_T_FF_Event


![LinksRechts_T_FF_Event_network](./LinksRechts_T_FF_Event_network.svg)

![LinksRechts_T_FF_Event](./LinksRechts_T_FF_Event.svg)

* * * * * * * * * *

## Einleitung

Der Baustein **LinksRechts_T_FF_Event** ist eine Subapplikation (SubApp) zur einfachen Links/Rechts-Umschaltung auf Basis eines Toggle-Flip-Flops. Er verwendet ausschließlich Event- und BOOL-basierte Signale und benötigt keine Adapter. Die SubApp ist als Wiederverwendungsbaustein konzipiert und stellt eine ereignisgesteuerte Alternative zu adapterbasierten Schwesterbausteinen dar.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ   | Beschreibung |
|------|-------|--------------|
| EI   | Event | Ereigniseingang zur Auslösung der Umschaltlogik. |

### **Ereignis-Ausgänge**

| Name | Typ   | Beschreibung |
|------|-------|--------------|
| EO   | Event | Ereignisausgang, signalisiert die abgeschlossene Aktualisierung der Ausgangssignale. |

### **Daten-Eingänge**

| Name | Typ  | Beschreibung |
|------|------|--------------|
| DI   | BOOL | Steuersignal für die Freigabe der Toggle-Funktion. |

### **Daten-Ausgänge**

| Name   | Typ  | Beschreibung |
|--------|------|--------------|
| Rechts | BOOL | Ausgangssignal für die Richtung „Rechts“, entspricht dem internen Flip-Flop-Zustand. |
| Links  | BOOL | Ausgangssignal für die Richtung „Links“, ist die invertierte Version von `Rechts`. |

### **Adapter**

Keine. Die SubApp verwendet ausschließlich direkte Event- und Datenverbindungen.

## Funktionsweise

Die SubApp realisiert eine Toggle-Funktion mit optionaler Freigabe:

1. Ein Ereignis an `EI` wird über den `E_SWITCH`-Baustein geführt.
2. Der Daten-Eingang `DI` steuert den Schalter `E_SWITCH.G`:
   - Ist `DI = TRUE`, wird das Ereignis an den Takteingang `CLK` des Toggle-Flip-Flops `E_T_FF` weitergeleitet.
   - Ist `DI = FALSE`, wird das Ereignis blockiert, und es findet keine Zustandsänderung statt.
3. Bei jedem wirksamen Takt wechselt der Ausgang `Q` des Flip-Flops seinen Zustand (Toggle):
   - `Q = FALSE` → `Rechts = FALSE` und `Links = TRUE`
   - `Q = TRUE`  → `Rechts = TRUE`  und `Links = FALSE`
4. Der Ausgang `Q` wird direkt an `Rechts` geführt und gleichzeitig über den Negator `F_NOT` invertiert, um `Links` zu erzeugen.
5. Nach der Negation wird das Bestätigungsereignis (`CNF`) von `F_NOT` an den Ausgang `EO` weitergegeben. Dieses Ereignis signalisiert, dass die Ausgänge `Rechts` und `Links` gültig sind.

## Technische Besonderheiten

- Die SubApp besteht aus den Funktionsbausteinen `E_T_FF`, `E_SWITCH` und `F_NOT`.
- `E_T_FF` ist ein ereignisgesteuertes Toggle-Flip-Flop aus der IEC-61499-Standardbibliothek.
- `E_SWITCH` arbeitet als ereignisbasierter Schalter: Nur bei `G = TRUE` wird das Eingangsereignis an den Ausgang `EO1` weitergeleitet.
- `F_NOT` ist ein bitweiser Negator aus der IEC-61131-Bibliothek und wird hier als logischer Inverter eingesetzt.
- Die Ausgänge `Rechts` und `Links` sind immer komplementär zueinander.
- Durch die SubApp-Kapselung entsteht ein wiederverwendbarer, adapterfreier Baustein, der sich leicht in größere Automatisierungsprojekte integrieren lässt.

## Zustandsübersicht

Die SubApp besitzt keinen eigenen expliziten Zustandsautomaten, ihr Verhalten lässt sich jedoch über den internen Flip-Flop-Zustand beschreiben:

| Zustand von `Q` | `Rechts` | `Links` | Bedeutung       |
|------------------|----------|---------|-----------------|
| `FALSE`          | `FALSE`  | `TRUE`  | Links aktiv     |
| `TRUE`           | `TRUE`   | `FALSE` | Rechts aktiv    |

Ein Wechsel zwischen den Zuständen erfolgt ausschließlich dann, wenn ein Ereignis an `EI` anliegt und `DI = TRUE` ist.

## Anwendungsszenarien

- Richtungsumschaltung bei Förderbändern oder Transportanlagen.
- Alternierende Ansteuerung von zwei Signalleuchten (z. B. „links/rechts“-Anzeige).
- Toggle-Steuerung für Antriebe mit zwei Richtungsausgängen.
- Ereignisbasierte Umschaltung in IEC-61499-Applikationen, bei denen keine Adapter verwendet werden sollen.

## Vergleich mit ähnlichen Bausteinen

| Baustein | Konzept | Unterschied |
|----------|---------|-------------|
| `LinksRechts_T_FF_Event` | Event-/BOOL-basiert, keine Adapter | Einfache Integration in reine Event-/Daten-Netzwerke. |
| `MyLib::sys::AX_LinksRechts_T_FF` | Adapter-basiert | Verwendet einen Adapter für strukturierte Verbindungen, ermöglicht kompaktere Applikationsnetze. |
| Klassisches RS-Flip-Flop | Set/Reset | Benötigt zwei getrennte Eingänge zum Setzen und Rücksetzen; hier genügt ein einzelnes Ereignis zum Umschalten. |

Der ereignisbasierte Baustein ist besonders dann vorteilhaft, wenn eine einfache, adapterfreie Lösung mit minimalem Verdrahtungsaufwand gefordert ist.

## Fazit

**LinksRechts_T_FF_Event** ist ein kompakter und klar strukturierter Funktionsbaustein für die Links/Rechts-Umschaltung mit Toggle-Verhalten. Durch die Kombination von `E_T_FF`, `E_SWITCH` und `F_NOT` entsteht eine robuste, ereignisgesteuerte Logik. Die adapterfreie Schnittstelle erleichtert die Wiederverwendung und Integration in unterschiedlichste IEC-61499-Applikationen.
