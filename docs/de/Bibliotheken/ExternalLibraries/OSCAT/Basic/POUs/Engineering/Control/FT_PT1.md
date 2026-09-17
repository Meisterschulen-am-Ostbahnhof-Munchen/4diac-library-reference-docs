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
| `RST` | `Event` | Setzt den Filterausgang `out` auf 0.0 zurück | |

### **Ereignis-Ausgänge**

| Name | Typ | Beschreibung | Mit Daten |
| :--- | :--- | :----------------------- | :-------- |
| `INITO` | `Event` | Initialisierungsbestätigung | |
| `CNF` | `Event` | Ausführungsbestätigung nach Berechnung | `out` |

### **Daten-Eingänge**

| Name | Typ | Initialwert | Beschreibung |
| :--- | :--- | :------------ | :------------------- |
| `in` | `REAL` | `0.0` | Analoges Eingangssignal |
| `TM` | `TIME` | `T#0s` | Zeitkonstante des Tiefpassfilters |
| `K` | `REAL` | `1.0` | Verstärkungsfaktor (Proportionalbeiwert) |

### **Daten-Ausgänge**

| Name | Typ | Beschreibung |
| :--- | :--- | :----------------------- |
| `out` | `REAL` | Gefilterter Ausgangswert |

## Funktionsweise

1. **Initialisierung (`EINIT`)**:  
   Bereitet den Baustein vor, setzt die interne Zeitstempel-Erfassung zurück und signalisiert Einsatzbereitschaft über `INITO`.

2. **Zyklische Berechnung (`REQ`)**:  
   Bei jedem `REQ`-Ereignis ermittelt der Baustein die verstrichene Zeit seit dem letzten Aufruf ($\Delta t$) über die interne Systemzeit. Die Zeitkonstante `TM` wird mittels `TIME_TO_REAL` in Sekunden umgerechnet.  
   Der neue Ausgangswert wird nach der Näherung:
   
   $$\text{out}_{\text{neu}} = \text{out}_{\text{alt}} + \left( K \cdot \text{in} - \text{out}_{\text{alt}} \right) \cdot \frac{\Delta t}{TM}$$
   
   berechnet und über `CNF` bereitgestellt.

3. **Filter-Reset (`RST`)**:  
   Setzt den intern gespeicherten Ausgangswert `out` sofort auf `0.0` zurück.

## Technische Besonderheiten

- **Genaue Zeitbasis**: Die Zeitdifferenz wird mikrosekundengenau ermittelt, wodurch die Filterfunktion unabhängig von Schwankungen der Aufrufzykluszeit exakt arbeitet.
- **RST-Handhabung**: Ein anstehendes Reset-Signal stellt sicher, dass der Filter bei Bedarf augenblicklich auf Null gesetzt werden kann (z. B. bei Sensor-Abschaltung).
- **Projektspezifische Zeitkonvertierung**: Verwendet die projekteigene Hilfsfunktion `TIME_TO_REAL.fct` zur sauberen Konvertierung des `TIME`-Eingangs `TM` in Sekunden (`REAL`).

## Anwendungsszenarien

- Dämpfung von stark schwankenden Sensormesswerten (z. B. Druck-, Temperatur- oder Spannungssignalen).
- Rauschfilterung für Steuerungseingänge.
- Sanftes Anfahren/Verzögern von Sollwerten.

## Siehe auch

- [`FT_PT1_AR`](FT_PT1_AR.md) – AR-Adapter-Wrapper für `FT_PT1`.
- [`FT_PT2`](FT_PT2.md) – Tiefpassfilter 2. Ordnung.
- [`TIME_TO_REAL`](TIME_TO_REAL.md) – Hilfsfunktion zur Zeitkonvertierung.
