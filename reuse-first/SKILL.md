---
name: reuse-first
description: >
  Research reusable implementations before adding a new module, subsystem,
  integration, or broadly available capability. Compare the project's existing
  code, platform features, libraries, and maintained open-source projects, then
  choose reuse or a small local module based on fit and lifetime cost. Do not
  use for routine edits that add no new capability.
---

# Reuse First

Avoid both reflexively adding a dependency and reflexively rebuilding a solved
problem. Make a brief, evidence-based choice that fits this project.

## Start before designing the implementation

When the request adds a module, subsystem, integration, or commonly available
capability, research reuse options before writing implementation code.

In the first investigation pass:

1. Read applicable repository rules and dependency manifests.
2. Inspect nearby code and installed dependencies for an existing capability.
3. Check the language standard library, platform, and framework features.
4. Search the current official package registry, upstream documentation, and
   GitHub for maintained candidates.

Do the local and Internet checks together when possible. If network access is
unavailable, say which external checks could not be completed; do not pretend
the ecosystem was searched.

Prefer primary sources: the candidate's official documentation, package
registry entry, source repository, releases, security policy, and license.
Blog posts and lists can identify candidates but are not enough to accept one.
Limit detailed comparison to the two or three credible candidates most likely
to fit. Stop when one option is clearly sufficient or all credible options have
a decisive shortcoming.

## Evaluate the real cost

Judge candidates against the current requirement and project size:

- exact feature fit and the amount of unused machinery;
- compatibility with the project's language, framework, versions, deployment,
  and existing conventions;
- maintenance activity, release history, API stability, issue responsiveness,
  and real adoption;
- direct and transitive dependencies, binary or service requirements, bundle
  size, startup cost, and operational complexity;
- known vulnerabilities, provenance, security policy, and update path;
- license compatibility and any attribution or distribution obligations;
- integration code, testing effort, documentation quality, and removal cost.

Stars and download counts are supporting signals, never the decision by
themselves. Never copy code whose license and provenance are unclear.

## Choose the smallest responsible option

Reuse a maintained implementation when the problem is complex, standardized,
security-sensitive, interoperability-heavy, or expensive to maintain correctly,
and the candidate's total cost is proportionate to the project.

Prefer a small local module when the behavior is narrow and stable, the standard
library or a direct implementation is easy to verify, or every external option
adds more API surface, dependencies, risk, or operational burden than it removes.
Do not use a mechanical line-count threshold.

Follow the repository's established dependency policy. Ordinary low-risk
dependencies may be added as part of an authorized implementation. Stop for the
user's decision before introducing a paid service, external account, restrictive
or unclear license, telemetry or data upload, new privileged access, native
runtime, unusually large dependency tree, or durable architectural lock-in.

## Record the decision

Before implementation, report or retain a concise decision record:

```text
Need: <capability being added>
Checked: <existing code, platform features, registries, and repositories>
Candidates: <credible options and decisive evidence>
Decision: <reuse one option or write a small local module>
Reason: <fit and lifetime-cost rationale>
Impact: <new dependencies, permissions, services, license, or none>
Verification: <how the choice will be tested>
```

Keep this in the task report unless the repository has an established place for
dependency decisions. Do not create an ADR or new documentation file for a
routine, reversible choice.

## Implementation boundary

Research does not authorize unrelated features, external accounts, purchases,
or copying another project's code. Once the choice is made, implement only the
requested capability and use the project's normal testing and review workflow.

If a one-off migration, maintenance, diagnostic, or data-repair helper is needed,
place a small script in the project's existing script directory, or `scripts/`
when none exists. Keep script-only behavior out of the application program and
do not create a script when one ordinary command is sufficient.
