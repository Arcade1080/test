---
meta:
  title: Input-Formatierung – Quellen und Standards
  description: Belastbare Quellen und Standards für die Formatierungsregeln von Währungsbeträgen, IBAN, Prozent, Stunden, Telefonnummern und Postleitzahlen
relatedTopics: ["patterns/input-formatting", "patterns/input-formatting-plattformen", "patterns/input-formatting-prozent", "patterns/input-formatting-telefonnummern"]
layout: common
---

Diese Übersicht ordnet die Regeln aus [input-formatting-plattformen.md](./input-formatting-plattformen.md) Normen, Spezifikationen und Empfehlungen zu.

**Einordnung der Belege**

| Stufe              | Bedeutung                                                                                       |
| ------------------ | ----------------------------------------------------------------------------------------------- |
| Bestätigt          | Inhalt per Websuche bestätigt; Originaltext der Norm nicht eingesehen                           |
| Indirekt           | Nur über Sekundärquellen (z. B. Wikipedia) belegt; Originalnorm prüfen                          |
| Nicht verifiziert  | Aus Fachwissen, Quelle nicht abgerufen (Stand November 2026); vor Verwendung gegenprüfen        |
| Empfehlung         | Keine Norm, sondern Designentscheidung oder Praxiserfahrung; Begründung im jeweiligen Dokument |

---

## Übergreifend

| Regel                                                  | Quelle / Standard                                                                                                                        | Stufe             |
| ------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------- | ----------------- |
| Trennzeichen, Symbolposition und Abstand je Locale     | [CLDR – Number and currency patterns](https://cldr.unicode.org/translation/number-currency-formats/number-and-currency-patterns), [UTS #35](https://unicode.org/reports/tr35/tr35-general.html) | Bestätigt         |
| Alle Schreibweisen akzeptieren, nicht live umformatieren | [W3C – Input formats](https://www.w3.org/WAI/WCAG2/supplemental/patterns/o4p08-input-formats/), [GOV.UK – Phone numbers](https://design-system.service.gov.uk/patterns/phone-numbers/) | Nicht verifiziert |
| Formatierung nach Fokusverlust                         | Designentscheidung; Beispiele: [GOV.UK](https://technology.blog.gov.uk/2020/02/24/why-the-gov-uk-design-system-team-changed-the-input-type-for-numbers/), [Adam Silver](https://adamsilver.io/blog/the-problem-with-input-masks-and-what-to-do-instead/) | Empfehlung        |
| Keine automatische Umstellung durch Eingabe            | [WCAG 2.2 – 3.2.2 On Input](https://www.w3.org/TR/WCAG22/#on-input)                                                                      | Nicht verifiziert |
| Label mit Einheit und Format-Hinweis                   | [WCAG 2.2 – 3.3.2 Labels or Instructions](https://www.w3.org/TR/WCAG22/#labels-or-instructions)                                          | Nicht verifiziert |
| Autofill-Zwecke (`tel`, `transaction-amount`)          | [WCAG 2.2 – 1.3.5 Identify Input Purpose](https://www.w3.org/TR/WCAG22/#identify-input-purpose), HTML Standard (autocomplete)            | Nicht verifiziert |
| Wert und Anzeige trennen                               | Designentscheidung                                                                                                                       | Empfehlung        |
| Formatter-APIs der Plattformen                         | [ECMA-402 / `Intl.NumberFormat`](https://tc39.es/ecma402/), [Apple `FormatStyle`](https://developer.apple.com/documentation/swiftui/textfield/init(_:value:format:prompt:), [Android ICU `NumberFormat`](https://developer.android.com/reference/android/icu/text/NumberFormat) | Nicht verifiziert |

---

## Währungsbeträge

| Regel                                          | Quelle / Standard                                                                                              | Stufe             |
| ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------- | ----------------- |
| EUR hat 2 Nachkommastellen                     | ISO 4217 (Minor Unit), bestätigt u. a. durch [Wikipedia: ISO 4217](https://ipfs.ssi.eecc.de/ipns/en.wikipedia-on-ipfs.org/wiki/ISO_4217) und Zahlungsanbieter-Doku ([Adyen](https://docs.adyen.com/development-resources/currency-codes)) | Bestätigt         |
| Andere Währungen: Nachkommastellen nach ISO 4217 | Z. B. ISK 0 Stellen; Anbieter weichen teils ab (Adyen: ISK mit 2 Stellen). Währung nie fest verdrahten      | Bestätigt         |
| Symbolposition und Trennzeichen nach Locale    | CLDR (siehe Übergreifend); [Polaris – Formatting localized currency](https://polaris.shopify.com/foundations/formatting-localized-currency) | Bestätigt (CLDR)  |
| Symbol außerhalb des Felds                     | [Apple Developer Forums](https://developer.apple.com/forums/thread/757272), Praxiserfahrung                    | Empfehlung        |
| Keine stille Rundung                           | Designentscheidung                                                                                             | Empfehlung        |

---

## IBAN

| Regel                                        | Quelle / Standard                                                                                                                                                      | Stufe     |
| -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| Struktur, maximal 34 alphanumerische Zeichen | [ISO 13616-1:2020](https://standards.iteh.ai/catalog/standards/iso/27fd365c-ab25-47d5-950f-47a0ee51f432/iso-13616-1-2020), [Banco de Portugal](https://www.bportugal.pt/sites/default/files/anexos/documentos-relacionados/international_bank_account_number_en.pdf) | Bestätigt |
| Papierformat: Vierergruppen mit Leerzeichen  | ISO 13616-1, Anhang A (laut Zusammenfassung des Standards)                                                                                                             | Bestätigt |
| Elektronisches Format: ohne Trennzeichen     | ISO 13616-1; [SWIFT – IBAN in commercial payments](https://www.swift.com/sites/default/files/documents/swift_pmpg_whitepaper_ibanincommercialpayments.pdf)                 | Bestätigt |
| Prüfziffer nach MOD 97-10                    | ISO/IEC 7064 MOD 97-10; Ablauf: Leerzeichen entfernen, vier Zeichen ans Ende, Buchstaben A=10…Z=35, Rest 1 = gültig                                                   | Bestätigt |
| Maske vermeiden, Gruppierung per Formatter   | [GOV.UK – Bank details](https://design-system.service.gov.uk/patterns/bank-details/)                                                                                   | Nicht verifiziert |

---

## Prozent

| Regel                                              | Quelle / Standard                                                                                                                                                  | Stufe             |
| -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------- |
| Leerzeichen zwischen Zahl und `%` (SI)             | SI-Brochure, ISO 31-0 (Nachfolger ISO 80000-1), laut [Wikipedia: Percent sign](https://en.wikipedia.org/wiki/Percent_sign)                                          | Indirekt          |
| Leerzeichen vor `%` im Deutschen                   | DIN 5008:2020-03, Abschnitt 10.7, laut [Wikipedia (de): %](https://dewiki.de/Lexikon/%25); Duden empfiehlt ein schmaleres, festes Leerzeichen                       | Indirekt          |
| `de`: `#,##0 %`; Abstand und Position je Locale     | [CLDR](https://cldr.unicode.org/translation/number-currency-formats/number-and-currency-patterns), [Android-CLDR-Daten `en_DE`](https://android.googlesource.com/platform/external/cldr/+/85c4052f81380347338c14a2e9bf27873454ec9e/common/main/en_DE.xml) | Bestätigt |
| `fr-FR` schmales Leerzeichen, `tr-TR` vorangestellt | CLDR-Locale-Daten                                                                                                                                                  | Nicht verifiziert |
| Formatter erwarten den Anteil (×100)               | ECMA-402 `style: 'percent'`, Apple `.percent`, ICU `getPercentInstance`                                                                                            | Nicht verifiziert |
| Gleitkomma-Artefakt (`0.1 * 100`)                  | IEEE 754                                                                                                                                                           | Nicht verifiziert |
| Eingefügter Anteil nicht automatisch ×100          | Designentscheidung                                                                                                                                                 | Empfehlung        |

---

## Stunden

| Regel                                | Quelle / Standard                                                                                                                       | Stufe             |
| ------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------- | ----------------- |
| Dauer-Notation und Austauschformat   | ISO 8601 (Dauer `PT1H30M`); Anzeige abweichend nach Locale                                                                              | Nicht verifiziert |
| Lokalisierte Dauer-Formatierung      | [MDN – `Intl.DurationFormat`](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Intl/DurationFormat/DurationFormat) | Nicht verifiziert |
| Dezimal- oder Zeitnotation wählen    | Fachliche Entscheidung je Anwendungsfall                                                                                                | Empfehlung        |

---

## Telefonnummern

| Regel                                           | Quelle / Standard                                                                                                                                                       | Stufe             |
| ----------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- |
| E.164: nur Ziffern, maximal 15 Stellen          | ITU-T E.164, bestätigt u. a. durch [Wikipedia: E.164](https://en.wikipedia.org/wiki/E.164), [Twilio – What is E.164](https://www.twilio.com/docs/glossary/what-e164.md)  | Bestätigt         |
| Ländervorwahl 1–3 Stellen, Rest nationale Nummer | ITU-T E.164 (siehe oben)                                                                                                                                                | Bestätigt         |
| `+` ist Notation, nicht Teil der Nummer         | ITU-T E.164 / E.123; im Datenwert dennoch `+` führen (E.164-Konvention der Bibliotheken)                                                                                | Bestätigt (E.164) |
| Schreibweise für Anzeige                        | ITU-T E.123 (Notation); [teltarif – DIN 5008](https://www.teltarif.de/festnetz/telefonnummer-schreibweise.html)                                                          | Indirekt          |
| `(0)` nach Ländervorwahl weglassen              | DIN 5008 / E.123, laut teltarif                                                                                                                                         | Indirekt          |
| Alle Schreibweisen akzeptieren, keine Maske     | [GOV.UK – Phone numbers](https://design-system.service.gov.uk/patterns/telephone-numbers), [ONS](https://service-manual.ons.gov.uk/design-system/patterns/phone-numbers) (Suchauszug), [W3C – Input formats](https://www.w3.org/WAI/WCAG2/supplemental/patterns/o4p08-input-formats/) | Bestätigt (GOV.UK, ONS) |
| Umformatieren bei Fokusverlust                  | Keine Norm. GOV.UK rät von umformatierter Anzeige ab (Notify-Beobachtung); Umformatieren ist eine bewusste Abweichung                   | Empfehlung        |
| Parsing und Formatierung                        | libphonenumber (Google), `libphonenumber-js`, `PhoneNumberKit`, `PhoneNumberUtils` (Android)                                                                            | Nicht verifiziert |

Hinweis: Die Angaben zur aktuellen Edition von E.164 weichen in den Quellen ab (Wikipedia: Juni 2020; ein Anbieter nennt 02/2026). Aktuelle Fassung bei der ITU prüfen.

---

## Postleitzahlen (PLZ)

| Regel                                     | Quelle / Standard                                                                                                          | Stufe             |
| ----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- | ----------------- |
| Format je Land verschieden                | UPU-Postadress-Daten; [W3C – Address formats](https://w3c.github.io/i18n-drafts/questions/qa-address-formats.en.html)      | Nicht verifiziert |
| Deutschland: 5 Ziffern, ohne Leerzeichen  | DIN 5008, Deutsche Post                                                                                                    | Nicht verifiziert |
| PLZ als String speichern (führende `0`)   | Folgt aus dem Format; keine eigene Norm                                                                                    | Empfehlung        |

---

## Offene Prüfungen

- Originaltexte von ISO 80000-1, ISO 13616-1, ISO 8601 und DIN 5008 beschaffen (kostenpflichtig) oder Aussagen über Normbüchereien prüfen.
- Alle als „Nicht verifiziert“ markierten Seiten abrufen und Zitate übernehmen (u. a. MDN, GOV.UK, Apple Developer, Android Developers, W3C).
- Locale-Beispiele (`fr-FR`, `tr-TR`, `de-CH`) direkt in den CLDR-Daten gegenprüfen.
- ITU-T E.164: aktuelle Edition bestätigen.
