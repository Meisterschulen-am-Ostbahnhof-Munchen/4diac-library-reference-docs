# AX_E_PERMIT_3

![AX_E_PERMIT_3](./AX_E_PERMIT_3.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock `AX_E_PERMIT_3` dient der selektiven Weiterleitung von drei unabhängigen Ereigniskanälen unter einer gemeinsamen Freigabebedingung. Er ist als generischer Baustein ausgelegt und verwendet einen unidirektionalen Adapter, um das Freigabesignal zu empfangen. Nur wenn die Freigabe aktiv ist, werden eingehende Ereignisse an die entsprechenden Ausgänge durchgereicht; andernfalls werden sie ignoriert. Dadurch eignet sich der Baustein für Anwendungen, bei denen Ereignisströme zentral gesperrt oder ermöglicht werden sollen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **EI1**: Event input channel 1  
- **EI2**: Event input channel 2  
- **EI3**: Event input channel 3  

### **Ereignis-Ausgänge**

- **EO1**: Event output channel 1  
- **EO2**: Event output channel 2  
- **EO3**: Event output channel 3  

### **Daten-Eingänge**

Keine Daten-Eingänge vorhanden.

### **Daten-Ausgänge**

Keine Daten-Ausgänge vorhanden.

### **Adapter**

- **PERMIT**: Typ `adapter::types::unidirectional::AX`  
  Kommentar: „Permit condition adapter input"  
  Dieser unidirektionale Adapter liefert die Freigabebedingung in den Baustein hinein. Typischerweise handelt es sich um ein boolesches Signal (TRUE = freigeben, FALSE = sperren).

## Funktionsweise

Der Baustein überwacht kontinuierlich den Wert am Adapter-Eingang `PERMIT`. Sofern an diesem Eingang eine aktive Freigabe (z. B. TRUE) anliegt, werden ankommende Ereignisse an den jeweiligen Ausgangskanal weitergegeben:

- Ein Ereignis an `EI1` wird dann an `EO1` durchgereicht.
- Ein Ereignis an `EI2` wird dann an `EO2` durchgereicht.
- Ein Ereignis an `EI3` wird dann an `EO3` durchgereicht.

Liegt keine Freigabe vor, werden alle Ereignisse verworfen und es erfolgt keinerlei Ausgabe. Die drei Kanäle arbeiten unabhängig voneinander, die Freigabe gilt jedoch für alle gleichzeitig. Es gibt keine interne Pufferung oder zeitliche Verzögerung; die Weitergabe erfolgt unmittelbar bei Eintreffen eines Ereignisses, sofern die Bedingung erfüllt ist.

## Technische Besonderheiten

- Der Baustein ist als **generischer Funktionsblock** definiert (Attribut `eclipse4diac::core::GenericClassName` = `'GEN_AX_E_PERMIT'`). Dadurch kann er je nach Kontext mit verschiedenen konkreten Adaptertypen instanziiert werden, solange diese das in der Adapterdefinition festgelegte Interface erfüllen.
- Es werden keine Daten-Eingänge oder -Ausgänge verwendet – ausschließlich Ereignisse und ein Adapter. Dies reduziert die Komplexität und macht den Baustein für reine Ereignissteuerung geeignet.
- Der Adapter ist **unidirektional**, d.h. er überträgt nur Daten in den Baustein hinein; eine Rückkopplung oder ein bidirektionaler Datenaustausch ist nicht vorgesehen.
- Die Schnittstelle ist auf drei Ereigniskanäle ausgelegt, was typische Anforderungen an eine Mehrfachfreigabe in Automatisierungssystemen abdeckt.

## Zustandsübersicht

Da der Baustein rein ereignisgesteuert ist, besitzt er keine klassischen internen Zustände im Sinne eines Moore- oder Mealy-Automaten. Das Verhalten wird direkt durch den Pegel am Adapter `PERMIT` bestimmt:  
- **Freigabe aktiv** (z.B. TRUE): Ereignisse werden transparent durchgeschaltet.  
- **Freigabe inaktiv** (z.B. FALSE): Ereignisse werden blockiert und verworfen.

Ein Umspringen der Freigabe hat keine verzögerte Wirkung – die aktuelle Bedingung gilt sofort für alle nachfolgenden Ereignisse.

## Anwendungsszenarien

- **Sicherheitsgerichtete Steuerungen**: Ereignisse (z.B. Not-Aus-Befehle) dürfen nur bei gesetztem Freigabesignal weitergeleitet werden.
- **Modulare IEC 61499-Anwendungen**: Kopplung von Funktionsblöcken, wenn die Weitergabe von Ereignissen an eine übergeordnete Berechtigung gebunden ist.
- **Test- und Simulationssysteme**: Leichtes Ein- und Ausschalten von Ereignispfaden zu diagnostischen Zwecken.
- **Mehrkanalige Verarbeitung**: Wenn mehrere parallele Abläufe gleichzeitig freigegeben oder gesperrt werden sollen.

## Vergleich mit ähnlichen Bausteinen

Gegenüber einem einfachen `E_PERMIT` (das nur einen Ereigniskanal bedient) erweitert `AX_E_PERMIT_3` die Funktionalität auf drei Kanäle, wobei die Freigabe zentral über einen Adapter erfolgt. Im Unterschied zu einem `E_SWITCH` oder Multiplexer wählt der Baustein nicht zwischen verschiedenen Quellen, sondern unterbricht die Signalweitergabe vollständig – es findet keine Umschaltung, sondern nur eine Freigabe/Sperre statt. Dadurch ist er ideal für Anwendungen, bei denen eine strikte Unterbrechung der Ereignisfolge gefordert ist. Die Adapter-basierte Anbindung der Freigabe unterscheidet ihn von Bausteinen mit direktem boolschen Eingang und erhöht die Flexibilität bei modularen Designs.

## Fazit

`AX_E_PERMIT_3` ist ein kompakter und wiederverwendbarer Funktionsbaustein zur zentralen Freigabe dreier Ereigniskanäle. Die Verwendung eines unidirektionalen Adapters entkoppelt die Freigabelogik vom eigentlichen Ereignisfluss und ermöglicht eine saubere Modellierung in IEC 61499-Systemen. Durch den generischen Aufbau lässt sich der Baustein in unterschiedlichen Kontexten einsetzen, ohne dass die Kernlogik verändert werden muss. Damit stellt er ein praktisches Werkzeug für alle Szenarien dar, in denen Ereignisse nur unter bestimmten Bedingungen wirksam werden dürfen.