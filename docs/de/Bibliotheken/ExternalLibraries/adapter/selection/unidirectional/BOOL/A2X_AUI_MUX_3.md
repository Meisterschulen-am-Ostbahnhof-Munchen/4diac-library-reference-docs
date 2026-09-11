# A2X_AUI_MUX_3

![A2X_AUI_MUX_3](./A2X_AUI_MUX_3.svg)

* * * * * * * * * *

## Einleitung

Der A2X_AUI_MUX_3 ist ein generischer Multiplexer-Baustein auf Basis unidirektionaler Adapter. Er wählt anhand eines Auswahlindexes K einen von drei A2X-Eingängen aus und stellt den zugehörigen Wert am Ausgang OUT bereit. Der Ausgang wird nur bei einer tatsächlichen Wertänderung aktualisiert; nur in diesem Fall wird das Ereignis CNF gesendet. Dadurch eignet er sich für wartungsarme und ereignisreduzierte Signalumschaltungen in IEC-61499-Applikationen.

## Schnittstellenstruktur

Der Baustein besitzt keine separaten Daten-Ein- oder Daten-Ausgänge. Alle Werte und der Auswahlindex werden über unidirektionale Adapteranschlüsse übertragen.

### **Ereignis-Eingänge**

Es sind keine Ereignis-Eingänge vorhanden.

### **Ereignis-Ausgänge**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| CNF | Event | Bestätigt die Übernahme des Auswahlindexes K. Wird nur bei einer tatsächlichen Wertänderung am Ausgang OUT ausgelöst. |

### **Daten-Eingänge**

Keine. Die Eingangswerte werden über die Adapter IN1, IN2 und IN3 eingelesen.

### **Daten-Ausgänge**

Keine. Der Ausgangswert wird über den Adapter OUT bereitgestellt.

### **Adapter**

| Name | Richtung | Typ | Beschreibung |
|------|----------|-----|--------------|
| K | Socket | adapter::types::unidirectional::AUI | Auswahlindex: K = 0 → IN1, K = 1 → IN2, K = 2 → IN3 |
| IN1 | Socket | adapter::types::unidirectional::A2X | Erster Eingangswert, ausgewählt bei K = 0 |
| IN2 | Socket | adapter::types::unidirectional::A2X | Zweiter Eingangswert, ausgewählt bei K = 1 |
| IN3 | Socket | adapter::types::unidirectional::A2X | Dritter Eingangswert, ausgewählt bei K = 2 |
| OUT | Plug | adapter::types::unidirectional::A2X | Ausgang; liefert den Wert des ausgewählten Eingangs |

## Funktionsweise

Der A2X_AUI_MUX_3 arbeitet als gesteuerter Multiplexer:

1. Über den AUI-Adapter K wird der gewünschte Eingangskanal ausgewählt.
2. Der Wert des gewählten A2X-Eingangs wird mit dem aktuellen Ausgangswert an OUT verglichen.
3. Bei einer Abweichung wird OUT auf den neuen Wert gesetzt.
4. Nach der Aktualisierung wird das Ereignis CNF gesendet.
5. Bleibt der Wert unverändert, wird OUT nicht aktualisiert und es wird kein CNF ausgelöst.

Die Werteübertragung erfolgt ausschließlich über die unidirektionalen Adapter. Der FB benötigt keinen separaten Ereignis-Eingang, da die Verarbeitung über die an den Adapter-Sockets anliegenden Werte angestoßen wird.

## Technische Besonderheiten

- Der Baustein ist als generischer Funktionsblock deklariert und verwendet den GenericClassName `'GEN_A2X_AUI_MUX'`.
- Er ist dem Paket `adapter::selection::unidirectional` zugeordnet.
- Die Kommunikation erfolgt vollständig über Adapter; klassische Datenports werden nicht verwendet.
- Der Ausgang wird nur bei tatsächlicher Wertänderung aktualisiert.
- Das Ereignis CNF wird ebenfalls nur bei einer echten Wertänderung ausgelöst.
- Der Auswahlindex K ist für die Werte 0, 1 und 2 definiert. Werte außerhalb dieses Bereichs müssen in der Anwendung vermieden werden.

## Zustandsübersicht

Die XML-Deklaration des Bausteins enthält keine explizite Zustandsmaschine. Das Laufzeitverhalten wird durch das generische Backend `GEN_A2X_AUI_MUX` bereitgestellt. Ablauflogisch lässt sich das Verhalten wie folgt beschreiben:

| Zustand | Beschreibung | Aktion |
|---------|---------------|--------|
| IDLE | Warten auf einen neuen Auswahlindex oder einen geänderten Eingangswert | Keine Ausgabe |
| COMPARE | Vergleich des gewählten Eingangswerts mit dem aktuellen OUT-Wert | Keine Ausgabe |
| UPDATE | Der neue Wert wird an OUT übernommen | OUT wird aktualisiert |
| CONFIRM | Nach erfolgreicher Aktualisierung wird der Vorgang bestätigt | CNF wird gesendet |
| NO_UPDATE | Kein Unterschied zwischen gewähltem Eingangswert und OUT | Kein CNF, OUT bleibt unverändert |

## Anwendungsszenarien

- **Signalumschaltung:** Auswahl zwischen drei verschiedenen Sensor- oder Prozesssignalen über den Index K.
- **Betriebsartenwahl:** K = 0 für Normalbetrieb, K = 1 für Wartungsmodus, K = 2 für Testsignal.
- **Redundanzumschaltung:** Bei Ausfall eines Sensors kann per K auf einen Ersatzsensor umgeschaltet werden.
- **Simulation und Test:** Umschaltung zwischen realen Prozesswerten und simulierten Werten in einer Anlage.

## Vergleich mit ähnlichen Bausteinen

| Baustein | Eigenschaft |
|----------|-------------|
| A2X_AUI_MUX_2 | Multiplexer-Variante mit zwei A2X-Eingängen |
| A2X_AUI_MUX_3 | Drei Eingänge, AUI-Indexauswahl, Change-only-Ausgabe |
| A2X_AUI_MUX_4 | Multiplexer-Variante mit vier A2X-Eingängen |
| AX_AUI_MUX_3 | Variante mit anderem Adaptertyp; ohne Change-only-Verhalten oder mit abweichender Ereignislogik |
| A2X_AUI_DEMUX | Inverse Funktion: Verteilt einen Eingangswert auf mehrere Ausgänge |

## Fazit

Der A2X_AUI_MUX_3 realisiert eine kompakte, adapterbasierte 3-zu-1-Auswahl für IEC-61499-Anwendungen. Durch die Aktualisierung nur bei tatsächlicher Wertänderung und die zugehörige CNF-Bestätigung eignet er sich besonders für effiziente, ereignisarme und modular aufgebaute Automatisierungslösungen.
