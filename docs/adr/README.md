# Architecture Decision Records (ADRs)

This directory contains Architecture Decision Records (ADRs) for the Commons OS ecosystem.

## What is an ADR?

An Architecture Decision Record is a short, text-based document that captures a significant architectural choice, its context, and its consequences. ADRs serve as a permanent, version-controlled record of design decisions, addressing the "why" behind decisions and preventing re-discussion of past choices.

## ADR Structure

Commons OS uses a **hybrid ADR approach**:

| Location | Scope | Examples |
|----------|-------|----------|
| `commons-os.github.io/docs/adr/` | System-wide decisions | TypeID adoption, shared conventions, cross-engine architecture |
| `patterns/docs/adr/` | Pattern Engine decisions | Pattern file format, classification structure, index generation |
| `lighthouses/docs/adr/` | Lighthouse Engine decisions | Lighthouse-specific formats and conventions |
| `context/docs/adr/` | Context Engine decisions | Context-specific formats and conventions |

## ADR Lifecycle

1. **Proposed** — Decision is under discussion
2. **Accepted** — Decision has been approved and implemented
3. **Rejected** — Decision was considered but not adopted
4. **Superseded** — Decision has been replaced by a newer ADR

Once accepted, an ADR is **immutable**. If a decision changes, a new ADR is created that supersedes the old one.

## Creating a New ADR

1. Copy `template.md` to a new file with the next sequential number: `NNNN-short-title.md`
2. Fill in all sections
3. Set status to "Proposed"
4. Submit a pull request for review
5. Once approved, update status to "Accepted"

## Index of System-Wide ADRs

| ADR | Title | Status | Date |
|-----|-------|--------|------|
| [0001](0001-use-typeid-for-identifiers.md) | Use TypeID for Entity Identifiers | Accepted | 2026-02-02 |
| [0002](0002-file-based-architecture-with-indexes.md) | File-Based Architecture with Derived Indexes | Accepted | 2026-02-02 |
| [0003](0003-three-engines-architecture.md) | Three Engines Architecture (Pattern/Context/Lighthouse) | Accepted | 2026-01-15 |
| [0004](0004-jekyll-github-pages.md) | Jekyll with GitHub Pages for Static Site Generation | Accepted | 2026-01-10 |
| [0005](0005-unified-search.md) | Unified Search Across All Engines | Accepted | 2026-01-20 |

## For AI Agents

**When starting a new session**, read the ADRs in this directory and in the relevant engine's `/docs/adr/` directory to understand the current architectural decisions and conventions. This prevents accidentally reversing past decisions.
