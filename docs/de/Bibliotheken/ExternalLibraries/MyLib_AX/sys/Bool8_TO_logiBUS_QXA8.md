# Bool8_TO_logiBUS_QXA8


![Bool8_TO_logiBUS_QXA8_network](./Bool8_TO_logiBUS_QXA8_network.svg)

![Bool8_TO_logiBUS_QXA8](./Bool8_TO_logiBUS_QXA8.svg)

* * * * * * * * * *

## Einleitung

Der Baustein **Bool8_TO_logiBUS_QXA8** ist eine Subapplikation zur Ansteuerung von acht digitalen Ausgängen über einen logiBUS-Bus. Er stellt eine generische, adapterbasierte Lösung dar, bei der die Zielkanäle pro Ausgang frei konfiguriert werden können. Der Baustein wird über ein gemeinsames Ereignis (CNF) getriggert und überträgt die anliegenden Bool-Werte auf die spezifizierten Ausgänge. Er ist als Schwester-Baustein zu `MyLib::sys::Bool8_TO_logiBUS_QX8` konzipiert und unterscheidet sich durch die Verwendung des QXA-Adaptertyps.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **CNF** (Event): Dieses Ereignis löst die Übernahme aller acht Eingangswerte und deren Übertragung auf die konfigurierten Ausgänge aus.

### **Ereignis-Ausgänge**

- Keine.

### **Daten-Eingänge**

- **Q_00** (BOOL): Binärwert für Ausgang 1.
- **Q_01** (BOOL): Binärwert für Ausgang 2.
- **Q_02** (BOOL): Binärwert für Ausgang 3.
- **Q_03** (BOOL): Binärwert für Ausgang 4.
- **Q_04** (BOOL): Binärwert für Ausgang 5.
- **Q_05** (BOOL): Binärwert für Ausgang 6.
- **Q_06** (BOOL): Binärwert für Ausgang 7.
- **Q_07** (BOOL): Binärwert für Ausgang 8.
- **Output_1** (logiBUS::io::DQ::logiBUS_DO_S): Kanalkonfiguration für Ausgang 1 (Initialwert: `logiBUS_DO::Invalid`).
- **Output_2** (logiBUS::io::DQ::logiBUS_DO_S): Kanalkonfiguration für Ausgang 2 (Initialwert: `logiBUS_DO::Invalid`).
- **Output_3** (logiBUS::io::DQ::logiBUS_DO_S): Kanalkonfiguration für Ausgang 3 (Initialwert: `logiBUS_DO::Invalid`).
- **Output_4** (logiBUS::io::DQ::logiBUS_DO_S): Kanalkonfiguration für Ausgang 4 (Initialwert: `logiBUS_DO::Invalid`).
- **Output_5** (logiBUS::io::DQ::logiBUS_DO_S): Kanalkonfiguration für Ausgang 5 (Initialwert: `logiBUS_DO::Invalid`).
- **Output_6** (logiBUS::io::DQ::logiBUS_DO_S): Kanalkonfiguration für Ausgang 6 (Initialwert: `logiBUS_DO::Invalid`).
- **Output_7** (logiBUS::io::DQ::logiBUS_DO_S): Kanalkonfiguration für Ausgang 7 (Initialwert: `logiBUS_DO::Invalid`).
- **Output_8** (logiBUS::io::DQ::logiBUS_DO_S): Kanalkonfiguration für Ausgang 8 (Initialwert: `logiBUS_DO::Invalid`).

### **Daten-Ausgänge**

- Keine.

### **Adapter**

An der Subapp-Schnittstelle sind keine Adapterports definiert. Intern werden jedoch Adapterverbindungen zwischen den Konvertern und den logiBUS-Ausgangsbausteinen verwendet.

## Funktionsweise

Die Subapplikation enthält acht Instanzen des Funktionsblocks `logiBUS::io::DQ::logiBUS_QXA` (jeweils für einen Ausgang) sowie acht Konverter `adapter::conversion::unidirectional::AX_BOOL_TO_X`. Beim Eintreten des Ereignisses `CNF` werden alle acht Bool-Werte parallel über die Konverter in Adapterdaten umgewandelt und an die zugehörigen `logiBUS_QXA`-Bausteine weitergeleitet. Diese setzen die digitalen Ausgänge entsprechend dem konfigurierten Kanal (über die `Output_x`-Variablen) auf den jeweiligen Binärzustand. Die Auswahl der Zielkanäle erfolgt über die Dateneingänge `Output_1` bis `Output_8`, die als logiBUS-Kanal-Identifikatoren dienen.

## Technische Besonderheiten

- **Adapterbasierte Konvertierung:** Die Umwandlung von BOOL in adapter-kompatible Daten erfolgt über die Bausteine `AX_BOOL_TO_X`, die eine unidirektionale Konvertierung ermöglichen.
- **Parallele Verarbeitung:** Alle acht Ausgänge werden gleichzeitig (parallel) bedient, daher ist die Reaktionszeit unabhängig von der Anzahl der Ausgänge.
- **Flexible Kanalzuordnung:** Durch die externen Variablen `Output_1` bis `Output_8` kann jeder Ausgang separat einem beliebigen logiBUS-Kanal zugeordnet werden.
- **Wiederverwendbarkeit:** Als Subapplikation lässt sich der Baustein in verschiedene Projekte einbinden und durch die Parametrierung der Ausgangskanäle universell einsetzen.

## Zustandsübersicht

Da es sich um eine Subapplikation ohne eigene Zustandslogik handelt, existieren keine internen Zustände. Die Funktionalität ist rein ereignisgesteuert: Jedes Auftreten von `CNF` führt zu einer einmaligen Ausgabeaktualisierung. Die internen `logiBUS_QXA`-Bausteine können jedoch eigene Zustände besitzen, die hier nicht näher betrachtet werden.

## Anwendungsszenarien

- **Automatisierungstechnik:** Steuerung von acht digitalen Ausgängen (z. B. Ventile, Lampen, Schütze) über einen logiBUS-Feldbus, wobei die Kanäle je nach Maschinenkonfiguration frei gewählt werden können.
- **Testumgebungen:** In Prüfständen oder Simulationsumgebungen, in denen mehrere Ausgänge schnell und gemeinsam geschaltet werden müssen.
- **Modulare Steuerungen:** Als wiederverwendbarer Baustein in übergeordneten Steuerungslogiken, wenn viele ähnliche Ausgangsgruppen benötigt werden.

## Vergleich mit ähnlichen Bausteinen

Der Schwester-Baustein `Bool8_TO_logiBUS_QX8` verwendet den Adaptertyp `logiBUS_QX` anstelle von `logiBUS_QXA`. Dies führt zu unterschiedlichen Adapterschnittstellen und kann bei Verwendung verschiedener logiBUS-Hardware oder Protokollvarianten relevant sein. `Bool8_TO_logiBUS_QXA8` ist somit für Systeme ausgelegt, die den QXA-Adapter unterstützen, während der Schwester-Baustein für QX-kompatible Umgebungen gedacht ist. Beide Bausteine bieten identische Funktionalität hinsichtlich der Anzahl und Ansteuerung der Ausgänge.

## Fazit

`Bool8_TO_logiBUS_QXA8` ist ein flexibel einsetzbarer Baustein zur Ansteuerung von acht digitalen Ausgängen über logiBUS. Durch die Kombination aus Bool-Eingängen, frei wählbaren Kanal-Identifikatoren und einem gemeinsamen Trigger bietet er eine einfache und effiziente Lösung für vielfältige Steuerungsaufgaben. Die adapterbasierte Architektur macht ihn kompatibel mit verschiedenen logiBUS-Implementierungen und ermöglicht eine klare Trennung zwischen logischer Steuerung und physischer Ausgabe.
