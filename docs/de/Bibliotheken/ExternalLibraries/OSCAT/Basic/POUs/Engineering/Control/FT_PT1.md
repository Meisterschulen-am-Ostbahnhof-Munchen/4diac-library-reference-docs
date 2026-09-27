# FT_PT1

![FT_PT1](FT_PT1.svg)

* * * * * * * * * *

## Einleitung

`FT_PT1` ist ein Tiefpassfilter 1. Ordnung (PT1-Glied) aus der OSCAT-Bibliothek. Er dient zur Glättung von analogen Signalverläufen und Rauschunterdrückung mit einer programmierbaren Zeitkonstante `TM` und einem Verstärkungsfaktor `K`.

Der Baustein ist als `SimpleFB` (zustandsbehafteter Funktionsbaustein mit `EINIT`/`REQ`/`RST`) implementiert und führt eine zeitdiskrete Integration nach der PT1-Übertragungsfunktion durch:

$$T_M \cdot \frac{dy}{dt} + y(t) = K \cdot x(t)$$

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Beschreibung | Mit Daten |
| :--- | :--- | :----------------------- | :-------- |
| `EINIT` | `Event` | Service-Initialisierung des Filters | |
| `REQ` | `Event` | Ausführungsanforderung für neuen Berechnungszyklus | `in`, `TM`, `K` |
| `RST` | `Event` | Invalidiert den Initialisierungszustand (`init = FALSE`) für Reinitialisierung beim nächsten `REQ` | |

### **Ereignis-Ausgänge**

| Name | Typ | Beschreibung | Mit Daten |
| :--- | :--- | :----------------------- | :-------- |
| `INITO` | `Event` | Initialisierungsbestätigung | |
| `CNF` | `Event` | Ausführungsbestätigung nach Berechnung | `delta_t`, `out` |

### **Daten-Eingänge**

| Name | Typ | Initialwert | Beschreibung |
| :--- | :--- | :------------ | :------------------- |
| `in` | `REAL` | `0.0` | Analoges Eingangssignal |
| `TM` | `TIME` | `T#0s` | Filter-Zeitkonstante. Bei `TM = T#0s` wird der Filter umgangen und `out = K * in` gesetzt. |
| `K` | `REAL` | `1.0` | Verstärkungsfaktor (Proportionalbeiwert) |

### **Daten-Ausgänge**

| Name | Typ | Beschreibung |
| :--- | :--- | :----------------------- |
| `delta_t` | `UDINT` | Vergangene Zeit seit dem letzten Aufruf in Mikrosekunden ($\mu s$) |
| `out` | `REAL` | Gefilterter Ausgangswert |

## Funktionsweise

1. **Initialisierung (`EINIT`)**:  
   Setzt den Initialisierungszustand zurück, setzt `out = 0.0` und signalisiert Einsatzbereitschaft über `INITO`.

2. **Zyklische Berechnung (`REQ`)**:  
   Bei jedem `REQ`-Ereignis ermittelt der Baustein die verstrichene Zeit seit dem letzten Aufruf ($\Delta t$) mikrosekundengenau über `T_PLC_US()`.
   - Ist der Baustein noch nicht initialisiert (`init = FALSE`) oder ist `TM = T#0s`, wird der Filter (re-)initialisiert: `init := TRUE`, `out := K * in` wird direkt aus dem frisch abgetasteten Eingangssignal gesetzt, `delta_t := 0` und der Zeitstempel `last` aktualisiert.
   - Bei `TM > T#0s` wird der neue Ausgangswert nach der zeitdiskreten PT1-Formel berechnet:
   
     $$\text{out}_{\text{neu}} = \text{out}_{\text{alt}} + \left( K \cdot \text{in} - \text{out}_{\text{alt}} \right) \cdot \frac{\Delta t}{T_{\text{eff}}}$$
   
     wobei $\Delta t = \text{delta\_t} \cdot 10^{-6}\,\text{s}$ das gemessene Aufrufintervall in Sekunden (aus `T_PLC_US()`), $T_M = \text{TIME\_TO\_REAL}(TM)$ die Filterzeitkonstante in Sekunden und $T_{\text{eff}} = \max(T_M, \Delta t)$ die effektive Zeitkonstante ist. Wenn das Aufrufintervall $\Delta t$ die eingestellte Filterzeit $T_M$ überschreitet, wird $T_{\text{eff}}$ auf $\Delta t$ begrenzt, sodass der Gewichtungsfaktor $\frac{\Delta t}{T_{\text{eff}}}$ auf maximal $1.0$ gedeckelt ist.
   - Um Unterläufe durch denormalisierte Fließkommazahlen zu vermeiden, werden Beträge $|out| < 1.0 \times 10^{-20}$ automatisch auf `0.0` gerundet.

3. **Filter-Reset (`RST`)**:  
   Setzt den Initialisierungszustand zurück (setzt `init := FALSE`), ohne `out` oder den Zeitstempel unverzüglich zu verändern. Dadurch wird das Seeding bis zum nächsten `REQ`-Ereignis verzögert, wodurch das frische Eingangssignal (statt veralteter Werte oder `0.0`) abgetastet und die Zeitbasis aktualisiert wird.

## Technische Besonderheiten

- **Zyklusunabhängige Zeitbasis**: Die Zeitdifferenz wird über `T_PLC_US()` in Mikrosekunden gemessen, wodurch Schwankungen der Aufrufzykluszeit kompensiert werden.
- **Begrenzung der effektiven Filterzeit ($T_{\text{eff}} = \max(T_M, \Delta t)$)**: Liegt das Aufrufintervall $\Delta t$ über der Filterzeit $T_M$, deckelt der Baustein den Diskretisierungsfaktor $\frac{\Delta t}{T_{\text{eff}}}$ auf maximal $1.0$. Dadurch wird verhindert, dass es bei langsamen Aufrufzyklen zu Oszillationen oder Euler-Überschwingen kommt.
- **Filter-Bypass bei `TM = 0`**: Ist `TM = T#0s`, schaltet der Baustein die Dämpfung ab und gibt das Eingangssignal direkt skaliert mit `K` aus.
- **Glatte Reset-Wiedereingliederung**: `RST` invalidiert den Initialisierungszustand (`init := FALSE`) ohne sofortiges Überschreiben des Ausgangs. Erst das nachfolgende `REQ`-Ereignis tastet das frische Eingangssignal ab, setzt `out := K * in` und aktualisiert den Zeitstempel `last`, wodurch verfälschte Sprünge durch veraltete Eingangswerte vermieden werden.
- **Projektspezifische Zeitkonvertierung**: Verwendet die projekteigene Hilfsfunktion `TIME_TO_REAL.fct` zur Umrechnung des `TIME`-Eingangs `TM` in Sekunden (`REAL`).

## Anwendungsszenarien

- Dämpfung von stark schwankenden Sensormesswerten (z. B. Druck-, Temperatur- oder Spannungssignalen).
- Rauschfilterung für Steuerungseingänge.
- Sanftes Anfahren/Verzögern von Sollwerten.

## Siehe auch

- [`FT_PT1_AR`](FT_PT1_AR.md) – AR-Adapter-Wrapper für `FT_PT1`.
- [`FT_PT2`](FT_PT2.md) – Tiefpassfilter 2. Ordnung.
- [`TIME_TO_REAL`](TIME_TO_REAL.md) – Hilfsfunktion zur Zeitkonvertierung.
