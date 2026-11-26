# ScrollControls_S

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: ISOBUS UT scroll list operator controls (6 buttons + direct-position input field)

Paket: `isobus::utils::scroll`

## Schnittstellenstruktur

### **Felder**

-   **u16BtnTopId** (Typ: `UINT`): Object ID of the softkey/button that fires the TOP event (Vorgabewert: `IDs::ID_NULL`)
-   **u16BtnPageUpId** (Typ: `UINT`): Object ID of the softkey/button that fires the UP_UP event (Vorgabewert: `IDs::ID_NULL`)
-   **u16BtnUpId** (Typ: `UINT`): Object ID of the softkey/button that fires the UP event (Vorgabewert: `IDs::ID_NULL`)
-   **u16BtnDownId** (Typ: `UINT`): Object ID of the softkey/button that fires the DOWN event (Vorgabewert: `IDs::ID_NULL`)
-   **u16BtnPageDownId** (Typ: `UINT`): Object ID of the softkey/button that fires the DOWN_DOWN event (Vorgabewert: `IDs::ID_NULL`)
-   **u16BtnBottomId** (Typ: `UINT`): Object ID of the softkey/button that fires the BOTTOM event (Vorgabewert: `IDs::ID_NULL`)
-   **u16GotoInputId** (Typ: `UINT`): Object ID of the InputNumber field for direct row-position entry (drives the POS/GOTO event's SET_POS value) (Vorgabewert: `IDs::ID_NULL`)
-   **u16BtnPageUpPtrId** (Typ: `UINT`): Object ID of the ObjectPointer (SoftKeyMask/Container child) that currently resolves to u16BtnPageUpId - redirected to ID_NULL to hide the button when qAtFirst, restored to u16BtnPageUpId otherwise (Vorgabewert: `IDs::ID_NULL`)
-   **u16BtnUpPtrId** (Typ: `UINT`): Object ID of the ObjectPointer that currently resolves to u16BtnUpId - redirected to ID_NULL to hide the button when qAtFirst, restored to u16BtnUpId otherwise (Vorgabewert: `IDs::ID_NULL`)
-   **u16BtnDownPtrId** (Typ: `UINT`): Object ID of the ObjectPointer that currently resolves to u16BtnDownId - redirected to ID_NULL to hide the button when qAtLast, restored to u16BtnDownId otherwise (Vorgabewert: `IDs::ID_NULL`)
-   **u16BtnPageDownPtrId** (Typ: `UINT`): Object ID of the ObjectPointer that currently resolves to u16BtnPageDownId - redirected to ID_NULL to hide the button when qAtLast, restored to u16BtnPageDownId otherwise (Vorgabewert: `IDs::ID_NULL`)

Hinweis: Diese Seite wurde aus der Typdefinition (`ScrollControls_S.dtp`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.
