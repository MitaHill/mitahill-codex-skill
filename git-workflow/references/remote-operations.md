# Remote Operations and History Changes

Read this reference only when the requested work involves branches, worktrees,
merge/rebase/cherry-pick/revert, tags, remotes, push, pull requests, conflicts,
discarding changes, or history recovery.

## Prepare the operation

Inspect the current branch or detached HEAD, worktree ownership, upstream,
ahead/behind counts, conflicts, and affected commits. Determine the intended base
from repository policy, the current upstream, or the task context. If more than
one base is plausible, ask the user before integrating.

Do not use `git pull` as a generic synchronization step. Explain the need to
synchronize, fetch, inspect the remote change, and then follow the repository's
documented merge or rebase policy. Fetch updates remote-tracking state; mention
it before running when it is needed for an accurate proposal.

Before any mutation, report:

- the exact command and refs involved;
- commits added, moved, replayed, removed, or made unreachable;
- working-tree and worktree effects;
- whether the operation contacts or changes a remote;
- expected conflicts and the recovery or abort path;
- checks already run and checks to run afterward.

Ask for explicit approval of that operation. Recheck HEAD, status, and relevant
remote OIDs immediately before execution; material drift invalidates approval.

## Branches and worktrees

Honor the repository's branch model. Without one, prefer short-lived task
branches for non-trivial or shared work, but let the user approve creation and
the exact name. Never assume the default branch is named `main` or `master`.

Do not remove a worktree merely because a task or pull request is complete.
Remove only a worktree created for this task, from outside that worktree, after
the user approves the exact path. Delete a branch only after confirming its work
is retained where the user expects.

## Integration and pull requests

After implementation, verify the final tree and offer only applicable choices:

1. Keep the branch and working tree as-is.
2. Create the approved local commit.
3. Push the named branch.
4. Create a pull request against the confirmed base.
5. Merge using the repository's approved method.

Do not combine these permissions. Follow repository PR templates, required
checks, reviews, CODEOWNERS, and merge settings. Never merge a pull request just
because it is green or because the agent created it.

## Published history

Never use plain `--force`. A rejected push is evidence to inspect, not permission
to overwrite the remote.

If the user explicitly requests rewriting a published branch:

1. Fetch the exact remote ref and record its OID.
2. Show the commits and collaborators' work that could be displaced.
3. Explain recovery using the recorded OID or reflog.
4. Obtain a new approval naming the remote, branch, expected OID, and replacement
   tip.
5. Use an exact lease such as
   `--force-with-lease=<ref>:<expected-oid>`; add `--force-if-includes` when the
   installed Git version supports it and its reachability check is applicable.

Do not treat a general approval to push as approval to rewrite history.

## Destructive and recovery operations

Commands that discard or hide work include `reset --hard`, `clean`, restoring a
path from another tree, forced branch deletion, stash drop, reflog expiration,
and aggressive garbage collection. Before any such command:

- identify every affected path, ref, and commit with read-only checks;
- prefer a recoverable alternative;
- state exactly what will be lost and what can be recovered;
- require explicit approval for the exact target;
- verify the target again immediately before execution.

Never broaden a destructive target with unresolved variables, globs, repository
roots, or user-home paths. Stop after the approved operation and report the new
state.
