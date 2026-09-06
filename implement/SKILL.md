---
name: implement
description: "Implement a piece of work based on a PRD or set of issues."
---

Implement the work described by the user in the PRD or issues.

Use /tdd where possible, at pre-agreed seams.

Run typechecking regularly, single test files regularly, and the full test suite once at the end.

Once done, use /review to review the work.

Leave the work uncommitted unless the user explicitly authorizes a commit. When
the work is ready, use `git-workflow` to report the complete Git state, proposed
staging paths, commit grouping, message, checks, commands, and consequences, then
wait for approval.
