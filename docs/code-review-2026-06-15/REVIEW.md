# Code Review — claude-config-public (tier 3)

**Review date:** 2026-06-15

## Summary

| Severity | Total | Verified-real | Uncertain | Refuted |
|----------|-------|---------------|-----------|---------|
| Critical | 0 | 0 | 0 | 0 |
| High | 0 | 0 | 0 | 0 |
| Medium | 3 | 1 | 0 | 2 |
| Low | 5 | 0 | 0 | 5 |
| **Total** | **8** | **1** | **0** | **7** |

Verification rule: a finding is **VERIFIED-REAL** when at least 2 of 3 independent skeptics confirmed it. Only **1** finding cleared that bar. The remaining 7 were refuted (skeptics found them to be false positives or mischaracterizations of intentional documentation design).

---

## Medium

### VERIFIED-REAL

**Inconsistent placeholder naming convention**
`CLAUDE.md:20, 36, 58-62, 79`

Placeholders use multiple stylistic formats: `<your choice>` (line 20), `<your push/PR policy here>` (line 36), `<your kickoff/plan skill>` (line 58), and `<your domains…>` (line 79). The varied phrasing (verb-noun, noun-only, with-explanatory-text, with-ellipsis) makes it harder for users to distinguish required customization points from examples.

**Fix:** Standardize on one format (e.g. `<your-value>` or `<USER: value>`), document the pattern in the header comment (lines 3-6), and apply consistently.

**Note:** Two of three skeptics confirmed the inconsistency is real but flagged severity as overstated — all placeholders are angle-bracket-delimited in an explicitly-labeled template, so this is polish, not a usability blocker. Treat as **low priority** despite the medium label.

---

## Low

_No low-severity findings cleared verification. All five are in the Refuted section below._

---

## Refuted / low-confidence

These findings did not reach the 2-of-3 verification bar. They are retained for the record, not deleted. The dominant skeptic reasoning: this repo is a **public pattern/template**, not a runtime system, and several findings mischaracterize intentional layered-documentation design as defects.

### Medium (refuted)

**Duplicate concept definition across CLAUDE.md and ARCHITECTURE.md** — `CLAUDE.md:51-81`
Claimed sections 4-5 of CLAUDE.md duplicate ARCHITECTURE.md lines 10-39, forcing readers to reconcile two authoritative definitions.
**Refuted (3/3, high confidence):** The two docs operate at different altitudes — CLAUDE.md sections 4-5 are operational routing tables; ARCHITECTURE.md is conceptual design explanation. ARCHITECTURE.md explicitly defers authority to CLAUDE.md ("the router in CLAUDE.md says which file for which domain", line 32). This is intentional hierarchical/layered documentation, not problematic duplication.

**Weak cohesion in section 1 (Non-negotiable rules)** — `CLAUDE.md:10-27`
Claimed section 1 groups five unrelated rules (tool autonomy, narrate-before-execute, package managers, TypeScript types, code-edit formatting) that should be split by category.
**Refuted (3/3, high confidence):** The section is intentionally unified by *function* — universal non-negotiable agent guardrails — not by technical domain. Reorganizing by category would fragment the "default is GO" safety/autonomy model and scatter high-signal constraints, reducing visibility. The cross-domain grouping is deliberate, pre-domain foundational design (sections 2-10 carry the per-domain organization).

### Low (refuted)

**`.gitignore` excludes auto-generated index file** — `.gitignore:21`
Claimed excluding `.line-navigator.md` conflicts with a "selective tooling" design principle.
**Refuted (3/3, high confidence):** No "selective tooling" principle exists in README/ARCHITECTURE/CLAUDE. `.line-navigator.md` is an auto-generated local dev artifact regenerated on every edit; gitignoring it in a sanitized public template is correct practice, not a conflict.

**Missing connection between related routing concepts** — `CLAUDE.md:51-81`
Claimed sections 4 and 5 both describe "routers" without stating their relationship.
**Refuted (3/3, high confidence):** The dual-router relationship is already documented in ARCHITECTURE.md ("two routers that point at the heavier layers: a capability table → skills and a context table → domain files"). The README directs readers there. Sections are also distinguished by purpose (skill selection vs. context loading) and sequenced numerically/temporally ("before exploring code" vs. "read before working in a domain").

**Context files table has inconsistent cross-domain references** — `CLAUDE.md:73-79`
Claimed the Code-quality/linting row lacks a corresponding rule section, breaking a one-row-per-section pattern.
**Refuted (3/3, high confidence):** Section 5 is a routing table to *external* context files, not a requirement that every domain maps to an internal numbered section. Code-quality rules do exist in section 1 (no `any`, no placeholder ellipsis). The asymmetry is not a structural error, and the file is an explicit template with placeholder rows.

**Mislabeled architecture layers — claims 'four' but describes six** — `docs/ARCHITECTURE.md:16`
Claimed the "four layers" heading is contradicted by later Hooks (line 41) and Subagents (line 49) sections.
**Refuted (3/3, high confidence):** "The four layers" (Global rules, Skills, Context files, Memory) is the context-loading/progressive-disclosure taxonomy. Hooks (runtime/harness level) and Subagents (parallelism strategy) are orthogonal concerns presented in their own clearly-labeled sections, not hidden sub-layers. No reader ambiguity.

**No explicit guard against circular dependencies between layers** — `docs/ARCHITECTURE.md:16-48`
Claimed the layered system lacks an explicit constraint forbidding circular references.
**Refuted (2/3):** Two skeptics noted this is a documentation/template repo with no runtime loading mechanism that could produce cycles — there is nothing to enforce against. One skeptic agreed the explicit statement is absent but rated it a real (low) doc gap. Below the 2-of-3 bar.

**Missing scalability guidance for capability routing table** — `CLAUDE.md:51-65`
Claimed the 5-entry example table provides no guidance on growing to 100+ capabilities (grouping, deprecation, max size).
**Refuted (2/3):** Two skeptics found ARCHITECTURE.md already solves scaling by architecture — capabilities live in separate on-demand skill files, so the always-loaded routing table stays small by design; explicit growth/deprecation guidance is an optional refinement, not a critical gap. One skeptic confirmed it as a genuine doc gap (no operational guidance on managing the table itself). Below the 2-of-3 bar.

---

## Bottom line

For a tier-3 public template repo, this is clean. The single actionable item is the placeholder-format inconsistency (verified, but low-impact polish). Everything else reflects intentional layered-documentation design that the skeptic panel found sound. No correctness, security, or architectural defects surfaced.
