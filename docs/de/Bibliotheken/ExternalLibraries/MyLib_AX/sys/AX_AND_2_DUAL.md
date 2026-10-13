# AX_AND_2_DUAL

![AX_AND_2_DUAL_network](./AX_AND_2_DUAL_network.svg)

* * * * * * * * * *

## Einleitung

`AX_AND_2_DUAL` ist ein gegatetes Heben/Senken-Paar mit gemeinsamer Freigabe: `OUT_UP = IN_UP AND IN_PERMIT` und `OUT_DOWN = IN_DOWN AND IN_PERMIT`. Zusätzlich wird `IN_PERMIT` unverändert als `OUT_PERMIT` durchgereicht, z. B. für einen dritten Verbraucher wie eine Statusanzeige. Die SubApp ersetzt zweimal `AX_AND_2` plus einmal `AX_SPLIT_3` als Einzelbausteine.

![AX_AND_2_DUAL](AX_AND_2_DUAL.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

Alle Adapter sind vom Typ `adapter::types::unidirectional::AX`.

- **IN_UP** (Socket): rohes Heben-Signal, wird mit `IN_PERMIT` gegated
- **IN_PERMIT** (Socket): gemeinsame Freigabe für `IN_UP`/`IN_DOWN`, zusätzlich unverändert an `OUT_PERMIT` durchgereicht
- **IN_DOWN** (Socket): rohes Senken-Signal, wird mit `IN_PERMIT` gegated
- **OUT_UP** (Plug): `IN_UP AND IN_PERMIT`
- **OUT_PERMIT** (Plug): `IN_PERMIT` unverändert (für einen dritten Verbraucher, z. B. Statusanzeige)
- **OUT_DOWN** (Plug): `IN_DOWN AND IN_PERMIT`

## Verwendete Funktionsbausteine (FBs)

- **`AX_SPLIT_3`** (`adapter::events::unidirectional::AX_SPLIT_3`): verteilt `IN_PERMIT` auf drei Ziele
- **`AX_AND_UP`**, **`AX_AND_DOWN`** (je `adapter::booleanOperators::AX_AND_2`): UND-Verknüpfung

## Funktionsweise

- `IN_PERMIT` → `AX_SPLIT_3.IN`
- `AX_SPLIT_3.OUT1` → `AX_AND_UP.IN2`; `IN_UP` → `AX_AND_UP.IN1`; `AX_AND_UP.OUT` → `OUT_UP`
- `AX_SPLIT_3.OUT3` → `AX_AND_DOWN.IN1`; `IN_DOWN` → `AX_AND_DOWN.IN2`; `AX_AND_DOWN.OUT` → `OUT_DOWN`
- `AX_SPLIT_3.OUT2` → `OUT_PERMIT`

## Technische Besonderheiten

- Da ein Adapter nicht auf mehrere Ziele verzweigen darf, verteilt `AX_SPLIT_3` die Freigabe auf beide UND-Glieder und den Durchreich-Ausgang
- Keine eigene Logik außerhalb der verwendeten Standardbausteine

## Anwendungsszenarien

- Jedes Heben/Senken-Signalpaar, das dieselbe Freigabe benötigt (z. B. Verriegelung), wobei die Freigabe zusätzlich für eine Anzeige zur Verfügung stehen soll

## Zusammenfassung

`AX_AND_2_DUAL` gatet ein Heben/Senken-Paar mit einer gemeinsamen Freigabe und gibt die Freigabe zusätzlich durch.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
