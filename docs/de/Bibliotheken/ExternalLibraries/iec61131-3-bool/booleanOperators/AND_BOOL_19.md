# AND_BOOL_19

* * * * * * * * * *

## Einleitung

Der Funktionsblock `AND_BOOL_19` ist ein generischer Baustein zur Berechnung der logischen UND-Verknüpfung (Klassifikation `standard boolean function`, IEC 61131-3). Er verknüpft 19 separate BOOL-Eingänge (`IN1` bis `IN19`) und liefert das Ergebnis am Ausgang `OUT`. Er ergänzt die Standard-AND_BOOL-Familie (bisher bis `AND_BOOL_10`) um die Variante mit 19 Eingängen.

![AND_BOOL_19](AND_BOOL_19.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **REQ (Normal Execution Request):** Löst die Berechnung aus. Das Ereignis ist mit allen 19 Daten-Eingängen (`IN1` bis `IN19`) verknüpft.

### **Ereignis-Ausgänge**

-   **CNF (Execution Confirmation):** Signalisiert den Abschluss der Berechnung. Das Ereignis ist mit dem Datenausgang `OUT` verknüpft.

### **Daten-Eingänge**

-   **IN1 (AND input 1):** Boolescher Eingang 1.
-   **IN2 (AND input 2):** Boolescher Eingang 2.
-   **IN3 (AND input 3):** Boolescher Eingang 3.
-   **IN4 (AND input 4):** Boolescher Eingang 4.
-   **IN5 (AND input 5):** Boolescher Eingang 5.
-   **IN6 (AND input 6):** Boolescher Eingang 6.
-   **IN7 (AND input 7):** Boolescher Eingang 7.
-   **IN8 (AND input 8):** Boolescher Eingang 8.
-   **IN9 (AND input 9):** Boolescher Eingang 9.
-   **IN10 (AND input 10):** Boolescher Eingang 10.
-   **IN11 (AND input 11):** Boolescher Eingang 11.
-   **IN12 (AND input 12):** Boolescher Eingang 12.
-   **IN13 (AND input 13):** Boolescher Eingang 13.
-   **IN14 (AND input 14):** Boolescher Eingang 14.
-   **IN15 (AND input 15):** Boolescher Eingang 15.
-   **IN16 (AND input 16):** Boolescher Eingang 16.
-   **IN17 (AND input 17):** Boolescher Eingang 17.
-   **IN18 (AND input 18):** Boolescher Eingang 18.
-   **IN19 (AND input 19):** Boolescher Eingang 19.

### **Daten-Ausgänge**

-   **OUT (AND result):** Das Ergebnis der UND-Verknüpfung aller 19 Eingänge. Der Ausgang ist nur TRUE, wenn **alle** Eingänge `IN1` bis `IN19` den Wert TRUE haben, sonst FALSE.

### **Adapter**

Dieser Funktionsblock verfügt über keine Adapterschnittstellen.

## Funktionsweise

Bei jedem Eintreffen des Ereignisses `REQ` werden alle 19 Eingänge gelesen und die Operation `OUT = IN1 AND IN2 AND ... AND IN19` berechnet. Anschließend wird das Ergebnis an `OUT` bereitgestellt und `CNF` ausgelöst.

## Technische Besonderheiten

-   **Generischer Baustein:** Der Typ trägt das Attribut `GenericClassName = 'GEN_AND_BOOL'`; diese Instanz ist fest auf 19 Eingänge ausgelegt.
-   **Package:** `iec61131::booleanOperators`.
-   **Zustandslos:** Der Baustein ist speicherlos: `OUT` hängt ausschließlich von den Werten der Eingänge zum Zeitpunkt von `REQ` ab.

## Anwendungsszenarien

-   **Freigabe- und Sicherheitsverknüpfungen:** Alle Bedingungen (z. B. Türkontakte, Not-Aus, Betriebsbereitschaft) müssen erfüllt sein, bevor eine Aktion starten darf.
-   **Mehrfachbedingungen:** Prüfen, ob viele Voraussetzungen gleichzeitig erfüllt sind, ohne mehrere kleinere AND-Bausteine kaskadieren zu müssen.

## ⚖️ Vergleich mit ähnlichen Bausteinen

-   [`OR_BOOL_19`](./OR_BOOL_19.md): Führt die ODER-Verknüpfung durch: Das Ergebnis ist TRUE, sobald mindestens ein Eingang TRUE ist, während `AND_BOOL_19` verlangt, dass alle Eingänge TRUE sind.
-   [`AND_BOOL_10`](./AND_BOOL_10.md): Gleichartiger Baustein der Familie mit 10 Eingängen.

## Fazit

`AND_BOOL_19` ist ein einfacher, ereignisgesteuerter Baustein für die UND-Verknüpfung von 19 booleschen Signalen mit fester Schnittstelle.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
