---
meta:
  title: Input-Formatierung – Prozent
  description: Plattformübergreifende Formatierungsregeln für Prozentwerte mit Vorgaben für Web, iOS und Android
relatedTopics: ["patterns/input-formatting", "patterns/input-formatting-plattformen", "patterns/form-layout"]
layout: common
---

Gliederung wie die Eingabetypen in [input-formatting-plattformen.md](./input-formatting-plattformen.md#prozent):

- **Plattformübergreifend**: Grundlagen, Formatierungsregeln mit Beispielen, Verhalten bei Copy/Paste, Verhalten bei Sprachwechsel
- **Web**, **iOS**, **Android**: Feldkonfiguration und Tastatur, Zeitpunkt und API der Formatierung, Copy/Paste, Barrierefreiheit

---

## Plattformübergreifend

- Festlegen und dokumentieren, ob `value` den **Prozentwert** (`12.5`) oder den **Anteil** (`0.125`) speichert. Prozent-Formatter aller Plattformen erwarten den Anteil und multiplizieren mit 100.
- Wertebereich fachlich definieren: 0–100 bei Anteilen, aber > 100 und negativ bei Veränderungsraten möglich.
- Beispiele unten: 1 Nachkommastelle, Datenwert als Prozentwert.

### Formatierungsregeln

| Regel                                 | Beschreibung                                                                                                                                                                                                                          | Eingabe | Anzeigewert                                                      | Datenwert |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | ---------------------------------------------------------------- | --------- |
| Formatierung nach Fokusverlust        | Während der Eingabe bleibt der Text unverändert; formatiert wird erst bei Fokusverlust                                                                                                                                                | `12,5`  | Im Fokus: `12,5`<br/>nach Fokusverlust de: `12,5 %`, en: `12.5%` | `12.5`    |
| Fehlende Nachkommastellen auffüllen   | Anzahl fachlich festgelegt (z. B. Zinssatz 2–3 Stellen, Rabatt 0 Stellen)                                                                                                                                                             | `12`    | de: `12,0 %`<br/>en: `12.0%`                                     | `12.0`    |
| Dezimaltrenner der Locale anzeigen    | Komma und Punkt werden beide angenommen; Trennzeichen-Regeln wie bei Währungsbeträgen                                                                                                                                                 | `12.5`  | de: `12,5 %`<br/>en: `12.5%`                                     | `12.5`    |
| Prozentzeichen aus dem Feld entfernen | `%` steht außerhalb des editierbaren Texts. Abstand und Position nach Locale: `de-DE` mit geschütztem Leerzeichen (DIN 5008), `de-CH` und `en-US` ohne, `fr-FR` mit schmalem geschütztem Leerzeichen, `tr-TR` vorangestellt (`%12,5`) | `12,5%` | de: `12,5 %`<br/>en: `12.5%`                                     | `12.5`    |
| Überzählige Nachkommastellen runden   | Auf die definierte Genauigkeit runden; vermeidet auch Gleitkomma-Artefakte (`0.1 * 100 = 10.000000000000002`)                                                                                                                         | `12,34` | de: `12,3 %`<br/>en: `12.3%`                                     | `12.3`    |
| Wertebereich prüfen                   | Werte über 100 sind je nach Fachlichkeit gültig (Veränderungsrate) oder ein Fehler (Anteil)                                                                                                                                           | `150`   | de: `150,0 %` oder Fehler<br/>en: `150.0%` oder Fehler           | `150.0`   |

### Verhalten bei Copy/Paste (Normalisierung)

- `%`, `Prozent`, `pct` und Leerzeichen entfernen; Trennzeichen-Regeln wie bei Währungsbeträgen.
- Eingefügter Anteil (`0,125`) wird **nicht** automatisch als 12,5 % interpretiert – Wert bleibt `0,125 %`, ggf. Plausibilitätshinweis.

### Verhalten bei Sprachwechsel

- **Daten bleiben, Anzeige wechselt**: Der Wert (`12.5`) ändert sich nicht. Dezimaltrenner, Leerzeichen und Position des `%` folgen der neuen Locale: `12,5 %` (`de-DE`) → `12.5%` (`en-US`).
- **Kein Umformatieren im Fokus**: Ein fokussiertes Feld behält seinen Text und wird erst beim nächsten Fokusverlust in der neuen Locale formatiert.
- **Parser und Formatter aus einer Quelle**: Der Parser liest die Trennzeichen aus dem Formatter des Felds, damit die angezeigte Schreibweise immer wieder eingelesen werden kann.

---

## Web

- **Feld**: `type="text"`, `inputmode="decimal"` (bzw. `numeric` ohne Nachkommastellen); `%` als visuelles Suffix.
- **Formatierung**: bei `blur` mit `Intl.NumberFormat(locale, { maximumFractionDigits })` – **nicht** `style: 'percent'` im Feld, da dann das `%` Teil des Texts wird; das Suffix übernimmt die Darstellung. Für reine Anzeige außerhalb von Feldern `style: 'percent'` mit Anteil verwenden.
- **Position des Suffix**: aus `formatToParts()` mit `style: 'percent'` ablesen (`tr-TR`: Prefix).
- **Copy/Paste**: Normalisierung bei `blur`.
- **Barrierefreiheit**: Label „Rabatt in Prozent“, Suffix `aria-hidden="true"`. Bei Stepper-Komponenten (React Aria NumberField) `aria-valuetext` mit formatiertem Wert.

## iOS

- **Feld**: `TextField(value: $percent, format: .number.precision(.fractionLength(1)))` mit separatem `Text("%")`; alternativ `format: .percent` mit Anteil als Wert – dann ist `%` Teil des Texts und muss beim Tippen mit eingegeben bzw. stehen gelassen werden, was fehleranfällig ist.
- **Tastatur**: `.decimalPad` bzw. `.numberPad`; Toolbar mit „Fertig“.
- **Formatierung**: bei Submit/Fokusverlust; Rundung über `.rounded(rule:increment:)`.
- **Barrierefreiheit**: `.accessibilityLabel("Rabatt in Prozent")`; VoiceOver liest `12,5 %` als „12,5 Prozent“.

## Android

- **Feld**: Compose `TextField` mit `suffix = { Text("%") }`; `KeyboardType.Decimal` bzw. `KeyboardType.Number`.
- **Formatierung**: bei Fokusverlust mit `android.icu.text.NumberFormat.getInstance(locale)`; Prozentformat (`getPercentInstance`) nur für reine Anzeige.
- **Copy/Paste**: `InputTransformation` darf `%` und Leerzeichen sofort entfernen (nur Trennzeichen, keine Bedeutungsänderung).
- **Barrierefreiheit**: Label mit Einheit; Suffix-Ansage durch TalkBack auf Geräten prüfen.
