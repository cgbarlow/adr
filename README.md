# ADR - Architecture Decision Records

This repository contains a feature request and specifications for implementing an **enhanced ADR mode** in [Claude-Flow v3](https://github.com/ruvnet/claude-flow/tree/v3), using the WH(Y) format.

## When is an ADR Required?

All **Architecturally Significant Decisions** must be captured as an ADR. A decision is architecturally significant when it meets any of the criteria defined in [Architecturally_Significant_Decision_Criteria.md](https://github.com/cgbarlow/adr/blob/architecturally_significant_decisions/Architecturally_Significant_Decision_Criteria.md):

| Criterion | Description |
|-----------|-------------|
| **Hard-to-Change** | Difficult to undo or reverse, with high strategic impact |
| **New** | Innovative or significantly different from established guardrails |
| **Not Strategically Aligned** | Deviates from approved strategies |
| **Risk** | Involves significant technology-related risk |
| **Budget/Delivery/Benefits** | Substantial financial, delivery, or benefit implications |
| **Requested** | Governance body requests a decision paper |

## The Problem

The current [Claude-Flow v3 ADRs](https://github.com/ruvnet/claude-flow/tree/v3/v3/implementation/adrs) exhibit two fundamental issues:

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

A **toggle-able enhanced ADR mode** that:
- Enforces a **standardized WH(Y) template** for all decisions
- **Separates** decision rationale from implementation specifications
- Adds **dependency tracking** between ADRs
- Includes **governance metadata** (review cadence, approvers, status history)

## Contents

### Feature Request

- [ADR-CF3-001-Enhanced-ADR-Format.md](https://github.com/cgbarlow/adr/blob/main/ADR-CF3-001-Enhanced-ADR-Format.md) - Full feature request in WH(Y) format proposing the toggle-able enhanced ADR mode for Claude-Flow v3.

### Specifications

| Specification | Description |
|---------------|-------------|
| [SPEC-CF3-001-A-WHY-Format.md](https://github.com/cgbarlow/adr/blob/main/specs/SPEC-CF3-001-A-WHY-Format.md) | WH(Y) statement format with 6-part structured decision template |
| [SPEC-CF3-001-B-Minimalism.md](https://github.com/cgbarlow/adr/blob/main/specs/SPEC-CF3-001-B-Minimalism.md) | ADR minimalism and separation of decisions from specifications |
| [SPEC-CF3-001-C-Dependencies.md](https://github.com/cgbarlow/adr/blob/main/specs/SPEC-CF3-001-C-Dependencies.md) | Dependency tracking between ADRs |
| [SPEC-CF3-001-D-Master-ADRs.md](https://github.com/cgbarlow/adr/blob/main/specs/SPEC-CF3-001-D-Master-ADRs.md) | Master ADRs for complex initiatives |
| [SPEC-CF3-001-E-Definition-of-Done.md](https://github.com/cgbarlow/adr/blob/main/specs/SPEC-CF3-001-E-Definition-of-Done.md) | Extended Definition of Done (ECADR) |

### Source Documents

| Document | Description |
|----------|-------------|
| [Recording_Architecture_Decisions_Expanded.md](https://github.com/cgbarlow/adr/blob/main/Recording_Architecture_Decisions_Expanded.md) | Comprehensive guide synthesizing ADR best practices from Nygard, Henderson, GitHub, and Zimmermann |
| [Architecturally_Significant_Decision_Criteria.md](https://github.com/cgbarlow/adr/blob/main/Architecturally_Significant_Decision_Criteria.md) | Criteria for determining when an ADR is required |

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

**Current format (ADR-008):**
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
In the context of Claude-Flow v3's testing infrastructure,
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

## Toggle Configuration

```yaml
# claude-flow.config.yaml
adr:
  mode: "enhanced"  # Options: "standard" | "enhanced"
```

Or via CLI:
```bash
claude-flow adr new --mode=enhanced "API Gateway Selection"
```

## References

- [Documenting Architecture Decisions](https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions) - Michael Nygard
- [Architecture Decision Record](https://github.com/joelparkerhenderson/architecture_decision_record) - Joel Parker Henderson
- [Why Write ADRs](https://github.blog/2020-08-13-why-write-adrs/) - GitHub Blog
- [Sustainable Architectural Design Decisions](https://www.ozimmer.ch/practices/2020/04/27/ArchitectureDecisionMaking.html) - Olaf Zimmermann
- [Definition of Done for ADRs](https://www.ozimmer.ch/practices/2020/05/22/ADDefinitionOfDone.html) - Olaf Zimmermann
