# Example groups and service accounts (anonymized for documentation)
# This file documents the intended access-control model.
# It is descriptive, not authoritative system state.

Groups:

core_team:x:1002:
  Description:
    Development team responsible for the Core subsystem.
    Owns all source code and documentation under:
      - ProjectX/core/
      - ProjectX/docs/core/
    Permissions:
      - Full read/write/execute within owned paths
      - Read/execute (no write) access to other teams’ code and docs
    Restrictions:
      - No write access outside core-owned namespaces
      - No authority over binaries or project structure

api_team:x:1003:
  Description:
    Development team responsible for public and internal APIs.
    Owns all source code and documentation under:
      - ProjectX/api/
      - ProjectX/docs/api/
    Permissions and restrictions mirror core_team.

ui_team:x:1004:
  Description:
    Development team responsible for user interface components.
    Owns all source code and documentation under:
      - ProjectX/ui/
      - ProjectX/docs/ui/
    Permissions and restrictions mirror core_team.

testers:x:1005:
  Description:
    Black-box testing role.
    Does not participate in development or documentation.
    Interacts only with compiled binaries.
    Permissions:
      - Execute-only access (--x) to ProjectX/tests/bin/
    Restrictions:
      - No read access to source code or documentation
      - No write access anywhere

reviewers:x:1006:
  Description:
    Read-only audit and proofing role.
    Provides feedback exclusively through ticketing or review systems.
    Permissions:
      - Read-only access (r--) to code and documentation
    Restrictions:
      - No write or execute permissions
      - No access to binary execution paths

emp_admin:x:1007:
  Description:
    Project stewardship and governance group.
    Coordinates cross-team activities without implementing code.
    Permissions:
      - Read/execute access at project root
      - Write access to designated governance paths only
    Restrictions:
      - No write access to team-owned code or binaries
      - Governance does not imply development authority

Service Accounts:

project_lead:x:997:984::/home/project_lead:/usr/sbin/nologin
  Description:
    Non-login service account representing single-point ownership.
    Acts as the authoritative owner of the entire project tree.
    Responsibilities:
      - Owns all files and directories
      - Sole authority for structural changes and binary generation
      - Represents CI/build systems when applicable
    Constraints:
      - Cannot be used for interactive login
      - Exists for accountability, not daily work
