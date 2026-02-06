# Access Control Lab

Access_control_lab is a practical security engineering project focused on
**Linux filesystem permissions, ACLs, and hardened directory structures**
in a multi-user environment.

This repository documents both the **design decisions** and the **implemented
permission model**, with an emphasis on correctness, least privilege, and auditability.

---

## Project Goals

- Design a realistic multi-role access control model
- Enforce least-privilege permissions using POSIX permissions and ACLs
- Validate access paths for different user roles
- Document security decisions and verification steps
- Produce a hardened “live” filesystem tree that reflects the model

This is not a theoretical exercise; it is a **hands-on implementation** with
verifiable permissions.

---

## Repository Structure

Access_control_lab/
├── docs/
│ ├── SECURITY_MODEL.md
│ ├── PROJECT_ROOTS.md
│ ├── TESTING_NOTES.md
│ └── permissions_diagrams/
├── ProjectX_hardened_live/
│ ├── engineering/
│ ├── testing/
│ ├── operations/
│ └── audit/
└── README.md

---

## Security Model

The access control model defines multiple roles with distinct responsibilities,
including (but not limited to):

- Project leads
- Engineers
- Testers
- Operations / audit roles

Permissions are enforced using:
- Traditional UNIX owner/group/mode bits
- Default and explicit ACLs
- Controlled inheritance on directory creation

All decisions are documented under `docs/SECURITY_MODEL.md`.

---

## Hardened Live Tree

`ProjectX_hardened_live/` represents the **final enforced state** of the filesystem:
- Permissions are applied and verified
- Unauthorized access paths are explicitly blocked
- Group ownership and ACL inheritance are intentional

This directory is the **artifact**, not a mockup.

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
- Practical Linux security knowledge
- Real permission modeling (not chmod spam)
- ACL usage beyond basic tutorials
- Defensive system design
- Documentation discipline

It is intended to show how I reason about **access boundaries and trust** in
multi-user systems.

---

## Status

- Security model: implemented
- Hardened tree: complete
- Documentation: in progress
- Automation: planned (future enhancement)