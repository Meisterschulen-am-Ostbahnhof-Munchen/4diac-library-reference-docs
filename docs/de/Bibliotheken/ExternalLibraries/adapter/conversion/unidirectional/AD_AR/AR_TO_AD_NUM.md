# AR_TO_AD_NUM

![AR_TO_AD_NUM](./AR_TO_AD_NUM.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsbaustein `AR_TO_AD_NUM` ist ein Composite-FB, der eine numerisch korrekte Umwandlung eines REAL-Adapter-Signals (AR) in ein DWORD-Adapter-Signal (AD) durchführt. Im Gegensatz zu einer einfachen Bit-Reinterpretation wird der REAL-Wert über einen internen UDINT-Zwischenschritt in die entsprechende DWORD-Zahl konvertiert. Dadurch bleibt der numerische Wert erhalten – ein REAL-Wert von `50.0` wird zu `DWORD#50`, nicht zum rohen IEEE-754-Bitmuster.

Dieser Baustein ist als drop-in-kompatible Alternative zu `AR_TO_AD` gedacht und vermeidet dessen typische Fehlerquelle bei der Übergabe von Sollwerten (z. B. Prozentwerten oder PWM-Tastgraden) an Ausgänge, die einen direkten Zahlenwert erwarten.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **E1** (vom Socket `AR_IN`): Wird ausgelöst, wenn der angeschlossene REAL-Adapter eine gültige Datenübertragung signalisiert. Dieses Ereignis startet die Konvertierungskette.

### **Ereignis-Ausgänge**

- **E1** (vom Plug `AD_OUT`): Wird nach Abschluss der Konvertierung ausgegeben und signalisiert, dass der DWORD-Wert am Daten-Ausgang gültig ist.

### **Daten-Eingänge**

- **D1** (vom Socket `AR_IN`): REAL-Eingangswert, der in einen DWORD-Wert umgewandelt werden soll.

### **Daten-Ausgänge**

- **D1** (vom Plug `AD_OUT`): Konvertierter DWORD-Wert, der den numerischen Wert des Eingangs als vorzeichenlose 32‑Bit-Ganzzahl enthält.

### **Adapter**

- **Socket `AR_IN`**: Typ `adapter::types::unidirectional::AR` (REAL-Adapter) – stellt den Eingang für den zu konvertierenden Wert dar.
- **Plug `AD_OUT`**: Typ `adapter::types::unidirectional::AD` (DWORD-Adapter) – stellt den Ausgang für den konvertierten Wert dar.

## Funktionsweise

Der Baustein arbeitet in zwei Schritten:

1. **Reale numerische Konvertierung REAL → UDINT**  
   Der REAL-Eingangswert wird über den internen Funktionsbaustein `F_REAL_TO_UDINT` in eine vorzeichenlose 32‑Bit-Ganzzahl (UDINT) umgewandelt. Dies ist ein echter numerischer Cast, kein Bit-Umschreiben.

2. **Bit-Identische Umwandlung UDINT → DWORD**  
   Der UDINT-Wert wird über `F_UDINT_TO_DWORD` in einen DWORD-Wert interpretiert. Da UDINT und DWORD beide exakt 32‑Bit breit sind und dieselbe Bit-Darstellung für vorzeichenlose Integer verwenden, ist dieser Schritt verlustfrei.

Die Ereigniskette ist: `AR_IN.E1` → `ToUDINT.REQ` → (nach Konvertierung) `ToUDINT.CNF` → `ToDWORD.REQ` → (nach Konvertierung) `ToDWORD.CNF` → `AD_OUT.E1`.

Die Datenkette: `AR_IN.D1` → `ToUDINT.IN` → `ToUDINT.OUT` → `ToDWORD.IN` → `ToDWORD.OUT` → `AD_OUT.D1`.

Damit wird der numerische Wert des REAL-Eingangs exakt als DWORD-Ausgang bereitgestellt.

## Technische Besonderheiten

- **Vermeidung der Bit-Reinterpretationsfalle**: Der direkte Weg über `F_REAL_TO_DWORD` (wie in `AR_TO_AD`) würde das IEEE-754-Bitmuster des REAL-Werts erzeugen. `AR_TO_AD_NUM` umgeht dies durch den Zwischenschritt über UDINT.
- **Drop-in-Kompatibilität**: Der Baustein hat exakt dieselbe Signatur wie `AR_TO_AD`, sodass er einfach ausgetauscht werden kann.
- **Unidirektionale Adapter**: Sowohl Ein- als auch Ausgang sind als unidirektionale Adapter (nur Senden bzw. Empfangen) ausgelegt.
- **Kein interner Zustand**: Es findet keine Speicherung statt; die Konvertierung erfolgt rein ereignisgesteuert.

## Zustandsübersicht

Der Baustein besitzt keinen expliziten internen Zustand. Er verhält sich wie eine reine Transformationskette: Nach einer Anforderung über `E1` am Eingang wird nach Durchlauf der beiden Konvertierungsstufen der Ergebniswert am Ausgang bereitgestellt. Es gibt keine Verzögerungen außer der reinen Verarbeitungszeit der enthaltenen Funktionsbausteine.

## Anwendungsszenarien

- **PWM-Tastgrad-Ausgabe**: Wenn ein REAL-Sollwert (z. B. `50.0` für 50 %) direkt als PWM-Tastgrad an einen Ausgang wie `logiBUS_QDA_PWM` übergeben werden soll, erwartet dieser Ausgang einen DWORD-Zahlenwert. `AR_TO_AD_NUM` liefert genau diesen Wert.
- **Prozentwert-Weitergabe**: Ein prozentualer Sollwert (0…100) kann als DWORD interpretiert und an nachgelagerte Bausteine übergeben werden, ohne dass eine Bit-Konvertierung die Bedeutung verfälscht.
- **Ersatz für manuelle Verkettung**: Die früher übliche manuelle Verdrahtung über `AR_TO_AUDI` + `AUDI_TO_AD` wird durch diesen Baustein in einem einzigen Baustein gebündelt.

## Vergleich mit ähnlichen Bausteinen

| Baustein            | Konvertierung                                  | Ergebnis                      |
|---------------------|------------------------------------------------|-------------------------------|
| `AR_TO_AD`          | REAL → DWORD (direkt, bitweise)                | IEEE-754-Bitmuster            |
| `AR_TO_AD_NUM`      | REAL → UDINT → DWORD (numerisch)               | Zahlenwert als DWORD          |
| `AR_TO_AUDI` + `AUDI_TO_AD` | REAL → UDINT (numerisch) + UDINT → DWORD (bitidentisch) | Zahlenwert als DWORD (manuell) |

Während `AR_TO_AD` für die reine Bit-Serialisierung von REAL-Werten geeignet ist (z. B. zur Übertragung über einen Kommunikationskanal), ist `AR_TO_AD_NUM` für numerische Anwendungen konzipiert, bei denen der DWORD-Wert denselben Zahlenwert wie der REAL-Eingang repräsentieren soll.

## Fazit

`AR_TO_AD_NUM` ist eine wichtige Ergänzung für Steuerungsanwendungen, die einen REAL-Sollwert als numerischen DWORD-Wert an Ausgabebausteine übergeben müssen. Durch die bewusste Umgehung der bitweisen Konvertierung vermeidet er häufig auftretende, schwer zu diagnostizierende Fehler und vereinfacht die Projektierung erheblich. Er ist die empfohlene Wahl, wenn der REAL-Wert als Zahl (z. B. Prozent- oder Tastgrad) interpretiert werden soll.
