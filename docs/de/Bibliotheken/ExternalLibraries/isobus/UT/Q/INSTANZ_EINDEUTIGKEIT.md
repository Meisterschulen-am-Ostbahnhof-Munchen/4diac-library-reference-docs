# Instanz-Eindeutigkeit der Q_*-Bausteine

Jeder `Q_*`-Baustein hier führt seine eigene, unabhängige Wert-/Pending-Pufferung (`var_qPending`, `var_*OldValue`, siehe `sendXxx()`/`resetPendingState()`). Werden zwei Instanzen DERSELBEN Klasse auf dasselbe Ziel (Objekt-ID, Masken-ID o. ä.) verdrahtet, wissen sie nichts voneinander - beide puffern/senden unabhängig, konkurrieren um denselben VT-Bus-Befehl. Deshalb hat jeder Baustein ein Eindeutigkeits-Kriterium: was darf es höchstens einmal geben.

Seit `792e35673` ("Detect duplicate object-ID claims across instances of the same FB class") gibt es dafür einen Mechanismus: `CQ_VTCommandBase::mTargetObjectId` (+ optional `mTargetSubId` für Bausteine mit einem zusammengesetzten Schlüssel, siehe `Q_Attribute` unten) + `IsObjectIdAlreadyClaimed(objId, subId = 0xFFFF)` (`CQ_VTCommandBase.h`/`.cpp`) - die zweite Instanz, die bei INIT ein bereits beanspruchte (Objekt-ID[, Sub-ID])-Paar vorfindet, wird mit `STATUS = "This objID is already in use"` deaktiviert. Aktuell nur für die unten als "durchgesetzt" markierten Bausteine verdrahtet.

## Kriterium: eine Instanz PRO OBJEKT-ID (aktuell durchgesetzt)

Ziel ist eine variable VT-Objekt-ID (`u16ObjId`, bei INIT per `var_u16ObjId` gesetzt und gegen eine klassenspezifische Bereichs-/Typprüfung validiert). Zwei Instanzen mit derselben ID würden unabhängig um denselben Objektwert konkurrieren (Beispiel aus der Praxis: zwei `Q_BackgroundColour` auf ObjID 4003).

| Baustein | Ziel-ID = |
|---|---|
| `Q_Attribute` | **Paar** (Objekt, Attribut-ID) - `mTargetObjectId=u16ObjId`, `mTargetSubId=u8IdAttribute`. Zwei Instanzen auf demselben Objekt, aber verschiedenen Attributen, sind KEIN Konflikt - nur dasselbe Paar zweimal ist einer. |
| `Q_BackgroundColour` | Objekt mit Hintergrundfarbe |
| `Q_ChangeObjectLabel` | beschriftbares Objekt |
| `Q_ChangePolygonPoint` | Polygon-Objekt |
| `Q_ChangePolygonScale` | Polygon-Objekt |
| `Q_ChildLocation` | Kind-Objekt (Child Location) |
| `Q_ChildPosition` | Kind-Objekt (Child Position) |
| `Q_EndPoint` | Linien-Objekt |
| `Q_FillAttributes` | Objekt mit Füllattributen |
| `Q_FontAttributes` | Objekt mit Schriftattributen |
| `Q_GraphicsContext` | Graphics-Context-Objekt |
| `Q_LineAttributes` | Objekt mit Linienattributen |
| `Q_ListItem` | **Paar** (Listen-Objekt, List-Index) - `mTargetObjectId=u16ObjId`, `mTargetSubId=u8ListIndex`. Zwei Instanzen auf derselben Liste, aber verschiedenen Indizes, sind KEIN Konflikt - nur dasselbe Paar zweimal ist einer. `u8ListIndex` wird bei `INIT` per `With Var="u8ListIndex"` eingelesen — Claim/Check erfolgt wie bei `Q_Attribute` direkt bei `INIT` in `iso_init()`. |
| `Q_NumericValue` | Numeric-Value-Objekt. **Ausnahme ObjectPointer:** darf dieselbe ID zusammen mit EINER `Q_NumericValueAux`-Instanz haben (unterschiedliche Klasse, kein Konflikt - siehe `Q_BackgroundColourAux` oben; relevant nur bei ObjectPointer, weil `Q_NumericValueAux` ausschließlich ObjectPointer-Objekte anspricht). Für jedes andere Zielobjekt (kein ObjectPointer) passt nur eine `Q_NumericValue`-Instanz. |
| `Q_ObjEnableDisable` | de-/aktivierbares Objekt |
| `Q_ObjHideShow` | Container (3000-3999) |
| `Q_ObjSelectInput` | eingabe-selektierbares Objekt |
| `Q_Size` | größenveränderliches Objekt |
| `Q_StringValue` | String-Value-Objekt |

## Kriterium: eine Instanz PRO OBJEKT-ID (gleiche Struktur, NOCH NICHT durchgesetzt)

Diese Bausteine haben exakt dasselbe `u16ObjId`/`var_bObjIdValid`-Muster wie oben, waren aber nicht Teil des ersten Rollouts (sie hängen nicht am Masken-Sichtbarkeits-Gate `VtMaskVisibility_IsObjectVisible()`, das ursprünglich den Anlass gab). Das Duplikat-Risiko ist identisch - **offene Lücke**, sollte im selben Muster nachgezogen werden.

| Baustein | Ziel-ID = | Bemerkung |
|---|---|---|
| `Q_NumericValueAux` | Objekt (AUX-VT) | eigenes Gate zusätzlich (`VtAuxAssignment`) |
| `Q_BackgroundColourAux` | AuxFunction-Objekt (AUX-VT, nur AuxFunctions - anders als `Q_BackgroundColour`, das für jedes Objekt mit Hintergrundfarbe geht) | eigenes Gate zusätzlich (`VtAuxAssignment`). **Wichtig:** unique nur INNERHALB der eigenen Klasse (`getFBTypeId()`) - dieselbe AuxFunction-ID darf gleichzeitig eine `Q_BackgroundColour`- UND eine `Q_BackgroundColourAux`-Instanz haben, das is KEIN Konflikt (unterschiedliche Klassen, `IsObjectIdAlreadyClaimed()` vergleicht `getFBTypeId()`) - nicht versehentlich klassenübergreifend "reparieren". |
| `Q_ExecuteMacro` | Macro-Objekt | |
| `Q_ExecuteExtendedMacro` | Macro-Objekt | |
| `Q_LockUnlockMask` | Masken-Objekt (`u16ObjId = var_u16MaskId`) | Ziel ist eine Maske, nicht ein beliebiges Objekt - Kriterium sonst identisch |
| `Q_Priority` | Alarm-Mask-Objekt | |

## Kriterium: genau EINE Instanz im ganzen Programm (Singleton)

Diese Bausteine haben KEINE variable Ziel-Objekt-ID - ihr eigentliches Ziel (Maske, Name, Colour-Map-ID, ...) ist ein dynamischer Parameter, der bei JEDEM `REQ` neu gelesen wird (nicht bei INIT fixiert), der Baustein selbst ist als wiederverwendbarer "Dienst" gedacht. Der Baustein "gehört" damit konzeptionell einer festen, im Programm nur einmal vorhandenen Sache (dem WorkingSet bzw. dessen einzigem CF). Zwei Instanzen würden unabhängig um denselben Dienst konkurrieren.

| Baustein | Eigentliches Ziel (Parameter bei REQ) | Status |
|---|---|---|
| `Q_ActiveMask` | `u16NewMaskId` (Ziel-ID = feste WorkingSet-ID 0) | **durchgesetzt** |
| `Q_SelectActiveWorkingSet` | `pau8Name` (Name des zu wählenden Working Sets) | offene Lücke |
| `Q_SelectColourMap` | Colour-Map-Objekt-ID | offene Lücke |
| `Q_SetAudioVolume` | Lautstärke (Working-Set-weite Einstellung) | offene Lücke |
| `Q_SoftKeyMask` | DataMask-/SoftKeyMask-IDs | offene Lücke |
| `Q_CtrlAudioSignal` | Signalparameter (Working-Set-weit) | offene Lücke |

## Kein Kriterium nötig (zustandslos)

| Baustein | Warum unkritisch |
|---|---|
| `Q_ESC` | Keine Objekt-ID, keine Old-Value-Pufferung überhaupt (`iso_init()` setzt nur `STATUS`) - jeder `REQ` sendet unmittelbar, nichts zum Verwechseln. Mehrere Instanzen sind harmlos. |

## Nicht in dieser Übersicht - eigene, ungeprüfte Lücke

Die generischen `_AUI`/`_AUDI`/`_AB`/`_AX`/`_AR`-Adapter-Varianten (z. B. `Q_NumericValue_AUDI`, `Q_ObjHideShow_AB`) sind Adapter-Wrapper um die oben gelisteten Basisklassen, aber **eigene FB-Typen** mit eigener `getFBTypeId()`. `IsObjectIdAlreadyClaimed()` vergleicht `getFBTypeId()` - eine `Q_NumericValue`- und eine `Q_NumericValue_AUDI`-Instanz auf derselben Objekt-ID würden sich **nicht** gegenseitig erkennen, obwohl beide dasselbe Objekt beschreiben.

`I_GetAttribute` (`isobus_UT/src/isobus/UT/I/`) hat Paar-Eindeutigkeit über (`u16ObjId`, `u8IdAttribute`).
