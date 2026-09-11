# Global_NAME_Functions

![Global_NAME_Functions](./Global_NAME_Functions.svg)

* * * * * * * * * *
## Einleitung
Dieser Baustein (GlobalConstants) definiert eine umfassende Sammlung globaler Konstanten für die Funktionseinstufung von Steuergeräten (ECUs) im ISOBUS-Protokoll (ISO 11783). Die Konstanten repräsentieren die **NAME Function**-Werte, die in der ISOBUS-Nomenklatur zur Identifizierung der primären Funktion eines Geräts im Fahrzeug‐ oder Maschinennetzwerk verwendet werden. Sie sind als numerische BYTE-Werte definiert und decken alle standardisierten Funktionen von 0 bis 94 sowie den Platzhalter 255 für „nicht verfügbar“ ab.

## Schnittstellenstruktur
Der Baustein stellt keine Ereignis- oder Dateneingänge bereit. Er definiert ausschließlich globale Konstanten, die als **Daten-Ausgänge** interpretiert werden können, da sie von anderen Bausteinen referenziert und abgefragt werden können.

### **Ereignis-Eingänge**
Keine.

### **Ereignis-Ausgänge**
Keine.

### **Daten-Eingänge**
Keine.

### **Daten-Ausgänge**
Die folgenden globalen Konstanten (Typ `BYTE`) werden bereitgestellt:

| Konstante | Wert | Beschreibung |
|-----------|------|--------------|
| F_ENGINE | 0 | Motor – typischerweise die mechanische Antriebsquelle der Maschine |
| F_AUX_POWER_UNIT | 1 | Hilfsaggregat (APU) – Energiequelle für Betriebssysteme ohne den Hauptantrieb |
| F_ELEC_PROPULSION_CONTROL | 2 | Elektrische Antriebssteuerung – Steuerung des elektrisch angetriebenen Antriebsmechanismus |
| F_TRANSMISSION | 3 | Getriebe – Mechanisches System zur Änderung von Drehzahl/Drehmoment des Motors |
| F_BATTERY_PACK_MONITOR | 4 | Batteriepack-Überwachung – überwacht Ladezustand, Temperatur, Restleistung usw. |
| F_SHIFT_CONTROL_CONSOLE | 5 | Schaltsteuerung/Konsole – bestimmt und sendet Gang, Bereich und Betriebsart auf das Netzwerk |
| F_POWER_TAKEOFF_MAIN_OR_REAR | 6 | Zapfwelle (Haupt oder Heck) – steuert die vom Motor abgeleitete mechanische Leistung für Zusatzgeräte |
| F_AXLE_STEERING | 7 | Achse – Lenkung – passt den Angriffswinkel in Abhängigkeit von der Lenkung an |
| F_AXLE_DRIVE | 8 | Achse – Antrieb |
| F_BRAKES_SYSTEMCONTROL | 9 | Bremssystem-Controller – elektronische Steuerung des Betriebsbremssystems |
| F_BRAKES_STEER_AXLE | 10 | Bremsen – Gelenkte Achse – Steuerung der Betriebsbremse an einer gelenkten Achse |
| F_BRAKES_DRIVE_AXLE | 11 | Bremsen – Antriebsachse – Steuerung der Betriebsbremse an einer Antriebsachse |
| F_RETARDER_ENGINE | 12 | Retarder – Motor – Steuerung der Motorbremse |
| F_RETARDER_DRIVELINE | 13 | Retarder – Antriebsstrang – Steuerung der Antriebsstrangbremse |
| F_CRUISE_CONTROL | 14 | Tempomat – Steuerungssystem zur Konstantgeschwindigkeitsregelung |
| F_FUEL_SYSTEM | 15 | Kraftstoffsystem – steuert den Kraftstofffluss vom Tank zum Filter und Motor |
| F_STEERING_CONTROL | 16 | Lenkungscontroller (Steer-by-Wire) |
| F_SUSPENSION_STEER_AXLE | 17 | Federung – Gelenkte Achse – Steuerung der Federung an einer gelenkten Achse |
| F_SUSPENSION_DRIVE_AXLE | 18 | Federung – Antriebsachse – Steuerung der Federung an einer angetriebenen Achse |
| F_INSTRUMENT_CLUSTER | 19 | Instrumententafel – Anzeige für Fahrzeugdaten, typischerweise im Armaturenbrett |
| F_TRIP_RECORDER | 20 | Fahrtenschreiber – erfasst Fahrzeugdaten (Distanz, Zeit) |
| F_CAB_CLIMATE_CONTROL | 21 | Kabinenklimasteuerung – steuert das Klima in der Kabine |
| F_AERODYNAMIC_CONTROL | 22 | Aerodynamiksteuerung – verändert z.B. Spoiler oder Luftleiteinrichtungen |
| F_VEHICLE_NAVIGATION | 23 | Fahrzeugnavigation – Position und Routenführung |
| F_VEHICLE_SECURITY | 24 | Fahrzeugsicherheit – Zugriffskontrolle und Diebstahlschutz |
| F_NETWORK_INTERCONNECT_UNIT | 25 | Netzwerk-Verbindungs-ECU – verbindet verschiedene Netzwerksegmente |
| F_BODY_CONTROLLER | 26 | Karosseriecontroller – steuert Karosseriekomponenten (nicht Chassis oder Kabine) |
| F_POWER_TAKEOFF_SECONDARY_OR_FRONT | 27 | Zapfwelle (Sekundär oder Front) – mechanische Leistungsabgabe für Zusatzgeräte |
| F_OFF_VEHICLE_GATEWAY | 28 | Off-Vehicle-Gateway – verbindet Fahrzeugnetzwerke mit externen Systemen |
| F_VTERMINAL | 29 | Virtuelles Terminal (im Fahrzeug) – intelligentes Display für Bedienung und Anzeige |
| F_MANAGEMENT_COMPUTER | 30 | Management-Computer – steuert Fahrzeugsysteme, z.B. Antriebsstrang |
| F_PROPULSION_BATTERY_CHARGER | 31 | Ladegerät für Antriebsbatterie – lädt Batterien aus externer Quelle |
| F_HEADWAY_CONTROLLER | 32 | Abstandsregelung – Kollisionsvermeidung, Warnung, Geschwindigkeitsanpassung |
| F_SYSTEM_MONITOR | 33 | Systemmonitor |
| F_HYDRAULIC_PUMP_CONTROLLER | 34 | Hydraulikpumpensteuerung – versorgt angebaute Geräte mit Hydraulikleistung |
| F_SUSPENSION_SYSTEM_CONTROLLER | 35 | Federungs-Systemcontroller – koordiniert die gesamte Federung des Fahrzeugs |
| F_PNEUMATIC_SYSTEM_CONTROLLER | 36 | Pneumatik-Systemcontroller |
| F_CAB_CONTROLLER | 37 | Kabinencontroller – verwaltet Funktionen in der Kabine |
| F_TIRE_PRESSURE_CONTROL | 38 | Reifendruckregelung – zentrale Reifenfüllung |
| F_IGNITION_CONTROL_MODULE | 39 | Zündsteuermodul – steuert die Zündung eines Motors |
| F_SEAT_CONTROL | 40 | Sitzsteuerung – bedient Sitz und Federung, Positionierung |
| F_LIGHTING_OPERATOR_CONTROLS | 41 | Beleuchtungssteuerung – Senden von Beleuchtungsbefehlen durch Bedienelemente |
| F_WATER_PUMP_CONTROL | 42 | Wasserpumpensteuerung – z.B. für Feuerwehrfahrzeuge oder Nutzfahrzeuge |
| F_TRANSMISSION_DISPLAY | 43 | Getriebe-Display – zeigt Getriebeinformationen |
| F_EXHAUST_EMISSION_CONTROL | 44 | Abgasemissionssteuerung |
| F_VEHICLE_DYNAMIC_STABILITY_CONTROL | 45 | Fahrzeugdynamik- und Stabilitätskontrolle |
| F_OIL_SENSOR_UNIT | 46 | Ölsensoreinheit |
| F_INFORMATION_SYSTEM_CONTROLLER | 47 | Informationssystem-Controller – z.B. Transit-, Fracht- oder Flottenmanagement |
| F_RAMP_CONTROL | 48 | Rampensteuerung – Ladung, Entladung, Hublifte, Heckklappen |
| F_CLUTCH_CONVERTER_CONTROL | 49 | Kupplungs-/Wandlersteuerung – steuert den Drehmomentwandler oder die Motor-Getriebe-Verbindung |
| F_AUXILIARY_HEATER | 50 | Zusatzheizung – Heizung ohne laufenden Hauptmotor |
| F_FORWARD_LOOKING_COLLISION_WARNING_SYSTEM | 51 | Vorausschauendes Kollisionswarnsystem – erkennt und warnt vor Hindernissen |
| F_CHASSIS_CONTROLLER | 52 | Chassis-Controller – steuert Chassis-Komponenten (nicht Karosserie oder Kabine) |
| F_ALTERNATOR_CHARGING_SYSTEM | 53 | Lichtmaschine/Ladegerät – primäre Ladeeinrichtung des Fahrzeugs |
| F_COMMUNICATIONS_UNIT_CELLULAR | 54 | Kommunikationseinheit, Mobilfunk – Kommunikation über Mobilfunknetz |
| F_COMMUNICATIONS_UNIT_SATELLITE | 55 | Kommunikationseinheit, Satellit – Kommunikation über Satellit |
| F_COMMUNICATIONS_UNIT_RADIO | 56 | Kommunikationseinheit, Funk – terrestrische Punkt-zu-Punkt-Kommunikation |
| F_AUXILIARY_DEVICE | 57 | Lenksäuleneinheit – sammelt Bedieneingaben von Lenkrad und Schaltern |
| F_FAN_DRIVE_CONTROL | 58 | Lüftersteuerung – steuert den Hauptkühllüfter des Motors |
| F_STARTER | 59 | Anlasser – startet den Motor, hier die Steuerung des Anlassers |
| F_CAB_DISPLAY | 60 | Kabinen-Display – erweiterte Anzeige (mehr als 30 Zeichen) |
| F_FILESERVER | 61 | Dateiserver/ Drucker – Druck- oder Speichereinheit im Netzwerk |
| F_ON_BOARD_DIAGNOSTIC_UNIT | 62 | Bordsystem-Diagnoseeinheit – Diagnosetool, ggf. dauerhaft installiert |
| F_ENGINE_VALVE_CONTROLLER | 63 | Motorventilsteuerung – steuert Einlass-/Auslassventile des Motors |
| F_ENDURANCE_BRAKING | 64 | Dauerbremsung – Gesamtheit aller verschleißfreien Bremsfunktionen |
| F_GAS_FLOW_MEASUREMENT | 65 | Gasdurchflussmessung – misst Gasdurchsatz und zugehörige Parameter |
| F_I_O_CONTROLLER | 66 | E/A-Controller – Berichts- und Steuergerät für externe Ein-/Ausgänge |
| F_ELECTRICAL_SYSTEM_CONTROLLER | 67 | Elektriksystem-Controller – z.B. Lastzentralen, Sicherungskästen |
| F_AFTERTREATMENT_SYSTEM_GAS_MEASUREMENT | 68 | Abgasnachbehandlung Gasmesstechnik – misst Gaseigenschaften vor/nach der Nachbehandlung |
| F_ENGINE_EMISSION_AFTERTREATMENT_SYSTEM | 69 | Motor-Abgasnachbehandlungssystem |
| F_AUXILIARY_REGENERATION_DEVICE | 70 | Zusätzliches Regenerationsgerät – Teil eines Nachbehandlungssystems |
| F_TRANSFER_CASE_CONTROL | 71 | Verteilergetriebesteuerung – steuert Anzahl der angetriebenen Räder (z.B. 2WD/4WD) |
| F_COOLANT_VALVE_CONTROLLER | 72 | Kühlmittelventilsteuerung – steuert Kühlmittelstrom für thermische Systeme |
| F_ROLLOVER_DETECTION_CONTROL | 73 | Überrollschutzsteuerung – erkennt drohenden Fahrzeugüberschlag |
| F_LUBRICATION_SYSTEM | 74 | Schmiersystem – pumpt Schmiermittel an Gelenke und bewegliche Teile |
| F_SUPPLEMENTAL_FAN | 75 | Zusatzlüfter – zusätzlicher Kühllüfter |
| F_TEMPERATURE_SENSOR | 76 | Temperatursensor – misst Temperatur |
| F_FUEL_PROPERTIES_SENSOR | 77 | Kraftstoffeigenschaftssensor – misst Kraftstoffeigenschaften |
| F_FIRE_SUPPRESSION_SYSTEM | 78 | Feuerlöschsystem |
| F_POWER_SYSTEMS_MANAGER | 79 | Energiesystemmanager – verwaltet Leistungsabgabe mehrerer Energiequellen |
| F_ELECTRIC_POWERTRAIN | 80 | Elektrischer Antriebsstrang – koordiniert ein elektrisches Antriebssystem |
| F_HYDRAULIC_POWERTRAIN | 81 | Hydraulischer Antriebsstrang – koordiniert ein hydraulisches Antriebssystem |
| F_FILE_SERVER | 82 | Dateiserver – Speichereinheit im Netzwerk |
| F_PRINTER | 83 | Drucker – Druckeinheit im Netzwerk |
| F_START_AID_DEVICE | 84 | Startunterstützungsgerät – z.B. Glühkerzen, Gitterheizung |
| F_ENGINE_INJECTION_CONTROL_MODULE | 85 | Motor-Einspritzsteuerung – steuert Kraftstoffeinspritzung |
| F_EV_COMMUNICATION_CONTROLLER | 86 | EV-Kommunikationscontroller – verwaltet Verbindung zu externer Ladeeinrichtung |
| F_DRIVER_IMPAIRMENT_DEVICE | 87 | Fahrerbeeinträchtigungs-Gerät – verhindert Start bei Beeinträchtigung (z.B. Alkohol-Interlock) |
| F_ELECTRIC_POWER_CONVERTER | 88 | Elektrischer Leistungswandler – Wechselrichter oder Konverter |
| F_SUPPLY_EQUIPMENT_COMMUNICATION_CONTROLLER_SECC | 89 | SECC – Kommunikationscontroller in Ladestation (EVSE) |
| F_VEHICLE_ADAPTER_COMMUNICATION_CONTROLLER_VACC | 90 | VACC – Kommunikationscontroller im Adapter zwischen EVSE und Fahrzeug |
| F_ACCESSORY_ELECTRIC_MOTOR_CONTROLLER | 91 | Elektrische Zusatzantriebssteuerung – für elektrifizierte Zubehörkomponenten |
| F_CURRENT_SENSOR | 92 | Stromsensor – misst elektrischen Strom |
| F_FUEL_CELL_SYSTEM | 93 | Brennstoffzellensystem – erzeugt Strom aus Wasserstoff |
| F_AUXILIARY_DISPLAY | 94 | Zusatzdisplay – externe oder interne Anzeige, separat zum Kabinendisplay |
| F_NOT_AVAILABLE | 255 | Nicht verfügbar – Platzhalter für unbekannte Funktion |

### **Adapter**
Keine.

## Funktionsweise
Die Konstanten werden in der ISOBUS-Kommunikation verwendet, um die Funktion eines Steuergeräts gemäß dem **NAME**-Parameter zu klassifizieren. Jeder numerische Wert entspricht einem standardisierten Funktionscode, der in den entsprechenden Nachrichten (z.B. BAM, RTS/CTS oder TP.DT) gesendet wird. Durch die Verwendung dieser Konstanten wird sichergestellt, dass alle Netzwerkteilnehmer (z.B. Traktor, Anbaugerät, virtuelles Terminal) dieselbe Bedeutung für einen Funktionscode verwenden. Der Baustein selbst enthält keine ausführbare Logik, sondern dient als zentrale Definitionsquelle für die 4diac-IDE-Anwendungen.

## Technische Besonderheiten
- **Typ:** Alle Konstanten sind als `BYTE` definiert.
- **Geltungsbereich:** Die Werte entsprechen den Definitionen der ISO 11783 (Teil 7) und sind branchenübergreifend anerkannt.
- **Erweiterbarkeit:** Die Liste kann bei neuen Fahrzeugfunktionen erweitert werden; der Platzhalter `F_NOT_AVAILABLE` (255) deckt unbekannte Funktionen ab.
- **Compiler-Einbindung:** Die Konstanten werden im Package `isobus::pgn::const` definiert und können in 4diac-Projekten über den GlobalConstants-Baustein referenziert werden.

## Zustandsübersicht
Entfällt – da der Baustein keine Zustandsautomaten oder prozessabhängige Logik besitzt, sondern ausschließlich passive Konstanten bereitstellt.

## Anwendungsszenarien
- **ISOBUS-Anwendungen:** Verwendung als Referenzwerte zur Identifizierung von Steuergerätefunktionen in der Netzwerkkommunikation.
- **Diagnose:** Analyse von Nachrichten und Zuordnung von Funktionscodes zu Geräten.
- **Visualisierung:** Anzeige von Geräteinformationen in virtuellen Terminals oder Diagnose-Tools.
- **Schnittstellendefinition:** Einheitliche Kennzeichnung von Funktionen in Softwareprojekten, die ISOBUS-basierte Steuergeräte implementieren.

## Vergleich mit ähnlichen Bausteinen
Da es sich um eine globale Konstantensammlung handelt, gibt es keine direkten funktionalen Bausteine zum Vergleich. Andere GlobalConstants-Elemente könnten ähnliche Listen für andere Protokolle oder Zwecke bereitstellen, aber `Global_NAME_Functions` ist speziell auf ISOBUS-Funktionscodes zugeschnitten und bietet eine vollständige, normkonforme Auflistung.

## Fazit
Der Baustein `Global_NAME_Functions` stellt eine wichtige, standardisierte Ressource für die Entwicklung von ISOBUS-Anwendungen in der 4diac-IDE dar. Er erleichtert die Implementierung, indem er alle gängigen Funktionscodes als symbolische Konstanten bereitstellt. Dadurch wird die Lesbarkeit des Codes verbessert, die Wartung vereinfacht und die Konformität mit ISO 11783 sichergestellt. Obwohl er keine aktive Steuerungslogik enthält, ist er ein unverzichtbares Werkzeug für die einheitliche Kommunikation in landwirtschaftlichen und mobilen Maschinennetzwerken.