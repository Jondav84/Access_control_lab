# Access Control Lab

Access_control_lab is a practical security engineering project focused on
**Linux filesystem permissions, ACLs, and hardened directory structures**
in a multi-user environment.

This repository documents both the **design decisions** and the **implemented
permission models**, with an emphasis on correctness, least privilege, and
auditability.

---

## Project Goals

- Design a realistic multi-role access control model
- Enforce least-privilege permissions using POSIX permissions and ACLs
- Validate access paths for different user roles
- Document security decisions and verification steps
- Demonstrate both greenfield design and legacy hardening scenarios

This is not a theoretical exercise; it is a **hands-on implementation** with
verifiable permissions.

---

## Repository Structure

## Repository Structure

Access_control_lab/
├── docs/
│   ├── SECURITY_MODEL.md
│   ├── PROJECT_ROOTS.md
│   ├── TESTING_NOTES.md
│   └── permissions_diagrams/
├── ProjectX_hardened_live/
│   ├── core/
│   ├── api/
│   ├── ui/
│   ├── docs/
│   ├── testing/
│   └── root_permissions.txt
├── ProjectX_policy_first/
│   ├── core/
│   ├── api/
│   ├── ui/
│   ├── docs/
│   ├── testing/
│   └── root_permissions.txt
└── README.md

The two ProjectX trees are intentionally structurally identical.

- ProjectX_policy_first demonstrates greenfield, policy-driven construction.
- ProjectX_hardened_live demonstrates legacy remediation of an existing tree.

The directory layout is the same so that differences in permissions,
ACLs, inheritance, and authority boundaries can be compared directly.


---

## Security Model

The access control model defines multiple roles with distinct responsibilities,
including (but not limited to):

- Project leads
- Engineers
- Testers
- Reviewers  (audit role)

Permissions are enforced using:
- Traditional UNIX owner/group/mode bits
- Default and explicit ACLs
- Controlled inheritance on directory creation

All design decisions and role definitions are documented under
`docs/SECURITY_MODEL.md`.

---

## Policy-First Reference Architecture (Greenfield Design)

`ProjectX_policy_first/` demonstrates how to design and construct a directory
hierarchy **correctly from the start**, with access-control policy driving
ownership, ACLs, inheritance, and boundaries at creation time.

It shows:
- how directories are created from an empty state
- when ownership, ACLs, and inheritance are applied
- why specific security boundaries exist
- how policy decisions translate directly into filesystem state

This tree represents **greenfield project setup**.
It is **not** a hardened legacy tree and is **not derived from**
`ProjectX_hardened_live/`.

If starting a new project:
- use `ProjectX_policy_first/` as the authoritative reference model
- replicate its construction patterns intentionally

---

## Hardened Live Tree (Legacy Remediation)

`ProjectX_hardened_live/` represents a **hardened legacy filesystem tree**.

It demonstrates how to:
- analyze an existing, populated directory structure
- identify unsafe or overly permissive defaults
- apply corrective ownership, permissions, and ACLs
- enforce least privilege without breaking operation

This tree represents **post-hoc security remediation** of a system that was
not originally designed with a formal access-control model.

It is a realistic example of securing legacy environments.

---

## Verification

The project includes:
- Permission inspection using `getfacl`
- Role-based access testing
- Documentation of expected vs actual behavior

Testing notes and validation steps are documented under `docs/`.

---

## Why This Matters (Portfolio Intent)

This project demonstrates:
- Practical Linux security engineering
- Real permission modeling (not chmod spam)
- ACL usage beyond basic tutorials
- Defensive system design
- Documentation discipline
- Clear reasoning about access boundaries and trust

It is intended to show how I reason about **both building secure systems
correctly and fixing insecure systems safely**.

---

## Status

- Security model: implemented
- Policy-first tree: complete
- Hardened live tree: complete
- Documentation: in progress
- Automation: planned (future enhancement)
