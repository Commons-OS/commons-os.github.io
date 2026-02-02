# Commons OS Agent Boot Procedure

> **Purpose**: This document provides AI agents and new contributors with the essential context needed to work effectively on Commons OS. Read this document at the start of every new session.

---

## 1. Quick Context Load

Execute these steps in order to load essential context:

### Step 1: Understand the System
```
Read: commons-os.github.io/docs/adr/README.md
```
This gives you the list of all system-wide architectural decisions.

### Step 2: Load Engine-Specific Context
Depending on which engine you're working on:

| Engine | Boot Files |
|--------|------------|
| **Pattern Engine** | `patterns/docs/adr/README.md`, `patterns/PATTERN_SPEC.md`, `patterns/ARCHITECTURE.md` |
| **Lighthouse Engine** | `lighthouses/docs/adr/README.md`, `lighthouses/LIGHTHOUSE_SPEC.md` |
| **Context Engine** | `context/docs/adr/README.md` |

### Step 3: Check Current State
```
Read: patterns/data/graph.json (metadata section)
```
This shows the current count of patterns, relationships, and domains.

---

## 2. System Overview

### What is Commons OS?

Commons OS is a knowledge system for **Commons Engineers** — practitioners building organizations and systems based on commons principles. It consists of three engines:

| Engine | Purpose | Entity Type | Repository |
|--------|---------|-------------|------------|
| **Pattern Engine** | Reusable organizational solutions | Patterns | `Commons-OS/patterns` |
| **Context Engine** | Domain-specific applications | Contexts | `Commons-OS/context` |
| **Lighthouse Engine** | Real-world implementations | Lighthouses | `Commons-OS/lighthouses` |

### Current Scale (as of 2026-02-02)

| Metric | Count |
|--------|-------|
| Patterns | 1,026 |
| Lighthouses | 6 |
| Domains | 13 |
| Relationships | 3,300+ |

---

## 3. Key Architectural Decisions

These decisions are **immutable** unless superseded by a new ADR. Do not reverse them.

### System-Wide

| Decision | Summary |
|----------|---------|
| **TypeID for Identifiers** | All entities use TypeID format (`pat_...`, `lh_...`) based on UUID7. This is the authoritative unique identifier. |
| **File-Based Architecture** | Entities are Markdown files with YAML frontmatter. JSON indexes are derived, not authoritative. |
| **Three Engines** | Pattern/Context/Lighthouse separation. Each has its own repository. |
| **Jekyll + GitHub Pages** | Static site generation, free hosting, version-controlled content. |
| **Unified Search** | Single search bar queries all engines via client-side Fuse.js. |

### Pattern Engine

| Decision | Summary |
|----------|---------|
| **Slug-Only Filenames** | No sequential numbers. Format is `{slug}.md`. TypeID is the unique identifier. |
| **`classification` not `tags`** | Jekyll reserves `tags`. Use `classification:` for nested metadata. |
| **Consolidated Index Script** | Single `build_indexes.py` generates all indexes. |
| **7 Pillars Assessment** | Every pattern has a Commons Alignment assessment against 7 pillars. |
| **5 Relationship Types** | `generalizes_from`, `specializes_to`, `enables`, `requires`, `related`. |
| **Staging Workflow** | New patterns go to `_staging/`, then promote to `_patterns/` after validation. |

---

## 4. Common Pitfalls

These mistakes have been made before. Avoid them.

| Pitfall | Why It's Wrong | Correct Approach |
|---------|----------------|------------------|
| Using sequential numbers in filenames | Causes conflicts, redundant with TypeID | Use slug-only: `steward-ownership.md` |
| Using `tags:` in YAML frontmatter | Jekyll reserved keyword, breaks template | Use `classification:` instead |
| Running old scripts | Deleted scripts still referenced in docs | Use only `build_indexes.py` |
| Hardcoding pattern counts | Gets stale, causes inconsistencies | Use `{{ site.patterns.size }}` in Jekyll |
| Adding Navigation section to patterns | Duplicates template footer | Template handles navigation |
| Pushing workflow changes | Requires special permissions | Ask user to update workflows manually |

---

## 5. File Structure

### Pattern Engine Repository

```
patterns/
├── _patterns/              # Canonical patterns (1,026 files)
├── _staging/patterns/      # Patterns awaiting promotion
├── _layouts/               # Jekyll templates
├── _data/                  # Generated data files
│   └── registry.json       # TypeID → pattern mapping
├── data/
│   └── graph.json          # Knowledge graph
├── scripts/
│   ├── build_indexes.py    # Main index generator
│   ├── validate_pattern.py # Pattern validation
│   ├── enrich_entity.py    # AI enrichment
│   └── generate_typeid.py  # TypeID generator
├── docs/adr/               # Architecture Decision Records
├── ARCHITECTURE.md         # System architecture
├── PATTERN_SPEC.md         # Pattern format specification
└── search-index.json       # Frontend search index
```

---

## 6. Workflow Commands

### Validate a Pattern
```bash
python3 scripts/validate_pattern.py _patterns/steward-ownership.md
```

### Rebuild All Indexes
```bash
python3 scripts/build_indexes.py
```

### Generate a New TypeID
```bash
python3 scripts/generate_typeid.py
```

---

## 7. Before Making Changes

1. **Read the relevant ADRs** — Understand why things are the way they are
2. **Check PATTERN_SPEC.md** — Ensure changes conform to the specification
3. **Test locally** — Run validation and index generation before pushing
4. **Verify on live site** — Check that Jekyll renders correctly after push

---

## 8. Asking for Help

If you encounter issues or need clarification:

1. Check the ADRs for documented decisions
2. Check git history for context on past changes
3. Ask the user for guidance on ambiguous situations
4. Document new decisions as ADRs to prevent future confusion

---

## 9. Version History

| Date | Change |
|------|--------|
| 2026-02-02 | Initial boot procedure created |

---

*This document should be updated whenever significant architectural changes are made or new pitfalls are discovered.*
