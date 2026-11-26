# PositionMarker_S

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: ISOBUS UT position-marker object pool properties (Change Child Position child/parent + travel bounds + center offset)

Paket: `isobus::utils::childposition`

## Schnittstellenstruktur

### **Felder**

-   **u16ChildId** (Typ: `UINT`): Object ID of the moved VT object (Change Child Position child, e.g. a CPolygon) (Vorgabewert: `IDs::ID_NULL`)
-   **u16ParentId** (Typ: `UINT`): Object ID of the parent VT container the child moves within (Change Child Position parent) (Vorgabewert: `IDs::ID_NULL`)
-   **r32MinPos** (Typ: `REAL`): Lower bound of the child's valid X-travel inside the parent, in raw pixels (normally 0.0) (Vorgabewert: `0.0`)
-   **r32MaxPos** (Typ: `REAL`): Upper bound of the child's valid X-travel inside the parent, in raw pixels = ContainerWidth - ChildBoundingBoxWidth (Vorgabewert: `0.0`)
-   **r32Center** (Typ: `REAL`): Offset added to the incoming value before clamping - the raw pixel X position that corresponds to value = 0.0 (Vorgabewert: `0.0`)
-   **s16YPosition** (Typ: `INT`): Fixed Y position passed straight through to Q_ChildPosition.s16Yposition (this FB drives only the X axis) (Vorgabewert: `0`)

Hinweis: Diese Seite wurde aus der Typdefinition (`PositionMarker_S.dtp`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.
