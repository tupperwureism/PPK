---
name: justifiqa-prompt-master
description: Create decision-complete, interruption-resistant Prompt Masters for delegating Justifiqa engineering, audit, documentation, and correction batches to another AI. Use when the user asks for a Prompt Master, executor handoff, fresh-session implementation prompt, recovery prompt after an interrupted agent, or a controlled batch with Git, security, DBB/DBS, testing, review, and commit boundaries.
---

# Justifiqa Prompt Master

Produce one copy-paste-ready prompt for one discrete batch. Do not implement the generated prompt unless the user separately authorizes implementation.

Use prompt-engineer as a companion only when the user also needs model-specific optimization, token reduction, few-shot examples, structured outputs, or prompt evaluation. This skill is authoritative for Justifiqa repository scope, safety, and completion gates.

## 1. Discover Before Drafting

1. Read the applicable AGENTS.md files.
2. Start code navigation from MarkDown/SYMBOLS_MAP.md and rg.
3. Read MarkDown/SQL_SECURITY_SYMBOLS.md only for RLS, grants, functions, or triggers.
4. Inspect the exact fixed point, current branch, staged index, active Git operations, relevant diff, and authoritative code/contracts.
5. Read only targeted design, migration, test, DBB, or DBS material needed for the batch.
6. Treat code and migrations as truth; treat reports and documentation as claims to verify.

Ask the user only when a missing choice would materially change architecture, security, business rules, or scope. Otherwise resolve facts from the repository.

## 2. Lock the Delegation Contract

Every Prompt Master must state explicitly:

- repository and required branch;
- exact fixed point/HEAD and ancestry rule;
- one batch objective and Definition of Done;
- in-scope and out-of-scope paths/features;
- authoritative contracts and required reading order;
- allowed and forbidden mutations;
- exact security and authorization boundaries;
- error, retry, idempotency, concurrency, loading, and invalidation behavior;
- UI preservation requirements when applicable;
- required behavioral tests and verification commands;
- staging allowlist/denylist and exact commit message;
- hard-stop conditions and final-report fields.

Never use placeholders such as “for example,” “version 1 perhaps,” “use dummy data,” or “choose whichever.” Never invent RPCs, prices, roles, policies, secrets, evidence, or production approvals. Convert a missing authoritative contract into an explicit blocker.

## 3. Protect Dirty Worktrees

Assume user work may be dirty.

- Require an empty staged index before implementation.
- Forbid reset, restore, checkout, stash, clean, destructive deletion, and unrelated formatting.
- Require provenance inspection before deleting or replacing inherited WIP.
- Stage only an explicit batch file set.
- Require the index to be empty after commit.
- Preserve unrelated modified, deleted, and untracked files.
- Never authorize push, deploy, merge, migration application, or production changes unless the user explicitly includes them.

If branch, fixed point, index, or an active Git operation violates the preflight contract, require a hard stop. Never instruct the executor to move HEAD automatically.

## 4. Make Fragile Decisions Explicit

For mutations or integrations, specify validation order, authorized actor/source of identity, canonical payload, server/browser boundary, replay semantics, conflict behavior, safe UI messages, and what must be refreshed after success.

For database work, when applicable:

- normalize data that requires relational constraints; do not hide it in JSON blobs;
- define lifecycle states and legal transitions;
- define immutability after activation/acceptance;
- define RLS, ACL, PUBLIC, anon, authenticated, and service_role behavior precisely;
- source identity from verified JWT/server context, never caller-supplied identity or user_metadata;
- label fixtures LOCAL_TEST_ONLY and never fabricate business pricing;
- forbid privilege broadening or RLS bypass merely to make tests pass.

For frontend work, keep Supabase/data access in services or hooks, preserve existing visual geometry and accessibility, prevent double submission, and require observable loading/error/success/retry states.

## 5. Design for Unstable Executors

Split long work into named checkpoints inside the same batch. Each checkpoint must end with:

- verified state;
- files intentionally changed;
- unresolved blocker or limitation;
- Next Exact Action.

Require the executor to continue automatically rather than repeatedly asking for “Continue.” If interrupted and later resumed, require it to reread the DBB, git status, and current diff, preserve valid WIP, and continue from Next Exact Action. Forbid partial commits unless the Prompt Master explicitly defines separate commits.

Do not request hidden chain-of-thought. Request observable evidence: commands, test results, diffs, file lists, and explicit decisions.

## 6. Require TDD and Real Gates

Require the narrowest behavioral test first, followed by proportionate integration gates. Tests must exercise public behavior or an injected boundary, not source regex, sleeps, assert.ok(true), or other green placeholders.

For Justifiqa TypeScript changes, normally require:

- relevant narrow tests;
- npm run test:phase2 or the applicable suite;
- application typecheck;
- a narrow test-file typecheck when application tsconfig excludes tests;
- lint;
- production build;
- git diff --check;
- secret/debug scan.

For Edge Functions, require handler tests and confirmation of JWT configuration. For SQL, require transactional regression tests, migration replay on a disposable clean database when relevant, and authorization/immutability checks.

When TypeScript exports or PostgreSQL objects change, require symbol-map generation and check according to repository instructions. Never generate maps from the user's dirty tree. Require a staged clean candidate/disposable snapshot; on Windows use git -c core.autocrlf=false archive to avoid CRLF false failures.

If sandbox restrictions cause spawn EPERM, permit rerunning the identical verification outside the sandbox. Never convert an infrastructure failure into a claimed pass.

## 7. Record DBB and DBS

For every implementation or correction batch, require:

- MarkDown/Batches/<BATCH>.md as DBB;
- MarkDown/Batches/<BATCH>_DBS.md as DBS.

DBB records the fixed point, inherited WIP, decisions, checkpoints, finding-to-fix-to-test matrix, actual commands/results, committed files, limitations, and Next Exact Action.

DBS teaches the software-engineering concepts applied by the batch in simple Indonesian, with small examples, direct file/symbol citations, and a mini-checklist or quiz.

Before external review, status must be READY FOR EXTERNAL RE-AUDIT, never self-certified PASS.

## 8. Require Two Review Axes

Before commit, require independent review of:

1. Spec/correctness — every requirement is implemented and behaviorally tested.
2. Standards/security — authorization, sensitive data, races, retries, idempotency, maintainability, generated artifacts, and scope hygiene.

Require all findings to be resolved and re-reviewed. External controller audit remains separate from executor self-review.

## 9. Prompt Output Contract

Return:

1. one self-contained <USER_REQUEST> block;
2. a hard preflight section;
3. locked technical decisions;
4. checkpoint/recovery protocol;
5. TDD and verification gates;
6. review, staging, and commit instructions;
7. hard-stop rules;
8. a mandatory final-report schema.

Keep instructions concrete and path-aware. Remove background that does not change executor behavior. Prefer a longer decision-complete prompt over a shorter ambiguous one, but avoid repeating the same rule in multiple sections.

Before delivery, audit the prompt for contradictory instructions, stale hashes, missing paths, invented contracts, irrelevant skills, incomplete verification, unsafe Git commands, and any way the executor could report success without proving it.
