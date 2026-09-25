# Lab 03: ACLs, Groups & Privilege Management

**Domain:** Access Control · **Maps to:** CWE-269 (Improper Privilege Management)
**Tools:** `groupadd`, `usermod`, `setfacl`, `getfacl`, `sudo`, `su`

## Objective
Extend the Lab 02 environment with group-based access and POSIX Access Control Lists. The goal was to express permissions that the basic owner/group/other model can't handle.

## Approach
- Created role groups with `groupadd` and assigned users with `usermod`, including changing home directories.
- Applied **ACL entries** with `setfacl` to give specific users or groups access beyond the file's owning group, and checked them with `getfacl`.
- Worked through the differences between `sudo`, `sudo -i`, `su`, `su -`, and `sudo su -`. These determine whose identity, environment, and home directory a command runs with.
- Validated the final state with the lab's evaluation script.

## Key concepts
| Concept | Why it matters |
|---|---|
| ACLs beyond `ugo` bits | They allow fine-grained exceptions (e.g., one contractor gets read access) without widening group membership. |
| ACL **mask** | It caps the effective rights of named users and groups. It's easy to overlook, and it can quietly override the permissions you meant to grant. |
| Default ACLs | They set what new files inherit, which keeps access consistent as a directory grows. |
| `sudo` vs `su` | `sudo` authenticates with *your* password and logs the action. `su` uses the *target* account's password. That difference affects both accountability and credential sharing. |

## Defensive takeaways
- Use `sudo` with narrowly scoped `sudoers` rules instead of sharing the root password. You get a per-user audit trail.
- Review ACLs regularly. A `+` at the end of the `ls -l` permission string is the only visible sign one exists.
- Manage access through groups instead of per-user exceptions wherever possible, so reviews stay manageable.
