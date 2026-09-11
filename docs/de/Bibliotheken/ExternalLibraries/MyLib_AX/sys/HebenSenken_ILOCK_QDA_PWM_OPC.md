# HebenSenken_ILOCK_QDA_PWM_OPC


![HebenSenken_ILOCK_QDA_PWM_OPC_network](./HebenSenken_ILOCK_QDA_PWM_OPC_network.svg)

![HebenSenken_ILOCK_QDA_PWM_OPC](./HebenSenken_ILOCK_QDA_PWM_OPC.svg)

* * * * * * * * * *

## Einleitung

Die SubApp `HebenSenken_ILOCK_QDA_PWM_OPC` realisiert die Ansteuerung eines ratiometrischen PWM-Ausgangs (z.B. für ein proportionales Wegeventil wie Danfoss PVEA) für zwei fernbedienbare Kommandos *Heben* und *Senken*. Sie integriert zwei OPC-UA-fähige Subscribe-/Publish-Paare für die Fernkommandos sowie zwei bestehende IO-Test-Kanäle und verknüpft diese logisch per ODER. Eine Interlock-Schaltung mit Schutzzeit (Last-Wins-Prinzip) verhindert, dass beide Richtungen gleichzeitig aktiv sind. Das resultierende 2-Bit-Signal wird über einen 3-Wege-Multiplexer in einen analogen Wert umgesetzt, der über einen digitalen PWM-Ausgang (QDA) ein proportionales Signal mit 25 % (Senken), 50 % (Neutral) oder 75 % (Heben) Tastgrad erzeugt. Eine Lastteiler-Korrektur und Skalierung sorgen für einen linearen Zusammenhang zwischen Kommando und physischem Ausgang.

Die SubApp ist generisch für jeden Aktor mit ratiometrischem 3-Stufen-PWM-Kanal einsetzbar.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Die SubApp besitzt keine expliziten Ereignis-Eingänge. Ereignisse werden ausschließlich über die internen Adapter (Subscribe/Publish) mit den angeschlossenen Kommunikationskanälen ausgetauscht.

### **Ereignis-Ausgänge**

Es sind keine Ereignis-Ausgänge an der SubApp-Schnittstelle definiert. Die Rückmeldungen (State-Publish) werden über die ADAPTER-Schnittstellen der internen Publikations-FBs gesendet.

### **Daten-Eingänge**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| `Output_PWM` | `logiBUS::io::DQ::logiBUS_DO_S` | Physischer PWM-Ausgang (logiBUS Digitalausgang mit PWM-Fähigkeit), wird auf 25/50/75 % Tastgrad gesetzt. |
| `DT_PROTECT` | `TIME` | Schutz-Totzeit vor Richtungswechsel (default: `T#300ms`). Verhindert schnelles Umschalten zwischen Heben und Senken. |
| `ID_HEBEN_READ` | `WSTRING` | Adresse für das echte Funktions-Kommando „Heben" (Subscribe). |
| `ID_HEBEN_WRITE` | `WSTRING` | Adresse für die echte Funktions-Rückmeldung „Heben" (Publish), gesendet hinter dem ILOCK. |
| `ID_SENKEN_READ` | `WSTRING` | Adresse für das echte Funktions-Kommando „Senken" (Subscribe). |
| `ID_SENKEN_WRITE` | `WSTRING` | Adresse für die echte Funktions-Rückmeldung „Senken" (Publish), gesendet hinter dem ILOCK. |
| `ID_TEST_READ_HEBEN` | `WSTRING` | Bestehende IO-Test-Subscribe-Adresse für Heben (ODER-verknüpft mit `ID_HEBEN_READ`). |
| `ID_TEST_WRITE_HEBEN` | `WSTRING` | Bestehende IO-Test-Publish-Adresse für Heben (gesendet hinter dem ILOCK). |
| `ID_TEST_READ_SENKEN` | `WSTRING` | Bestehende IO-Test-Subscribe-Adresse für Senken (ODER-verknüpft mit `ID_SENKEN_READ`). |
| `ID_TEST_WRITE_SENKEN` | `WSTRING` | Bestehende IO-Test-Publish-Adresse für Senken (gesendet hinter dem ILOCK). |

### **Daten-Ausgänge**

Die SubApp besitzt keine expliziten Daten-Ausgänge. Die Ausgangswerte werden über die internen Publisher und den physischen PWM-Ausgang `Output_PWM` bereitgestellt.

### **Adapter**

Die SubApp besitzt keine öffentlichen Adapter-Schnittstellen. Alle Kommunikation erfolgt über die konfigurierbaren OPC-UA-Adressen (`WSTRING`-Parameter) und den logiBUS-Ausgang.

## Funktionsweise

1. **Kommando-Empfang:** Zwei unabhängige Subscribe-Dienste (`SUBSCRIBE_HEBEN`, `SUBSCRIBE_SENKEN`) empfangen die echten Funktionskommandos von der OPC-UA-Schnittstelle. Parallel dazu empfangen zwei weitere Subscribe-Dienste (`SUBSCRIBE_TEST_HEBEN`, `SUBSCRIBE_TEST_SENKEN`) die IO-Test-Signale.

2. **ODER-Verknüpfung:** Die empfangenen BOOL-Werte werden je Richtung über ein ODER-Gatter (`OR_HEBEN`, `OR_SENKEN`) zusammengeführt. Somit kann ein Befehl entweder von der Fernsteuerung oder vom IO-Test kommen.

3. **Interlock:** Der ILOCK-Baustein (`ILOCK_SWITCH_PROTECT_AX`) nimmt die beiden ODER-Ausgänge als `UP_IN` und `DOWN_IN`. Er stellt sicher, dass nie beide Signale gleichzeitig `TRUE` sind (Last-Wins-Prinzip). Zusätzlich schützt eine Totzeit (`DT_PROTECT`) vor schnellen Richtungswechseln.

4. **Signalverteilung:** Die Ausgänge des ILOCK (`UP_OUT`, `DOWN_OUT`) werden über Split-Bausteine (`SPLIT_HEBEN_OUT`, `SPLIT_SENKEN_OUT`) auf drei Pfade verteilt:
   - **Pfad 1:** Rückmeldung an den jeweiligen Publish-Dienst für den echten Funktions-State.
   - **Pfad 2:** Rückmeldung an den Publish-Dienst für den IO-Test-State.
   - **Pfad 3:** Zusammenführung der beiden Bits (`ASSEMBLE_AB_FROM_AX`) zu einem 2-Bit-Wort (Bit0 = Heben, Bit1 = Senken).

5. **Adress-Umsetzung:** Das 2-Bit-Wort wird über den Konverter `AB_TO_AUI` in einen analogen Integer-Wert (0, 1, 2, 3) umgewandelt und dient als Auswahlwert für den 3-Wege-Multiplexer `AR_AUI_MUX_3`.

6. **Wertauswahl:** Die SubApp `values_50_75_25` liefert drei Konstanten: 50 % (Neutral), 75 % (Heben) und 25 % (Senken). Diese werden entsprechend dem Multiplexer-Ausgang gewählt.

7. **Korrektur und Skalierung:** Der ausgewählte Wert wird mit zwei Multiplikationsstufen verarbeitet:
   - **Korrekturfaktor:** `FACTOR = 1.17619` (entspricht 1/0.8502) – kompensiert die Nichtlinearität der PVEA-Ansteuerung.
   - **Skalierung auf 13-Bit-PWM:** `FACTOR = 81.91` (8191/100) – skaliert den Prozentwert auf den vollen Wertebereich des QDA-Ausgangs.

8. **Digitaler PWM-Ausgang:** Der resultierende Analogwert wird über `AR_TO_AD_NUM` in einen numerischen Wert für den `logiBUS_QDA_PWM`-Baustein gewandelt, der den physischen Ausgang `Output_PWM` entsprechend setzt.

## Technische Besonderheiten

- **Interlock mit Schutzzeit:** Der Baustein `ILOCK_SWITCH_PROTECT_AX` implementiert ein Last-Wins-Verhalten. Bei simultanen Kommandos gewinnt das zuletzt ankommende Signal, und nach einer Änderung wird eine einstellbare Totzeit `DT_PROTECT` eingehalten, um mechanische Belastungen zu reduzieren.
- **ODER-Verknüpfung von Fern- und Testbefehlen:** Die Trennung der echten Kommandos von den IO-Test-Pfaden ermöglicht eine sichere Inbetriebnahme und Wartung, ohne die Fernsteuerung zu beeinträchtigen.
- **Ratiometrische PWM-Steuerung:** Die Ausgangswerte (25 %, 50 %, 75 %) entsprechen den drei Zuständen eines proportionalen Wegeventils (z.B. Danfoss PVEA). Durch die Korrektur wird ein linearer Zusammenhang zwischen Kommando und hydraulischer Bewegung hergestellt.
- **Verwendung eines 3-Wege-Multiplexers:** Da der ILOCK den Zustand "beide aktiv" (K=3) verhindert, genügt ein 3-Wege-Mux; der vierte Fall (beide Bits gesetzt) kann nicht auftreten.
- **Wiederverwendbarkeit:** Die SubApp ist aus einem größeren System ausgelagert und damit unabhängig einsetzbar; die Freigabe-Logik (DO) wurde entfernt, um die PWM-Ansteuerung separat verwenden zu können.

## Zustandsübersicht

Die SubApp besitzt durch den ILOCK definierte Zustände für die beiden Richtungen:

| Zustand | Heben (`UP`) | Senken (`DOWN`) | PWM-Tastgrad | Bedeutung |
|---------|--------------|-----------------|--------------|-----------|
| Neutral | 0 | 0 | 50 % | Kein Kommando, Aktor in Mittelstellung |
| Heben aktiv | 1 | 0 | 75 % | Aktor fährt aus (Heben) |
| Senken aktiv | 0 | 1 | 25 % | Aktor fährt ein (Senken) |
| Unzulässig | 1 | 1 | – | Durch ILOCK verhindert |

Während der Schutzzeit (`DT_PROTECT`) nach einem Richtungswechsel wird der alte Zustand gehalten, bis die Totzeit abgelaufen ist.

## Anwendungsszenarien

- **Hydraulische Steuerung mit proportionalem Wegeventil:** Ansteuerung eines PVEA-Ventils für Hub-/Senkbewegungen (z.B. an Kränen, Bühnen, Landmaschinen).
- **Fernbedienung über OPC-UA:** Integration in Leitsysteme oder SCADA, bei denen Heben/Senken über dedizierte OPC-UA-Nachrichten erfolgt.
- **Inbetriebnahme und Test:** Nutzung der IO-Test-Kanäle, um ohne Fernsteuerungssignal den Aktor direkt anzusteuern und die Verdrahtung zu prüfen.
- **Wiederverwendbare Bausteinbibliothek:** Als Teil einer Bibliothek für logiBUS-basierte Steuerungen, die mehrere Aktoren mit gleicher PWM-Charakteristik ansteuern.

## Vergleich mit ähnlichen Bausteinen

- **`ILOCK_SWITCH_2_QXA_OPC`:** Ein Vorgänger, der ohne PWM-Ausgang nur zwei digitale Ausgänge (QXA) ansteuert. Die vorliegende SubApp erweitert dies um eine ratiometrische PWM-Ausgabe.
- **Direkte PWM-Ansteuerung ohne Interlock:** Einfache Bausteine, die Heben/Senken direkt als analoge Werte setzen, riskieren bei gleichzeitigen Kommandos einen Kurzschluss oder mechanische Schäden. Durch den ILOCK wird dies sicher verhindert.
- **Bausteine mit 4-Wege-Multiplexer:** Manche Implementierungen benötigen einen 4-Wege-Mux, da sie den Fall "beide aktiv" zulassen; hier wird durch die Interlock-Logik ein 3-Wege-Mux verwendet, was Ressourcen spart.

## Fazit

Die SubApp `HebenSenken_ILOCK_QDA_PWM_OPC` bietet eine robuste und flexible Lösung zur Ansteuerung eines proportionalen PWM-Ausgangs für Heben/Senken. Durch die Kombination von OPC-UA-Kommunikation, Interlock-Schutz, ODER-Verknüpfung mit Testsignalen und sorgfältiger Skalierung wird eine zuverlässige und wartungsfreundliche Funktionalität erreicht. Sie ist ideal für den Einsatz in industriellen Steuerungen, bei denen Sicherheit und Präzision erforderlich sind. Die klare Trennung von Fern- und Testkommandos erleichtert Inbetriebnahme und Diagnose erheblich und macht den Baustein zu einem wertvollen Bestandteil einer Automatisierungsbibliothek.