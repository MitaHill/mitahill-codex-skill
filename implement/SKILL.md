---
name: implement
description: "Implement a piece of work based on a PRD or set of issues."
---

Implement the work described by the user in the PRD or issues.

If the work adds a module, subsystem, integration, or commonly available
capability, use `reuse-first` before designing the implementation. Record why an
existing implementation or a small local module is the better fit.

Use /tdd where possible, at pre-agreed seams.

Run typechecking regularly, single test files regularly, and the full test suite once at the end.

For graphical behavior, use `runtime-verification` after automated checks. Run
the actual application and exercise affected flows with the available browser,
simulator, or Computer Use capability; a build or test suite alone is not enough.

Once done, use /review to review the work.

Leave the work uncommitted unless the user explicitly authorizes a commit. When
the work is ready, use `git-workflow` to report the complete Git state, proposed
staging paths, commit grouping, message, checks, commands, and consequences, then
wait for approval.
