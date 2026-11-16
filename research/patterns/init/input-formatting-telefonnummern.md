---
meta:
  title: Input-Formatierung – Telefonnummern (plattformübergreifend)
  description: Plattformübergreifende Aufstellung der Formatierungsregeln für Telefonnummern in Web, iOS und Android
relatedTopics: ["patterns/input-formatting", "patterns/input-formatting-plattformen", "patterns/form-layout"]
layout: common
---

## Verwendung

Telefonnummern sind Kennungen, keine Zahlen. Formatierung macht sie lesbar und abgleichbar: `+49 30 1234567` ist schneller erfassbar als `+49301234567`. Gleichzeitig sind Telefonfelder fehleranfällig: Nutzende tippen und fügen Nummern in vielen Schreibweisen ein.

Leitsätze:

- **Formatierung ist Darstellung, nicht Daten.** Gespeichert wird immer E.164 (`+49301234567`).
- **Alle Schreibweisen akzeptieren**, erst bei Fokusverlust formatieren.
- **Nummer als String behandeln**, niemals als Zahl (führende `0` und `+` gehen sonst verloren).

Quelle: Auszug und Vertiefung aus [input-formatting-plattformen.md](./input-formatting-plattformen.md#telefonnummern).

---

## Zielformate

| Format                           | Beispiel          | Einsatz                                              |
| -------------------------------- | ----------------- | ---------------------------------------------------- |
| E.164 (`value`)                  | `+49301234567`    | Speicherung, Schnittstellen                          |
| International (DIN 5008 / E.123) | `+49 30 1234567`  | Anzeige in internationalen Kontexten                 |
| National (DIN 5008)              | `030 1234567`     | Anzeige, wenn Land der Nummer = Region der Nutzenden |
| Durchwahl                        | `+49 30 12345-67` | Bindestrich vor Durchwahl                            |

---

## Regeln im Überblick (alle Plattformen)

| Regel                                 | Beschreibung                                                           | Eingabe               | Anzeigewert (nach Fokusverlust) | Datenwert       |
| ------------------------------------- | ---------------------------------------------------------------------- | --------------------- | ------------------------------- | --------------- |
| Formatierung nach Fokusverlust        | Im Fokus nicht umformatieren; dann parsen, formatieren, E.164 speichern | `0301234567`          | `030 1234567`                   | `+49301234567`  |
| Nach Land der Nummer formatieren      | Format folgt dem Land der Nummer, nicht der UI-Sprache                 | `+41 44 668 18 00`    | `+41 44 668 18 00`              | `+41446681800`  |
| Leerzeichen nach Vorwahl              | DIN 5008: Leerzeichen nach Länder- und Ortsvorwahl                     | `0171/1234567`        | `0171 1234567`                  | `+491711234567` |
| `(0)` entfernen                       | Die `0` der Ortsvorwahl entfällt nach `+49`                            | `+49 (0)30 123 45 67` | `+49 30 1234567`                | `+49301234567`  |
| `00` als `+` darstellen               | Internationale Vorwahl immer mit `+`                                   | `0049 30 1234567`     | `+49 30 1234567`                | `+49301234567`  |
| `tel:`-Präfix entfernen               | Beim Einfügen aus Links                                                | `tel:+4930123456`     | `+49 30 123456`                 | `+4930123456`   |
| Nicht parsebare Eingabe stehen lassen | Keine Umformatierung, stattdessen Fehlermeldung                        | `030 12`              | `030 12` + Fehlermeldung        | –               |

Weitere übergreifende Vorgaben:

- Ein einzelnes Feld; optional ein suchbares Ländervorwahl-Auswahlfeld. Keine Aufteilung in mehrere Felder.
- Keine Längenbegrenzung des Feldinhalts (7–15 Ziffern plus Formatierungszeichen).
- Klammern, Bindestriche, Schrägstriche, Punkte und Leerzeichen akzeptieren.
- Standardregion für das Parsen: gewähltes Land, sonst Region der Nutzenden.
- Weicht die Ländervorwahl im eingefügten Text vom gewählten Land ab: Auswahl nicht automatisch umstellen, sondern Hinweis anzeigen (WCAG 3.2.2).
- Sprachwechsel ändert die Formatierung nicht; nur Label, Hinweis-, Fehlertexte und Ländernamen werden übersetzt.

---

## Plattformvergleich

| Aspekt                 | Web                                                                     | iOS                                                        | Android                                                                   |
| ---------------------- | ----------------------------------------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------------- |
| Feldtyp                | `<input type="tel">`                                                    | `TextField` mit `.keyboardType(.phonePad)`                 | `TextField` mit `KeyboardType.Phone`; Views `inputType="phone"`           |
| Autofill                | `autocomplete="tel"` (getrennt: `tel-country-code` + `tel-national`)    | `.textContentType(.telephoneNumber)`                       | Compose `ContentType.PhoneNumber`; Views `autofillHints="phone"`          |
| Tastatur               | iOS: Ziffernblock mit `+ * #`; Android: Ziffernblock mit `+ ( ) -`      | `.phonePad`: `0–9 + * #`, keine Return-Taste               | Ziffernblock mit `+ * # ( ) -`                                            |
| Zeitpunkt Formatierung | `blur`                                                                  | Fokusverlust / Submit                                      | Fokusverlust                                                              |
| Formatter              | `libphonenumber-js` (`formatInternational()`, `.number` für E.164)      | `PhoneNumberKit` (keine öffentliche Apple-API)             | `PhoneNumberUtils.formatNumber(number, countryIso)` oder libphonenumber   |
| Live-Formatierung      | Nicht empfohlen                                                         | Systemweite Live-Formatierung für Drittfelder nicht verfügbar | `PhoneNumberFormattingTextWatcher` (Views), nur bei Formularen mit einem Land |
| Einfügen               | Autofill liefert oft `+49…`; Ländervorwahl nur vorbelegen, wenn leer    | QuickType-Vorschlag im Kontaktformat; `.phonePad` nur Einfügen über Kontextmenü | `InputTransformation` darf `tel:` entfernen; sonst alle Zeichen zulassen |
| Nicht verwenden        | `pattern`, `maxlength`                                                  | –                                                          | –                                                                         |

---

## Barrierefreiheit

| Plattform | Vorgaben                                                                                                          |
| --------- | ----------------------------------------------------------------------------------------------------------------- |
| Web       | Sichtbares `<label>`; Länderauswahl als `<select>` oder Combobox mit Textnamen; Flaggen nur dekorativ (`alt=""`) |
| iOS       | `.accessibilityLabel("Telefonnummer")`; Hinweis zur Ländervorwahl als `.accessibilityHint`                         |
| Android   | TalkBack liest gruppierte Nummern blockweise; Länderauswahl mit Textnamen                                         |

Übergreifend:

- Format-Hinweis (Beispielnummer) als Hilfetext, nicht nur als Platzhalter.
- Fehlermeldung nennt Ursache und Lösung, z. B. „Telefonnummer unvollständig. Beispiel: +49 30 1234567“.
- Vorlesen formatierter Nummern mit VoiceOver, TalkBack, NVDA und JAWS testen.

---

## Quellen

- [GOV.UK – Phone numbers](https://design-system.service.gov.uk/patterns/phone-numbers/)
- [W3C – Input formats](https://www.w3.org/WAI/WCAG2/supplemental/patterns/o4p08-input-formats/)
- [teltarif – Telefonnummern nach DIN 5008](https://www.teltarif.de/festnetz/telefonnummer-schreibweise.html)
