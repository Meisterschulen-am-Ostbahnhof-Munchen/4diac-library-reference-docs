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
| `RST` | `Event` | Setzt den Filterausgang und die Zeitbasis zurück (`out = K * in`) | |

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
   - Ist der Baustein noch nicht initialisiert oder ist `TM = T#0s`, wird intern `RST` aufgerufen und `out = K * in` direkt ausgegeben.
   - Bei `TM > T#0s` wird der neue Ausgangswert nach der Diskretisierungsformel berechnet:
   
     $$\text{out}_{\text{neu}} = \text{out}_{\text{alt}} + \left( K \cdot \text{in} - \text{out}_{\text{alt}} \right) \cdot \frac{\Delta t}{TM}$$
   
   - Um Unterläufe durch denormalisierte Fließkommazahlen zu vermeiden, werden Beträge $|out| < 1.0 \times 10^{-20}$ automatisch auf `0.0` gerundet.

3. **Filter-Reset (`RST`)**:  
   Setzt `out` unverzüglich auf den skalierten Eingangswert `K * in` und aktualisiert den internen Zeitstempel `last := T_PLC_US()`. Dadurch wird verhindert, dass beim nachfolgenden `REQ`-Aufruf ein verfälschter Sprung durch verstrichene Zeit entsteht.

## Technische Besonderheiten

- **Zyklusunabhängige Zeitbasis**: Die Zeitdifferenz wird über `T_PLC_US()` in Mikrosekunden gemessen, wodurch Schwankungen der Aufrufzykluszeit kompensiert werden.
- **Filter-Bypass bei `TM = 0`**: Ist `TM = T#0s`, schaltet der Baustein die Dämpfung ab und gibt das Eingangssignal direkt skaliert mit `K` aus.
- **Glatte Reset-Wiedereingliederung**: `RST` aktualisiert den Zeitstempel `last`, sodass bei Wiederaufnahme des Filterbetriebs keine Ausreißer auftreten.
- **Projektspezifische Zeitkonvertierung**: Verwendet die projekteigene Hilfsfunktion `TIME_TO_REAL.fct` zur Umrechnung des `TIME`-Eingangs `TM` in Sekunden (`REAL`).

## Anwendungsszenarien

- Dämpfung von stark schwankenden Sensormesswerten (z. B. Druck-, Temperatur- oder Spannungssignalen).
- Rauschfilterung für Steuerungseingänge.
- Sanftes Anfahren/Verzögern von Sollwerten.

## Siehe auch

- [`FT_PT1_AR`](FT_PT1_AR.md) – AR-Adapter-Wrapper für `FT_PT1`.
- [`FT_PT2`](FT_PT2.md) – Tiefpassfilter 2. Ordnung.
- [`TIME_TO_REAL`](TIME_TO_REAL.md) – Hilfsfunktion zur Zeitkonvertierung.
