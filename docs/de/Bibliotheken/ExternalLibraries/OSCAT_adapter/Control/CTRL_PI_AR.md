# CTRL_PI_AR

## Einleitung

Der Baustein `CTRL_PI_AR` ist ein AR-Adapter-Wrapper um den OSCAT PI-Regler `OSCAT::Basic::POUs::Engineering::Control::CTRL_PI`. Er kapselt die Reglerlogik in der IEC 61499 Adapter-Architektur: Istwert (`AR_ACT`), Sollwert (`SET`) und die optionale Handbetrieb-Umschaltung (`AX_MAN`) sind als Adapter-Sockets ausgeführt. Stellwert (`AR_Y`), Regelabweichung (`AR_DIFF`) und Begrenzungs-Flag (`AB_LIM`) stehen als Adapter-Plugs bereit.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- `REQ`: Explizite Ausführungsanforderung (durchgereicht an `CTRL_PI.REQ`)
- `RST`: Reset des Integrators (durchgereicht an `CTRL_PI.RST`)

### **Ereignis-Ausgänge**

- `CNF`: Ausführungsbestätigung (von `CTRL_PI.CNF`)

### **InputVars (Reglerparameter)**

- `SUP` (REAL): Rauschunterdrückung (Default: 0.0)
- `OFS` (REAL): Offset (Default: 0.0)
- `M_I` (REAL): Handwert (Default: 0.0)
- `KP` (REAL): Proportionalbeiwert (Default: 1.0)
- `KI` (REAL): Integralbeiwert (Default: 1.0)
- `LL` (REAL): Unterer Grenzwert (Default: -1000.0)
- `LH` (REAL): Oberer Grenzwert (Default: 1000.0)

### **Sockets (Adapter-Eingänge)**

- `AR_ACT` (`adapter::types::unidirectional::AR`): Istwert (REAL)
- `SET` (`adapter::types::unidirectional::AR`): Sollwert (REAL, dynamisch berechnet oder via `initval_AR`)
- `AX_MAN` (`adapter::types::unidirectional::AX`): Handbetrieb-Umschaltung (optional)

### **Plugs (Adapter-Ausgänge)**

- `AR_Y` (`adapter::types::unidirectional::AR`): Stellwert (REAL)
- `AR_DIFF` (`adapter::types::unidirectional::AR`): Regelabweichung (`DIFF = SET - ACT`, REAL)
- `AB_LIM` (`adapter::types::unidirectional::AB`): Begrenzung aktiv Flag (BOOL)

## Funktionsweise

Bei Ankunft eines neuen Werts an `AR_ACT.E1`, `SET.E1` oder `AX_MAN.E1` wird der interne PI-Regler `CTRL_PI` automatisch getriggert. Alternativ kann die Reglerausführung über das explizite Event `REQ` ausgelöst werden.

## Anwendungsszenarien

- Regelung von Hydraulik-Ventilen, Drehzahl- oder Druckregelungen im ISOBUS- und logiBUS-Umfeld.
- Anbindung an `initval_AR` für statische Sollwerte oder an dynamische Sollwertgeber.
