# Design System Vergleich – Input-Formatierung

Dieser Vergleich zeigt, wie etablierte Design Systeme strukturierte Eingabefelder (Currency, IBAN, Datum) handhaben und dokumentieren.

---

## GOV.UK Design System

### Beschreibung

GOV.UK hat einen der tiefgründigsten evidenzbasierten Ansätze: Das System trennt konsequent zwischen Date Input (drei separate Felder: Tag, Monat, Jahr) und Date Picker (Kalender-Overlay). Die Entscheidung zwischen beiden Varianten folgt klar definierten Kriterien. Für numerische Eingaben hat GOV.UK 2020 bewusst von `type="number"` auf `type="text"` mit `inputmode="numeric"` gewechselt, da `type="number"` auf mobilen Geräten und mit Screenreadern erhebliche Probleme verursacht.

### Vorteile

- Evidenzbasierte Entscheidungen durch umfangreiche Nutzerforschung (z. B. Beobachtung, dass Nutzende Monatsnamen eingeben, nicht Zahlen)
- Klare Trennung: Date Input für bekannte Daten, Date Picker für Kalenderauswahl
- Starker Fokus auf Fehlerbehandlung mit spezifischen Fehlertexten pro Fehlertyp
- Automatisches Tab-Verbot zwischen Datumsfeldern (verhindert Verwirrung)
- Separate Felder mit gemeinsamem `<fieldset>` und `<legend>` für Screenreader

### Nachteile

- Datum-Komponente verwendet 3 separate Felder (Tag, Monat, Jahr), was Copy-Paste erschwert
- Kein nativer IBAN-Support
- Weniger Locale-Flexibilität (primär britischer Kontext)

### Quelle

[GOV.UK Design System – Date Input](https://design-system.service.gov.uk/components/date-input/)

---

## Material Design 3 (Google)

### Beschreibung

Material Design 3 bietet zwei Eingabemethoden für Daten: direktes Tippen ins Textfeld und Kalender-Picker. Beide sind über ein einziges Eingabeelement erreichbar, wobei ein Kalender-Icon als exklusiver Einstiegspunkt für den Picker dient. Material Design empfiehlt explizit, Input-Masken während der Eingabe zu vermeiden, da sie Screenreader verwirren; stattdessen wird erst nach Blur oder Enter formatiert. Für Currency verwendet Material Design `type="text"` mit `inputmode="decimal"` und `Intl.NumberFormat`.

### Vorteile

- Zwei Eingabemethoden (Tippen + Picker) für unterschiedliche Nutzerpräferenzen
- Detaillierte Keyboard-Navigation dokumentiert (Page Up/Down für Monate, Shift+M/Y für Listen)
- Klare Accessibility-Anforderungen: 48×48dp Mindest-Touch-Target
- Explizite Warnung vor Input-Masken während der Eingabe
- Gute Locale-Unterstützung durch `Intl.DateTimeFormat`

### Nachteile

- Date-Picker-Accessibility-Tests werden an implementierende Teams delegiert
- Kein dediziertes IBAN-Muster dokumentiert
- Komplexere Implementierung durch duale Eingabemethoden

### Quelle

[Material Design 3 – Date Pickers Accessibility](https://m3.material.io/components/date-pickers/accessibility)

---

## U.S. Web Design System (USWDS)

### Beschreibung

USWDS bietet seit 2025 (v3.12.0) einen Date Picker mit automatischer Lokalisierung über die `Date.toLocaleString` API basierend auf dem `lang`-Attribut. Der Ansatz kombiniert Textfeld (immer aktiviert) und Kalender-Overlay. Kernprinzip: Nutzende müssen immer manuell tippen können, auch wenn ein Picker vorhanden ist. Das System testet aktiv auf Screenreader-Kompatibilität und dokumentiert bekannte Probleme transparent.

### Vorteile

- Automatische Übersetzung der Kalenderbezeichnungen via `Date.toLocaleString` (seit v3.12.0)
- Transparente Changelog-Dokumentation aller Accessibility-Fixes
- Starker Fokus auf Tastaturnavigation (klar dokumentierte Shortcuts)
- Pflicht zur manuellen Datumseingabe parallel zum Picker

### Nachteile

- Primär auf US-Kontext ausgelegt (MM/DD/YYYY als Default)
- Kein Currency- oder IBAN-Muster
- Accessibility-Tests müssen pro Projekt wiederholt werden

### Quelle

[USWDS – Date Picker](https://designsystem.digital.gov/components/date-picker/)

---

## React Aria / Adobe Spectrum

### Beschreibung

React Aria bietet die umfangreichste Internationalisierungsunterstützung unter den analysierten Design Systemen. Der `NumberField` nutzt intern einen Custom-Parser, der alle Locales unterstützt, und `Intl.NumberFormat` für die Ausgabe. Besonders stark: Der `DatePicker` versteht über `@internationalized/date` verschiedene Kalendersysteme (Gregorianisch, Buddhistisch, Hebräisch, Islamisch). Die Library wurde speziell mit dem Ziel entwickelt, Internationalisierung „richtig zu machen" statt sie nachträglich hinzuzufügen.

### Vorteile

- Volle Locale-Unterstützung für Currency, Datum und Zahlen aus einer Hand
- Mehrere Kalendersysteme nativ unterstützt
- Flexibles Parsing: akzeptiert sowohl `,` als auch `.` als Dezimaltrenner je nach Locale
- Sehr gute Accessibility durch WAI-ARIA-Muster

### Nachteile

- Fokus auf React-Ökosystem (kein Vanilla-JS)
- Höhere Implementierungskomplexität
- Kein explizit dokumentiertes IBAN-Pattern

### Quelle

[React Aria – How we internationalized our NumberField](https://react-aria.adobe.com/blog/how-we-internationalized-our-numberfield)

---

## Carbon Design System (IBM)

### Beschreibung

Carbon dokumentiert Formulare mit expliziten Anti-Patterns. Für numerische Eingaben empfiehlt Carbon `type="text"` + `inputmode="decimal"`, da `type="number"` keine Locale-Formatierung unterstützt (offene Issue seit 2022). Die Validierungsstrategie ist besonders durchdacht: nach dem ersten Fehler wird sofort bei jeder Änderung validiert (`hasBeenValidated`-Pattern), vor dem ersten Fehler nur bei Blur. Placeholder-Text wird nur für Format-Hinweise verwendet, nie als Label-Ersatz.

### Vorteile

- Klares Anti-Pattern-Dokumentation (Spinner-Buttons, `type="number"`, Placeholder als Label)
- Durchdachte „Validate-after-first-error"-Strategie
- Gute Accessibility-Dokumentation für Formularkomponenten
- YYYY-MM-DD Datum-Placeholder als Standard empfohlen

### Nachteile

- Locale-Formatierung für Numbers noch nicht vollständig implementiert (aktive Issue #19722)
- Kein IBAN-Pattern
- Datum-Picker hat weniger Internationalisierung als Material Design oder React Aria

### Quellen

[Carbon – Forms Pattern](https://carbondesignsystem.com/patterns/forms-pattern/)  
[Carbon – Number Input](https://carbondesignsystem.com/components/number-input/usage)

---

## Shopify Polaris

### Beschreibung

Polaris verfolgt einen Mobile-First-Ansatz mit klarer Trennung zwischen Display- und Datenwert. Die `MoneyField`-Komponente ist eine dedizierte Currency-Komponente auf Basis des CLDR (Common Locale Data Repository) und unterstützt zwei Modi: Short Format (Single-Currency) und Explicit Format (Multi-Currency mit Währungscode). Polaris definiert explizit: „Always store and transmit the raw, unformatted value. Formatting is a presentation concern, not a data concern."

### Vorteile

- Dedizierte `MoneyField`-Komponente für Currency
- CLDR-basierte Formatierung für maximale Locale-Korrektheit
- Klares Principle: Rohwert senden, Formatierung ist Darstellungssache
- Gute Dokumentation zu Multi-Currency-Szenarien

### Nachteile

- Kein IBAN-Pattern
- Datum-Picker weniger detailliert dokumentiert als GOV.UK oder Material Design
- Shopify-spezifische Annahmen (E-Commerce-Kontext)

### Quelle

[Polaris – Formatting Localized Currency](https://polaris-react.shopify.com/foundations/formatting-localized-currency)

---

## Ant Design

### Beschreibung

Ant Design bietet mit `InputNumber` eine flexible Komponente mit expliziter `formatter`- und `parser`-Prop-Architektur. Das Prinzip lautet: „The formatter is for display, the parser is for extracting the raw value. Never store formatted values in state or send them to the backend." Paste-Verhalten ist automatisch: alle Formatierungszeichen werden entfernt, der Rohwert extrahiert. Der `DatePicker` unterstützt Locale-Wechsel zur Laufzeit.

### Vorteile

- Klare `formatter`/`parser`-Trennung in der API
- Automatisches Parsing bei Paste ohne Fehlermeldung
- Gute Locale-Unterstützung für Datum und Zahlen
- Explizite Warnung gegen `type="number"` in der Dokumentation

### Nachteile

- Custom-Formatter kann fehleranfällig sein (Regex-basiert)
- Kein IBAN-Pattern
- Weniger Guidance zu Accessibility als GOV.UK oder Carbon

### Quelle

[Ant Design – InputNumber](https://ant.design/components/input-number)  
[Ant Design – Data Format Guidelines](https://ant.design/docs/spec/data-format/)

---

---

## GOV.UK Design System – Telefonnummern

### Beschreibung

GOV.UK dokumentiert ein eigenständiges Phone-Number-Pattern mit klarer Empfehlung: ein einzelnes `type="tel"`-Feld, kein getrenntes Vorwahl-Feld für den nationalen Kontext. Für internationale Nummern wird im Hint-Text erklärt, dass die Ländervorwahl mitangegeben werden soll. Das Pattern ist evidenzbasiert und enthält spezifische Fehlermeldungstexte für verschiedene Fehlerzustände.

### Vorteile

- Klare Empfehlung: ein Feld statt mehrerer
- Spezifische Fehlermeldungen je Fehlertyp dokumentiert
- `autocomplete="tel"` explizit vorgeschrieben
- Feldbreite am erwarteten Inhalt orientiert (nicht Fullwidth)

### Nachteile

- Primär UK-Kontext, kein internationaler Vorwahl-Selektor dokumentiert
- Keine Empfehlung zur Formatierung bei Blur

### Quelle

[GOV.UK Design System – Phone Numbers](https://design-system.service.gov.uk/patterns/phone-numbers/)

---

## VA.gov Design System – Telephone Input

### Beschreibung

VA.gov (US-Veteranenbehörde) hat ein umfangreiches Architecture Decision Record für ihr Telephone Input-Pattern veröffentlicht. Besonders wertvoll: Die explizite Entscheidung, Länder-Dropdown und Nummernfeld nicht automatisch miteinander zu verknüpfen, weil das WCAG 3.2.2 verletzt. Flags werden gezeigt, aber als Icon mit Text-Äquivalent.

### Vorteile

- Transparente ADR-Dokumentation mit Begründungen für jede Entscheidung
- Explicit: kein Cross-Field-Update zwischen Länder-Selektor und Nummernfeld
- Klare Mobile-Stacking-Regel ab 360px Viewport

### Nachteile

- US-spezifischer Kontext
- MVP-Ansatz mit bekannten Einschränkungen (z. B. kein i18n)

### Quelle

[VA.gov Design System – Telephone Input Design Decisions](https://design.va.gov/components/form/telephone-input/design-decisions/)

---

## U.S. Web Design System (USWDS) – Time Picker

### Beschreibung

USWDS bietet einen Time Picker als Dropdown-Overlay über einem Textfeld. Die Kernkonfiguration erfolgt über Data-Attribute: `data-min-time`, `data-max-time`, `data-step` (Schrittweite in Minuten, Standard 30). Interner Wert immer im 24h-Format `HH:MM`. Starke Accessibility-Anforderungen: kein Auto-Submit, immer ein sichtbares Label.

### Vorteile

- Einfache Konfiguration via Data-Attribute
- Konsequent 24h intern, unabhängig von der Anzeige
- Transparente Changelog-Dokumentation aller Accessibility-Fixes
- Explizites Verbot von Auto-Submit

### Nachteile

- Primär US-Kontext
- Kein nativer Timezone-Support

### Quelle

[USWDS – Time Picker](https://designsystem.digital.gov/components/time-picker/)

---

## GitLab Pajamas – Date and Time

### Beschreibung

Pajamas dokumentiert einen Decision Flow für Datum- und Zeit-Darstellung (nicht nur Eingabe), der zwischen relativen Formaten („vor 30 Minuten") und absoluten Formaten (lokalisiert oder ISO 8601) unterscheidet. Besonders wertvoll für Systeme, in denen Datum/Zeit sowohl eingegeben als auch angezeigt wird.

### Vorteile

- Klarer Decision Flow: wann relativ, wann absolut, wann ISO 8601
- Unterscheidung zwischen normalem und kompaktem Anzeigeformat
- Berücksichtigt Browser-Locale-Erkennung

### Nachteile

- Fokus auf Darstellung, weniger auf Eingabe-UX
- Kein Kreditkarten- oder IBAN-Pattern

### Quelle

[GitLab Pajamas – Date and Time](https://design.gitlab.com/content/date-and-time)

---

## Zusammenfassung

| Design System         | Currency       | Datum          | IBAN | Telefon      | Kreditkarte | PLZ | Uhrzeit     | Locale           | Accessibility  |
| --------------------- | -------------- | -------------- | ---- | ------------ | ----------- | --- | ----------- | ---------------- | -------------- |
| **GOV.UK**            | ⚠️ Grundlegend | ✅ Exzellent   | ❌   | ✅ Sehr gut  | ❌          | ⚠️  | ❌          | ⚠️ UK-fokussiert | ✅ Exzellent   |
| **Material Design 3** | ✅ Gut         | ✅ Sehr gut    | ❌   | ❌           | ❌          | ❌  | ✅ Gut      | ✅ Gut           | ✅ Sehr gut    |
| **USWDS**             | ❌             | ✅ Gut         | ❌   | ✅ Gut       | ❌          | ❌  | ✅ Sehr gut | ⚠️ US-fokussiert | ✅ Sehr gut    |
| **VA.gov**            | ❌             | ❌             | ❌   | ✅ Exzellent | ❌          | ❌  | ❌          | ⚠️ US-fokussiert | ✅ Exzellent   |
| **React Aria**        | ✅ Exzellent   | ✅ Exzellent   | ❌   | ❌           | ❌          | ❌  | ✅ Sehr gut | ✅ Exzellent     | ✅ Exzellent   |
| **Carbon**            | ⚠️ In Arbeit   | ✅ Gut         | ❌   | ❌           | ❌          | ❌  | ❌          | ⚠️ In Arbeit     | ✅ Sehr gut    |
| **Polaris**           | ✅ Sehr gut    | ⚠️ Grundlegend | ❌   | ❌           | ❌          | ❌  | ❌          | ✅ Sehr gut      | ✅ Gut         |
| **Ant Design**        | ✅ Sehr gut    | ✅ Gut         | ❌   | ❌           | ❌          | ❌  | ✅ Gut      | ✅ Gut           | ⚠️ Grundlegend |
| **GitLab Pajamas**    | ❌             | ✅ Gut         | ❌   | ❌           | ❌          | ❌  | ✅ Gut      | ✅ Gut           | ✅ Gut         |

**Auffällig**: Kein einziges der analysierten Design Systeme dokumentiert ein dediziertes IBAN- oder Kreditkarten-Pattern. IBAN-Eingaben werden von der UX-Community in Fachforen behandelt; Kreditkarten-UX durch Baymard Institute und E-Commerce-fokussierte Ressourcen. Postleitzahlen sind meist Teil von Adressformular-Patterns, selten eigenständig dokumentiert.
