# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a design system research and documentation project focused on analyzing best practices from major design systems (Material Design, Ant Design, Carbon, Polaris, Atlassian, Fluent, Lightning, Spectrum) and creating standardized documentation patterns.

## Repository Structure

- `docs/pdca/` - Contains research plans and documentation following the PDCA (Plan-Do-Check-Act) methodology (can be in English)
  - `design-system-research/` - Research on design system best practices
- `docs/tasks/` - Reusable task descriptions for common workflows (can be in English)
- `template/` - Documentation templates for the design system
  - `patterns/` - Pattern documentation templates with mermaid decision flows
  - `tone-of-voice/` - Documentation writing guidelines
- `research/` - Research artifacts and findings
  - `patterns/` - Pattern documentation output (MUST be in German)

## Language Usage

**IMPORTANT**: Distinguish between internal documentation and output documentation:

- **Internal documentation** (planning, task descriptions in `docs/pdca/`, `docs/tasks/`): Can be in English
- **Output documentation** (pattern documentation in `research/patterns/`): MUST be in German

## Documentation Standards

All **output** documentation follows these principles (from `template/tone-of-voice/tone-of-voice.md`):

- **Language**: German for general content, English for technical terms (component names, properties)
- **No direct address**: Avoid "du" or "Sie" to accommodate diverse audiences
- **Gender-neutral**: Use gender-neutral language without asterisks or slashes
- **Concise**: Keep text short and clear; prefer bullet points over prose
- **Simple**: Accessible with minimal prior knowledge
- **Motivating**: Introduction should highlight the value of the topic

## Pattern Documentation Template

When creating pattern documentation (based on `template/patterns/documentation-template.md`), include:

1. **Metadata**: Title, description, related topics, layout
2. **Verwendung** (Usage): What problem does the pattern solve and how?
3. **Beispiele** (Examples): Use cases where the pattern applies
4. **Abgrenzung** (Delimitation): What the pattern is NOT suitable for
5. **Decision Flow**: Mermaid flowchart for component selection
6. **Prinzipien** (Principles): Guidelines for creating and using components
7. **Barrierefreiheit** (Accessibility): Specific accessibility considerations
8. **Insights**: Links to patterns in other design systems and research results

## Design System Research Scope

The project analyzes:
- **Component categories**: Inputs, Navigation, Feedback, Data Display, Layout
- **UX patterns**: Searching, Empty States, Loading States, Error Handling, Forms, Data Tables, Navigation, Notifications
- **Focus areas**: Component functionality, accessibility (ARIA, keyboard navigation), responsive behavior, customization options
