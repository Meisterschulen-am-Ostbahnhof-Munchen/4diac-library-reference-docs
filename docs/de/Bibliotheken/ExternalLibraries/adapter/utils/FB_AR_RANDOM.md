# FB_AR_RANDOM

![FB_AR_RANDOM](./FB_AR_RANDOM.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **FB_AR_RANDOM** ist ein Wrapper um den Baustein `FB_RANDOM` aus der Bibliothek `eclipse4diac::utils`. Er erweitert dessen Funktionalität um eine unidirektionale AR‑Adapter‑Schnittstelle (`OUT`), sodass Zufallswerte direkt über einen Adapter an andere Applikationsteile weitergegeben werden können. Dadurch wird eine einfache Integration in adapterbasierte Kommunikationsstrukturen ermöglicht.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (EInit): Initialisierungsanforderung – lädt den Start‑Seed für den Zufallsgenerator.
- **REQ** (Event): Normaler Ausführungsauftrag – löst die Erzeugung eines neuen Zufallswerts aus.

### **Ereignis-Ausgänge**

- **INITO** (EInit): Initialisierungsbestätigung – wird nach erfolgreicher Initialisierung gesendet.

### **Daten-Eingänge**

- **SEED** (UINT, Initialwert `0`): Startwert für den Zufallsgenerator.

### **Daten-Ausgänge**

- (Keine direkten Datenausgänge; der erzeugte Wert wird ausschließlich über den Adapter bereitgestellt.)

### **Adapter**

- **OUT** (Typ `adapter::types::unidirectional::AR`): Ausgangs‑Adapter, der das Ereignis `E1` und die Daten‑Variable `D1` bereitstellt. Über diesen Adapter wird der erzeugte Zufallswert (als `D1`) und ein zugehöriges Ereignis (`E1`) an verbundene Bausteine übertragen.

## Funktionsweise

Der Baustein enthält intern ein FBNetzwerk, in dem eine Instanz von `FB_RANDOM` eingebunden ist. Die Ereignis‑ und Daten‑Verbindungen sind wie folgt realisiert:

- Das **INIT**‑Ereignis wird direkt mit dem **INITO**‑Ausgang verbunden – es erfolgt keine Weitergabe an den internen Zufallsgenerator. Somit dient INIT lediglich der Bestätigung, ohne weitere Aktionen auszulösen.
- Das **REQ**‑Ereignis löst den internen `FB_RANDOM.REQ` aus.
- Das Ausgangsereignis `FB_RANDOM.CNF` (Bestätigung) wird über den Adapter als `OUT.E1` weitergegeben.
- Der **SEED**‑Wert wird direkt an `FB_RANDOM.SEED` übergeben.
- Der erzeugte Zufallswert `FB_RANDOM.VAL` wird an die Adapter‑Variable `OUT.D1` übertragen.

Damit erzeugt der Baustein bei jedem **REQ** einen neuen Zufallswert, der über den Adapter gesendet wird. Die Initialisierung hat keine Auswirkung auf den Zufallsgenerator, da sie nur intern mit `INITO` quittiert wird.

## Technische Besonderheiten

- Der Baustein ist als reiner **Wrapper** implementiert und enthält keine eigene Logik – die eigentliche Zufallserzeugung übernimmt `FB_RANDOM`.
- Die Adapter‑Schnittstelle ist **unidirektional** (`AR` = „Adapter Receive“ bzw. hier als Ausgang), d.h. sie unterstützt nur das Senden von Daten und Ereignissen.
- Der Baustein ist für die Verwendung in adapterbasierten Architekturen optimiert, wie sie typisch für IEC 61499‑Applikationen sind.
- Es sind keine zusätzlichen Attribute oder Konfigurationsparameter außer dem Seed vorhanden.

## Zustandsübersicht

Da der Baustein keinen eigenen Zustandsautomaten besitzt, beschränkt sich die Zustandslogik auf das ereignisgesteuerte Verhalten:

- Nach dem erstmaligen **INIT** wird `INITO` ausgegeben – der Baustein ist dann bereit für `REQ`.
- Bei jedem **REQ** wird ein Zufallswert erzeugt und über `OUT` gesendet. Es gibt keinen dauerhaften internen Zustand; der Seed wird nur beim Start gesetzt und kann über `SEED` beim Aufruf von `REQ` geändert werden.

Der interne `FB_RANDOM` kann je nach Implementierung einen Zustand (z.B. letzter Generatorwert) halten, aber nach außen hin ist der Baustein zustandslos.

## Anwendungsszenarien

- **Simulationen**: Erzeugung von Zufallswerten in Simulationsmodellen, z.B. für stochastische Prozesse oder Testdaten.
- **Adapterbasierte Kommunikation**: Einbindung von Zufallswerten in komplexe Vernetzungen über Adapter, ohne zusätzliche Verdrahtung von Einzel‑Datenpunkten.
- **Test‑ und Validierungsumgebungen**: Bereitstellung von Zufallseingaben für andere Bausteine über einen standardisierten Adapter‑Kanal.

## Vergleich mit ähnlichen Bausteinen

- **Direkter Einsatz von `FB_RANDOM`**: Dieser bietet die gleiche Zufallserzeugung, aber ohne Adapter‑Anschluss. Der Wert muss über zusätzliche Datenverbindungen weitergegeben werden.
- **Alternative Zufallsgeneratoren**: Es gibt andere Bausteine mit unterschiedlichen Algorithmen oder Konfigurationsmöglichkeiten, aber `FB_AR_RANDOM` zeichnet sich durch seine direkte Adapter‑Anbindung aus.
- **Bausteine mit mehreren Ausgängen**: Manche Zufallsgeneratoren liefern mehrere Werte oder sogar Verteilungen, während `FB_AR_RANDOM` einen einzelnen Wert pro Ereignis ausgibt.

## Fazit

**FB_AR_RANDOM** ist ein einfacher, aber effektiver Wrapper, der die Zufallserzeugung von `FB_RANDOM` um eine unidirektionale AR‑Adapter‑Schnittstelle erweitert. Dadurch wird die Integration in adapterbasierte Applikationen erheblich vereinfacht. Der Baustein ist leichtgewichtig, benötigt nur einen Seed‑Parameter und eignet sich hervorragend für Simulations‑ und Testumgebungen, in denen Zufallswerte über Adapter an andere Komponenten verteilt werden sollen.
