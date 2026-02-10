# Security & Access Control Model

## Purpose

This repository demonstrates a deliberate access-control and governance model for software projects.

The goal is not convenience.  
The goal is containment, accountability, and damage prevention.

This repo intentionally contains two “templates”:

- **Operational Hardening Template**: realistic build-first, then harden/normalize in place.
- **Reference Architecture Template**: ideal policy-first construction order (secure by inheritance).

Both are valuable. They teach different skills.

These templates are implemented as separate trees:
- ProjectX_policy_first demonstrates policy-first (greenfield) construction.
- ProjectX_hardened_live demonstrates legacy hardening of an existing tree.
The roles defined below apply identically in both contexts.

---

## Assumptions

- Projects involve multiple teams with overlapping dependencies.
- Mistakes are more common than malice, but malice must be assumed possible.
- Insider threats are more realistic than external attackers.
- Trust is not static and should not be encoded as permanent write access.

---

## Roles

### Project Lead (Owner)
- Single accountable authority.
- Owns the entire project tree.
- Full read/write/execute across all project paths.
- **Sole publisher** into production paths (including published docs and binaries).

Ownership exists for accountability, not collaboration.

---

### Project Stewards (Administrative / Governance Layer)
- Small, explicitly assigned group (e.g., project lead + team leads).
- No ownership of the project.
- No blanket write access to team implementation directories or binary outputs.
- Coordinates governance paths only.

Permissions (typical):
- Read/traverse at the project root.
- Write only to explicitly designated governance areas.
- No write access to team-owned implementation directories or binary outputs.

Governance access does not imply development authority.

---

### Development Teams
- Full write access only to their own scoped **implementation** directories.
- Read/traverse access to other teams’ code where required for integration.
- No write access to other teams’ code.
- Teams author docs in **separate team-owned work areas** (outside the published docs tree).
- Teams submit docs through team leads to the project lead.
- Project lead proofs and **publishes** to `ProjectX/docs/<team>/`.

Teams have **read-only** access to published docs.

---

### Testers
- Black-box role.
- Execute-only access to compiled binaries.
- No read access to source code or documentation.
- No write access anywhere.

Purpose:
- Prevent source exposure and code tampering.
- Enforce executable-only testing.

---

### Reviewers (Audit Role)
- Read-only audit role.
- No write permissions.
- No execution permissions.

Feedback is delivered through ticketing / PR review systems, not filesystem write paths.

---

### All Others
- No access (no read, no execute, no metadata visibility).

---

## Permission Strategy

This model uses:
- UNIX ownership (single owner, single group)
- Group-based permissions for primary access control
- ACLs for cross-cutting visibility and corridor access
- Default ACLs to prevent policy drift on newly created files/directories

### Baseline permissions (chmod/chown)

- **Project root**: `750` (owner: project lead, group: emp_admin/stewards)  
  Root is a structural boundary, not a working directory.

- **Team implementation directories**: `770` (owner: project lead, group: owning team)  
  Team-only writes; no outsider access by default.

- **Published docs namespaces**: `750` (owner: project lead, group: emp_admin/stewards)  
  Publish-only. Teams/readers get visibility via ACLs; no team write.

- **Testing and binary output paths**:
  - `testing/` and `testing/bin/` are `750` (owner: project lead, group: stewards)
  - Build authority is singular (project lead or CI acting as project lead)

Write access is never implicit. Writable locations are explicitly designated.

---

## ACL Strategy

ACLs are applied **dir-by-dir** (boundary-by-boundary), not group-by-group.

Reason:
- ACLs attach to filesystem objects.
- Boundary-by-boundary avoids privilege spill and makes verification unambiguous.

### Access ACL vs Default ACL

- **Access ACL**: affects the directory itself (“who can do what here now”).
- **Default ACL**: inheritance template (“what should newborn objects inherit here”).

Default ACLs are required to prevent policy drift.

---

## Operational Templates Included

### 1) Operational Hardening Template (build-first, normalize later)

This template mirrors real environments:

- Structure often exists before policy is perfect.
- Hardening is applied after the fact:
  - normalize ownership
  - normalize baseline permissions
  - apply access ACLs
  - apply default ACLs (to stop drift)

This demonstrates operational security competence: detect → correct → normalize.

### 2) Reference Architecture Template (policy-first construction order)

This template demonstrates ideal construction ordering for maximum security by inheritance:

1. create directory
2. set ownership (chown/chgrp)
3. set baseline permissions (chmod)
4. apply access ACL (exceptions)
5. apply default ACL (inheritance template)
6. create children (they inherit correctly)

This is the recommended approach for new projects and templates.

---

## Threat Models Addressed

### Accidental Breakage
Developers changing shared code to fix local issues, breaking other components.

Mitigation:
- Scoped write permissions.
- Cross-team read/traverse visibility without cross-team mutation.

### Insider Sabotage
Actors intentionally introducing regressions or destructive changes.

Mitigation:
- Single-owner accountability.
- Explicit write boundaries.
- Limited blast radius per team.

### Unauthorized Reconnaissance
Non-participants discovering structure/interfaces/timelines.

Mitigation:
- No outsider access.
- Corridor traversal only where required.

---

## Non-Goals

This model does not attempt to:
- Replace version control
- Replace code review processes
- Solve social or organizational conflict

It reduces the damage such failures can cause.
