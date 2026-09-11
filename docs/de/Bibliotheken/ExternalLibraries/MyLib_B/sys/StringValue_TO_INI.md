# StringValue_TO_INI


![StringValue_TO_INI_network](./StringValue_TO_INI_network.svg)

![StringValue_TO_INI](./StringValue_TO_INI.svg)

* * * * * * * * * *
## Einleitung

Der Funktionsblock **StringValue_TO_INI** ist eine generische Subapplikation, die einen String‑Wert von einem ISOBUS‑Gerät einliest und in einer INI‑Datei persistent speichert. Die Konfiguration erfolgt über die Angabe von Sektion, Key und der ISOBUS‑Objekt‑ID. Zusätzlich wird beim Start der gespeicherte Wert aus der INI‑Datei gelesen und über den Ausgang `VALUEO` bereitgestellt. Der Baustein ist als Wiederverwendungsmodul ausgelegt und kapselt die Kommunikation mit dem ISOBUS sowie den Zugriff auf das INI‑Dateisystem.

## Schnittstellenstruktur

### **Ereignis-Eingänge**
Keine Ereignis-Eingänge vorhanden.

### **Ereignis-Ausgänge**
- **IND** – Signalisiert, dass entweder ein Wert in die INI‑Datei geschrieben oder der gespeicherte Wert gelesen und ausgegeben wurde.

### **Daten-Eingänge**
- **KEY** (`STRING`) – Bezeichner des Schlüssels innerhalb der Sektion, unter dem der Wert gespeichert wird.
- **SECTION** (`STRING`) – Name der Sektion in der INI‑Datei.
- **u16ObjId** (`UINT`, Initialwert `ID_NULL`) – ISOBUS‑Objekt‑ID, unter der der String‑Wert vom Bus gelesen wird.

### **Daten-Ausgänge**
- **VALUEO** (`STRING`) – Ausgang, der den zuletzt gelesenen Wert aus der INI‑Datei bzw. den gerade verarbeiteten Wert bereitstellt.

### **Adapter**
Keine Adapter vorhanden.

## Funktionsweise

Die Subapplikation kombiniert drei Funktionsblöcke:

- **StringValue_IS** – Liest einen String‑Wert von einem ISOBUS‑Gerät basierend auf der angegebenen `u16ObjId`.
- **INI** – Speichert bzw. liest Werte aus einer INI‑Datei unter der angegebenen `SECTION` und `KEY`.
- **Q_StringValue** – Ein Warteschlangen‑Baustein, der den gelesenen Wert puffert (der konkret genutzte Zweck ist derzeit nicht vollständig ausgebaut, dient aber zur Vorbereitung auf spätere Erweiterungen).

**Ablauf:**

1. **Initialisierung:** Nach dem Start wird das Event `INITO` des INI‑Bausteins ausgelöst, das intern einen `GET`‑Vorgang startet. Dadurch wird der unter `KEY`/`SECTION` gespeicherte Wert aus der INI‑Datei gelesen. Das `GETO`‑Event wird anschließend an den `Q_StringValue` (REQ) und auch an den Ausgang `IND` weitergeleitet. Der Wert liegt gleichzeitig am Datenausgang `VALUEO` an.

2. **Schreiben eines neuen Werts:** Sobald `StringValue_IS` einen neuen String‑Wert vom ISOBUS empfängt (Event `IND` von `StringValue_IS`), wird dieser über die Datenverbindung an den `INI`‑Baustein übergeben und dort durch das Event `SET` gespeichert. Nach erfolgreichem Schreiben wird das Event `SETO` von `INI` an den Ausgang `IND` weitergegeben.

3. **Lesen nach einem Leseimpuls:** Der INI‑Baustein kann auch durch ein externes `GET`‑Event (das intern über die Init‑Schleife erzeugt wird) angestoßen werden. Jedes `GETO`‑Event triggert den `Q_StringValue` und gibt den gelesenen Wert über `VALUEO` aus sowie über `IND` als Ereignis.

Die Parameter `KEY`, `SECTION` und `u16ObjId` werden während der Laufzeit über die Eingänge gesetzt und können so für verschiedene Anwendungen dynamisch konfiguriert werden.

## Technische Besonderheiten

- **ISOBUS‑Integration:** Über den Baustein `StringValue_IS` wird die Kommunikation mit dem ISOBUS abgewickelt. Die Objekt‑ID (`u16ObjId`) bestimmt, welcher String‑Wert vom Bus gelesen wird.
- **Generische INI‑Speicherung:** Die Verwendung von `INI` ermöglicht das Speichern in beliebigen Sektionen und Schlüsseln. Der Baustein unterstützt die Angabe eines Default‑Werts, der beim ersten Zugriff verwendet wird.
- **Warteschlangen‑Baustein:** `Q_StringValue` dient als Puffer für den gelesenen String‑Wert und kann in erweiterten Szenarien weitere Verarbeitungsschritte anstoßen.
- **Wiederverwendbarkeit:** Durch die ausgelagerte Implementierung kann der Baustein in verschiedenen Projekten ohne Anpassungen eingesetzt werden.

## Zustandsübersicht

Eine explizite Zustandsmaschine ist in der Subapp nicht implementiert. Der Ablauf lässt sich jedoch in folgende Zustände gliedern:

- **Initialisierungszustand:** Beim Start wird einmalig ein `GET` ausgeführt und der gespeicherte Wert über `VALUEO` ausgegeben.
- **Wartezustand:** Nach der Initialisierung wartet der Baustein auf ein neues Event von `StringValue_IS` oder auf einen internen `GET`‑Impuls (der gegenwärtig nur durch die Init‑Schleife erzeugt wird).
- **Schreibzustand:** Bei Eintreffen eines neuen String‑Werts wird dieser in die INI‑Datei geschrieben und das `SETO`‑Event ausgelöst.
- **Lesezustand:** Bei jedem `GET`‑Event wird der aktuelle Wert aus der INI‑Datei gelesen und über `VALUEO` sowie `IND` ausgegeben.

## Anwendungsszenarien

- **Persistente Speicherung von ISOBUS‑Einstellungen:** Geräteparameter wie z. B. Maschineneinstellungen, die über ISOBUS gelesen werden, können dauerhaft in einer INI‑Datei abgelegt werden, damit sie nach einem Neustart wiederhergestellt werden.
- **Konfigurationsverwaltung:** Soll ein Wert aus einer zentralen Konfigurationsdatei in eine ISOBUS‑Steuerung zurückgeschrieben werden, kann der Baustein um eine Rückrichtung erweitert werden.
- **Test‑ und Diagnosefunktionen:** Der gelesene Wert kann über `VALUEO` an übergeordnete Überwachungssysteme weitergegeben werden, während die Speicherung für spätere Auswertungen erfolgt.

## Vergleich mit ähnlichen Bausteinen

- **Direkte INI‑Zugriffe:** Im Gegensatz zu Bausteinen, die nur direkt auf eine INI‑Datei zugreifen, integriert `StringValue_TO_INI` den ISOBUS‑Datenfluss. Das erspart dem Anwender die manuelle Verbindung von ISOBUS‑Lese‑ und INI‑Schreibfunktionen.
- **Generische String‑Speicherbausteine:** Andere Bausteine erwarten möglicherweise feste Sektionen oder Schlüssel; dieser Baustein erlaubt die dynamische Konfiguration über die Eingänge.
- **ISOBUS‑spezifische Speicherlösungen:** Es existieren spezielle ISOBUS‑Speicherbausteine, die aber oft nur die Objekt‑ID speichern, nicht den Wert selbst. Vorliegender Baustein speichert den Wert und lässt sich flexibel einsetzen.

## Fazit

Der Funktionsblock **StringValue_TO_INI** stellt eine robuste und wiederverwendbare Lösung dar, um ISOBUS‑Stringwerte dauerhaft in INI‑Dateien zu speichern und bei Bedarf wieder auszulesen. Durch die klare Schnittstellenstruktur und die interne Verwendung etablierter Bibliotheksbausteine lässt er sich einfach in bestehende Projekte integrieren. Die Kombination aus ISOBUS‑Anbindung und generischer INI‑Konfiguration macht ihn zu einem nützlichen Werkzeug für Anwendungen, die eine dauerhafte Speicherung von Buskommunikationswerten erfordern.