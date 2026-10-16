# UX Research – Input-Formatierung

Erkenntnisse aus unabhängigen UX-Forschungsquellen zu strukturierten Eingabefeldern: Currency, IBAN und Datumseingaben.

---

## Kernprinzip: Robustheitsprinzip (Postel's Law)

„Be liberal in what you accept, conservative in what you send."

Dieses Prinzip aus dem Netzwerkprotokoll-Design gilt direkt für Input-Formatierung: Eingaben so tolerant wie möglich akzeptieren, intern normalisieren, und nur valide Rohwerte senden. Nutzende verwenden unterschiedliche Formate für dieselben Daten – das ist keine Ausnahme, sondern die Regel.

**Quelle**: [Smashing Magazine – A Complete Guide To Live Validation UX](https://www.smashingmagazine.com/2022/09/inline-validation-web-forms-ux/)

---

## Validierungszeitpunkt: „Validate late, suggest early"

### Kernaussage

Fehlermeldungen erst zeigen, wenn die Eingabe abgeschlossen ist (onBlur), aber positive Hinweise bereits während der Eingabe geben.

### Belege

- **Nielsen Norman Group**: „Don't wait until submit to validate. Inline validation reduces errors and increases completion rates." Validierung erst beim Absenden führt zu Frustration bei mehreren Fehlern, Kontextverlust und erhöhter kognitiver Last.

- **Carbon Design System**: „Validate as close to the point of interaction as possible, while being mindful not to interrupt the user's flow." Empfehlung: Bei bereits invaliden Feldern sofort validieren, bei noch nicht validierten erst bei Blur.

- **Ausnahme**: Kurze Formulare mit wenigen Feldern können auf Submit-Validierung setzen, da Nutzende dort weniger Kontext verlieren.

**Quellen**: [Nielsen Norman Group – Inline Validation in Web Forms](https://www.nngroup.com/articles/inline-validation/) | [Smashing Magazine – Live Validation UX](https://www.smashingmagazine.com/2022/09/inline-validation-web-forms-ux/)

---

## Formatvarianten: Nutzende ignorieren Format-Hinweise

### Befund (Baymard Institute)

Baymard Institute hat in Usability-Tests beobachtet, dass **89% der Desktop-Nutzenden** Format-Beispiele in Eingabefeldern ignorieren und ihr eigenes bevorzugtes Format verwenden. Getestet an:

- Telefonnummern: `(555) 555-5555` / `555 555 5555` / `555-555-5555` / `5555555555`
- Kreditkartennummern, Mitgliedsnummern, Ablaufdaten, Geburtsdaten

### Konsequenz

Strikte Format-Anforderungen führen zu Validierungsfehlern, die Nutzende als willkürlich erleben. Die Lösung ist technisches flexibles Parsing, nicht strengere Format-Vorgaben.

**Quelle**: [Baymard Institute – 8 Recommendations for Creating Effective Input Fields](https://baymard.com/learn/input-fields)

---

## Datumseingabe: Drei Eingabemethoden im Vergleich

### Methoden und ihre Eignung (Nielsen Norman Group)

| Methode | Geeignet für | Nicht geeignet für |
|---------|-------------|-------------------|
| **Freitexteingabe** | Bekannte Daten (Geburtstag, Vertragsdaten), weit entfernte Vergangenheit/Zukunft | Unbekannte Daten, Datumsauswahl aus verfügbaren Optionen |
| **Kalender-Picker** | Buchungsdienste, nahe Termine, Datumsbereich-Auswahl | Geburtstage, historische Daten (viel Scrollen nötig) |
| **Dropdown (Tag/Monat/Jahr)** | Nur noch < 10 Optionen | Jede andere Situation (zu viel Interaktionskosten) |

### Kritische Erkenntnisse

- Nutzende, die ihr Datum kennen (Geburtstag, Vertragsstart), tippen es am liebsten
- Kalender-Picker für weit zurückliegende oder zukünftige Daten erfordern viel Scrollen
- Dreiteilige Dropdowns (Tag/Monat/Jahr) sind die ineffizienteste Methode: zu viele Klicks und Scrolls
- **Immer beide Methoden anbieten**: Tippen + Picker, nie nur einen Weg erzwingen

**GOV.UK-Forschung**: In Tests wurden Nutzende beobachtet, die Monatsnamen statt -zahlen eingaben. Nach Anpassung der Komponente (Monatsnamen akzeptieren) sanken Fehler dramatisch.

**Quellen**: [Nielsen Norman Group – Date-Input Form Fields](https://www.nngroup.com/articles/date-input/) | [GOV.UK Design System – Date Input Research](https://design-system.service.gov.uk/components/date-input/)

---

## Input-Masken: Mehr Schaden als Nutzen

### Problem

Input-Masken (automatisches Einfügen von Trennzeichen während der Eingabe) verursachen häufig:

- Cursor-Sprünge bei Backspace oder Korrekturen
- Kollision mit Copy-Paste (eingefügter Text wird falsch interpretiert)
- Verwirrung bei Screenreadern (geänderter Wert wird neu vorgelesen)
- Unterschiedliche Erwartungen je nach Gerät (Desktop vs. Mobile)

### Material Design explizit dagegen

> „Don't automatically format the date by adding slashes or other special characters while the user is typing (also known as input masks). This can cause confusion for people using screen readers because it changes what they typed."

### Ausnahme: IBAN-Leerzeichen

Das Hinzufügen von Leerzeichen zur Gruppenbildung (IBAN: `DE89 3704 0044`) ist akzeptabel, wenn:
1. Nur Leerzeichen hinzugefügt werden (kein anderes Zeichen)
2. Die Cursor-Position stabil bleibt
3. Backspace das Leerzeichen überspringt, nicht das letzte Zeichen entfernt

**Quellen**: [Material Design – Accessible Date Input](https://m3.material.io/components/date-pickers/accessibility) | [Smashing Magazine – Live Validation UX](https://www.smashingmagazine.com/2022/09/inline-validation-web-forms-ux/)

---

## Mobilgeräte: Korrekte Tastatur ist kritisch

### `inputmode`-Verhalten nach Gerät

| inputmode | iOS-Tastatur | Android-Tastatur | Empfehlung |
|-----------|-------------|-----------------|------------|
| `decimal` | Ziffern + Punkt (`.`) | Locale-abhängig | Currency |
| `numeric` | Ziffern + Symbole | Ziffern | IBAN (nicht ideal) |
| `tel` | Telefontastatur | Telefontastatur | Telefonnummern |
| `none` | Keine Tastatur | Keine Tastatur | Kalender-Picker |

### iOS-Problem bei Currency

iOS zeigt bei `inputmode="decimal"` nur den Punkt (`.`) als Dezimaltrenner, auch in deutschen Locales. **Konsequenz**: Parser muss immer beide Trennzeichen (`,` und `.`) akzeptieren, unabhängig von der angezeigten Tastatur.

### Keine getrennten Felder auf Mobile

Smashing Magazine und Nielsen Norman Group warnen: Getrennte Felder für strukturierte Eingaben (z. B. drei Felder für Tag/Monat/Jahr oder vier Felder für IBAN-Blöcke) erzeugen auf Mobile mehr Friction als ein einzelnes Feld. Nutzende müssen mehrfach die Tastatur öffnen/schließen und zwischen Feldern wechseln.

**Quellen**: [Nielsen Norman Group – Mobile Input Checklist](https://www.nngroup.com/articles/mobile-input-checklist/) | [Smashing Magazine – Live Validation](https://www.smashingmagazine.com/2022/09/inline-validation-web-forms-ux/)

---

## IBAN-Spezifika: Forschungslage

### UX Stack Exchange Konsens

Da kein etabliertes Design System ein IBAN-Pattern dokumentiert, basieren Best Practices auf Community-Forschung und Banking-UX-Projekten:

**Einfaches Textfeld mit Live-Gruppenformatierung** wird gegenüber getrennten Feldern bevorzugt, weil:
- Copy-Paste aus Online-Banking oder E-Mail-Anhängen funktioniert (häufiger Use Case)
- Nutzende IBAN oft von Papierdokumenten abtippen und dabei Blöcke vergleichen
- Getrennte Felder Tab-Navigation erfordern, die Screenreader-Nutzende besonders belastet

**Validierungsstrategie bei IBAN**:
- Format-Check (Länge, Ländercode-Prefix) bei Blur
- Prüfsummen-Validierung (Mod-97) bei Blur oder Submit
- Keine Prüfsummenvalidierung während der Eingabe (Nutzer tippt noch)

**Uppercase-Transformation**: Automatische Konvertierung `de89...` → `DE89...` bei Eingabe ist akzeptabel und erwartet.

**Quelle**: [UX Stack Exchange – IBAN Input by Separate Fields](https://ux.stackexchange.com/questions/59979/html4-insert-iban-by-separate-fields) | [Baymard Institute – Effective Input Fields](https://baymard.com/learn/input-fields)

---

## Do's

- Beide Dezimaltrennzeichen (`,` und `.`) akzeptieren, Locale-Formatierung nur bei Blur anwenden
- Paste-Verhalten testen: alle gängigen Formate müssen akzeptiert werden
- Für Datum immer Freitexteingabe parallel zum Picker anbieten
- Flexibles Parsing implementieren, bevor Format-Anforderungen verschärft werden
- `inputmode` korrekt setzen: `decimal` für Currency, `none` für Picker-gesteuerte Felder
- Fehler erst bei Blur oder Submit zeigen, nicht während der Eingabe
- Bei bereits invaliden Feldern sofort nach jeder Änderung validieren (Korrektur-Feedback)
- Rohwert immer getrennt vom Anzeigewert halten und nur Rohwert übermitteln
- Bei IBAN: Leerzeichen-Gruppenformatierung live zulassen, Buchstaben uppercase konvertieren

**Quellen**: [Baymard Institute](https://baymard.com/learn/input-fields) | [Nielsen Norman Group](https://www.nngroup.com/articles/date-input/) | [Material Design](https://m3.material.io/components/date-pickers/accessibility)

---

## Don'ts

- Input-Masken (automatisches Einfügen von Sonderzeichen) während der Eingabe verwenden
- `type="number"` für strukturierte Eingaben (Currency, IBAN, Datum) verwenden
- Formate strikt erzwingen, ohne flexibles Parsing als Fallback
- Fehler bei jedem Tastendruck anzeigen
- Nutzende automatisch zwischen Feldern springen lassen (Auto-Tab)
- Formatierte Werte ans Backend senden
- Auf Mobile mehrere separate Felder für eine zusammengehörende Eingabe erzwingen
- Dropdown-Trilogie (Tag / Monat / Jahr) für Datumseingaben verwenden

**Quellen**: [GOV.UK Design System](https://design-system.service.gov.uk/components/date-input/) | [Baymard Institute](https://baymard.com/learn/input-fields) | [Smashing Magazine](https://www.smashingmagazine.com/2022/09/inline-validation-web-forms-ux/)

---

---

## Telefonnummern: Kein fester Ländercode als Standard

### Befund

Die Rufnummer eines Nutzenden entspricht nicht zwangsläufig dem Land, in dem er gerade ist oder die Website nutzt. Ein voreingestellter Ländercode (z. B. automatisch Deutschland) trifft regelmäßig Nutzende, die mit einer anderen Vorwahl angerufen werden wollen.

Vitaly Friedman formuliert dies als Kernproblem: „The number customers use isn't necessarily the one issued in the country where they use it. So don't set a fixed country code as default."

### Konsequenzen für das Design

- Kein Default-Ländercode vorbelegen, oder nur wenn per IP/Profil mit Fallback-Option
- Einziges Pflichtfeld: Nummernfeld; Ländervorwahl optional falls national genug
- Masken und Reformatting erzwingen eine Schreibweise, die der Nutzende nicht nutzt
- 89% der Nutzenden kopieren Telefonnummern aus anderen Quellen (Visitenkarte, E-Mail) – Copy-Paste darf nicht brechen

**Quelle**: [Vitaly Friedman – Designing Phone Number Input UX](https://www.linkedin.com/posts/vitalyfriedman_ux-design-activity-7272160863359270914-SwjN)

---

## Kreditkartennummern: Luhn-Validierung reduziert Abbrüche

### Befund (Baymard Institute)

Baymard Institute hat in einer Benchmark-Studie von 60 Top-E-Commerce-Sites gemessen:

- **53%** der Sites führen keine Luhn-Validierung durch (Stand 2024, Verbesserung gegenüber 67% in 2016)
- **51%** der Sites formatieren Kreditkartennummern nicht automatisch in Leerzeichen-Blöcke
- Fast 1 von 4 Nutzenden macht beim Eingeben der Kreditkartennummer einen Fehler

### Warum Luhn früh validieren

Der Luhn-Algorithmus erkennt jeden Einzelzifferntippfehler sofort – ohne eine Serveranfrage. Das gibt Nutzenden sofortiges Feedback, bevor sie das Formular absenden. Luhn prüft nur, ob die Ziffernfolge prinzipiell gültig sein kann, nicht ob die Karte existiert oder gedeckt ist.

### Nutzerverhalten

Nutzende imitieren die Formatierung der physischen Karte: Sie geben die Nummer in 4er-Blöcken ein, nicht als eine Ziffernfolge. Die automatische Gruppenformatierung spiegelt dieses Verhalten wider und reduziert Fehler.

**Quelle**: [Baymard Institute – Credit Card Form UX](https://baymard.com/blog/credit-card-form-ux-llbean) | [Smashing Magazine – Billing Form UX](https://www.smashingmagazine.com/2017/03/improve-billing-form-ux/)

---

## Postleitzahlen: Regex reicht nicht für Produktion

### Problem

Regex validiert das Format, nicht die Existenz. `99999` besteht den deutschen 5-Ziffern-Test, ist aber keine gültige deutsche Postleitzahl. `SW9Z` entspricht dem UK-Postcode-Muster, existiert nicht.

Für viele Länder gibt es gar kein Postleitzahl-System (Angola, Teile Afrikas und Ozeaniens). Ein Pflichtfeld blockiert dort jede Eingabe.

### Empfohlene Zwei-Schicht-Validierung

1. **Client-seitig (Format)**: Loose-Regex blockiert offensichtlich falsche Eingaben (`abc`, zu kurz/lang)
2. **Server-seitig (Existenz)**: API-Validierung gegen echte Postleitzahl-Datenbanken (z. B. GeoPostcodes, Zippopotam.us) für Produktionssysteme mit Lieferadress-Anforderungen

### Pflichtfeld-Regel

Postleitzahl darf nur Pflichtfeld sein, wenn das gewählte Land ein PLZ-System hat und die Anwendung das weiß. Andernfalls: optional markieren oder dynamisch nach Land ein-/ausblenden.

**Quelle**: [W3C – Address formats around the world](https://w3c.github.io/i18n-drafts/questions/qa-address-formats.en.html) | [Stack Overflow – Ultimate postal code regex](https://stackoverflow.com/questions/578406/what-is-the-ultimate-postal-code-and-zip-regex)

---

## Uhrzeiteingaben: AM/PM-Verwechslung ist häufig

### Problem

12-Stunden-Format ist international missverständlich: AM/PM-Unterschied wird leicht übersehen, besonders bei digitalen Formularen ohne klare visuelle Hervorhebung. Für globale Produkte ist 24h-Format klarer.

`<input type="time">` delegiert die Formatierung vollständig ans Browser-Locale, was bedeutet, dass die Seite keinen Einfluss auf 12h vs. 24h hat. Deutsche Browser zeigen 24h, US-Browser 12h.

### Wichtigste UX-Regeln

- Wenn Zeitauswahl aus einer Liste (nicht freie Eingabe): Dropdown oder Listbox mit klaren Schrittweiten (15 Min, 30 Min)
- Radio-Buttons für „Morgen / Nachmittag / Abend" wenn keine Präzision nötig
- Für zeitzonensensitive Buchungen: Zeitzone separat erfassen und im UI anzeigen
- Kein Auto-Submit beim Auswählen eines Zeitslots

**Quelle**: [Eleken – Time Picker UX Best Practices 2025](https://www.eleken.co/blog-posts/time-picker-ux) | [Mobbin – Time Picker Design](https://mobbin.com/glossary/time-picker)

---

## Weiterführende Ressourcen

- [Nielsen Norman Group – Date-Input Form Fields: UX Design Guidelines](https://www.nngroup.com/articles/date-input/)
- [Nielsen Norman Group – A Checklist for Designing Mobile Input Fields](https://www.nngroup.com/articles/mobile-input-checklist/)
- [Baymard Institute – 8 Recommendations for Creating Effective Input Fields](https://baymard.com/learn/input-fields)
- [Baymard Institute – Credit Card Form UX: 5 Best-in-Class Implementations](https://baymard.com/blog/credit-card-form-ux-llbean)
- [Smashing Magazine – A Complete Guide To Live Validation UX](https://www.smashingmagazine.com/2022/09/inline-validation-web-forms-ux/)
- [Smashing Magazine – Designing The Perfect Date And Time Picker](https://www.smashingmagazine.com/2017/07/designing-perfect-date-time-picker/)
- [Smashing Magazine – How To Improve Your Billing Form's UX](https://www.smashingmagazine.com/2017/03/improve-billing-form-ux/)
- [Material Design – Date Pickers Accessibility](https://m3.material.io/components/date-pickers/accessibility)
- [GOV.UK Design System – Date Input](https://design-system.service.gov.uk/components/date-input/)
- [GOV.UK Design System – Phone Numbers Pattern](https://design-system.service.gov.uk/patterns/phone-numbers/)
- [USWDS – Date Picker](https://designsystem.digital.gov/components/date-picker/)
- [USWDS – Time Picker](https://designsystem.digital.gov/components/time-picker/)
- [VA.gov – Telephone Input Design Decisions](https://design.va.gov/components/form/telephone-input/design-decisions/)
- [UX Stack Exchange – IBAN Input Best Practices](https://ux.stackexchange.com/questions/59979/html4-insert-iban-by-separate-fields)
- [GOV.UK Technology Blog – Why we changed the input type for numbers](https://technology.blog.gov.uk/2020/02/24/why-the-gov-uk-design-system-team-changed-the-input-type-for-numbers/)
- [Vitaly Friedman – Designing Phone Number Input UX](https://www.linkedin.com/posts/vitalyfriedman_ux-design-activity-7272160863359270914-SwjN)
- [Vitaly Friedman – Address Form UX](https://www.linkedin.com/posts/vitalyfriedman_ux-design-activity-7133780302031654912-leGC)
- [W3C – Address formats around the world](https://w3c.github.io/i18n-drafts/questions/qa-address-formats.en.html)
- [Eleken – Time Picker UX Best Practices 2025](https://www.eleken.co/blog-posts/time-picker-ux)
- [MDN – input type=time](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/input/time)
