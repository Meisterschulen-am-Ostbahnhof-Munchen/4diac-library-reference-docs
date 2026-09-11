# AID_EIA

![AID_EIA](./AID_EIA.svg)

* * * * * * * * * *
## Einleitung

Der Baustein `AID_EIA` (Extended Input Attributes Object Attribute IDs) definiert globale Konstanten zur Identifizierung von Attribut-IDs im Kontext erweiterter Eingabeattribute. Er dient als zentrale Referenz für eindeutige Werte, die in ISOBUS-Anwendungen verwendet werden, um Validierungsarten oder ähnliche Attributkennungen festzulegen. Durch die Kapselung dieser Konstanten wird eine konsistente und wiederverwendbare Basis für weitere Bausteine geschaffen.

## Schnittstellenstruktur

Da `AID_EIA` ein **GlobalConstants**-Baustein ist, besitzt er **keine** Ereignis-, Daten- oder Adapteranschlüsse. Stattdessen stellt er ausschließlich globale Konstanten bereit, die von anderen Bausteinen über ihre Namen referenziert werden können.

### **Ereignis-Eingänge**
Keine.

### **Ereignis-Ausgänge**
Keine.

### **Daten-Eingänge**
Keine.

### **Daten-Ausgänge**
Keine.

### **Adapter**
Keine.

## Funktionsweise

`AID_EIA` aggregiert Konstanten, die als unveränderliche Werte im globalen Kontext einer 4diac-Anwendung zur Verfügung stehen. Die definierte Konstante `VALTYPE` besitzt den Datentyp `USINT` (8-Bit-unsigned Integer) und ist mit dem Wert `1` initialisiert. Dieser Wert repräsentiert laut Kommentar eine "Validation Type", wobei `1` bedeutet, dass ungültige Zeichen aufgelistet sind (im Gegensatz zu `0`, wo gültige Zeichen aufgeführt würden). Durch die Bereitstellung als globale Konstante können andere Funktionsbausteine auf diesen Wert zugreifen, ohne ihn lokal duplizieren zu müssen.

## Technische Besonderheiten

- **Struktur**: Der Baustein ist als `GlobalConstants`-Objekt implementiert, nicht als klassischer FB mit einem Zustandsautomaten. Er wird nicht instanziiert, sondern direkt über den qualifizierten Namen referenziert (z. B. `AID_EIA.VALTYPE`).
- **Compiler-Informationen**: Der Baustein ist dem Compiler-Paket `isobus::UT::Q::const::AID` zugeordnet, was auf eine Verwendung im ISOBUS-Umfeld hindeutet.
- **Typisierung**: Die Konstante ist strikt typisiert (`USINT`), wodurch Typkonflikte bei der Verwendung vermieden werden.
- **Initialwert**: Der vordefinierte Startwert (`1`) gibt eine Standardkonfiguration vor, die in der Regel nicht verändert werden sollte, da es sich um eine semantische Festlegung handelt.

## Zustandsübersicht

Da `AID_EIA` keine Logik oder Ereignisbehandlung besitzt, existiert **kein Zustandsdiagramm** und keine Laufzeit-Zustände. Der Baustein ist statisch und liefert nur konstante Daten.

## Anwendungsszenarien

- **Validierung von Eingaben**: Die Konstante `VALTYPE` kann in Funktionen verwendet werden, die die Gültigkeit von Zeichenfolgen prüfen. Beispielsweise kann ein anderer FB den Wert abfragen, um zu entscheiden, ob eine Validierung als "Whitelist" (gültige Zeichen aufgelistet) oder "Blacklist" (ungültige Zeichen aufgelistet) erfolgt.
- **Konfiguration von Attributedefinitionen**: Im ISOBUS-Kontext können erweiterte Eingabeattribute verwendet werden, um gerätespezifische Eigenschaften zu beschreiben. `AID_EIA` stellt die zugehörigen IDs bereit.
- **Vereinheitlichung von Konstanten:** Durch die zentrale Ablage werden Duplikate und Inkonsistenzen vermieden, was die Wartung größerer Steuerungsprojekte erleichtert.

## Vergleich mit ähnlichen Bausteinen

Andere `GlobalConstants`-Bausteine (z. B. `AID_K`, `AID_V` oder ähnliche) definieren ebenfalls Konstanten für unterschiedliche Attributgruppen. Im Gegensatz zu solchen Bausteinen fokussiert `AID_EIA` spezifisch auf die „Extended Input Attributes“ und bietet nur eine einzelne, aber klar definierte Konstante. Während andere Bausteine möglicherweise mehrere Konstanten bündeln, hebt sich `AID_EIA` durch seine minimale und spezialisierte Ausrichtung ab.

## Fazit

`AID_EIA` ist ein schlanker GlobalConstants-Baustein, der eine essenzielle Konstante für die Validierungstypen in erweiterten Eingabeattributen bereitstellt. Seine Verwendung fördert die Lesbarkeit und Wartbarkeit von 4diac-Applikationen, da semantische Werte zentral und typisiert definiert werden. Obwohl er keine prozessurale Logik besitzt, ist er ein wichtiger Baustein für die konsistente Konfiguration in ISOBUS-basierten Automatisierungslösungen.