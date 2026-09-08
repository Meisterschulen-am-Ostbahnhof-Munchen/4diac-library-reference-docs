# TODO

## AUX Type 2 – Wandler-Abdeckung prüfen

Prüfen, ob `Bibliotheken/ExternalLibraries/isobus/...` für **jeden**
Auxiliary Function Type 2 Sub-Typ aus Tabelle J.5 der ISO 11783-6 einen
passenden Wandler-FB hat, nicht nur für einen Teil davon.

Quelle: `G:\Geteilte Ablagen\Classroom\Students\Literatur\Normen\ISO 11783 ISOBUS\ISO 11783-6_2018-06-00_EN_2866291.pdf`,
Table J.5 — Auxiliary Function Type 2 types.

Beispielwerte aus dieser Tabelle (Function-State-Semantik, z. B. für
eine 3-Stellungs-Steuerung):

- 0 = Off = centre
- 1 = On = forward, up or right
- 4 = On = backward, down or left
