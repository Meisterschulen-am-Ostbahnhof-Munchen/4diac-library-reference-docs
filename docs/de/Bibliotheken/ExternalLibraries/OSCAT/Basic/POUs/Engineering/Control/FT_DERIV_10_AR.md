# FT_DERIV_10_AR

![FT_DERIV_10_AR_network](./FT_DERIV_10_AR_network.svg)

* * * * * * * * * *

## Einleitung

`FT_DERIV_10_AR` ist ein AR-Adapter-Wrapper um den OSCAT-Baustein `OSCAT::Basic::POUs::Engineering::Control::FT_DERIV_10` (Ableitung mit wählbarem Fenster von 1 bis 10 Perioden). Er überbrückt zwischen Adaptern und OSCAT, ohne dass eine der beiden Basisbibliotheken von der anderen abhängt. Gegenüber [`FT_DERIV_AR`](./FT_DERIV_AR.md) ist das Fenster `K` ein dynamischer `AUI`-Adapter-Socket. `Faktor` und `run` bleiben einfache Eingangsvariablen.

![FT_DERIV_10_AR](FT_DERIV_10_AR.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Service-Initialisierung (durchgereicht an `FT_DERIV_10.EINIT`)
- **REQ**: explizite Ausführungsanforderung (durchgereicht an `FT_DERIV_10.REQ`, zusätzlich zu `AR_IN.E1`/`K.E1`)
    - **Verbundene Daten**: `Faktor`, `run`
- **RST**: Reset der Ableitungshistorie (durchgereicht an `FT_DERIV_10.RST`)

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierungsbestätigung (von `FT_DERIV_10.INITO`)

### **Daten-Eingänge**

- **Faktor** (`REAL`, Initialwert `1.0`): Multiplikationsfaktor (durchgereicht an `FT_DERIV_10.Faktor`)
- **run** (`BOOL`, Initialwert `TRUE`): Berechnung aktiv (durchgereicht an `FT_DERIV_10.run`)

### **Daten-Ausgänge**

Keine. Alle Ergebniswerte werden über Adapter-Plugs ausgegeben.

### **Adapter**

| Schnittstelle | Richtung | Adaptertyp | Datentyp | Beschreibung |
| :--- | :--- | :--- | :--- | :--- |
| `AR_IN` | Socket | `AR` | `REAL` | Eingangssignal |
| `K` | Socket | `AUI` | `UINT` | Window Select (dynamisch): 0 = 1 Periode, 9 = 10 Perioden |
| `AR_OUT` | Plug | `AR` | `REAL` | Berechnete Ableitung (`FT_DERIV_10.out`, basierend auf `K`) |
| `AR_DELTA_IN` | Plug | `AR` | `REAL` | Differenz des Eingangssignals über 1 Periode (`DI_0`) |
| `AUDI_DELTA_T` | Plug | `AUDI` | `UDINT` | Zeitdifferenz in Mikrosekunden über 1 Periode (`DT_0`) |
| `AB_VALID` | Plug | `AB` | `BOOL` | Gültigkeit der Messung (`FT_DERIV_10.valid`) |

## Verwendete Funktionsbausteine (FBs)

- **`FT_DERIV_10`** (`OSCAT::Basic::POUs::Engineering::Control::FT_DERIV_10`): Ableitungsberechnung, siehe [FT_DERIV_10](./FT_DERIV_10.md)
- **`E_D_FF_ANY`** (`iec61499::events::E_D_FF_ANY_4`): vierkanaliger Änderungsfilter/Synchronisation der Ergebnisse

## Funktionsweise

- `AR_IN.E1`, `K.E1` und `REQ` führen alle auf `FT_DERIV_10.REQ`
- `AR_IN.D1` → `FT_DERIV_10.in`; `K.D1` → `FT_DERIV_10.K`; `Faktor` → `FT_DERIV_10.Faktor`; `run` → `FT_DERIV_10.run`
- `INIT` → `FT_DERIV_10.EINIT`; `FT_DERIV_10.INITO` → `INITO`; `RST` → `FT_DERIV_10.RST`
- `FT_DERIV_10.CNF` → `E_D_FF_ANY.CLK`; die Ergebnisse `out`, `DT_0`, `DI_0`, `valid` gehen an `E_D_FF_ANY.D1` bis `D4`
- `E_D_FF_ANY.Q1`…`Q4` → `AR_OUT.D1`, `AUDI_DELTA_T.D1`, `AR_DELTA_IN.D1`, `AB_VALID.D1`; `E_D_FF_ANY.EO` löst `E1` aller vier Plugs aus

## Technische Besonderheiten

- **Reine Adapter-Grenze**: Alle Hauptausgänge von `FT_DERIV_10` werden als eigene Adapter ausgegeben, kein Klartext-Ereignis/-Datum verlässt den Baustein (außer `INITO`)
- **`K` als Adapter** (Version 1.2, 2026-10-03): `K.E1` löst wie `AR_IN.E1` eine Berechnung aus, `K.D1` liefert den Wert; `K` ist deshalb nicht mehr per `With` an `REQ` gebunden
- **Explizites `REQ`** (Version 1.1, 2026-09-20): Bugfix, damit `Faktor` und `run` per `With`-Verknüpfung zuverlässig gelesen werden (nach IEC 61499 werden ungebundene Variablen nicht zuverlässig gelesen); erlaubt außerdem manuellen oder periodischen Aufruf
- `FT_DERIV_10` ist eine reine OSCAT-Funktion ohne Adapter-Schnittstelle; die Brücke liegt im dritten Bibliotheksprojekt `OSCAT_adapter`

## Anwendungsszenarien

- Frequenz-/Drehzahlberechnung aus einem Impulszählerwert (`AR_IN`, z. B. von `AD_TO_AR_NUM.AR_OUT`) mit zur Laufzeit wählbarem Fenster
- Weitergabe der Ableitung z. B. an `CTRL_PID_AR.AR_ACT`

## Zusammenfassung

`FT_DERIV_10_AR` kapselt `FT_DERIV_10` hinter einer reinen Adapter-Schnittstelle mit dynamisch wählbarem Fenster `K` und stellt Ableitung, Eingangsdifferenz, Zeitdifferenz und Gültigkeit als Adapter bereit.

## 🛠️ Zugehörige Übungen

- [Uebung_152_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_152_AX/)
- [Uebung_153_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_153_AX/)
