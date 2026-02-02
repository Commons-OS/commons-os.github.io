# ADR-0002: File-Based Architecture with Derived Indexes

**Status:** Accepted

**Date:** 2026-02-02

**Deciders:** Commons OS maintainers

## Context

Commons OS needs to store and serve 1,000+ patterns, lighthouses, and contexts. We needed to decide on the storage architecture:

1. How to store entity content (patterns, lighthouses, contexts)
2. How to enable fast queries and relationship lookups
3. How to support version control and collaboration
4. How to enable static site generation via Jekyll/GitHub Pages

The system needs to scale to potentially 10,000+ entities while remaining accessible to contributors who may not have database expertise.

## Decision

We adopt a **file-based architecture with derived indexes**:

1. **Source of Truth**: Markdown files with YAML frontmatter stored in Git
   - Patterns in `_patterns/*.md`
   - Lighthouses in `_lighthouses/*.md`
   - Contexts in `_contexts/*.md`

2. **Derived Indexes**: JSON files generated from source files
   - `search-index.json` — For frontend search functionality
   - `data/graph.json` — Knowledge graph with relationships
   - `_data/registry.json` — TypeID to entity mapping

3. **Index Generation**: Single consolidated script (`scripts/build_indexes.py`) that:
   - Reads all source files
   - Generates all indexes in one pass
   - Runs automatically via GitHub Actions on each push

## Consequences

### Positive

- **Version control**: Full Git history for all content changes
- **Collaboration**: Standard pull request workflow for contributions
- **Portability**: No database dependencies; works on any system with Git
- **Transparency**: All content is human-readable Markdown
- **Static hosting**: Works with GitHub Pages without server infrastructure
- **Offline access**: Clone the repo and have everything locally

### Negative

- **Query limitations**: Complex queries require scanning files or using indexes
- **Index staleness**: Indexes must be regenerated when content changes
- **No real-time updates**: Changes require commit + rebuild cycle
- **Scale concerns**: Very large datasets (100,000+ entities) may strain file system

### Neutral

- Jekyll collections provide automatic page generation from Markdown files
- Relationship queries use the `graph.json` index, not direct file traversal
- Search uses `search-index.json` loaded client-side with Fuse.js

## Alternatives Considered

### Alternative 1: Database (PostgreSQL/SQLite)

Using a relational database for storage.

**Rejected because:**
- Requires server infrastructure or build-time database
- Less accessible to non-technical contributors
- Harder to version control content changes
- Overkill for current scale (1,000-10,000 entities)

### Alternative 2: Headless CMS

Using a headless CMS like Contentful or Strapi.

**Rejected because:**
- Vendor lock-in concerns
- Cost at scale
- Less control over data format
- Harder to maintain offline/local development

### Alternative 3: Pure File-Based (No Indexes)

Relying only on file system operations without derived indexes.

**Rejected because:**
- Too slow for relationship queries across 1,000+ files
- No efficient search capability
- Poor user experience for navigation

## References

- `scripts/build_indexes.py` — Index generation implementation
- `ARCHITECTURE.md` — System architecture overview
- ADR-0001 for TypeID usage in relationships
