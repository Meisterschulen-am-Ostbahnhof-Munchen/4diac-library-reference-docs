# ScrollObjectPool_S

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: ISOBUS UT scroll list object pool properties (list + scrollbar geometry)

Paket: `isobus::utils::scroll`

## Schnittstellenstruktur

### **Felder**

-   **u16ListParentId** (Typ: `UINT`): Object ID of the list viewport container (Change Child Position parent) (Vorgabewert: `IDs::ID_NULL`)
-   **u16ListContentId** (Typ: `UINT`): Object ID of the moved list content container (Change Child Position child) (Vorgabewert: `IDs::ID_NULL`)
-   **i32RowHeight** (Typ: `DINT`): Row height in pixels (Vorgabewert: `0`)
-   **u16BarParentId** (Typ: `UINT`): Object ID of the scrollbar viewport container (Change Child Position parent) (Vorgabewert: `IDs::ID_NULL`)
-   **u16BarContentId** (Typ: `UINT`): Object ID of the moved scrollbar indicator content container (Change Child Position child) (Vorgabewert: `IDs::ID_NULL`)
-   **i32BarBaseOffset** (Typ: `DINT`): BarContent.Top at position 0 (negative of the indicator's fixed internal Top) (Vorgabewert: `0`)
-   **i32BarTravel** (Typ: `DINT`): Total pixels the indicator travels: window height minus indicator height (Vorgabewert: `0`)
-   **i32PosMax** (Typ: `DINT`): Upper bound of the scroll position: row count minus visible rows (Vorgabewert: `0`)
-   **i32Step** (Typ: `DINT`): Step size for the fast (page) scroll events (Vorgabewert: `1`)

Hinweis: Diese Seite wurde aus der Typdefinition (`ScrollObjectPool_S.dtp`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.
