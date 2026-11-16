---
meta:
  title: Input-Formatierung – Telefonnummern
  description: Plattformübergreifende Formatierungsregeln für Telefonnummern mit Vorgaben für Web, iOS und Android
relatedTopics: ["patterns/input-formatting", "patterns/input-formatting-plattformen", "patterns/form-layout"]
layout: common
---

Gliederung wie die Eingabetypen in [input-formatting-plattformen.md](./input-formatting-plattformen.md#telefonnummern):

- **Plattformübergreifend**: Grundlagen, Formatierungsregeln mit Beispielen, Verhalten bei Copy/Paste, Verhalten bei Sprachwechsel
- **Web**, **iOS**, **Android**: Feldkonfiguration und Tastatur, Zeitpunkt und API der Formatierung, Copy/Paste, Barrierefreiheit

Quellen und Standards: [input-formatting-quellen.md](./input-formatting-quellen.md#telefonnummern)

---

## Plattformübergreifend

| Format                           | Beispiel          | Einsatz                                              |
| -------------------------------- | ----------------- | ---------------------------------------------------- |
| E.164 (`value`)                  | `+49301234567`    | Speicherung, Schnittstellen                          |
| International (DIN 5008 / E.123) | `+49 30 1234567`  | Anzeige in internationalen Kontexten                 |
| National (DIN 5008)              | `030 1234567`     | Anzeige, wenn Land der Nummer = Region der Nutzenden |
| Durchwahl                        | `+49 30 12345-67` | Bindestrich vor Durchwahl                            |

- Ein einzelnes Feld; optional ein suchbares Ländervorwahl-Auswahlfeld. Keine Aufteilung in mehrere Felder.
- Keine Längenbegrenzung des Feldinhalts (7–15 Ziffern plus Formatierungszeichen).
- Telefonnummern sind Kennungen und werden als String gespeichert, nie als Zahl.
- Beispiele unten: Standardregion DE.

### Formatierungsregeln

| Regel                                 | Beschreibung                                                                                                                                                                                                                                                              | Eingabe               | Anzeigewert                                                 | Datenwert       |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- | ----------------------------------------------------------- | --------------- |
| Formatierung nach Fokusverlust        | Nicht live umformatieren, GOV.UK empfiehlt, alle Schreibweisen zu akzeptieren ([GOV.UK](https://design-system.service.gov.uk/patterns/phone-numbers/)). Bei Fokusverlust parsen (Standardregion = gewähltes Land oder Region der Nutzenden), formatieren, E.164 speichern | `0301234567`          | Im Fokus: `0301234567`<br/>nach Fokusverlust: `030 1234567` | `+49301234567`  |
| Nach Land der Nummer formatieren      | Format folgt dem Land der Nummer (libphonenumber bzw. Plattform-APIs), nicht der UI-Sprache                                                                                                                                                                               | `+41 44 668 18 00`    | `+41 44 668 18 00`                                          | `+41446681800`  |
| Leerzeichen nach Vorwahl setzen       | DIN 5008: Leerzeichen nach Länder- und Ortsvorwahl, keine weiteren Gruppen im Teilnehmeranschluss ([teltarif](https://www.teltarif.de/festnetz/telefonnummer-schreibweise.html))                                                                                          | `0171/1234567`        | `0171 1234567`                                              | `+491711234567` |
| `(0)` entfernen                       | Die `0` der Ortsvorwahl entfällt nach `+49`; `+49 (0)30 …` gilt als falsch                                                                                                                                                                                                | `+49 (0)30 123 45 67` | `+49 30 1234567`                                            | `+49301234567`  |
| `00` als `+` darstellen               | Internationale Vorwahl immer mit `+`                                                                                                                                                                                                                                      | `0049 30 1234567`     | `+49 30 1234567`                                            | `+49301234567`  |
| Nicht parsebare Eingabe stehen lassen | Text bleibt unverändert, Fehlermeldung statt Umformatierung                                                                                                                                                                                                               | `030 12`              | `030 12` + Fehlermeldung                                    | –               |

### Verhalten bei Copy/Paste (Normalisierung)

- Klammern, Bindestriche, Schrägstriche, Punkte, Leerzeichen akzeptieren ([W3C](https://www.w3.org/WAI/WCAG2/supplemental/patterns/o4p08-input-formats/)).
- `(0)` nach Ländervorwahl entfernen; `00` am Anfang als `+` interpretieren; `tel:`-Präfix entfernen.
- Weicht die Ländervorwahl im eingefügten Text vom ausgewählten Land ab: Auswahl **nicht automatisch** umstellen, sondern Hinweis anzeigen (WCAG 3.2.2).

### Verhalten bei Sprachwechsel

- **Format folgt dem Land der Nummer**: Ein Sprachwechsel ändert die Formatierung nicht. `+49 30 1234567` bleibt in jeder Sprache gleich.
- **Nur Texte wechseln**: Ländernamen in der Vorwahl-Auswahl, Label, Hinweis- und Fehlertexte werden übersetzt.

---

## Web

- **Feld**: `type="tel"`, `autocomplete="tel"` (bei getrennter Ländervorwahl `tel-country-code` + `tel-national`); kein `pattern`, kein `maxlength`.
- **Tastatur**: iOS Telefon-Ziffernblock mit `+ * #`; Android Ziffernblock der Tastatur-App mit `+ ( ) -`.
- **Formatierung**: bei `blur` mit `libphonenumber-js` (`parsePhoneNumber(text, defaultCountry).formatInternational()` / `.number` für E.164).
- **Copy/Paste**: Autofill liefert häufig `+49…` – Ländervorwahl-Auswahl daraus nur vorbelegen, wenn sie leer ist.
- **Barrierefreiheit**: Länderauswahl als `<select>` oder Combobox mit Textnamen; Flaggen nur dekorativ (`alt=""`).

## iOS

- **Feld**: `.keyboardType(.phonePad)`, `.textContentType(.telephoneNumber)`; Toolbar mit „Fertig“ (keine Return-Taste).
- **Formatierung**: bei Fokusverlust mit `PhoneNumberKit` (Port von libphonenumber); Apple bietet keine öffentliche API zur Telefonnummern-Formatierung. Die systemweite Live-Formatierung aus der Telefon-App ist für Drittfelder nicht verfügbar.
- **Copy/Paste**: AutoFill-Vorschlag in der QuickType-Leiste liefert die eigene Nummer im Format der Kontakte – Parser muss Leerzeichen, Klammern und Bindestriche akzeptieren. `.phonePad` bietet nur über das Kontextmenü Einfügen.
- **Barrierefreiheit**: `.accessibilityLabel("Telefonnummer")`, Hinweis zur Ländervorwahl als `.accessibilityHint`.

## Android

- **Feld**: `KeyboardOptions(keyboardType = KeyboardType.Phone)`; Autofill über `semantics { contentType = ContentType.PhoneNumber }`; Views `inputType="phone"` + `autofillHints="phone"`.
- **Formatierung**: bei Fokusverlust mit `PhoneNumberUtils.formatNumber(number, countryIso)` oder libphonenumber. `PhoneNumberFormattingTextWatcher` (Views) formatiert live nach Geräteregion – nur für Formulare mit einem Land verwenden.
- **Copy/Paste**: `InputTransformation` darf `tel:` entfernen; sonst alle Zeichen zulassen.
- **Barrierefreiheit**: TalkBack liest gruppierte Nummern blockweise; Länderauswahl mit Textnamen.
