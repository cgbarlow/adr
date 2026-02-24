# ADR - Architecture Decision Records

This repository contains a proposal and specifications for an **enhanced ADR format** using the WH(Y) method.

## The Problem

Many ADR implementations exhibit two fundamental issues:

### 1. Structural Inconsistency
Each ADR follows a different format:

| ADR | Structure |
|-----|-----------|
| ADR-008 (Vitest) | Context → Decision → Rationale → Implementation → Performance |
| ADR-006 (Memory) | Context → Decision → Backends → Implementation → Updates (dated) |
| ADR-016 (Claims) | Context → Decision → Types → Patterns → API → Integration |

### 2. Conflation of Decisions and Implementation
ADRs contain embedded configs, type definitions, code examples, and migration scripts—turning decision records into implementation documentation that must be updated as code changes.

### Root Cause
The current format conflates two distinct concerns:

| Concern | Purpose | Stability |
|---------|---------|-----------|
| **Decision Record** | Why we chose this approach | Stable (immutable once approved) |
| **Implementation Spec** | How we built it | Evolving (updates expected) |

## The Solution

An **enhanced ADR mode** that:
- Enforces a **standardized WH(Y) template** for all decisions
- **Separates** decision rationale from implementation specifications
- Adds **dependency tracking** between ADRs
- Includes **governance metadata** (review cadence, approvers, status history)

## Contents

### Proposal

- **[ADR-001-Enhanced-ADR-Format.md](./ADR-001-Enhanced-ADR-Format.md)** - Full proposal in WH(Y) format for the enhanced ADR mode.

### Specifications

| Specification | Description |
|---------------|-------------|
| [SPEC-001-A-WHY-Format.md](./specs/SPEC-001-A-WHY-Format.md) | WH(Y) statement format with 6-part structured decision template |
| [SPEC-001-B-Minimalism.md](./specs/SPEC-001-B-Minimalism.md) | ADR minimalism and separation of decisions from specifications |
| [SPEC-001-C-Dependencies.md](./specs/SPEC-001-C-Dependencies.md) | Dependency tracking between ADRs |
| [SPEC-001-D-Master-ADRs.md](./specs/SPEC-001-D-Master-ADRs.md) | Master ADRs for complex initiatives |
| [SPEC-001-E-Definition-of-Done.md](./specs/SPEC-001-E-Definition-of-Done.md) | Extended Definition of Done (ECADR) |

### Source Document

- **[Recording_Architecture_Decisions_Expanded.md](./Recording_Architecture_Decisions_Expanded.md)** - Comprehensive guide synthesizing ADR best practices from Nygard, Henderson, GitHub, and Zimmermann.

## WH(Y) Format

The WH(Y) statement provides a structured decision record:

```
In the context of <context>,
facing <non-functional concern>,
we decided for <decision>,
and neglected <alternatives>,
to achieve <benefits>,
accepting that <trade-offs>.
```

### Example: Before and After

**Typical format:**
```markdown
## Decision
Migrate to Vitest for v3.

## Implementation
// vitest.config.ts
export default defineConfig({...});
```

**Enhanced format:**
```markdown
## WH(Y) Decision Statement
In the context of the project's testing infrastructure,
facing slow test execution (~30s) and poor ESM support,
we decided for Vitest as the testing framework,
and neglected Jest, Mocha, and Node test runner,
to achieve 10x faster execution and native ESM support,
accepting that teams need minor syntax adjustments (jest.fn → vi.fn).

## References
| Reference ID | Title | Location |
|--------------|-------|----------|
| SPEC-008-A | Vitest Configuration | specs/SPEC-008-A.md |
```

## Key Concepts

| Concept | Description |
|---------|-------------|
| **ADR Minimalism** | Decisions in ADRs, implementation details in separate specs |
| **Dependency Tracking** | Typed relationships: Depends On, Supersedes, Relates To, Refines, Part Of |
| **Master ADRs** | Parent ADRs for complex multi-decision initiatives |
| **Definition of Done** | 8 criteria: Evidence, Criteria, Agreement, Documentation, Review, Dependencies, References, Master |

## Configuration

The enhanced ADR mode can be configured at the project level:

```yaml
adr:
  mode: "enhanced"  # Options: "standard" | "enhanced"
```

Or via CLI when using ADR tooling:
```bash
adr new --mode=enhanced "API Gateway Selection"
```

## References

- [Documenting Architecture Decisions](https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions) - Michael Nygard
- [Architecture Decision Record](https://github.com/joelparkerhenderson/architecture_decision_record) - Joel Parker Henderson
- [Why Write ADRs](https://github.blog/2020-08-13-why-write-adrs/) - GitHub Blog
- [Sustainable Architectural Design Decisions](https://www.ozimmer.ch/practices/2020/04/27/ArchitectureDecisionMaking.html) - Olaf Zimmermann
- [Definition of Done for ADRs](https://www.ozimmer.ch/practices/2020/05/22/ADDefinitionOfDone.html) - Olaf Zimmermann

## License

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](./LICENSE).
