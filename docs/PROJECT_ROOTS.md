# Project Roots and Boundary Semantics

## Purpose

This document defines what the “roots” and boundary directories mean in this access-control model.

It exists to prevent a common failure mode:
People treat security boundaries as working directories.

---

## Root Boundary: `ProjectX/`

### Intended meaning
`ProjectX/` is a **structural gate**, not a workspace.

It is allowed to be:
- traversed by authorized roles (to reach their scoped areas)
- readable by stewards (for governance)

It is not allowed to be:
- a dumping ground for files
- a location where multiple roles write “because it’s convenient”

### Baseline expectation
- mode: `750`
- owner: `project_lead`
- group: `emp_admin` (stewards)

### ACL expectations
- Dev teams: `--x` on `ProjectX/` (traverse only; cannot list)
- Reviewers: `--x` on `ProjectX/` (traverse only)
- Testers: typically `--x` on `ProjectX/` ONLY if binaries live under the root
  - testers should never get `r-x` at root

### Default ACL expectations
Default ACLs exist to prevent drift when new top-level directories are created.

Recommended:
- Dev teams: default `--x`
- Reviewers: default `--x`
- Testers: omit from defaults (they should not automatically traverse future top-level dirs)

---

## Team Boundary Roots: `core/`, `api/`, `ui/`

### Intended meaning
Each team directory is a **team-owned namespace**.

- Owning team writes (`rwx`)
- Other teams read/traverse (`r-x`) via ACL
- No cross-team writes

### Baseline expectation
- mode: `770`
- owner: `project_lead`
- group: `<team_group>`

### ACL expectations (access ACL)
- Other dev teams: `r-x`
- Reviewers: `r-x` (directories require `x` to traverse)
- Testers: no access

### Default ACL expectations
Set defaults so newly created files/subdirs remain compliant:
- Other dev teams default: `r-x`
- Reviewers default: `r-x`
- (Optional) use a stricter file strategy if desired, but ensure inheritance is explicit.

---

## Governance Root: `docs/` (if used)

### Intended meaning
`docs/` is a controlled collaboration surface, not a free-for-all.

Two valid patterns:

**Pattern A (team namespaces):**
- `docs/core/`, `docs/api/`, `docs/ui/` each owned by the corresponding team group.
- Cross-team read via ACL.
- No global “reviewers notes” or “testers notes” required if feedback uses tickets/PRs.

**Pattern B (steward-owned docs):**
- `docs/` writable only by `emp_admin`
- Teams contribute docs through ticket/PR workflow or designated intake paths.

This repo favors Pattern A for clarity.

---

## Test Corridor Root: `tests/bin/`

### Intended meaning
This is the **execution corridor** for black-box testing.

Testers should only be able to:
- traverse to this directory
- execute binaries inside

They should not be able to:
- list source directories
- read source code
- write binaries

### Baseline expectation (lead-only build authority)
- `tests/` : `750 project_lead:emp_admin`
- `tests/bin/` : `750 project_lead:emp_admin`

### ACL expectations
- testers: `--x` on:
  - `ProjectX/` (only if needed to reach tests)
  - `tests/`
  - `tests/bin/`
- reviewers: typically none here (or `--x` if needed for traversal only)

### Default ACL expectations
Defaults are optional here depending on whether binaries are created by CI.
If CI writes here as `project_lead`, defaults are not strictly required.

---

## Summary Rule

A “root” directory in this model is always one of:
- **structural gate** (Project root)
- **namespace boundary** (team dirs, docs namespaces)
- **execution corridor** (tests/bin)

Treating a boundary as a general workspace is a policy violation.
