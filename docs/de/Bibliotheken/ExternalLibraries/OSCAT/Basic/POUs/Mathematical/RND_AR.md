# RND_AR

![RND_AR_network](./RND_AR_network.svg)

* * * * * * * * * *

## Einleitung

`RND_AR` ist ein AR-Adapter-Wrapper um die OSCAT-Funktion `OSCAT::Basic::POUs::Mathematical::RND` (rundet `X` auf `N` **signifikante Stellen**, nicht auf Nachkommastellen). Er überbrückt zwischen Adaptern und OSCAT, ohne dass eine der beiden Basisbibliotheken von der anderen abhängt. `N` wird als einfache Eingangsvariable (Default 3) durchgereicht.

![RND_AR](RND_AR.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **REQ**: explizite Ausführungsanforderung (durchgereicht an `RND.REQ`, zusätzlich zu `AR_IN.E1`)
    - **Verbundene Daten**: `N`

### **Ereignis-Ausgänge**

Der Funktionsblock verfügt über keine direkten Ereignis-Ausgänge.

### **Daten-Eingänge**

- **N** (`INT`): Signifikante Stellen gesamt, nicht Nachkommastellen (durchgereicht an `RND.N`); Platzhalter-Default 3, keine funktionale Vorgabe, per Parameter überschreibbar

### **Daten-Ausgänge**

Der Funktionsblock verfügt über keine direkten Daten-Ausgänge.

### **Adapter**

- **AR_IN** (Socket, `adapter::types::unidirectional::AR`): Eingangssignal (`REAL`)
- **AR_OUT** (Plug, `adapter::types::unidirectional::AR`): auf `N` signifikante Stellen gerundeter Wert (`REAL`)

## Verwendete Funktionsbausteine (FBs)

- **`RND`** (`OSCAT::Basic::POUs::Mathematical::RND`): Rundungsfunktion, siehe [RND](./RND.md)
- **`E_D_FF_ANY_1`** ([`iec61499::events::E_D_FF_ANY`](../../../../../StandardLibraries/events/E_D_FF_ANY.md)): Änderungsfilter

## Funktionsweise

- `AR_IN.E1` → `RND.REQ` und `AR_IN.D1` → `RND.X`; `N` → `RND.N`.
- `REQ` führt ebenfalls auf `RND.REQ`, parallel zu `AR_IN.E1`.
- `RND.CNF` → `E_D_FF_ANY_1.CLK`; das Funktionsergebnis geht an `E_D_FF_ANY_1.D`.
- `E_D_FF_ANY_1.EO` → `AR_OUT.E1` und `E_D_FF_ANY_1.Q` → `AR_OUT.D1`, aber nur, wenn sich das gerundete Ergebnis seit dem letzten Aufruf tatsächlich geändert hat.

## Technische Besonderheiten

- Das explizite `REQ` mit `With N` wurde in Version 1.1 (2026-09-20) als Bugfix ergänzt: Zuvor wurde `N` nur per Datenverbindung an `RND` durchgereicht und einzig über `AR_IN.E1` ausgelöst; da der Wrapper kein Ereignis-Eingang hatte, gab es keine `With`-Verknüpfung (nach IEC 61499 werden ungebundene Variablen nicht zuverlässig gelesen). `REQ` erlaubt manuellen oder periodischen Aufruf zusätzlich zum automatischen Auslösen über `AR_IN.E1`
- Änderungsfilter: `AR_OUT.E1` feuert nur bei tatsächlicher Änderung des gerundeten Werts (gleiches Muster wie bei `CTRL_PI_AR`/`FT_DERIV_AR`, dort mit `E_D_FF_ANY_3`/`_4` für mehrere Werte gleichzeitig)
- `RND` ist eine reine OSCAT-Funktion ohne Adapter-Schnittstelle; `adapter` soll nicht von `OSCAT` abhängen (und umgekehrt), die Brücke liegt daher im dritten Bibliotheksprojekt `OSCAT_adapter`
- Unterschied zu [`ROUND_AR`](./ROUND_AR.md): `RND_AR` rundet auf signifikante Stellen, `ROUND_AR` auf Nachkommastellen

## Anwendungsszenarien

- Runden eines `AR`-Signals auf eine feste Anzahl signifikanter Stellen in einer Adapter-Kette, z. B. für Anzeigewerte

## Zusammenfassung

`RND_AR` rundet einen `AR`-Wert mit `RND` auf `N` signifikante Stellen und gibt das Ergebnis nur bei Änderung als `AR`-Adapter aus.
