# AX_E_PERMIT_2

![AX_E_PERMIT_2](./AX_E_PERMIT_2.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **AX_E_PERMIT_2** dient zur permissiven Weiterleitung von zwei Ereigniskanälen. Er ist als generischer Baustein konzipiert und verwendet einen Adapter, um die Ausführung der Ereignisse an eine externe Bedingung zu koppeln. Die Ereignisse EI1 und EI2 werden nur dann an die Ausgänge EO1 und EO2 weitergegeben, wenn der Adapter die Freigabe erteilt. Dadurch eignet sich der Baustein zur Steuerung von Abläufen in IEC 61499-Applikationen, bei denen Ereignisse gezielt unterbrochen oder blockiert werden müssen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **EI1** (Event): Erster Ereignis-Eingang, der bei Freigabe an den Ausgang EO1 weitergeleitet wird.
- **EI2** (Event): Zweiter Ereignis-Eingang, der bei Freigabe an den Ausgang EO2 weitergeleitet wird.

### **Ereignis-Ausgänge**

- **EO1** (Event): Ereignis-Ausgang für den Kanal 1, aktiviert nach Freigabe des Adapters.
- **EO2** (Event): Ereignis-Ausgang für den Kanal 2, aktiviert nach Freigabe des Adapters.

### **Daten-Eingänge**

Es sind keine Daten-Eingänge vorhanden.

### **Daten-Ausgänge**

Es sind keine Daten-Ausgänge vorhanden.

### **Adapter**

- **PERMIT** (Socket, Typ `adapter::types::unidirectional::AX`): Dieser Adapter stellt die Freigabebedingung bereit. Nur wenn das Signal des Adapters aktiv ist, werden die Ereignisse durchgeschaltet. Die genaue Logik wird durch den konkreten Adapter-Typ `AX` bestimmt, der eine unidirektionale Kommunikation ermöglicht.

## Funktionsweise

Der Funktionsblock arbeitet als ereignisgesteuerter Pfad mit Freigabe durch den Adapter. Im Normalzustand wartet er auf Ereignisse an EI1 oder EI2. Sobald ein Ereignis eintrifft, wird der Zustand des Adapters PERMIT überprüft. Ist die Bedingung erfüllt (Freigabe aktiv), wird das entsprechende Ereignis an EO1 bzw. EO2 weitergeleitet. Bei nicht aktivem Freigabesignal wird das Ereignis verworfen und es erfolgt keine Ausgabe.

Der Baustein ist generisch aufgebaut, d.h. die eigentliche Freigabelogik wird über den Adaptertyp `AX` und dessen Parameter realisiert. Dies ermöglicht eine flexible Anpassung an verschiedene Anforderungen, ohne die interne Struktur des Bausteins zu verändern.

## Technische Besonderheiten

- **Generische Implementierung**: Durch die Nutzung des Adapters und der Eigenschaft `GenericClassName = 'GEN_AX_E_PERMIT'` ist der Baustein als generischer FB ausgelegt. Er kann mit unterschiedlichen Adapter-Instanzen des Typs `AX` konfiguriert werden.
- **Unidirektionale Adapterkommunikation**: Der Adapter ist vom Typ `adapter::types::unidirectional::AX`, was eine einfache und effiziente Signalübertragung von der aufrufenden zur aufgerufenen Seite ermöglicht.
- **Keine Datenvariablen**: Der Baustein besitzt weder Daten-Ein- noch -Ausgänge, wodurch er sich auf reine Ereignissteuerung beschränkt und keine Datenhaltung erfordert.
- **Skalierbarkeit**: Aufgrund des modularen Aufbaus kann der Baustein leicht erweitert oder angepasst werden, falls mehrere Ereigniskanäle benötigt werden (analog zu `AX_E_PERMIT`).

## Zustandsübersicht

Der Funktionsblock besitzt keine expliziten internen Zustände. Sein Verhalten ist rein kombinatorisch: Die Ausgabe hängt ausschließlich vom aktuellen Eingangsereignis und dem Zustand des Adapters ab. Eine Zustandsänderung erfolgt nur durch den Adapter, der außerhalb des Bausteins definiert wird.

## Anwendungsszenarien

- **Steuerung von Ereignisflüssen**: Einsatz in Produktionsanlagen, bei denen Maschinenbewegungen oder Prozessschritte nur bei erfüllter Sicherheitsbedingung ausgeführt werden dürfen.
- **Bedingte Verarbeitung**: In Automatisierungssystemen kann der Baustein verwendet werden, um Ereignisse nur dann zu verarbeiten, wenn eine übergeordnete Steuerung dies erlaubt (z.B. bei manuellen Eingriffen).
- **Resourcenverwaltung**: Bei der Parallelausführung mehrerer Tasks kann der Baustein als Gate dienen, um Ressourcenkonflikte zu vermeiden.

## Vergleich mit ähnlichen Bausteinen

- **AX_E_PERMIT** (ohne Suffix): Der Baustein `AX_E_PERMIT` ist die Variante für einen einzelnen Ereigniskanal. `AX_E_PERMIT_2` erweitert diese Funktionalität auf zwei unabhängige Kanäle, wobei beide über denselben Adapter freigegeben werden.
- **Event-Propagations-Bausteine**: Im Gegensatz zu einfachen Weiterleitungs-Bausteinen (z.B. `E_DEMUX`) besitzt `AX_E_PERMIT_2` eine zusätzliche Freigabe-Logik. Dadurch ist er besser für sicherheitskritische Anwendungen geeignet.
- **Adapterbasierte Bausteine**: Andere Bausteine mit Adaptern bieten oft eine bidirektionale Kommunikation; dieser Baustein nutzt bewusst eine unidirektionale Schnittstelle, was die Implementierung vereinfacht.

## Fazit

Der Funktionsblock `AX_E_PERMIT_2` ist ein flexibler und kompakter Baustein zur bedarfsgesteuerten Ereignisweiterleitung. Durch seine generische Adapter-Schnittstelle lässt er sich in vielfältigen industriellen Steuerungsumgebungen einsetzen. Die Beschränkung auf zwei Kanäle und den unidirektionalen Adaptertyp macht ihn besonders transparent und leicht wartbar. Er stellt eine effiziente Lösung dar, um Ereignisse gezielt zu blockieren oder zu ermöglichen, und trägt zur robusten Gestaltung von IEC 61499-Applikationen bei.
