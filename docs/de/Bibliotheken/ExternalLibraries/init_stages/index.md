# init_stages

## Einleitung

Die Bibliothek `init_stages` (Paket `events::init`) stellt Bausteine für eine **gestufte Initialisierung** bereit. Anlass: Remote-OPC-UA-Clients warten beim Start bis zu 60 s pro fehlendem Node, wenn die Server-Seite zu spät initialisiert wird. Mit Stufen lässt sich die Reihenfolge beim Start festlegen.

## Funktionsblöcke

| Baustein | Ereignistyp | Aufgabe |
| -------- | ----------- | ------- |
| [`INIT0`](events/init/INIT0.md) | `E0Init` | Stufe 0, vor `INIT` |
| [`INIT1`](events/init/INIT1.md) | `E1Init` | Stufe 1 |
| [`INIT`](events/init/INIT.md) | `EInit` | Standardstufe |
| [`INIT2`](events/init/INIT2.md) | `E2Init` | Stufe 2, nach `INIT` |
| [`INIT3`](events/init/INIT3.md) | `E3Init` | Stufe 3, nach Stufe 2 |
| [`INIT_ON`](events/init/INIT_ON.md) | `EInit`, `EInitOn` | Standardstufe plus eigener Kanal für den Einschaltpfad |
| [`INIT_OFF`](events/init/INIT_OFF.md) | `EInit`, `EInitOff` | Standardstufe plus eigener Kanal für den Ausschaltpfad |
| [`INIT_ON_OFF`](events/init/INIT_ON_OFF.md) | `EInit`, `EInitOn`, `EInitOff` | Standardstufe plus beide Kanäle |

Die Bausteine `INIT` und die `INIT_ON`-Familie lagen früher in `iec61131-3-bool` (Paket `iec61131::booleanOperators`) und sind in diese Bibliothek umgezogen.

## Prinzip

- [`E_TRIG`](../../StandardLibraries/events/E_TRIG.md) löst alle **unverbundenen** Eingänge eines Ereignistyps (`EVENTTYPE`) in einer Ressource aus.
- Die Reihenfolge innerhalb eines Typs ist **zufällig**: Sie richtet sich nach den Adressen der StringIds. Eine feste Reihenfolge entsteht nur zwischen den Typen.
- Deshalb werden in `EMB_RES` mehrere `E_TRIG` verkettet, je Stufe einer: `START.COLD` → `E_TRIG0` (`'E0Init'`) → `E_TRIG` (`'EInit'`) → `E_TRIG2` (`'E2Init'`) → `E_TRIG3` (`'E3Init'`). Eine `E1Init`-Stufe ist möglich: Dafür wird ein `E_TRIG` mit `'E1Init'` an die gewünschte Stelle der Kette gesetzt. Der `E_TRIG` der niedrigeren Stufe muss in der Kette vor dem der höheren stehen, sonst läuft die höhere Stufe zuerst. Alle `E_TRIG` müssen in derselben Ressource liegen wie die Ziele.
- `INITn` wird direkt neben das Ziel gesetzt, `INITnO` wird mit dessen `INIT` verbunden. So muss in Ressourcen und SubApps beliebiger Tiefe nichts durch die Hierarchie verdrahtet werden. **Nicht erreicht** wird ein `INITn` im Inneren eines Composite-FB: `E_TRIG` läuft nicht in Composite-FBs hinein. Dort muss der `INITn`-Eingang an der Schnittstelle des Composite herausgeführt und nach innen durchverdrahtet werden. `E_TRIG` wirkt nur in seiner eigenen Ressource, jede Ressource braucht ihren eigenen.
- Eine Stufe gilt als durch, wenn alles **angestoßen** ist, nicht wenn alles **fertig** ist.

## Zugehörige Übungen

- [Uebung_006g_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006g_AX/): SR-Flip-Flop über `INIT_ON_OFF`, ausgelöst von zwei `E_TRIG` (Vorbild für Ereignistypen per `E_TRIG`). Solange die Übung nicht angepasst ist, steht dort noch der alte Typname `iec61131::booleanOperators::INIT_ON_OFF`; der aktuelle Name ist `events::init::INIT_ON_OFF`.
