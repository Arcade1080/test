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
| Formatierung nach Fokusverlust        | Nicht live umformatieren; alle Schreibweisen akzeptieren ([GOV.UK](https://design-system.service.gov.uk/patterns/telephone-numbers)). Bei Fokusverlust parsen (Standardregion = gewähltes Land oder Region der Nutzenden), formatieren, E.164 speichern. **Abweichung von GOV.UK:** GOV.UK rät davon ab, Nutzenden eine umformatierte Version ihrer Eingabe zu zeigen. Alternative: Eingabe unverändert lassen und nur normalisiert speichern | `0301234567`          | Im Fokus: `0301234567`<br/>nach Fokusverlust: `030 1234567` | `+49301234567`  |
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

## Design Systeme im Vergleich

Stand: Suchauszüge, Originalseiten nicht abgerufen. Nicht untersucht: Ant Design, Chakra, Fluent, Spectrum.

**Ohne eigene Vorgaben zu Telefonnummern gefunden** (Suche ohne Treffer, keine Aussage über den tatsächlichen Stand):

- **Atlassian Design System**: keine offizielle Seite gefunden. Auf `atlassian.design` direkt prüfen.
- **IBM Carbon**: keine Telefon-Komponente gefunden; nur ein Community-Request für ein Telefonfeld mit Ländervorwahl-Auswahl (Titel eines Issues, Inhalt und Status nicht geprüft).
- **Material Design 3**: keine offizielle Vorgabe gefunden. Das Community-gepflegte Material-Web-Dokument nennt `type="tel"` (Telefontastatur) und `inputmode` für mehr Kontrolle ([Material Web Text field](https://github.com/material-components/material-web/blob/main/docs/components/text-field.md)).

| Design System / Ansatz     | Eingabe und Formatierung                                                                                                                                         | Quelle                                                                                                                         |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| **GOV.UK**                 | `type="tel"`, `autocomplete="tel"`; alle Schreibweisen zulassen; keine Input-Maske; keine umformatierte Anzeige der Eingabe (z. B. ergänztes `+44`); Fehlermeldung mit Beispielformaten | [Phone numbers](https://design-system.service.gov.uk/patterns/telephone-numbers)                                               |
| **ONS**                    | Übernimmt GOV.UK: Feld muss alle Varianten zulassen                                                                                                              | [ONS Service Manual](https://service-manual.ons.gov.uk/design-system/patterns/phone-numbers)                                   |
| **Salesforce Lightning**   | Eingabe `lightning-input type="tel"`; Anzeige separat über `lightning-formatted-phone` (`tel:`-Link); Format nach Locale, bei `en-US`/`en-CA` `(999) 999-9999`; `+` davor verhindert das | [lightning-formatted-phone](https://developer.salesforce.com/docs/platform/lightning-component-reference/guide/lightning-formatted-phone) |
| **Shopify Polaris**        | Eigene Komponente „Phone field“ mit Ländervorwahl-Formatierung; `type` nur `mobile` oder leer, keine zusätzliche Validierung nach Typ; `autocomplete="tel"` immer setzen (Best Practice des Text field) | [Phone field](https://shopify.dev/docs/api/checkout-ui-extensions/2025-10-rc/polaris-web-components/forms/phonefield), [Text field](https://polaris.shopify.com/components/text-field) |
| **USWDS**                  | Empfiehlt Input-Maske `___-___-____` für US-Nummern plus Validierung auf Client und Server (steht im Widerspruch zu GOV.UK; eigene Tests der Input-Mask-Komponente zeigen Barrierefreiheits-Mängel, siehe Insights in `input-formatting-plattformen.md`) | [Phone number pattern](https://designsystem.digital.gov/patterns/create-a-user-profile/phone-number/) |
| **Salt**                    | Eigenes Muster „International phone number input“; bei formatierten Eingaben Hilfetext mit Beispiel, Platzhalter nie als einzige Anleitung | [International phone number](https://saltdesignsystem.com/salt/patterns/international-phone-number-input), [Formatted input](https://www.saltdesignsystem.com/salt/patterns/formatted-input) |
| **Porsche Design System**  | Eigene Komponente `input-tel`; nutzt die Browser-Validierung für das Telefonformat | [input-tel](https://designsystem.porsche.com/v3/components/input-tel/usage/) |
| **Komponenten mit libphonenumber** (z. B. Michelin, Signicat, react-phone-number-input) | Parsen und Formatieren über libphonenumber; Ländervorwahl in eigener, suchbarer Auswahl; Gültigkeit je Land | [react-phone-number-input](https://cdn.jsdelivr.net/npm/react-phone-number-input@3.4.17/README.md), [Michelin](https://designsystem.michelin.com/components/phone-number), [Signicat](https://catnip.signicat.com/components/input-phone/guidelines.html) |

**Einordnung**

| Ansatz                                | Vorteil                                                   | Nachteil                                                            | Wann sinnvoll                                    |
| ------------------------------------- | --------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------ |
| Eingabe unverändert, nur normalisiert | Keine Irritation, Nutzende sehen ihre Schreibweise        | Uneinheitliche Anzeige im Formular                                  | Freitextnahe Felder, Behörden-Services (GOV.UK) |
| Formatierung bei Fokusverlust         | Zeigt, wie die Nummer verstanden wurde; einheitliche Anzeige | Kann Nutzende verwirren; setzt Länderwissen voraus (Standardregion) | Internationale Formulare mit Ländervorwahl-Auswahl |
| Anzeige-Komponente getrennt vom Feld  | Format nur beim Lesen (Salesforce)                        | Zwei Komponenten                                                    | Detail- und Listenansichten                      |

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
