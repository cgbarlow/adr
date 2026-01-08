# ADR-CF3-001: Enhanced ADR Format for Claude-Flow v3

## Feature Request: Toggle-able WH(Y) Architecture Decision Record (ADR) Mode

| Field | Value |
|-------|-------|
| **Decision ID** | ADR-CF3-001 |
| **Initiative** | Claude-Flow v3 ADR Enhancement |
| **Proposed By** | Architecture Team |
| **Date** | 2026-01-08 |
| **Status** | Proposed |

---

## ADR (WH(Y) Statement format)

**In the context of** Claude-Flow v3's ADR generation capabilities, where current ADRs serve as implementation records combining architectural decisions with code examples, file listings, and technical specifications in a single document,

**facing** the challenge that implementation-centric ADRs obscure decision rationale over time, lack relationship tracking between decisions, provide no governance workflow support, and become difficult to maintain as implementation details evolve independently of the original decision,

**we decided for** implementing an enhanced WH(Y) ADR mode as a toggle-able alternative that separates decision rationale from implementation specifications, introduces dependency tracking between ADRs, and adds governance metadata for enterprise decision management,

**and neglected** extending the current implementation-focused format (which would perpetuate the conflation of decisions and specifications), and heavyweight TOGAF-style decision frameworks (which would discourage adoption through excessive ceremony),

**to achieve** clear decision traceability for future teams, impact analysis through dependency graphs, governance-ready documentation with review workflows, and maintainable records where decisions remain stable while specifications evolve,

**accepting that** this introduces a learning curve for the WH(Y) syntax, requires managing specification files as separate artefacts, and adds configuration complexity for teams choosing between standard and enhanced modes.

---

## When is an ADR Required?

All **Architecturally Significant Decisions** must be captured as an ADR. A decision is architecturally significant when it meets any one of the following criteria (see [Architecturally_Significant_Decision_Criteria.md](https://github.com/cgbarlow/adr/blob/architecturally_significant_decisions/Architecturally_Significant_Decision_Criteria.md)):

| Criterion | Description |
|-----------|-------------|
| **Hard-to-Change** | Difficult to undo or reverse, with high strategic impact |
| **New** | Innovative or significantly different from established guardrails |
| **Not Strategically Aligned** | Deviates from approved strategies (e.g., extending retiring assets, duplicating solutions) |
| **Risk** | Involves significant technology-related risk |
| **Budget/Delivery/Benefits** | Substantial financial, delivery, or benefit realisation implications |
| **Requested** | Governance body requests a decision paper due to potential impacts |

Decisions meeting these criteria must be presented to the authorised decision maker or decision body for approval.

---

## Problem Statement

The current Claude-Flow v3 ADRs (see [/v3/implementation/adrs/](https://github.com/ruvnet/claude-flow/tree/v3/v3/implementation/adrs)) exhibit two fundamental problems: **structural inconsistency** and **conflation of decisions with implementation**.

### Problem 1: No Standard Structure

Each ADR follows a different format:

| ADR | Structure | Sections |
|-----|-----------|----------|
| [ADR-008 (Vitest)](https://github.com/ruvnet/claude-flow/blob/v3/v3/implementation/adrs/ADR-008-VITEST.md) | Compact | Context → Decision → Rationale → Implementation → Performance |
| [ADR-006 (Memory)](https://github.com/ruvnet/claude-flow/blob/v3/v3/implementation/adrs/ADR-006-UNIFIED-MEMORY.md) | Evolving | Context → Decision → Backends → Implementation → Updates (dated) |
| [ADR-016 (Claims)](https://github.com/ruvnet/claude-flow/blob/v3/v3/implementation/adrs/ADR-016-collaborative-issue-claims.md) | Elaborate | Context → Decision → Types → Patterns → API → Integration |

This inconsistency makes ADRs difficult to navigate, compare, and review systematically.

### Problem 2: Implementation Details Embedded in Decisions

| ADR | Embedded Implementation Content |
|-----|--------------------------------|
| ADR-008 | `vitest.config.ts` configuration, Jest-to-Vitest migration code |
| ADR-006 | `IMemoryService` interface definition, `MemoryEntry` type, backend comparison tables, CLI command details |
| ADR-016 | `IssueClaim` type definition, `IClaimService` API surface, 6 collaboration patterns with code |

### Problem 3: Living Documents with Appended Updates

ADR-006 contains dated update sections (2026-01-07, 2026-01-08) appended to the document, turning the ADR into a changelog rather than a stable decision record.

### Problem 4: Missing Decision Infrastructure

| Missing Element | Impact |
|-----------------|--------|
| Structured decision statement | "Why" buried in implementation "how" |
| Rejected alternatives with rationale | No record of what was considered and why it was dismissed |
| Dependencies between ADRs | No way to assess impact of changing a decision |
| Governance metadata | No review cadence, approvers, or status history |

### Root Cause

The current format conflates two distinct concerns:

| Concern | Purpose | Stability | Audience |
|---------|---------|-----------|----------|
| **Decision Record** | Why we chose this approach | Stable (immutable once approved) | Future teams, architects, governance |
| **Implementation Spec** | How we built it | Evolving (updates expected) | Current developers, operators |

When these are combined, neither is served well: decisions become unmaintainable, and implementation docs lack the detail developers need.

---

## Opportunity

By introducing an **enhanced WH(Y) mode** as a toggle-able option, Claude-Flow v3 can address the structural inconsistency and separation of concerns problems while preserving backward compatibility.

### Two Modes, Two Purposes

| Mode | Purpose | Audience | Stability |
|------|---------|----------|-----------|
| **Standard** (current) | Implementation documentation | Developers, current team | Evolves with code |
| **Enhanced** (proposed) | Decision rationale capture | Architects, future teams, governance | Stable historical record |

### What Enhanced Mode Solves

| Current Problem | Enhanced Mode Solution |
|-----------------|------------------------|
| Inconsistent ADR structure | **Standardized WH(Y) template** enforced for all enhanced ADRs |
| Implementation details in ADRs | **Separation**: decisions in ADRs, specs in `/specs/` directory |
| No rejected alternatives | **"And neglected"** section requires documenting what was considered |
| No dependency tracking | **Dependencies table** with typed relationships (Depends On, Supersedes, etc.) |
| No governance metadata | **Governance section** with review boards, dates, and cadence |
| Living documents with updates | **Immutable decisions** + versioned specifications |
| Buried decision rationale | **WH(Y) statement** at top of every ADR |

### Value Proposition

This positions Claude-Flow v3 as a comprehensive architecture documentation tool suitable for:

- **Agile teams** using standard mode for rapid implementation docs
- **Enterprise governance** using enhanced mode for auditable decision records
- **Distributed teams** needing clear decision traceability across time zones
- **Regulated industries** requiring documented rationale for architectural choices

---

## Summary

This feature request proposes adding an **enhanced WH(Y) ADR mode** to Claude-Flow v3 that complements the existing implementation-focused format. When enabled, this mode generates ADRs optimized for decision capture, dependency tracking, and governance workflows—while delegating implementation details to separate specification files.

### Key Capabilities

| Capability | Description | Specification |
|------------|-------------|---------------|
| WH(Y) Statement Format | Structured 6-part decision statement | [SPEC-CF3-001-A](https://github.com/cgbarlow/adr/blob/main/specs/SPEC-CF3-001-A-WHY-Format.md) |
| ADR Minimalism | Separation of decisions from specifications | [SPEC-CF3-001-B](https://github.com/cgbarlow/adr/blob/main/specs/SPEC-CF3-001-B-Minimalism.md) |
| Dependency Tracking | Relationship management between ADRs | [SPEC-CF3-001-C](https://github.com/cgbarlow/adr/blob/main/specs/SPEC-CF3-001-C-Dependencies.md) |
| Master ADRs | Parent ADRs for complex initiatives | [SPEC-CF3-001-D](https://github.com/cgbarlow/adr/blob/main/specs/SPEC-CF3-001-D-Master-ADRs.md) |
| Extended Definition of Done | ECADR + Dependencies + References | [SPEC-CF3-001-E](https://github.com/cgbarlow/adr/blob/main/specs/SPEC-CF3-001-E-Definition-of-Done.md) |

---

## Toggle Configuration

The enhanced ADR mode should be configurable at the project level:

```yaml
# claude-flow.config.yaml
adr:
  mode: "enhanced"  # Options: "standard" | "enhanced"
  enhanced_options:
    why_format: true
    separate_specs: true
    dependency_tracking: true
    master_adr_support: true
    governance_metadata: true
```

Or via CLI flag:

```bash
claude-flow adr new --mode=enhanced "API Gateway Selection"
```

---

## Options Considered

### Option 1: Toggle-able WH(Y) Enhanced Mode (Selected)

Introduce an alternative ADR mode that users can enable via configuration, preserving the current format as the default.

**Pros:**
- **Non-breaking**: Existing workflows unchanged; teams opt-in when ready
- **Decision-focused**: WH(Y) statement forces explicit rationale capture
- **Separation of concerns**: Specifications evolve independently of stable decision records
- **Dependency tracking**: Enables impact analysis ("what breaks if we change this?")
- **Governance-ready**: Review cadence, status history, and approval workflows built-in
- **Master ADRs**: Supports complex multi-decision initiatives

**Cons:**
- Learning curve for WH(Y) syntax
- Additional specification files to manage
- Configuration complexity for mode selection

### Option 2: Extend Current Implementation-Focused Format (Rejected)

Add governance fields to the existing format while retaining code examples and file listings.

**Pros:**
- Familiar to current users
- Single file per decision

**Cons:**
- **Perpetuates conflation**: Decisions still mixed with implementation details
- **Maintenance burden**: Code examples require updates as implementation changes
- **Buried rationale**: No structured decision statement; "why" lost in "how"
- **No dependency model**: Relationships between ADRs remain implicit

### Option 3: TOGAF / Enterprise Architecture Framework (Rejected)

Adopt a heavyweight enterprise decision framework with formal decision registers, stakeholder matrices, and RACI charts.

**Pros:**
- Comprehensive governance coverage
- Familiar to enterprise architects

**Cons:**
- **Adoption friction**: Excessive ceremony discourages regular use
- **Overkill**: Most decisions don't require formal stakeholder matrices
- **Poor fit**: Claude-Flow serves developers and architects, not governance committees
- **Slow iteration**: Heavy process conflicts with agile decision-making

### Option 4: Status Quo (Rejected)

Continue with current format only; accept limitations as acceptable trade-offs.

**Pros:**
- No implementation effort
- No learning curve

**Cons:**
- **Missed opportunity**: Growing demand for governance-ready ADRs
- **Competitive gap**: Other tools (ADR-tools, Log4brains) offer structured formats
- **Technical debt**: Decision rationale continues to be lost over time

---

## Governance

| Review Board | Date | Outcome | Action | Review Cadence | Next Review |
|--------------|------|---------|--------|----------------|-------------|
| Claude-Flow Architecture | TBD | Pending | Feature specification review | 6 months | TBD |

---

## Status History

| Status | Approver | Date |
|--------|----------|------|
| Proposed | Architecture Team | 2026-01-08 |

---

## Dependencies

| Relationship | ADR ID | Title | Notes |
|--------------|--------|-------|-------|
| Part Of | CF3-ROADMAP | Claude-Flow v3 Roadmap | Parent initiative |
| Enables | TBD | Future ADR tooling integrations | Graph visualization, impact analysis |

---

## References

| Reference ID | Title | Type | Location |
|--------------|-------|------|----------|
| SPEC-CF3-001-A | WH(Y) Statement Format | Technical Specification | [specs/SPEC-CF3-001-A-WHY-Format.md](https://github.com/cgbarlow/adr/blob/main/specs/SPEC-CF3-001-A-WHY-Format.md) |
| SPEC-CF3-001-B | ADR Minimalism and Separation | Technical Specification | [specs/SPEC-CF3-001-B-Minimalism.md](https://github.com/cgbarlow/adr/blob/main/specs/SPEC-CF3-001-B-Minimalism.md) |
| SPEC-CF3-001-C | Dependencies and Relationships | Technical Specification | [specs/SPEC-CF3-001-C-Dependencies.md](https://github.com/cgbarlow/adr/blob/main/specs/SPEC-CF3-001-C-Dependencies.md) |
| SPEC-CF3-001-D | Master ADRs | Technical Specification | [specs/SPEC-CF3-001-D-Master-ADRs.md](https://github.com/cgbarlow/adr/blob/main/specs/SPEC-CF3-001-D-Master-ADRs.md) |
| SPEC-CF3-001-E | Extended Definition of Done | Technical Specification | [specs/SPEC-CF3-001-E-Definition-of-Done.md](https://github.com/cgbarlow/adr/blob/main/specs/SPEC-CF3-001-E-Definition-of-Done.md) |
| SOURCE-001 | Recording Architecture Decisions (Expanded) | Supporting Artefact | [Recording_Architecture_Decisions_Expanded.md](https://github.com/cgbarlow/adr/blob/main/Recording_Architecture_Decisions_Expanded.md) |
| SOURCE-002 | Architecturally Significant Decision Criteria | Governance Criteria | [Architecturally_Significant_Decision_Criteria.md](https://github.com/cgbarlow/adr/blob/main/Architecturally_Significant_Decision_Criteria.md) |

---

## Supporting Artefact

This feature request is based on the recommendations contained in:

**[Recording_Architecture_Decisions_Expanded.md](https://github.com/cgbarlow/adr/blob/main/Recording_Architecture_Decisions_Expanded.md)**

This source document synthesizes best practices from:
- Michael Nygard's original ADR concept (Cognitect, 2011)
- Joel Parker Henderson's ADR repository
- GitHub's "Why Write ADRs" guidance
- Olaf Zimmermann's WH(Y) statement template and Definition of Done

---

## Implementation Notes

### Mode Comparison

When generating ADRs, Claude-Flow v3 would behave differently based on mode:

| Aspect | Standard Mode (Current) | Enhanced Mode (Proposed) |
|--------|------------------------|-------------------------|
| **Template** | Implementation-focused with code examples | WH(Y) statement with decision rationale |
| **Output** | Single ADR file | ADR file + specification stubs |
| **Dependencies** | Not tracked | Explicit relationship metadata |
| **Governance** | Not tracked | Status history, review cadence, approvers |
| **Validation** | Basic structure check | Extended Definition of Done (ECADR) |

### Enhanced Mode Behaviour

When the enhanced ADR mode is enabled, Claude-Flow v3 should:

1. **Generate ADRs** using the WH(Y) statement template with structured decision capture
2. **Prompt for dependencies** when creating new ADRs (Depends On, Relates To, Supersedes, Refines, Part Of)
3. **Create specification stubs** in a `specs/` directory when detailed implementation documentation is needed
4. **Validate ADRs** against the extended Definition of Done checklist before approval
5. **Support Master ADRs** for grouping related decisions under complex initiatives
6. **Track governance metadata** including status history, review schedules, and approval workflows
7. **Generate dependency graphs** for visualization and impact analysis (optional tooling)

### Migration Path

For existing Claude-Flow v3 users with implementation-focused ADRs:

1. **No forced migration**: Standard mode remains default
2. **Gradual adoption**: New decisions can use enhanced mode while legacy ADRs remain unchanged
3. **Optional conversion**: Tooling to extract decision statements from existing ADRs (future enhancement)
4. **Parallel use**: Teams can use standard mode for implementation docs and enhanced mode for strategic decisions

---

## Comparison Example

Using ADR-008 (Vitest) as a real example:

### Current Format (from Claude-Flow v3)
```markdown
# ADR-008: Vitest Over Jest

**Status:** Implemented | **Date:** 2026-01-03

## Context
v2 uses Jest for testing. Vitest is a modern alternative...

## Decision
Migrate to Vitest for v3.

## Rationale
- 10x faster (uses Vite)
- Better ESM support (native)
- Compatible Jest API

## Implementation
// vitest.config.ts
export default defineConfig({
  test: {
    globals: true,
    environment: 'node',
    ...
  }
});

## Migration Strategy
// Before (Jest)          // After (Vitest)
jest.fn()          →      vi.fn()
jest.resetAllMocks →      vi.resetAllMocks

## Performance Comparison
| Metric | Jest | Vitest |
|--------|------|--------|
| Test execution | ~30s | ~3s |
| Watch mode | ~5s | ~1s |
```

**Problems:** Config embedded, migration code included, no rejected alternatives explained, no dependencies, no governance.

### Proposed Format (Enhanced Mode)
```markdown
# ADR-008: Testing Framework Selection

## WH(Y) Decision Statement
**In the context of** Claude-Flow v3's testing infrastructure,
**facing** slow test execution (~30s), poor ESM support, and migration friction from v2's Jest setup,
**we decided for** Vitest as the testing framework,
**and neglected** Jest (slow, ESM issues), Mocha (no built-in mocking), and Node test runner (immature ecosystem),
**to achieve** 10x faster test execution, native ESM support, and Jest-compatible API for easier migration,
**accepting that** teams familiar with Jest will need minor syntax adjustments (jest.fn → vi.fn).

## Dependencies
| Relationship | ADR ID | Title | Notes |
|--------------|--------|-------|-------|
| Relates To | ADR-004 | Plugin Architecture | Plugins must be testable with Vitest |
| Enables | ADR-009 | Hybrid Memory Backend | Memory tests use Vitest |

## References
| Reference ID | Title | Location |
|--------------|-------|----------|
| SPEC-008-A | Vitest Configuration Standard | specs/SPEC-008-A.md |
| SPEC-008-B | Jest Migration Guide | specs/SPEC-008-B.md |

## Governance
| Board | Date | Outcome | Next Review |
|-------|------|---------|-------------|
| Platform Team | 2026-01-03 | Approved | 2026-07-03 |
```

**Benefits:** Decision rationale clear, alternatives documented, dependencies tracked, implementation details in separate specs, governance recorded.

---

*This ADR was created following the WH(Y) format as specified in [SPEC-CF3-001-A](https://github.com/cgbarlow/adr/blob/main/specs/SPEC-CF3-001-A-WHY-Format.md).*
