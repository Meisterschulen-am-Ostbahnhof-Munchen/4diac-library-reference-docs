# AID_BO

![AID_BO](./AID_BO.svg)

* * * * * * * * * *
## Einleitung  
Der globale Konstantenbaustein **AID_BO** definiert die Attribut-IDs für Button-Objekte im ISOBUS-Kontext. Er stellt numerische Konstanten bereit, die zur Identifizierung und Konfiguration von Button-Eigenschaften verwendet werden.

## Schnittstellenstruktur  
### **Ereignis-Eingänge**  
Keine (GlobalConstants-Baustein)  

### **Ereignis-Ausgänge**  
Keine  

### **Daten-Eingänge**  
Keine  

### **Daten-Ausgänge**  
Keine  

### **Adapter**  
Keine  

## Funktionsweise  
Der Baustein enthält sechs Konstanten vom Typ `USINT`, die jeweils eine eindeutige Attribut-ID für ein Button-Objekt repräsentieren. Die Werte sind fest verdrahtet und können in ISOBUS-Programmen direkt referenziert werden, um auf die entsprechenden Attribute zuzugreifen. Die IDs sind wie folgt definiert:  

| Name                | Wert | Beschreibung                                                                 |
|---------------------|------|------------------------------------------------------------------------------|
| `WIDTH`             | 1    | Breite in Pixeln                                                             |
| `HEIGHT`            | 2    | Höhe in Pixeln                                                               |
| `BACKGROUND_COLOUR` | 3    | Index der Hintergrundfarbe                                                   |
| `BORDER_COLOUR`     | 4    | Index der Rahmenfarbe                                                        |
| `KEY_CODE`          | 5    | Tastencode                                                                   |
| `OPTIONS`           | 6    | Bitmaske für Optionen (Latch, Zustand, Unterdrückung, Transparenz, etc.)     |

## Technische Besonderheiten  
- Verwendung des Typs `USINT` (8-Bit unsigned integer) für kompakte Speicherung.  
- Die Konstanten sind im Paket `isobus::UT::Q::const::AID` organisiert.  
- Die `OPTIONS`-Konstante enthält eine Bitmaske, deren einzelne Bits spezifische Eigenschaften steuern:  
  - Bit 0: latchable  
  - Bit 1: Zustand (0 = losgelassen, 1 = verriegelt)  
  - Bit 2: Rahmen unterdrücken  
  - Bit 3: transparenter Hintergrund  
  - Bit 4: deaktiviert  
  - Bit 5: kein Rahmen  

## Zustandsübersicht  
Nicht zutreffend – es handelt sich um einen Konstantenbaustein ohne Laufzeitlogik oder Zustände.

## Anwendungsszenarien  
- In ISOBUS-Bediengeräten zur Definition und Anpassung von Button-Eigenschaften.  
- Als zentrale Referenz für Attribut-IDs in Applikationen, die auf das UT (Universal Terminal) zugreifen.  
- Zur Sicherstellung konsistenter Attributwerte über verschiedene Module hinweg.

## Vergleich mit ähnlichen Bausteinen  
Es existieren weitere globale Konstantenblöcke für andere Objekttypen (z. B. `AID_AL` für Alarme, `AID_OP` für Operationen), die nach demselben Muster aufgebaut sind. `AID_BO` ist auf Buttons spezialisiert und liefert die entsprechenden Attribut-IDs.

## Fazit  
Der Baustein `AID_BO` bietet eine einfache und typsichere Möglichkeit, die standardisierten Attribut-IDs für ISOBUS-Buttons bereitzustellen. Durch die zentrale Definition wird die Wartbarkeit und Fehlervermeidung in der Anwendungsentwicklung verbessert.