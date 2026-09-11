# AID_NV

![AID_NV](./AID_NV.svg)

* * * * * * * * * *

## Einleitung

Der Baustein `AID_NV` ist eine globale Konstantendefinition im Bereich der ISOBUS‑Kommunikation (ISO 11783). Er definiert Attribut‑IDs für **Number Variable Objects (NV)**. Diese Objekte dienen dazu, numerische Werte (z. B. Messwerte oder Einstellungen) zwischen Steuergeräten und Terminals auszutauschen. Die hier bereitgestellte Konstante `VALUE` referenziert das Attribut **1**, welches den aktuellen Wert (`AID_NV_VALUE`) eines NV‑Objekts kennzeichnet.

## Schnittstellenstruktur

Da `AID_NV` ein `GlobalConstants`‑Baustein ist, besitzt er keine klassischen Ein‑/Ausgangs‑Schnittstellen.

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine (es werden keine Werte exportiert, sondern symbolische Konstanten zur Verfügung gestellt).

### **Adapter**

Keine.

## Funktionsweise

`AID_NV` definiert eine einzige globale Konstante:

- `VALUE` : `USINT = 1`  
  Diese Konstante entspricht der Attribut‑ID `1`, die laut ISO‑11783 den **aktuellen Wert** eines *Number Variable Object* angibt. In der Anwendung kann diese Konstante verwendet werden, um auf das Attribut eines NV‑Objekts zuzugreifen, ohne magische Zahlen hartkodieren zu müssen.

Die Definition erfolgt im Paket `isobus::UT::Q::const::AID` und ist damit für alle Module des ISOBUS‑Stacks zugänglich.

## Technische Besonderheiten

- Der Baustein ist als `GlobalConstants` mit **konstantem** Wert (`CONSTANT`) deklariert und kann nicht zur Laufzeit verändert werden.
- Der Quellcode ist in der Anfangssprache ST (Structured Text) verfasst und wird durch den 4diac‑Compiler zu einem entsprechenden C‑Code umgesetzt.
- Die Copyright‑Hinweise weisen auf die Eclipse Public License 2.0 (EPL‑2.0) hin.
- Die Version 1.0 wurde am 20.06.2026 von Franz Höpfinger erstellt.

## Zustandsübersicht

Nicht zutreffend – der Baustein besitzt keinen Zustandsautomaten, da er nur Konstanten bereitstellt.

## Anwendungsszenarien

- **ISOBUS‑Anwendungen:** Zugriff auf die Attribut‑ID des aktuellen Werts eines *Number Variable Object* (z. B. bei der Konfiguration eines Terminals oder bei der Übertragung von Messdaten).
- **Code‑Lesbarkeit:** Vermeidung von „Magic Numbers“ durch Verwendung aussagekräftiger Konstanten wie `AID_NV.VALUE`.
- **Wartung:** Änderungen an der ISOBUS‑Spezifikation können zentral in dieser Konstantendefinition angepasst werden, ohne dass jede Verwendungsstelle angepasst werden muss.

## Vergleich mit ähnlichen Bausteinen

Es existieren weitere globale Konstantendefinitionen für andere ISOBUS‑Objekttypen, z. B. für *Data Dictionary* (`AID_DD`) oder *Process Variable* (`AID_PV`). Während diese ähnlich aufgebaut sind, ist `AID_NV` speziell auf *Number Variable Objects* ausgerichtet und definiert nur die hier benötigte Attribut‑ID. Andere Bausteine können zusätzliche Konstanten für weitere Attribute (z. B. minimaler/maximaler Wert, Einheit) enthalten.

## Fazit

`AID_NV` ist eine einfache, aber wichtige Konstantendefinition für ISOBUS‑Anwendungen. Sie stellt sicher, dass die Attribut‑ID für den aktuellen Wert eines *Number Variable Object* einheitlich und fehlerfrei referenziert wird. Durch die Einbettung in eine globale Konstantenstruktur wird die Wartbarkeit und Lesbarkeit des Gesamtsystems verbessert.
