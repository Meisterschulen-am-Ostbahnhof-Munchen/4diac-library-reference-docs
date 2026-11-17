# NmGetCfInfo

* * * * * * * * * *

## Einleitung

Der Funktionsblock `NmGetCfInfo` dient innerhalb eines ISOBUS-Netzwerks dazu, Informationen über verbundene Kommunikationspartner (Connected CFs) abzurufen. Er ermöglicht es, sowohl interne als auch externe Teilnehmer im Netzwerk zu identifizieren und deren Details zu erfassen. Der Baustein ist Teil der ISOBUS-PGN-Kommunikation und wird typischerweise für Netzwerkmanagement-Aufgaben eingesetzt.

![NmGetCfInfo](NmGetCfInfo.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **INIT (Typ: EInit)**: Initialisiert den Dienst. Bei Auslösung werden die konfigurierten Daten-Eingänge übernommen und der Initialisierungsprozess gestartet.
-   **RSP (Typ: Event)**: Bestätigt den Empfang einer Indikation (`IND`). Dieses Ereignis signalisiert dem Baustein, dass eine zuvor gesendete Indikation verarbeitet wurde.

### **Ereignis-Ausgänge**

-   **INITO (Typ: EInit)**: Bestätigt den erfolgreichen Abschluss der Initialisierung.
-   **IND (Typ: Event)**: Wird ausgelöst, um eine empfangene Netzwerkindikation (z.B. gefundene Kommunikationspartner) an die Anwendung weiterzugeben.

### **Daten-Eingänge**

-   **u8CanIdx (Typ: USINT, Initialwert: ISO_CAN_NODE::INVALID)**: Filter auf den CAN-Knoten (CAN-Controller-Index). `INVALID` bedeutet: alle CAN-Knoten.
-   **member (Typ: SINT, Initialwert: ISOUSERHOME_e::notdef)**: Filter auf die Art des Teilnehmers (intern oder extern, siehe `eMemberRefer`). `notdef` bedeutet: alle.
-   **address (Typ: isobus::pgn::NAMEFIELD_T)**: Filterwert für den ISOBUS-Namen (NAME) der Teilnehmer. Verglichen werden nur die Bits, die in `mask` gesetzt sind. Es ist keine Adresse, für die Informationen abgerufen werden.
-   **mask (Typ: isobus::pgn::NAMEFIELD_T)**: Bitmaske zu `address`. Nur gesetzte Bits (1) werden verglichen. Mit dem Standardwert (alle Bits 0) passt jeder Teilnehmer.

### **Daten-Ausgänge**

-   **bwaitingForRSP (Typ: BOOL)**: `TRUE` ab dem `IND`, bis die Anwendung mit `RSP` antwortet. Solange es `TRUE` ist, gibt der Baustein kein weiteres `IND` aus.
-   **sNetEv (Typ: isobus::pgn::ISONETEVENT_T)**: Das Netzwerk-Ereignis zum Teilnehmer (siehe Abschnitt „Netzwerk-Ereignisse“), unter anderem `eNetEvent` und `eMemberRefer`.
-   **sCfInfo (Typ: isobus::pgn::CF_INFO_T)**: Die abgerufenen Informationen über den gefundenen Kommunikationspartner (CF = Communication Function).
-   **sNameField (Typ: isobus::pgn::NAMEFIELD_T)**: Der ISOBUS-Name des gefundenen Kommunikationspartners.

### **Adapter**

Dieser Funktionsblock verwendet keine Adapter-Schnittstellen.

## Funktionsweise

1.  **Initialisierung**: Mit `INIT` übernimmt der Baustein die Filter (`u8CanIdx`, `member`, `address`, `mask`). Danach gibt er alle bereits bekannten Teilnehmer, die zum Filter passen, noch einmal als Ereignis `MemberActive` aus (Wiedergabe aus einem Zwischenspeicher im Treiber). So sieht die Anwendung auch Teilnehmer, die schon vor dem Start am Netz waren. Dann folgt `INITO`.
2.  **Meldung (`IND`)**: Der Baustein sucht nicht selbst. Der Stack meldet jede Änderung im Netz, und der Baustein reicht die Meldungen weiter, die zum Filter passen. Jede Meldung löst `IND` aus, mit `sNetEv`, `sCfInfo` und `sNameField`, und setzt `bwaitingForRSP` auf `TRUE`. Das betrifft neue Teilnehmer ebenso wie Abmeldungen, Adresskonflikte und Zeitüberschreitungen.
3.  **Quittierung (`RSP`)**: Die Anwendung muss jedes `IND` mit `RSP` quittieren (Ausgang `IND` mit Eingang `RSP` verbinden). Der Baustein setzt dann `bwaitingForRSP` auf `FALSE` und gibt, falls Meldungen wartend sind, die nächste aus. **Ohne `RSP` kommt kein weiteres `IND`.** Alle folgenden Meldungen, auch Abmeldungen, bleiben in einer Warteschlange im Baustein, die Anwendung sieht abgemeldete Teilnehmer dann nie verschwinden.

## Technische Besonderheiten

-   Der Baustein arbeitet nach dem Muster `IND`/`RSP`: Jede Meldung muss quittiert werden, bevor die nächste kommt.
-   Die Verwendung einer Bit-`mask` ermöglicht flexible Suchanfragen, z.B. nach Teilnehmern einer bestimmten Geräteklasse.
-   Die Datentypen (`ISONETEVENT_T`, `CF_INFO_T`, `NAMEFIELD_T`) sind spezifisch für die ISOBUS-PGN-Kommunikation und enthalten strukturierte Informationen gemäß dem ISO 11783-Standard.

## Netzwerk-Ereignisse

`sNetEv.eNetEvent` gibt an, was passiert ist. `IND` meldet jede dieser Änderungen, nicht nur neue Teilnehmer, die Anwendung muss `eNetEvent` daher auswerten.

| Wert | Ereignis | Bedeutung |
| ---: | --- | --- |
| 0 | `MemberActive` | Teilnehmer ist aktiv (neu im Netz, oder Wiedergabe nach `INIT`). |
| 1 | `MemberAddressConflict` | Teilnehmer hat einen Adresskonflikt verloren. |
| 2 | `MemberInactive` | Teilnehmer ist nicht mehr aktiv: abgemeldet, Zeitüberschreitung oder konnte keine Adresse belegen. |
| 3 | `MemberNewAddress` | Der eigene Teilnehmer hat nach einem Konflikt eine neue Adresse belegt. |
| 4 | `MemberAddressViolation` | Der eigene Teilnehmer wurde verletzt: Eine externe Nachricht kam mit derselben Quelladresse. |

## Hinweise zum Betrieb

-   **`RSP` ist Pflicht.** Ohne `RSP` bleibt der Baustein in „Wartend auf RSP“ stehen (siehe oben).
-   **Meldungen schon vor `INIT`:** Der Baustein meldet sich beim Anlegen beim Treiber an. Es kann daher schon vor `INIT` ein `IND` kommen, dann noch ungefiltert (mit den Standardwerten der Filter).
-   **Doppelte Meldungen sind möglich:** Zusammen mit der Wiedergabe bei `INIT` kann derselbe Teilnehmer mehrfach als `MemberActive` kommen. Die Anwendung muss das vertragen.


## Zustandsübersicht

1.  **Inaktiv**: Vor der Initialisierung.
2.  **Initialisiert**: Nach `INITO`. Der Baustein ist betriebsbereit und kann Anfragen bearbeiten.
3.  **Wartend auf RSP**: Nach dem Auslösen von `IND`. Weitere Meldungen werden in einer Warteschlange gesammelt und erst nach `RSP` ausgegeben.

## Anwendungsszenarien

-   **Netzwerk-Discovery**: Beim Start eines Geräts, um alle aktiven Teilnehmer im ISOBUS-Netzwerk zu identifizieren (die Wiedergabe bei `INIT` liefert auch die schon bekannten).
-   **Diagnose und Monitoring**: Zur Überwachung, ob bestimmte erwartete Steuergeräte (z.B. eines Anbaugeräts) verbunden und erreichbar sind.
-   **Dynamische Konfiguration**: Um Informationen über neu hinzugekommene Geräte zu erhalten und die Applikation entsprechend anzupassen.

## ⚖️ Vergleich mit ähnlichen Bausteinen

Im Gegensatz zu einfachen Lese- oder Abfrage-Bausteinen ist `NmGetCfInfo` speziell auf die Abfrage von ISOBUS-spezifischen Netzwerkinformationen ausgelegt. Er bietet mehr Kontext (durch `member`, `address`, `mask`) und strukturierte Ergebnisdaten (`CF_INFO_T`) als ein generischer Kommunikationsbaustein. Bausteine wie `E_SWITCH` oder `E_DEMUX` leiten nur Ereignisse weiter, während `NmGetCfInfo` aktive Netzwerkkommunikation und Protokollhandlung durchführt.

## Querverweise

-   **Kommunikationsbausteine** (zum Beispiel [`SERVER_0`](../../../StandardLibraries/net/SERVER_0.md) und [`SUBSCRIBE_0`](../../../StandardLibraries/net/SUBSCRIBE_0.md) der Bibliothek `net`, auch die AX-Adapterbausteine dazu): Sie haben ebenfalls das Paar `IND`/`RSP`. Der Unterschied: Beim Subscriber bewirkt `RSP` nichts, es wird nichts gesendet. Beim Server sendet `RSP` die Antwort. Bei `NmGetCfInfo` dagegen steuert `RSP` den Ablauf, es gibt die nächste Meldung frei.
-   **Warteschlangen-Bausteine** (Bibliothek FIFO, zum Beispiel `FIFO_UINT`): `NmGetCfInfo` puffert die Meldungen intern ebenfalls in einer Warteschlange nach dem Prinzip „first in, first out“. Der Puffer ist fest im Baustein eingebaut, gibt aber immer nur nach `RSP` die nächste Meldung weiter.


## 🛠️ Zugehörige Übungen

- [Uebung_120](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_120/)
- [Uebung_121](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_121/)
- [Uebung_122](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_122/)
- [Uebung_122b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_122b/)
- [Uebung_123](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_123/)
- [Uebung_124](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_124/)
- [Uebung_125](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_125/)
- [Uebung_126](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_126/)
- [Uebung_126b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_126b/)
- [Uebung_126b2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_126b2/)
- [Uebung_127](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_127/)
- [Uebung_128](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_128/)
- [Uebung_128b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_128b/)
- [Uebung_130](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_130/)
- [Uebung_131](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_131/)
- [Uebung_132](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_132/)
- [Uebung_133](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_133/)
- [Uebung_134](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_134/)

## Fazit

Der `NmGetCfInfo`-Funktionsblock ist ein essenzielles Werkzeug für das Management von ISOBUS-Netzwerken. Er abstrahiert die komplexe Protokollkommunikation zur Erkennung von Netzwerkteilnehmern in eine einfach zu verwendende, ereignisgesteuerte Schnittstelle. Er meldet nicht nur neue, sondern auch abgemeldete Teilnehmer und Adressänderungen, und das gefiltert nach NAME, CAN-Knoten und Art des Teilnehmers. Das macht ihn wertvoll für robuste und intelligente landwirtschaftliche Steuerungssysteme.
