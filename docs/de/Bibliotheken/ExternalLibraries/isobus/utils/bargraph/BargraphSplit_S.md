# BargraphSplit_S

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: ISOBUS UT split-bargraph object pool properties (two adjacent Linear Bar Graphs visualizing one signed value as two unsigned magnitude fills)

Paket: `isobus::utils::bargraph`

## Schnittstellenstruktur

### **Felder**

-   **stLeft** (Typ: `logiBUS::utils::conversion::phys::NumericObjectPool_S`): Object pool properties of the Linear Bar Graph filling outward for negative values
-   **stRight** (Typ: `logiBUS::utils::conversion::phys::NumericObjectPool_S`): Object pool properties of the Linear Bar Graph filling outward for positive values
-   **r32MinMagnitude** (Typ: `REAL`): Shared lower clamp bound for both sides' magnitude, matches both Bargraphs' own Min (Vorgabewert: `0.0`)
-   **r32MaxMagnitude** (Typ: `REAL`): Shared upper clamp bound for both sides' magnitude, matches both Bargraphs' own Max (Vorgabewert: `0.0`)

Hinweis: Diese Seite wurde aus der Typdefinition (`BargraphSplit_S.dtp`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.
