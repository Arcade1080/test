# Reusable Task: Design System Pattern Research

## Objective

Conduct comprehensive research on a specific design system pattern and create structured documentation according to defined standards.

## Context

A pattern is a reusable solution to a recurring problem in user interfaces. This research task enables systematic analysis and documentation of patterns from established design systems.

## Input Parameters

When executing this task, the following parameters must be provided:

1. **Pattern Name** (required): The name of the pattern to research (e.g., "Searching", "Empty States", "Loading States", "Error Handling", "Data Tables")
2. **Design System Sources** (optional): Specific design systems to analyze
   - Default: Use https://designsystems.surf/design-systems as starting point
   - Alternative: Specific design systems like Material Design, Ant Design, Carbon, Polaris, etc.

## Research Process

### 0. Identify Design Systems with the Pattern (FIRST STEP)

**Before conducting detailed research, determine which design systems actually document the pattern:**

**Goal**: Create a curated list of design systems that have pattern-level documentation (not just component documentation) for the researched topic.

**Important Distinction**:

- **Pattern Documentation**: Strategic guidance on WHEN and WHY to use something, combining multiple components, showing workflows and decision-making
- **Component Documentation**: Technical details on HOW to implement a single UI element (props, variants, styling)

**Process**:

1. Start with the design systems directory: https://designsystems.surf/design-systems and https://component.gallery/design-systems/
2. For each major design system, search for the pattern name (e.g., "searching pattern", "empty states pattern")
3. Evaluate each result:
   - ✅ **Include**: Has dedicated pattern page with usage guidance, principles, examples, decision flows
   - ⚠️ **Review carefully**: Has pattern section but limited content
   - ❌ **Exclude**: Only has component documentation without pattern-level guidance

4. Document your findings in a summary table:
   - Design systems WITH pattern documentation (include URLs)
   - Design systems with LIMITED pattern content (note what's missing)
   - Design systems with component-only documentation (exclude from detailed research)

**Expected Output**: A focused list of 5-10 design systems with quality pattern documentation to use for detailed analysis.

**Why This Matters**: Researching only design systems with actual pattern documentation ensures high-quality, strategic insights rather than just technical implementation details.

**Example - Searching Pattern:**

- ✅ **Carbon Design System** - Has dedicated [Search Pattern](https://carbondesignsystem.com/patterns/search-pattern/) page with Basic/Active/Focused search types, usage scenarios, and decision guidance
- ✅ **GitLab Pajamas** - Has [Searching Pattern](https://design.gitlab.com/patterns/searching) with Global Search vs List Search distinction
- ❌ **Ant Design** - Only has Search component documentation, no pattern-level guidance on when/why to use different search approaches
- ❌ **Fluent UI** - Component documentation only, missing strategic pattern insights

### 1. Identify Additional Sources

**Design Systems:**

- Use https://designsystems.surf/design-systems and https://component.gallery/design-systems/ as directory if no specific sources are provided
- Focus on established design systems: Material Design, Ant Design, Carbon, Polaris, Atlassian, Fluent, Lightning, Spectrum, Adobe Spectrum, Salesforce Lightning
- Extract content from design system documentation pages
- Crawl design system sites for comprehensive pattern documentation when needed

### 2. Analyze Pattern

For each identified design system, document:

- **Context and Domain** In which domain does the products the design system support operate. What type of users does the product have?
- **Mobile included?** Determine if the mobile side is documented
- **Usage**: What problem does the pattern solve? How is it solved? How it is described in the design system? A high level description, a high level goal of search?
- **Examples**: Concrete use cases and applications
- **Types**: Is there a distinction between different type within the pattern, e.g. different types of search? And by which dimension is it distinguished (e.g. sccope or interaction)
- **Delimitation**: What is the pattern NOT suitable for?
- **Components**: Which UI components are used? What is actually part of this pattern (Search Input AND SERPS, only Searchinput?). Also investigate if mobile is respected.
- **Principles**: Fundamental guidelines for creation and usage
- **Do´s and Don´ts**: What explicit does and donts are documented? Also look on the mobile side and the subparts of a pattern, e.g. for search: Searchinput + SERPS
- **Accessibility**: ARIA roles, keyboard navigation, screen reader support
- **Sources**: Capture direct URLs to documentation pages for citation

### 2a. Create Design System Comparison Metadata File

**Create a single metadata file** containing all design system comparisons:

**File Location**: `research/patterns/[pattern-name]/design-systems_comparison.md`

Example: `research/patterns/searching/design-systems.md`

**File Structure**:

```markdown
# Design System Vergleich - [Pattern Name]

Dieser Vergleich zeigt die verschiedenen Ansätze etablierter Design Systeme für das [Pattern Name] Pattern.

---

## [Design System Name 1] (z.B. Carbon Design System)

### Beschreibung

[Kurze Beschreibung (2-3 Sätze), wie dieses Design System das Pattern implementiert. Was ist der charakteristische Ansatz?]

### Vorteile

- [Stärke 1: z.B. "Klare Unterscheidung zwischen Basic, Active und Focused Search"]
- [Stärke 2: z.B. "Umfassende Accessibility-Guidelines mit ARIA-Beispielen"]
- [Stärke 3: z.B. "Gut dokumentierte Loading States und Error Handling"]

### Nachteile

- [Schwäche 1: z.B. "Keine mobile-spezifischen Guidelines"]
- [Schwäche 2: z.B. "Begrenzte Beispiele für komplexe Filter-Kombinationen"]
- [Limitation: z.B. "Fokus auf Enterprise-Anwendungen, weniger E-Commerce"]

### Quelle

[Direkte URL zur Pattern-Dokumentation]

---

## [Design System Name 2]

### Beschreibung

...

### Vorteile

...

### Nachteile

...

### Quelle

...

---

[Weitere Design Systeme nach demselben Schema]
```

**Purpose**: Diese Datei ermöglicht einen schnellen Vergleich aller analysierten Design System Ansätze in einem Dokument und hilft bei der Entscheidung, welches Design System für spezifische Anwendungsfälle am besten geeignet ist.

### 3. Consult Authoritative UX Research

After analyzing design systems, enrich documentation with evidence-based research:

- **Nielsen Norman Group**: Search for articles on the pattern topic (e.g., "search UX", "empty states best practices")
- **Baymard Institute**: Look for data-driven studies, particularly for e-commerce patterns

Top UX-Focused Blogs
Smashing magazine: https://www.smashingmagazine.com/
UX Collective (Medium): https://uxdesign.cc/
UX Planet: https://uxplanet.org/
UX Booth: http://uxbooth.com/
Muzli: https://muz.li/
​

## NN/g Free Content

- NN/g Articles: https://www.nngroup.com/articles/ [nngroup](https://www.nngroup.com/articles/)
- NN/g Study Guides: https://www.nngroup.com/topic/study-guide/ [x](https://x.com/nngroup)
- NN/g Article Archive: https://www.nngroup.com/articles/nielsen-norman-group-20-years/ [nngroup](https://www.nngroup.com/articles/nielsen-norman-group-20-years/)

## UXPA Free Publications

- Journal of Usability Studies: https://uxpa.org/publication/journal-of-usability-studies/ [uxpa](https://uxpa.org/publication/journal-of-usability-studies/)
- UXPA Magazine: https://uxpa.org/uxpa-publications/ [uxpa](https://uxpa.org/uxpa-publications/)

## ACM SIGCHI Open Access

- CHI 2026 Papers: https://chi2026.acm.org/authors/papers/ [chi2026.acm](https://chi2026.acm.org/authors/papers/)
- SIGCHI Proceedings Archive: https://archive.sigchi.org/publications/ [archive.sigchi](https://archive.sigchi.org/publications/)

## Fully Open-Access Journals

- Advances in Human-Computer Interaction: https://www.hindawi.com/journals/ahci/ [anais](https://www.anais.digital/5-best-academic-journals-in-ux-design-and-research/)
- AIS Transactions on HCI: https://aisel.aisnet.org/thci/ [aisel.aisnet](https://aisel.aisnet.org/thci/)

Document research findings with:

- User behavior insights (e.g., "Users abandon search after 3 failed attempts")
- Performance benchmarks (e.g., "50% of sites fail to provide adequate no-results guidance")
- Best practice validation (cross-reference design system recommendations with independent research)

### 3a. Create UX Research Best Practices File

**Create a dedicated file** containing findings from authoritative UX research sources:

**File Location**: `research/patterns/[pattern-name]/ux-research.md`

Example: `research/patterns/searching/ux-research.md`

**File Structure**:

```markdown
# UX Research - [Pattern Name]

Erkenntnisse aus unabhängigen UX-Forschungsquellen zu Best Practices, Nutzerverhalten und evidenzbasierten Empfehlungen.

---

## Best Practices

### [Best Practice 1]

[Beschreibung der Best Practice]

**Quelle**: [Nielsen Norman Group / Baymard Institute / etc.](URL)

### [Best Practice 2]

...

---

## Do's

- ✅ [Empfohlenes Verhalten 1]
- ✅ [Empfohlenes Verhalten 2]
- ✅ [Empfohlenes Verhalten 3]

**Quellen**: [Link 1], [Link 2]

---

## Don'ts

- ❌ [Zu vermeidendes Verhalten 1]
- ❌ [Zu vermeidendes Verhalten 2]
- ❌ [Zu vermeidendes Verhalten 3]

**Quellen**: [Link 1], [Link 2]

---

## Nutzerverhalten & Statistiken

- [Statistik 1: z.B. "50% der Nutzer verlassen eine Site nach fehlgeschlagener Suche"]
- [Statistik 2: z.B. "Nutzer erwarten Suchergebnisse innerhalb von 1-2 Sekunden"]

**Quellen**: [Link 1], [Link 2]

---

## Weiterführende Ressourcen

- [Nielsen Norman Group - Article Title](URL)
- [Baymard Institute - Study Title](URL)
- [Additional Source](URL)
```

**Purpose**: Diese Datei sammelt evidenzbasierte UX-Forschung und validiert Design System Empfehlungen mit unabhängigen Studien und Nutzerdaten.

### 4. Synthesis and Comparison

- Identify common patterns across design systems
- Document differences and unique approaches
- Validate design system recommendations with UX research findings
- Create decision flow for component selection (Mermaid diagram)

## Output Structure

The documentation follows the template structure from `template/patterns/documentation-template.md`:

```markdown
---
meta:
  title: [Pattern Name]
  description: [Short description in German]
relatedTopics: ["patterns/[related]", "patterns/[related]"]
layout: common
---

## Verwendung

[Problem statement and solution approach in German]
[Include source citations for referenced best practices]

### Beispiele

[Use cases as bullet points in German]

### Abgrenzung

[Not suitable for... as bullet points in German]

## Decision Flow

[Mermaid diagram for decision making]

## Prinzipien

### [Principle 1]

[Description with source citation if based on design system guidelines]

### [Principle 2]

[Description with source citation if based on design system guidelines]
...

## Barrierefreiheit

[Accessibility requirements and best practices in German]
[Include sources for WCAG guidelines, ARIA specifications, or design system recommendations]

---

## Insights

[Links to pattern implementations in various design systems]
[Each design system reference should include direct URL to pattern documentation]

**Design System Implementierungen:**

- [Material Design - Empty States](https://material.io/design/...)
- [Ant Design - Empty State](https://ant.design/components/...)
- [Carbon Design System - Empty State](https://carbondesignsystem.com/patterns/...)

**UX Research und Best Practices:**

- [Nielsen Norman Group - Pattern Research](https://www.nngroup.com/articles/...)
- [Baymard Institute - Pattern Studies](https://baymard.com/blog/...)
- [Additional authoritative source if applicable]
```

## Documentation Standards (Tone of Voice)

**IMPORTANT**: The output documentation must be written in German.

- **German Language**: All text in German, except proper names (e.g., component names, properties)
- **No Direct Address**: Avoid "du" or "Sie"
- **Terminological**: Use English technical terms when commonly established (e.g., "Pattern", "Component", "UI")
- **Gender-neutral**: No gender asterisks or slashes, prefer neutral formulations
- **Concise**: Keep text short and clear, prefer bullet points over long prose
- **Simple**: Understandable with minimal prior knowledge
- **Motivating**: Introduction should highlight the value of the pattern
- **No Dashes in Text**: Avoid em-dashes (—) and en-dashes (–) as punctuation. Use commas, parentheses, or restructure sentences instead

## Source Citation Requirements

**CRITICAL**: All best practices, benchmarks, guidelines, and recommendations MUST include source links:

- When describing a best practice, include the source URL inline or as a reference
- When citing benchmarks or statistics, link to the original data
- When referencing design system guidelines, link directly to the relevant documentation page
- Use markdown link format: `[descriptive text](URL)` or inline references like `(Quelle: [Design System Name](URL))`
- In the **Insights** section, provide comprehensive links to all referenced design systems

**Example citations:**

- "Material Design empfiehlt die Verwendung von Skeleton Screens für initiale Ladezustände ([Quelle](https://material.io/design/...))."
- "Studien zeigen, dass Nutzer Empty States als hilfreich empfinden ([Nielsen Norman Group](https://www.nngroup.com/...))."
- "Untersuchungen belegen, dass Nutzer 50% mehr Vertrauen in Anwendungen mit klaren Ladezuständen haben ([Baymard Institute](https://baymard.com/...))."

## Output Location

The finished documentation will be saved in a structured folder with three key files:

```
research/patterns/[pattern-name]/
├── [pattern-name].md           # Main pattern documentation (synthesis)
├── design-systems.md           # Design system comparison metadata
└── ux-research.md              # UX research best practices
```

**Example for "Searching" pattern:**

```
research/patterns/searching/
├── searching.md                # Main comprehensive documentation
├── design-systems.md           # All design system comparisons in one file
└── ux-research.md              # Best practices from Nielsen Norman Group, Baymard, etc.
```

**Main File** (`[pattern-name].md`): Comprehensive synthesis following the template structure with Verwendung, Prinzipien, Barrierefreiheit, Decision Flow, and Insights

**Design Systems File** (`design-systems.md`): Structured comparison of all analyzed design systems with Description, Pros (Vorteile), Cons (Nachteile), and Source (Quelle) for each system

**UX Research File** (`ux-research.md`): Evidence-based best practices, do's and don'ts, user behavior insights, and statistics from authoritative UX research sources

## Quality Criteria

The research is considered complete when:

- [ ] Pattern identification phase completed - documented which design systems have pattern documentation vs component-only
- [ ] At least 3-5 design systems WITH pattern documentation have been analyzed (not just components)
- [ ] **Design system comparison metadata file created** (`design-systems.md`) with description, pros, and cons for each analyzed system
- [ ] **UX research best practices file created** (`ux-research.md`) with best practices, do's, don'ts, and user behavior insights
- [ ] At least 1-2 authoritative UX research sources consulted (Nielsen Norman Group, Baymard Institute, etc.)
- [ ] All sections of the template structure are filled in main pattern file
- [ ] Decision flow exists as Mermaid diagram
- [ ] At least 3 concrete examples are documented
- [ ] Accessibility requirements are described in detail
- [ ] Insights section contains links to 3+ design system implementations
- [ ] Documentation follows all tone-of-voice principles
- [ ] Text is in German (except proper names)
- [ ] All best practices, benchmarks, and guidelines include source links
- [ ] Every referenced design system has a direct URL to its documentation
- [ ] UX research findings (if cited) include direct links to research articles or studies

## Usage Examples

```
/sc:task research "Empty States Pattern" using design systems from https://designsystems.surf/design-systems
```

Or with specific sources:

```
/sc:task research "Loading States Pattern" in Material Design, Ant Design, and Carbon Design System
```

**Expected Output Structure:**

```
research/patterns/empty-states/
├── empty-states.md              # Main synthesis document
├── design-systems.md            # All design system comparisons:
│                                # - Carbon: Description + Pros/Cons
│                                # - Material Design: Description + Pros/Cons
│                                # - Spectrum: Description + Pros/Cons
└── ux-research.md               # Best practices from Nielsen Norman Group,
                                 # Baymard Institute, etc. with do's/don'ts
```

## MCP Integration

### Tavily MCP Tools (RECOMMENDED)

Use Tavily MCP when researching multiple design systems or conducting broad web searches:

- **`mcp__tavily__tavily_search`**: Efficient for searching design system documentation
  - Use `search_depth: "advanced"` for thorough research
  - Set `max_results: 10` or higher to get comprehensive coverage
  - Include design system name in query for targeted results

- **`mcp__tavily__tavily_extract`**: Extract detailed content from design system documentation URLs
  - Use `extract_depth: "advanced"` for comprehensive content extraction
  - Set `format: "markdown"` for structured content
  - Ideal for extracting pattern guidelines, principles, and examples

- **`mcp__tavily__tavily_crawl`**: Crawl design system sites for comprehensive pattern documentation
  - Use when pattern documentation spans multiple pages
  - Set appropriate `max_depth` and `max_breadth` for thorough coverage
  - Useful for discovering related pattern documentation

### Alternative Research Tools

When Tavily is not necessary or for specific use cases:

- **WebSearch/WebFetch**: For direct access to known URLs or quick lookups
- **Context7 MCP**: For framework-specific implementation patterns (if available)
- **Direct URL access**: When specific design system URLs are already known

## Notes

- **Always start with Step 0** - Identify which design systems have pattern documentation before detailed research. This saves time and ensures quality sources.
- **Focus on patterns, not components** - Only research design systems that provide strategic pattern guidance (when/why), not just technical component specs (how)
- **Create comparison metadata file** - All design system comparisons (description, pros, cons) go in one `design-systems.md` file for easy comparison and decision-making.
- **Create UX research file** - Collect best practices, do's and don'ts from authoritative sources (Nielsen Norman Group, Baymard Institute) in `ux-research.md`
- **Three-file structure** - Main pattern documentation, design system comparisons, and UX research best practices as separate files
- Use the most appropriate research tools for the task - Tavily MCP is recommended for broad searches, WebSearch/WebFetch for specific known sources
- For extensive patterns, research can be split into multiple sub-tasks
- Decision flows should focus on the most common use cases
- When uncertainties arise in design system documentation, consult multiple sources and cross-reference with authoritative UX research
- **Always consult established UX research sources** (Nielsen Norman Group, Baymard Institute, etc.) for evidence-based best practices and user research findings
- Screenshots or diagrams from sources can be referenced but not copied
- **Always capture the exact URL** while researching for proper citation
- When using Tavily MCP, prefer `search_depth: "advanced"` and `extract_depth: "advanced"` for comprehensive research
- Cross-reference design system recommendations with independent UX research to validate best practices
