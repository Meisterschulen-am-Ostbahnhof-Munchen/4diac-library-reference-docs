# SRT_RPC_FROM_Remote_QXA_OPC_Adapter


![SRT_RPC_FROM_Remote_QXA_OPC_Adapter_network](./SRT_RPC_FROM_Remote_QXA_OPC_Adapter_network.svg)

![SRT_RPC_FROM_Remote_QXA_OPC_Adapter](./SRT_RPC_FROM_Remote_QXA_OPC_Adapter.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock `SRT_RPC_FROM_Remote_QXA_OPC_Adapter` ist eine SubApp (SubApplication) vom Typ `MyLib::sys` und dient als gekapselte Lösung zur Fernsteuerung eines digitalen Ausgangs (QXA) über ein Netzwerkprotokoll. Er bündelt die Funktionen eines Server-/Client‑Adapters, eines Signalverteilers und einer SR/Toggle‑Flipflop‑Logik in einem einzigen bidirektionalen Adapter‑Zugang. Das Protokoll wird dabei vollständig in der SubApp selbst implementiert und nicht in der Ressource des Geräts – ein Ansatz, der die Wiederverwendung und Modularität erhöht.

## Schnittstellenstruktur

Die SubApp besitzt ausschließlich **Daten‑Eingänge**; es sind weder Ereignis‑Eingänge noch Ereignis‑Ausgänge oder Adapter‑Schnittstellen nach außen vorhanden. Die Eingänge werden innerhalb der SubApp an die internen Funktionsblöcke weitergeleitet.

### **Ereignis-Eingänge**

- Keine

### **Ereignis-Ausgänge**

- Keine

### **Daten-Eingänge**

| Name | Typ | Beschreibung |
|------|-----|--------------|
| `Output` | `logiBUS::io::DQ::logiBUS_DO_S` | Identifiziert den gewünschten digitalen Ausgang (Q1..Q8). Initialwert: `logiBUS_DO::Invalid`. |
| `ID_SET_METHOD` | `WSTRING` | Lokale Methodenadresse (ACTION = `CREATE_METHOD`) für den Set‑Methodenaufruf. Wird von einem entfernten Gerät per `CALL_METHOD` angesprochen. |
| `ID_RESET_METHOD` | `WSTRING` | Lokale Methodenadresse (ACTION = `CREATE_METHOD`) für den Reset‑Methodenaufruf. |
| `ID_TOGGLE_METHOD` | `WSTRING` | Lokale Methodenadresse (ACTION = `CREATE_METHOD`) für den Toggle‑Methodenaufruf. |
| `ID_STATE_WRITE` | `WSTRING` | Remote‑Zieladresse (BOOL, ACTION = `WRITE`) für den Flipflop‑Zustand, der auf dem entfernten Gerät geschrieben wird. |

### **Daten-Ausgänge**

- Keine

### **Adapter**

- Keine direkten Adapter‑Schnittstellen nach außen. Der zugrunde liegende Adapter `ASRT_AX_SERVER_0_CLIENT_1_0` wird intern instanziiert.

## Funktionsweise

Die SubApp realisiert einen Fernzugriff auf einen digitalen Ausgang eines anderen Geräts. Die Logik ist in vier Funktionsblöcke unterteilt:

1. **`TRIGGER`** – Baustein vom Typ `adapter::net::ASRT_AX_SERVER_0_CLIENT_1_0`.  
   Dies ist der zentrale Kommunikations‑Adapter, der einen Server‑ und einen Client‑Anschluss bündelt. Über die Eingangsvariablen `ID_SET_METHOD`, `ID_RESET_METHOD`, `ID_TOGGLE_METHOD` und `ID_STATE_WRITE` werden die Methodenadressen bzw. Zieladressen bereitgestellt, die der Adapter nutzt, um entfernte Aufrufe zu empfangen bzw. zu senden.

2. **`SPLIT`** – Baustein vom Typ `adapter::events::bidirectional::ASRT_AX_AX_SPLIT`.  
   Er teilt das vom Adapter kommende Signal `S_R_T` (Set/Reset/Toggle) in zwei Pfade:  
   - `OUT` → an die Flipflop‑Logik  
   - `AX_OUT` → an den digitalen Ausgangsbaustein

3. **`FLIPFLOP`** – Baustein vom Typ `adapter::events::bidirectional::ASRT_AX_T_FF_SR_2`.  
   Eine SR/Toggle‑Flipflop‑Logik, die den Zustand eines Bits verwaltet. Dieser Zustand kann über die `ID_STATE_WRITE`‑Adresse an ein entferntes Gerät zurückgeschrieben werden.

4. **`DigitalOutput_Q1`** – Baustein vom Typ `logiBUS::io::DQ::logiBUS_QXA`.  
   Steuert den realen digitalen Ausgang auf Grundlage des empfangenen Signals. Der Eingang `Output` (aus den SubApp‑Eingängen) bestimmt dabei, welcher Ausgang (Q1..Q8) aktiviert wird.

Die Datenverbindungen verdrahten die externen Eingänge mit dem `TRIGGER`‑Baustein, während die Adapterverbindungen die Signalweiterleitung zwischen den internen Blöcken sicherstellen. Die Konstante `QI = TRUE` an `TRIGGER` und `DigitalOutput_Q1` aktiviert die jeweilige Verarbeitung permanent.

## Technische Besonderheiten

- **Gekapseltes Protokoll** – Die gesamte Kommunikationslogik liegt in der SubApp, nicht in der Geräteressource. Das erleichtert die Portierung und Wiederverwendung.
- **Bidirektionaler Adapter** – Der Bündelungsadapter `ASRT_AX_SERVER_0_CLIENT_1_0` kombiniert Server‑ und Client‑Funktionen in einem Baustein und reduziert so die Anzahl externer Schnittstellen.
- **Signalsplit** – Der `ASRT_AX_AX_SPLIT` trennt die Steuerbefehle für den Ausgang von denen für die Flipflop‑Logik, wodurch beide unabhängig voneinander aktualisiert werden können.
- **Flexible Auswahl des Ausgangs** – Über den Parameter `Output` wird der zu steuernde logiBUS‑Ausgang (Q1..Q8) dynamisch festgelegt.
- **Verwendung von WSTRING‑Adressen** – Die Methoden‑ und Zieldressen sind als WSTRING definiert, was eine einfache Anpassung an verschiedene Netzwerkadressen erlaubt.

## Zustandsübersicht

Die SubApp selbst hat keinen expliziten Zustandsautomaten, da sie über Funktionsblöcke realisiert wird. Die internen Zustände ergeben sich aus:

- **Flipflop‑Zustand** (`FLIPFLOP`): Die Bausteine `ASRT_AX_T_FF_SR_2` unterstützen die Zustände *Set*, *Reset* und *Toggle*. Die Ausgabe steuert den logischen Pegel des digitalen Ausgangs.
- **Digitalausgang** (`DigitalOutput_Q1`): Der Ausgang kann aktiv (`TRUE`) oder inaktiv (`FALSE`) sein, abhängig vom empfangenen Signal.
- **Kommunikationszustand** (`TRIGGER`): Der Adapter wechselt zwischen *Empfangen* und *Senden* von Nachrichten, abhängig von den eingehenden Methodenaufrufen und den Zieladressen.

Eine detaillierte Zustandsbeschreibung müsste sich auf die einzelnen Funktionsblock‑Dokumentationen stützen.

## Anwendungsszenarien

- **Fernsteuerung von Industrieausgängen** – Ein zentrales Steuerungssystem (Gerät A) sendet Set/Reset/Toggle‑Befehle über das Netzwerk an einen dezentralen Ausgangsbaustein (Gerät B, hier Station 12).
- **Redundante Steuerung per RPC** – Über die bereitgestellten Methodenadressen können entfernte Systeme den Ausgangszustand lesen oder schreiben, ohne dass eine direkte Verdrahtung erforderlich ist.
- **Modulare Wiederverwendung** – Da das Protokoll in der SubApp gekapselt ist, kann derselbe Baustein auf verschiedenen Geräten ohne Anpassung der Ressourcen‑Logik eingesetzt werden.

## Vergleich mit ähnlichen Bausteinen

Gegenüber einer direkten Implementierung in der Ressource (z. B. in einem SRT_RPC_FROM_Remote_QXA_OPC‑Baustein) bietet die SubApp‑Variante folgende Vorteile:

- **Erhöhte Wartbarkeit** – Änderungen am Protokoll betreffen nur die SubApp, nicht die gesamte Ressource.
- **Geringere externe Vernetzung** – Alle internen Verbindungen sind in der SubApp verborgen; die Schnittstelle nach außen ist auf die Eingangsvariablen reduziert.
- **Austauschbarkeit** – Die SubApp kann durch eine andere Implementierung mit gleicher Schnittstelle ersetzt werden, ohne die umgebende Applikation zu verändern.

Anders als direkt verdrahtete Lösungen benötigt dieser Baustein einen Adapter zur Kommunikation, was eine zusätzliche Abstraktionsebene darstellt.

## Fazit

Der `SRT_RPC_FROM_Remote_QXA_OPC_Adapter` ist eine leistungsfähige SubApp zur fernsteuerbaren Ansteuerung eines logiBUS‑Ausgangs. Durch die Kombination von Adapter, Splitter und Flipflop‑Logik in einem gekapselten Modul wird eine hohe Flexibilität und Wiederverwendbarkeit erreicht. Die mehrschichtige Struktur ermöglicht eine klare Trennung von Kommunikations- und Steuerungslogik und eignet sich ideal für verteilte Automatisierungssysteme, bei denen ein einheitliches Schnittstellenverhalten erforderlich ist.