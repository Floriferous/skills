---
name: dependency-gardening
description: Refresh dependency versions and their adoption across the repo. Use when upgrading packages, reviewing dependency update PRs, or reassessing how we use a library.
---

# Dependency Gardening

Keep dependencies current and their adoption idiomatic, simple, and efficient. Each pass reassesses our entire use of the selected library with today's knowledge and capabilities. Release notes are one input; existing code deserves fresh scrutiny even when upstream hasn't changed.

Work on the selected dependency or coupled family. This is intentional maintenance, not a schedule to update everything every day.

## Workflow

1. **Establish the current state.** Identify installed versions, all consumers, and the latest stable release. Prefer staying current; explain concrete constraints when choosing an older target. Treat tightly coupled packages as one upgrade.

2. **Read upstream.** Read current official documentation and recommended practices, plus the full changelog and migration guides between our installed and target versions. Include intermediate releases. Follow linked details that could affect our usage; state documentation gaps.

3. **Reassess the whole adoption.** Inspect every usage across the repo: callers, configuration, wrappers, abstractions, patches, and workarounds. Evaluate the approach afresh rather than limiting changes to the release delta. Look for:
   - Misused APIs, outdated patterns, and missed built-in capabilities.
   - Unnecessary wrappers, duplicated logic, and avoidable complexity.
   - Correctness, lifecycle, and performance problems.
   - Workarounds and overrides whose justification no longer holds.
   - Inconsistent patterns across consumers.

4. **Choose and implement improvements.** Recommend **adopt now**, **retain**, or **defer**, grounded in concrete code and upstream evidence. Prefer the simplest sound implementation for our actual needs. Include clear improvements within the approved scope; route consequential redesigns through the existing planning process. Verify that upstream behavior covers our requirements before removing workarounds. Keep good existing code; novelty alone earns no rewrite.

5. **Verify and report.** Follow repository dependency conventions and validation gates. Exercise affected behavior and measure performance when claiming a local improvement. Report versions, source links, improvements adopted, code removed, worthwhile deferrals, and checks run. Distinguish upstream claims from locally verified benefits.

## Completion

Every usage is accounted for: improved, retained with a reason, or explicitly deferred. Group equivalent usages in the report. A version-only update—or no change—is valid when the full assessment supports it.

## Sources

- Florian's dependency-gardening brief, 2026-10-20: stay current and reassess the whole adoption with each pass, beyond changelog-driven fixes.
