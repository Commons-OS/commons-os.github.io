# ADR-0003: Three Engines Architecture (Pattern/Context/Lighthouse)

**Status:** Accepted

**Date:** 2026-01-15 (retrospective)

**Deciders:** Commons OS maintainers

## Context

Commons OS needed a structure to organize different types of knowledge assets. The system would contain reusable solutions (patterns), domain-specific applications, and real-world examples of implementations. A single flat structure would become unwieldy as the system grew, and different types of content have different characteristics and use cases.

The challenge was to create a structure that would be intuitive for users while maintaining clear separation of concerns and enabling meaningful connections between different types of content.

## Decision

Commons OS is organized into **three distinct engines**, each serving a specific purpose:

| Engine | Entity Type | Purpose | Repository |
|--------|-------------|---------|------------|
| **Pattern Engine** | Patterns | Reusable solutions and organizational practices | `patterns` |
| **Context Engine** | Contexts | Domain-specific applications and industry adaptations | `context` |
| **Lighthouse Engine** | Lighthouses | Real-world implementations and case studies | `lighthouses` |

Each engine has its own repository, specification, and workflows, but they share common infrastructure including unified navigation, search, and the TypeID system for cross-referencing.

The engines are connected through relationships. Lighthouses implement Patterns, Contexts apply Patterns to specific domains, and all three can reference each other through the knowledge graph.

## Consequences

### Positive

The separation provides clear mental models for contributors and users. When someone wants to add a reusable practice, they know to use the Pattern Engine. When documenting a real company's implementation, they use the Lighthouse Engine. This clarity reduces confusion and improves content quality.

The modular structure allows each engine to evolve independently. The Pattern Engine can add new metadata fields without affecting Lighthouses, and vice versa. Teams can work on different engines in parallel without conflicts.

Cross-engine search and navigation create a unified user experience despite the separation. Users can discover related content across all engines from a single search bar.

### Negative

The three-repository structure adds complexity to maintenance. Changes to shared components (navigation, footer, search) must be replicated across repositories. This has led to inconsistencies when updates are made to one repository but not others.

Contributors must understand which engine is appropriate for their content, which creates a learning curve. Some content doesn't fit cleanly into one category, requiring judgment calls.

### Neutral

The three-engine structure maps naturally to the Pattern Language concept from Christopher Alexander, where patterns exist in relationship to each other and to their implementations. This theoretical grounding provides a solid foundation for the architecture.

## Alternatives Considered

### Alternative 1: Single Monolithic Repository

All content in one repository with subdirectories for different types.

This approach was not chosen because it would create a very large repository that's difficult to navigate and maintain. Different content types have different workflows and validation requirements, which would be harder to manage in a single repository.

### Alternative 2: Two Engines (Patterns + Examples)

Combine Contexts and Lighthouses into a single "Examples" engine.

This approach was not chosen because Contexts (domain applications) and Lighthouses (real implementations) serve fundamentally different purposes. Contexts are abstract adaptations while Lighthouses are concrete case studies. Combining them would blur this important distinction.

### Alternative 3: Four or More Engines

Add additional engines for tools, resources, or other content types.

This approach was not chosen initially to keep the system simple. The three-engine structure can be extended in the future if needed, but starting with more engines would add unnecessary complexity.

## References

- Christopher Alexander's Pattern Language theory
- About page explaining the three engines
- Unified search implementation across engines
