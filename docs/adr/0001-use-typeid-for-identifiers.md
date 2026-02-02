# ADR-0001: Use TypeID for Entity Identifiers

**Status:** Accepted

**Date:** 2026-02-02

**Deciders:** Commons OS maintainers

## Context

The Commons OS ecosystem contains multiple types of entities: patterns, lighthouses, contexts, and potentially more in the future. Each entity needs a unique identifier that:

1. Is globally unique across all entities
2. Is immutable (never changes even if the entity is renamed)
3. Is sortable by creation time
4. Clearly indicates the entity type
5. Works well in URLs and file systems

We needed to choose an identifier format that would scale as the system grows and support cross-referencing between entities.

## Decision

We adopt **TypeID** as the identifier format for all entities in Commons OS.

TypeID format: `{type_prefix}_{base32_encoded_uuidv7}`

Examples:
- Pattern: `pat_01kg5023vveprts2at71w6e29g`
- Lighthouse: `lth_01kg5023vveprts2at71w6e29g`
- Context: `ctx_01kg5023vveprts2at71w6e29g`

The TypeID is stored in the `id` field of each entity's YAML frontmatter and serves as the primary key for all relationships.

## Consequences

### Positive

- **Type safety**: The prefix immediately identifies what kind of entity an ID refers to
- **Uniqueness**: UUIDv7 guarantees global uniqueness without coordination
- **Sortability**: UUIDv7 is time-ordered, so IDs sort chronologically
- **Immutability**: IDs never change, even if slugs or titles are updated
- **URL-safe**: Base32 encoding is URL-safe without escaping

### Negative

- **Length**: TypeIDs are longer than simple sequential numbers (28+ characters)
- **Not human-memorable**: Users cannot easily remember or type TypeIDs
- **Requires generation**: New entities need TypeID generation tooling

### Neutral

- Slugs remain the human-readable identifier for URLs and filenames
- Both TypeID and slug are needed: TypeID for machine references, slug for human navigation

## Alternatives Considered

### Alternative 1: Sequential Numbers

Using simple sequential numbers like `1025` for patterns.

**Rejected because:**
- Requires central coordination to avoid conflicts
- No type information embedded
- Conflicts occurred when multiple contributors added patterns simultaneously
- Numbers can collide across different entity types

### Alternative 2: UUID v4

Using standard random UUIDs.

**Rejected because:**
- Not sortable by creation time
- No type prefix to identify entity type
- Longer than TypeID when including type prefix

### Alternative 3: Slug-Only

Using only the slug (e.g., `steward-ownership`) as the identifier.

**Rejected because:**
- Slugs can change if patterns are renamed
- Would break all existing references when a pattern is renamed
- No guarantee of uniqueness across entity types

## References

- [TypeID Specification](https://github.com/jetpack-io/typeid)
- Pattern PATTERN_SPEC.md for implementation details
