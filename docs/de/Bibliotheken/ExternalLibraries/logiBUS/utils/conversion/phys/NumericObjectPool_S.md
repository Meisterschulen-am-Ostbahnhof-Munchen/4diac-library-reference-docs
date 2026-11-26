# NumericObjectPool_S

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: ISOBUS UT Numeric Object (InputNumber/OutputNumber) pool properties

Paket: `logiBUS::utils::conversion::phys`

## Schnittstellenstruktur

### **Felder**

-   **u16ObjId** (Typ: `UINT`): ISOBUS Object ID (Vorgabewert: `IDs::ID_NULL`)
-   **r32Scale** (Typ: `REAL`): Scale factor from object pool (e.g. 0.01) (Vorgabewert: `1.0`)
-   **i32Offset** (Typ: `DINT`): Offset from object pool (e.g. -100000) (Vorgabewert: `0`)
-   **u8Decimals** (Typ: `USINT`): Number of decimal places (Vorgabewert: `0`)

Hinweis: Diese Seite wurde aus der Typdefinition (`NumericObjectPool_S.dtp`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.
