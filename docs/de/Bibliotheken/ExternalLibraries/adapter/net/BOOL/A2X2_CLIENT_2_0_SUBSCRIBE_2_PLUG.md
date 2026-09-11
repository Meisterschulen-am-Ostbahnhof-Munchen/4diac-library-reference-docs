# A2X2_CLIENT_2_0_SUBSCRIBE_2_PLUG

![A2X2_CLIENT_2_0_SUBSCRIBE_2_PLUG](./A2X2_CLIENT_2_0_SUBSCRIBE_2_PLUG.svg)

* * * * * * * * * *
## Einleitung

Der Funktionsblock `A2X2_CLIENT_2_0_SUBSCRIBE_2_PLUG` dient als bidirektionale Schnittstelle zwischen einem A2X2‑Adapter (Plug) und einem OPC‑UA‑Server. Er schreibt die beiden BOOL‑Signale UP und DOWN des Adapters über einen `CLIENT_2_0` als Remote‑Write an eine definierte OPC‑UA‑Adresse und liest gleichzeitig die gleichen Signale über einen `SUBSCRIBE_2` zurück. Der FB puffert jeden BOOL‑Wert mit einem eigenen `E_D_FF`‑Baustein, um einen sauberen Signalwechsel zu gewährleisten. Er ist das Pendant zum FB `A2X2_CLIENT_2_0_SUBSCRIBE_2`, der einen Socket verwendet – hier wird die Kommunikation über einen Adapter‑Plug realisiert.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Typ   | Kommentar                                   |
|----------|-------|---------------------------------------------|
| `INIT`   | EInit | Initialisierung des FB, ausgelöst durch `QI`.|

### **Ereignis-Ausgänge**

| Ereignis | Typ     | Kommentar                                                        |
|----------|---------|------------------------------------------------------------------|
| `INITO`  | EInit   | Bestätigung der erfolgreichen Initialisierung.                   |
| `CNF`    | Event   | Wird ausgelöst, wenn `QO`, `STATUS_WRITE` und `STATUS_READ` aktualisiert wurden. |

### **Daten-Eingänge**

| Name       | Typ     | Kommentar                                                        |
|------------|---------|------------------------------------------------------------------|
| `QI`       | BOOL    | Steuert die Initialisierung. Bei `TRUE` wird der FB aktiviert.   |
| `ID_WRITE` | WSTRING | OPC‑UA‑Knotenadresse für die Schreib‑Operationen (ACTION=WRITE).|
| `ID_READ`  | WSTRING | OPC‑UA‑Knotenadresse für die Lese‑Operationen (ACTION=READ).     |

### **Daten-Ausgänge**

| Name           | Typ     | Kommentar                                                        |
|----------------|---------|------------------------------------------------------------------|
| `QO`           | BOOL    | `TRUE`, wenn sowohl `WRITE_CLIENT.QO` als auch `READ_SUBSCRIBE.QO` aktiv sind. |
| `STATUS_WRITE` | WSTRING | Statusmeldung des `WRITE_CLIENT` (Schreibvorgang).               |
| `STATUS_READ`  | WSTRING | Statusmeldung des `READ_SUBSCRIBE` (Lesevorgang).                |

### **Adapter**

| Name | Typ   | Kommentar                                                    |
|------|-------|---------------------------------------------------------------|
| `IO` | A2X2  | Bidirektionaler Adapter‑Plug für die BOOL‑Signale `UP` und `DOWN`.|

## Funktionsweise

Der FB besteht aus mehreren internen Funktionsbausteinen: `WRITE_CLIENT` (CLIENT_2_0), `READ_SUBSCRIBE` (SUBSCRIBE_2), `AND_QO` (Zwei‑Eingang‑UND) sowie vier `E_D_FF`‑Bausteinen zur Pufferung der BOOL‑Werte.

1. **Initialisierung:**  
   Beim Anlegen des FB wird `INIT` ausgelöst. Über eine Sequenz wird zuerst `READ_SUBSCRIBE.INIT` und dann – nach dessen Bestätigung `INITO` – `WRITE_CLIENT.INIT` aktiviert. Nach erfolgreicher Initialisierung beider Netzwerkbausteine wird das Ereignis `INITO` des Gesamt‑FB ausgegeben.

2. **Schreiben (TX):**  
   Die Daten‑Eingänge `DI_UP` und `DI_DOWN` des Adapters werden über `E_D_FF_TX_UP` bzw. `E_D_FF_TX_DOWN` gepuffert. Wenn sich ein Wert ändert (`EI_UP` oder `EI_DOWN` Ereignis), wird der neue Wert am Ausgang `Q` des jeweiligen `E_D_FF` an die Daten‑Eingänge `SD_1` bzw. `SD_2` des `WRITE_CLIENT` übergeben und ein Schreibauftrag (`REQ`) ausgelöst.

3. **Lesen (RX):**  
   Der `READ_SUBSCRIBE` empfängt kontinuierlich die Daten `RD_1` und `RD_2` vom OPC‑UA‑Server. Bei jeder Aktualisierung (`IND`) werden diese Werte in die `E_D_FF_RX_*` Bausteine übernommen. Erst wenn ein Flankenwechsel am `CLK`‑Eingang auftritt, wird der jeweilige Ausgang `Q` aktualisiert und das Ergebnis an `DO_UP` bzw. `DO_DOWN` des Adapters ausgegeben (Ereignisse `EO_UP`/`EO_DOWN`).

4. **Zusammenführung von Qualität und Status:**  
   Die Ausgänge `QO` der beiden Netzwerkbausteine (`WRITE_CLIENT` und `READ_SUBSCRIBE`) werden über den UND‑Baustein `AND_QO` verknüpft. Das Ergebnis wird am Ausgang `QO` des FB bereitgestellt und das Ereignis `CNF` ausgelöst (zusammen mit den Statuswerten). Dadurch signalisiert der FB, dass sowohl der Schreib- als auch der Lesevorgang fehlerfrei funktionieren.

## Technische Besonderheiten

- **Pufferung mit `E_D_FF`:**  
  Jeder BOOL‑Wert wird durch einen eigenen `E_D_FF` geführt. Dies verhindert, dass bei schnellen Signalwechseln Daten verloren gehen oder unerwünschte Mehrfach‑Sendeaufträge entstehen.
- **Eigenständige Initialisierungskette:**  
  Die Reihenfolge `READ_SUBSCRIBE` → `WRITE_CLIENT` stellt sicher, dass der Abonnent vor dem Client aktiviert wird, was für OPC‑UA‑Subscriptions notwendig ist.
- **Asynchrone Aktualisierung:**  
  Da Lesen und Schreiben asynchron über OPC‑UA erfolgen, wird `CNF` erst nach Abschluss beider Operationen (oder einer Aktualisierung vom Lesen) ausgelöst.
- **Plug statt Socket:**  
  Im Gegensatz zum Schwestern‑FB `A2X2_CLIENT_2_0_SUBSCRIBE_2`, der einen Socket verwendet, nutzt dieser FB einen `Plug`. Dadurch kann er direkt in ein übergeordnetes FB‑Netzwerk eingebettet werden, das über einen kompatiblen Socket verfügt.

## Zustandsübersicht

Da der FB keine expliziten Zustände in der XML definiert, lassen sich aus dem internen Netzwerk folgende Phasen ableiten:

- **Initialisierungsphase:** `QI = TRUE` → `INIT` → `READ_SUBSCRIBE.INIT` → `WRITE_CLIENT.INIT` → `INITO`  
- **Betriebsphase:**  
  - *Senden:* Bei Änderung an `DI_UP`/`DI_DOWN` wird gepuffert und ein Write ausgelöst.  
  - *Empfangen:* Bei Änderung vom Server wird gepuffert und an den Adapter ausgegeben.  
  - *Überwachung:* Die `QO`‑Signale der Netzwerk‑FBs werden über UND verknüpft.

Eine detaillierte Zustandsmaschine ist im FB selbst nicht sichtbar, die internen `E_D_FF` können jedoch in die Zustände „Set“ und „Reset“ wechseln, je nach Datenwert und Taktsignal.

## Anwendungsszenarien

- **Fernsteuerung von Binärausgängen:**  
  Der FB eignet sich, um zwei digitale Ausgänge eines (z. B. industriellen) Geräts über OPC‑UA zu steuern und den aktuellen Zustand gleichzeitig in die Gegenrichtung zurückzulesen.
- **Steuerung mit Rückmeldung:**  
  Wenn die Schreib‑ und Leseadresse auf denselben OPC‑UA‑Knoten zeigen, kann der FB als Spiegel für eine wechselseitige Kommunikation zwischen zwei Steuerungen dienen.
- **Integration in IEC 61499‑Systeme:**  
  Durch die Verwendung eines Adapter‑Plugs ist der FB einfach in größere Applikationen einbindbar, die bidirektionale A2X2‑Schnittstellen verwenden.

## Vergleich mit ähnlichen Bausteinen

Der direkte Verwandte ist `A2X2_CLIENT_2_0_SUBSCRIBE_2`, der statt eines Plugs einen Socket verwendet. Der vorliegende FB (`_PLUG`) bietet die gleiche Funktionalität, setzt aber auf die Adapter‑Mechanik von 4diac. Dadurch ist er flexibler in der Topologie, da er an beliebigen Stellen eines FB‑Netzwerks über einen Socket eingebunden werden kann.

Weitere Alternativen könnten sein:
- Verwendung von zwei getrennten FBs für Schreiben und Lesen, was jedoch eine manuelle Synchronisation erfordert.
- Einsatz von `E_SR`‑Bausteinen zur Pufferung, die jedoch weniger Flanken‑orientiert arbeiten.

## Fazit

Der `A2X2_CLIENT_2_0_SUBSCRIBE_2_PLUG` implementiert eine robuste bidirektionale OPC‑UA‑Anbindung für zwei BOOL‑Signale, gekapselt in einen einzelnen, gut wiederverwendbaren Funktionsblock. Die Kombination aus `CLIENT_2_0` und `SUBSCRIBE_2` mit `E_D_FF`‑Puffern gewährleistet eine zuverlässige Datenübertragung und eine klare Statusanzeige. Dank der Adapter‑Schnittstelle lässt sich der FB direkt in bestehende 4diac‑Applikationen integrieren und eignet sich besonders für Remote‑Steuerungs‑ und Regelungsaufgaben in Automatisierungsumgebungen.