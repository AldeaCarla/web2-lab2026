---
name: build
description: Implement an approved plan or a small, clear fix. Follow repository guidance, use tests first where tests exist, run documented checks, and report results.
---

# Build

Implement only the approved plan. If it is impossible or conflicts with repository guidance, stop and report the issue.

## Branch and scope

Check the current branch and worktree. Use a task branch when the repository has a usable default branch. Never push, merge, tag, release, or deploy without the user's approval for that action.

## Verify first

For repositories with tests, add tests for the approved behavior and run them to confirm they fail for the expected reason. If no test suite exists, describe how each behavior was checked.

## Implement

Follow approved patterns and match nearby code. Do not weaken tests. Fix defects introduced by this work; report unrelated defects without expanding scope. Stop when behavior or requirements are unclear.

## Verify and record

Run the checks documented in `AGENTS.md` and report every result, including failures and skipped checks. Record reusable practices as candidate patterns, settled constraints as proposals, and unresolved points as Q- entries.

## Report

List changed files, tests and checks with results, deviations from the plan, defects fixed, new patterns or questions, and remaining work. Do not commit or merge unless the user asks.
