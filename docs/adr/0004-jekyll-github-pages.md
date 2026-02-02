# ADR-0004: Jekyll with GitHub Pages for Static Site Generation

**Status:** Accepted

**Date:** 2026-01-10 (retrospective)

**Deciders:** Commons OS maintainers

## Context

Commons OS needed a web platform to publish and serve the pattern library, lighthouses, and contexts. The platform needed to support Markdown content, version control, collaborative editing, and be accessible to contributors with varying technical skills. Cost and maintenance overhead were also considerations, as the project operates without dedicated infrastructure budget.

The content is primarily text-based documentation with structured metadata, making it well-suited for static site generation rather than dynamic content management.

## Decision

Commons OS uses **Jekyll** as the static site generator, hosted on **GitHub Pages**.

The implementation uses Jekyll's collections feature to treat patterns, lighthouses, and contexts as first-class content types. Each entity is a Markdown file with YAML frontmatter, which Jekyll processes into HTML pages using layout templates.

Key implementation details include Jekyll collections defined in `_config.yml` for patterns, lighthouses, and contexts. Liquid templating is used for layouts and includes, and GitHub Actions handle automated builds and deployments. Custom JavaScript provides client-side search using Fuse.js.

## Consequences

### Positive

GitHub Pages provides free, reliable hosting with automatic SSL certificates and CDN distribution. There are no server costs or maintenance requirements, and the infrastructure scales automatically with traffic.

The Git-based workflow enables version control for all content changes. Contributors can use pull requests for review, and the full history of every pattern is preserved. This aligns with the project's values of transparency and collaboration.

Markdown is accessible to non-technical contributors. Writers can focus on content without learning complex CMS interfaces, and the files are portable and future-proof.

The static nature of the site provides excellent performance. Pages load quickly, there are no database queries, and the site works well on slow connections.

### Negative

Jekyll has limitations for dynamic functionality. Features like user authentication, real-time collaboration, or personalized content require workarounds or external services.

Build times increase as the content grows. With 1,000+ patterns, full site rebuilds take several minutes, which can slow down the development feedback loop.

Jekyll's Liquid templating language has quirks and limitations. Some features that would be simple in other frameworks require workarounds, such as the reserved `tags` keyword issue that led to renaming the field to `classification`.

### Neutral

GitHub Pages enforces certain constraints, such as a limited set of Jekyll plugins. This has not been a significant limitation for Commons OS, but it does restrict some customization options.

The static site approach requires separate solutions for features like search (implemented client-side with Fuse.js) and form submissions (using external services).

## Alternatives Considered

### Alternative 1: Dynamic CMS (WordPress, Drupal)

A traditional content management system with database backend.

This approach was not chosen because it would require hosting infrastructure and ongoing maintenance. The dynamic features of a CMS are not needed for primarily static content, and version control would be more complex.

### Alternative 2: Headless CMS (Contentful, Strapi)

A headless CMS with API-driven content delivery.

This approach was not chosen due to cost concerns at scale, vendor lock-in risks, and the desire to keep content in portable Markdown files under version control.

### Alternative 3: Custom Static Site Generator

Building a custom generator tailored to Commons OS needs.

This approach was not chosen because Jekyll is mature, well-documented, and has a large ecosystem. Building custom tooling would divert resources from content creation.

### Alternative 4: Documentation Platforms (GitBook, ReadTheDocs)

Specialized documentation hosting platforms.

This approach was not chosen because these platforms are optimized for linear documentation rather than interconnected pattern libraries. The customization options are also more limited.

## References

- Jekyll documentation
- GitHub Pages documentation
- `_config.yml` for Jekyll configuration
- System ADR-0002 for file-based architecture decision
