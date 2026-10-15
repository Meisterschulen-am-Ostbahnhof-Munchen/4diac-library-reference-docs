# logiBUS_PI_Speed_Publish_OPC

![logiBUS_PI_Speed_Publish_OPC_network](./logiBUS_PI_Speed_Publish_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`logiBUS_PI_Speed_Publish_OPC` gibt einen Puls-Eingang (`logiBUS_PI_IDA`) auf zwei Wegen aus: als **Frequenz in Hz** (nur lokal für Dashboard/Diagnose) und als **bereits auf die physikalische Einheit umgerechneten Endwert** (SCALE + RASTER, M2M-Publish für andere Stationen). Es gibt **keine** VT-Interaktion. Der Baustein erweitert `logiBUS_PI_Raw_Publish_OPC` um die Hz-zu-Einheit-Umrechnung, die früher zentral auf STG1 lief (`AR_HZ_TO_Q_Publish_OPC`) und jetzt am Sensor liegt: Nur der fertige physikalische Endwert verlässt die Station per M2M, der rohe Hz-Wert bleibt lokal.

![logiBUS_PI_Speed_Publish_OPC](logiBUS_PI_Speed_Publish_OPC.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialization Request

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialization Confirm

### **Daten-Eingänge**

- **QI** (BOOL, Vorgabe `TRUE`): Quality Input - TRUE, um den Dienst freizugeben
- **Input** (`logiBUS::io::PI::logiBUS_PI_S`): Auswahl des Puls-Eingangs `PulseInput_I1`..`I9` (Default `logiBUS_PI::Invalid`)
- **ID_HZ_WRITE** (WSTRING): Frequenz-Publish-Adresse (REAL Hz, `ACTION=PUBLISH`), lokal; nur Dashboard/Diagnose, keine andere Station soll sie remote abonnieren (Scope LOC)
- **stObjFilterTimeS** (`NumericObjectPool_S`): Object ID + Skalierung/Offset/Dezimalstellen der CNumberVariable für die PT1-Filterzeit in Sekunden (VT-Eingabefeld); Default `u8Decimals := 1`
- **ID_FILTER_TIME_READ** (WSTRING): Remote-Subscribe-Adresse (REAL Sekunden, `ACTION=SUBSCRIBE`) für einen Web-Override der Filterzeit
- **ID_FILTER_TIME_WRITE** (WSTRING): lokale Echo-/Re-Publish-Adresse (REAL Sekunden, `ACTION=PUBLISH`) der aktuellen Filterzeit
- **sSection** (STRING): INI-Sektion in `settings.ini` für die persistierten Werte (Filterzeit und `DerivK`), eindeutig pro Sensor-Instanz
- **SCALE** (REAL): Skalierungsfaktor Hz → physikalische Einheit (z. B. `3.6/53` für km/h, `60.0` für U/min bei 1 Impuls/Umdrehung)
- **RASTER** (REAL): Rundungsraster des **fertigen** physikalischen Werts (nach `SCALE`) in der Anzeigeeinheit (z. B. 0.125 für U/min, 0.1 für km/h); Platzhalter-Default `0.001` (praktisch kein Runden)
- **ID_WRITE** (WSTRING): Publish-Adresse des umgerechneten physikalischen Endwerts (REAL, `ACTION=PUBLISH`); diese Adresse abonnieren andere Stationen remote (Scope M2M)
- **stObjDerivK** (`NumericObjectPool_S`): Object ID + Skalierung/Offset/Dezimalstellen der CNumberVariable für `FT_DERIV_10_AR.K` (Fensterbreite 0-9, 0 Dezimalstellen); bleibt auf `ID_NULL`, solange kein VT-Feld platziert ist (reine Dashboard/INI-Einstellung)
- **ID_DERIV_K_READ** (WSTRING): Remote-Subscribe-Adresse (REAL, `ACTION=SUBSCRIBE`) für einen Web-Override von `FT_DERIV_10_AR.K`
- **ID_DERIV_K_WRITE** (WSTRING): lokale Echo-/Re-Publish-Adresse (REAL, `ACTION=PUBLISH`) des aktuellen `FT_DERIV_10_AR.K`

### **Daten-Ausgänge**

- **QO** (BOOL): Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde (UND der `QO` von `logiBUS_PI_IDA`, `AR_PUBLISH_HZ`, `AR_PUBLISH_Speed`, `INI_FilterTimeS`, `INI_DerivK`, `FilterTimeS` und `DerivK`; Baustein `AND_QO`, Typ `iec61131::booleanOperators::AND_BOOL_7`)

### **Adapter**

Es werden keine Adapter verwendet.

## Funktionsweise

**Messkette (Frequenz)**: `logiBUS_PI_IDA` (`logiBUS::io::PI::logiBUS_PI_IDA`, `QI` vom Baustein-Eingang, `ImpulseDelta = DWORD#50`, `TimeDelta = DWORD#250`, `TimeRateLimit = DWORD#50`) → `AD_TO_AR_NUM` → `FT_DERIV_10_AR` (`OSCAT_adapter::Control`, `Faktor = 1.0`, `run = TRUE`) → `FT_PT1_AR` (`K = 1.0`) → `AR_SPLIT_2`.

**Zwei Ausgabewege** aus `AR_SPLIT_2`:

- `OUT1` → `AR_PUBLISH_HZ` (`AR_PUBLISH_1`, `QI` vom Baustein-Eingang): publiziert die Frequenz in Hz an `ID_HZ_WRITE` (lokal, ungerundet).
- `OUT2` → `AR_MUL.IN1` (`AR_MUL_2`), `LIT_Scale.OUT` → `AR_MUL.IN2` (`LIT_Scale` ist ein `initval_AR` mit `INIT_VAL` aus `SCALE`) → `Round_Final` (`OSCAT_adapter::Mathematical::ROUND_STEP_AR`, `RASTER`) → `AR_PUBLISH_Speed` (`AR_PUBLISH_1`, `QI` vom Baustein-Eingang): publiziert den umgerechneten, auf `RASTER` gerundeten Endwert an `ID_WRITE`.

**Einstellbare PT1-Filterzeit**: `FilterTimeS` ([`MyLib::sys::NumericValue_TO_AR2_OPC`](NumericValue_TO_AR2_OPC.md); `stObjFilterTimeS`, `ID_FILTER_TIME_READ`, `ID_FILTER_TIME_WRITE`) liefert die Filterzeit in Sekunden. `FilterTimeS.VALUEO` ↔ `INI_FilterTimeS.VAL` (`eclipse4diac::storage::INI_AR2`, `QI` vom Baustein-Eingang, `SETM = TRUE`, `KEY = 'FilterTimeS'`, `DEFAULT_VALUE = REAL#2.0`, `SECTION` aus `sSection`) persistiert sie. `FilterTimeS.VALUE_OUT` → `ToATM.IN1` ([`MyLib::sys::AR_S_TO_ATM`](AR_S_TO_ATM.md)), `ToATM.OUT` → `FT_PT1_AR.TM`.

**Einstellbare Ableitungs-Fensterbreite**: `DerivK` (`NumericValue_TO_AR2_OPC`; `stObjDerivK`, `ID_DERIV_K_READ`, `ID_DERIV_K_WRITE`) liefert `K`. `DerivK.VALUEO` ↔ `INI_DerivK.VAL` (`INI_AR2`, `KEY = 'DerivK'`, `DEFAULT_VALUE = REAL#9.0`, `SECTION` aus `sSection`) persistiert den Wert. `DerivK.VALUE_OUT` → `ToAUI.AR_IN` (`AR_TO_AUI`), `ToAUI.AUI_OUT` → `FT_DERIV_10_AR.K`.

**Initialisierung und Status**: Die Initialisierungskette lautet `INIT` → `logiBUS_PI_IDA.INIT`, `logiBUS_PI_IDA.INITO` → `AR_PUBLISH_HZ.INIT`, `AR_PUBLISH_HZ.INITO` → `AR_PUBLISH_Speed.INIT`, `AR_PUBLISH_Speed.INITO` → `INI_FilterTimeS.INIT`, `INI_FilterTimeS.INITO` → `INI_DerivK.INIT`, `INI_DerivK.INITO` → `FilterTimeS.INIT`, `FilterTimeS.INITO` → `DerivK.INIT`, `DerivK.INITO` → `INITO` und → `AND_QO.REQ`. `QI` wird an `logiBUS_PI_IDA`, `AR_PUBLISH_HZ`, `AR_PUBLISH_Speed`, `INI_FilterTimeS`, `INI_DerivK`, `FilterTimeS` und `DerivK` weitergegeben; deren `QO` → `AND_QO.IN1`..`IN7`, `AND_QO.OUT` → `QO`.

## Technische Besonderheiten

- Der Hz-Wert (`ID_HZ_WRITE`) bleibt bewusst ungerundet und rein lokal (Scope LOC); nur `ID_WRITE` ist für M2M-Abonnenten gedacht
- `RASTER` rundet den **fertigen** physikalischen Wert nach der `SCALE`-Multiplikation (nicht mehr in Hz wie früher `RASTER_HZ` bei `logiBUS_PI_Raw_Publish_OPC`), sodass intuitive Werte direkt in der Anzeigeeinheit entstehen
- Filterzeit (`FilterTimeS`) und Fensterbreite (`DerivK`) sind per VT-Feld, Web-Override und INI-Persistenz einstellbar; `K` von `FT_DERIV_10_AR` ist selbst ein AUI-Adapter-Socket, sodass die frühere Entpack-Umgehung entfiel
- Die Konfiguration gilt automatisch für alle fünf Puls-Sensoren, die diesen gemeinsamen Baustein nutzen (Aufnahme B03, Igelband 1/2 B10/B11, Zapfwelle B21, Sägeblatt B08)
- Beide INI-Einträge teilen sich dieselbe `sSection`, besitzen aber unterschiedliche `KEY`s (`FilterTimeS`, `DerivK`)

## Anwendungsszenarien

- Geschwindigkeits- oder Drehzahlmessung (km/h, U/min) an Stationen ohne VT-Anschluss mit stationsübergreifender Bereitstellung des fertigen Werts

## ⚖️ Vergleich mit ähnlichen Bausteinen

Gegenüber `logiBUS_PI_Raw_Publish_OPC` rundet dieser Baustein im Anzeigeeinheit-Raster und liefert zusätzlich den umgerechneten Endwert. Der abonnierende Gegenpart ist [`AR_M2M_Subscribe_TO_Q_Publish_OPC`](AR_M2M_Subscribe_TO_Q_Publish_OPC.md).

## Fazit

`logiBUS_PI_Speed_Publish_OPC` bildet die komplette Sensor-Seite einer Geschwindigkeits-/Drehzahlmessung ab: Frequenzermittlung mit einstellbarem Filter, Umrechnung und Rundung in die Anzeigeeinheit sowie lokale und M2M-Publikation.
