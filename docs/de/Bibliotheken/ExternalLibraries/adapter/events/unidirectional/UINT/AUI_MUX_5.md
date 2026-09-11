# AUI_MUX_5

![AUI_MUX_5](./AUI_MUX_5.svg)

* * * * * * * * * *
## Einleitung
Der Funktionsblock **AUI_MUX_5** ist ein Ereignis-Multiplexer, der fünf Ereignis-Eingänge (EI1–EI5) besitzt und über einen Adapter-Ausgang (Typ `adapter::types::unidirectional::AUI`) ein ausgewähltes Ereignis an eine nachgeschaltete Einheit weitergibt. Er stellt eine Adapter-basierte Variante des klassischen E_MUX_5 dar und ermöglicht eine flexible, ereignisgesteuerte Auswahl ohne separate Ereignis-Ausgänge.

## Schnittstellenstruktur
Der FB besitzt ausschließlich Ereignis-Eingänge und einen Adapter-Plug. Es sind keine Daten-Eingänge, Daten-Ausgänge oder Ereignis-Ausgänge vorhanden.

### **Ereignis-Eingänge**
- **EI1** – Ereignis, das bei Auswahlindex 0 (K=0) weitergeleitet wird.
- **EI2** – Ereignis, das bei Auswahlindex 1 (K=1) weitergeleitet wird.
- **EI3** – Ereignis, das bei Auswahlindex 2 (K=2) weitergeleitet wird.
- **EI4** – Ereignis, das bei Auswahlindex 3 (K=3) weitergeleitet wird.
- **EI5** – Ereignis, das bei Auswahlindex 4 (K=4) weitergeleitet wird.

### **Ereignis-Ausgänge**
Keine vorhanden. Die Ereignisweiterleitung erfolgt ausschließlich über den Adapter.

### **Daten-Eingänge**
Keine vorhanden.

### **Daten-Ausgänge**
Keine vorhanden.

### **Adapter**
- **K** (Plug) – Adapter vom Typ `adapter::types::unidirectional::AUI`. Über diesen Adapter wird das ausgewählte Ereignis ausgegeben und gegebenenfalls der aktuelle Auswahlindex (0–4) bereitgestellt.

## Funktionsweise
Der Funktionsblock arbeitet als Ereignis-Multiplexer. Der über den Adapter **K** bereitgestellte Auswahlindex bestimmt, welcher der fünf Ereignis-Eingänge bei einem anliegenden Ereignis aktiviert und über den Adapter nach außen weitergegeben wird.  
- Wenn K den Wert 0 hat, wird ein an EI1 ankommendes Ereignis über den Adapter ausgegeben.  
- Bei K=1 wird EI2 durchgeschaltet, bei K=2 EI3, bei K=3 EI4 und bei K=4 EI5.  

Ein Ereignis an einem nicht ausgewählten Eingang wird ignoriert. Der Adapter stellt dabei sowohl das Ereignis als auch den verwendeten Index bereit.

## Technische Besonderheiten
- **Generische Implementierung**: Der Baustein ist als generischer FB mit dem Klassennamen `GEN_E_MUX` definiert, wodurch eine parametrisierbare Nutzung möglich ist.
- **Adapter-basierte Ausgabe**: Statt diskreter Ereignis-Ausgänge (EO1–EO5) wird ein einzelner unidirektionaler Adapter verwendet. Dies vereinfacht die Verbindung zu Schnittstellen, die einen einheitlichen Event-Kanal erwarten.
- **Keine Datenhaltung**: Der FB ist rein ereignisgesteuert und besitzt keine internen Datenzustände.

## Zustandsübersicht
Da der Baustein rein ereignisbasiert arbeitet, existiert kein expliziter Zustandsautomat. Der interne Zustand wird ausschließlich durch den aktuellen Auswahlindex K bestimmt, der über den Adapter gelesen wird. Die Verarbeitung erfolgt ereignisgetrieben: Jedes ankommende Eingangsereignis wird abhängig von K entweder weitergeleitet oder verworfen.

## Anwendungsszenarien
- **Ereignis-Routing**: Auswahl einer von mehreren Ereignisquellen (z. B. Sensoren, Alarme) und Weiterleitung über einen gemeinsamen Adapterausgang an eine Verarbeitungseinheit.
- **Modulare Systemkopplung**: Einsatz in Systemen, bei denen eine einheitliche Adapter‑Schnittstelle (z. B. für Bus-Kommunikation) verwendet wird und mehrere Ereignisquellen dynamisch zugeschaltet werden sollen.
- **Test- und Simulationsumgebungen**: Gezielte Steuerung, welches Ereignis aktiviert wird, ohne die Verdrahtung zu ändern.

## Vergleich mit ähnlichen Bausteinen
- **E_MUX_5** (Standard): Besitzt fünf Ereignis-Eingänge und fünf separate Ereignis-Ausgänge (EO1–EO5). Die Auswahl erfolgt über einen Daten-Eingang K.  
  **Unterschied**: AUI_MUX_5 verwendet einen Adapter-Ausgang statt einzelner Ereignis-Ausgänge. Dadurch wird die Schnittstelle kompakter und besser für Verbindungen mit Adapter-basierten Komponenten geeignet.
- **AUI_MUX_N** (verallgemeinert): Kann mehrere Eingänge besitzen; AUI_MUX_5 ist eine spezielle Instanz mit fünf Eingängen.

Der Hauptvorteil der Adapter-Variante liegt in der einheitlichen Schnittstellengestaltung, die insbesondere dann nützlich ist, wenn adäquate Adapter-Typen im Projekt bereits verwendet werden.

## Fazit
Der **AUI_MUX_5** ist ein spezialisierter Ereignis-Multiplexer, der die Vorteile einer Adapter-basierten Ausgabe mit einer klaren, fünf Eingängen umfassenden Ereignisauswahl verbindet. Durch die Reduktion auf einen Adapterausgang wird die Verdrahtung vereinfacht und die Wiederverwendbarkeit in Adapter-orientierten Architekturen erhöht. Die generische Ausführung erlaubt zudem eine flexible Anpassung an unterschiedliche Anforderungen.