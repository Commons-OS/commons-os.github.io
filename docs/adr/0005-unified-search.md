# ADR-0005: Unified Search Across All Engines

**Status:** Accepted

**Date:** 2026-01-20 (retrospective)

**Deciders:** Commons OS maintainers

## Context

With three separate engines (Pattern, Context, Lighthouse) hosted in different repositories, users needed a way to discover content across the entire Commons OS ecosystem. Having separate search functionality in each engine would fragment the user experience and make it harder to find related content across boundaries.

The challenge was to implement search that works across all engines while respecting the static site architecture and avoiding the need for server-side infrastructure.

## Decision

Commons OS implements **unified client-side search** using Fuse.js, with a single search bar that queries content from all three engines.

The implementation generates a `search-index.json` file in each engine repository containing titles, summaries, and URLs. The main site aggregates these indexes at build time. Fuse.js provides fuzzy matching on the client side, and search results display the source engine with visual indicators.

The same search bar component is included in the navigation of all sites, providing a consistent experience regardless of which engine the user is currently viewing.

## Consequences

### Positive

Users can discover related content across engines from any page. A search for "steward ownership" returns both the pattern and any lighthouses that implement it, creating a more connected experience.

The client-side approach requires no server infrastructure. Search works entirely in the browser, maintaining the static site architecture and zero hosting costs.

The unified search bar creates visual consistency across all engines, reinforcing that they are part of a single system despite being separate repositories.

### Negative

Client-side search has performance limitations. As the content grows, the search index becomes larger and takes longer to load. With 1,000+ patterns, the index is several hundred kilobytes.

Fuzzy matching can produce unexpected results. Users searching for exact phrases may get results that don't contain the exact text, which can be confusing.

The aggregation of indexes requires coordination between repositories. When a new engine is added or the index format changes, all repositories must be updated.

### Neutral

Search results are limited to title and summary matching. Full-text search of pattern content would require a much larger index or server-side search infrastructure.

## Alternatives Considered

### Alternative 1: Separate Search Per Engine

Each engine has its own search that only searches its content.

This approach was not chosen because it fragments the user experience and makes cross-engine discovery difficult. Users would need to know which engine contains the content they're looking for.

### Alternative 2: Server-Side Search (Algolia, Elasticsearch)

Use a hosted search service for more powerful search capabilities.

This approach was not chosen due to cost concerns and the desire to maintain a fully static architecture. Hosted search services charge based on usage, which could become expensive as the site grows.

### Alternative 3: Google Custom Search

Embed Google's custom search engine.

This approach was not chosen because it introduces ads, has limited customization, and depends on Google's indexing schedule. The user experience is also less integrated than a native search component.

## References

- Fuse.js documentation
- `search-index.json` generation in each engine
- Navigation component with unified search bar
