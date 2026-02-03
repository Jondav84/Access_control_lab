# Build Methodology: Operational Hardening vs Policy-First Construction

## Purpose

This repository intentionally demonstrates two different methods of producing a secure filesystem structure:

1. **Operational Hardening (Live Normalization)**
2. **Policy-First Construction (Reference Architecture)**

Both are critical skills.

Most real-world systems require the first.  
Mature organizations strive toward the second.

This document explains the reasoning behind each.

---

# Model 1 — Operational Hardening (Live Environment Simulation)

## Philosophy

Systems are rarely born secure.

They are hardened over time through:

- discovery
- correction
- normalization
- enforcement

This model reflects real infrastructure conditions.

---

## Starting State

The project structure was created first, before full ACL strategy was finalized.

This mirrors common industry realities:

- rapid project creation
- evolving security requirements
- delayed governance
- organic team growth

Rather than rebuilding, the structure was hardened in place.

---

## Hardening Process

The following normalization sequence was applied:

1. Correct ownership (project_lead as root authority)
2. Normalize baseline permissions (chmod)
3. Establish trust boundaries (team directories)
4. Apply Access ACLs
5. Apply Default ACLs to prevent permission drift
6. Restrict execution corridors (tests/bin)

---

## Skills Demonstrated

Operational hardening proves the ability to:

- analyze existing permission structures
- detect security gaps
- safely modify live access controls
- avoid privilege spill
- enforce least privilege without downtime

These are production-critical capabilities.

---

## Key Insight

Perfect construction is rare.

The ability to **secure imperfect systems** is what distinguishes operational security engineers.

---

# Model 2 — Policy-First Construction (Reference Architecture)

## Philosophy

Security should ideally be inherited — not repaired.

This model demonstrates the correct construction order for new projects so that:

- permissions are never retroactively fixed
- inheritance prevents drift
- trust boundaries exist immediately
- the filesystem is born compliant

---

## Hardened Construction Sequence

For every boundary directory:

1. Create directory
2. Set ownership (`chown`)
3. Set baseline permissions (`chmod`)
4. Apply Access ACL
5. Apply Default ACL
6. Create child directories

**Defaults must exist before children are created.**

Failure to do so results in permission drift.

---

## Security Advantages

Policy-first construction provides:

- deterministic inheritance
- reduced operational overhead
- fewer emergency corrections
- clearer audit posture
- predictable access patterns

It is the recommended method for greenfield projects.

---

# Why This Repository Contains Both

Showing only a perfect build suggests academic knowledge.

Showing only remediation suggests reactive operations.

Showing both demonstrates:

> The ability to secure chaos AND prevent it.

This is the foundation of senior-level infrastructure design.

---

# When To Use Each Model

## Use Operational Hardening when:

- inheriting legacy systems
- joining existing organizations
- auditing environments
- performing security remediation
- integrating governance into live projects

## Use Policy-First when:

- creating new platforms
- building internal templates
- defining organizational standards
- designing secure-by-default infrastructure

---

# Final Principle

Security is not a single action.

It is a lifecycle:

design → build → verify → enforce → evolve

This repository demonstrates that lifecycle intentionally.
