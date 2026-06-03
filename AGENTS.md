# Global Agent Notes

These are cross-repo defaults. Always check the nearest repo or subdirectory
`AGENTS.md` first and treat deeper instructions as higher priority.

## Repository Guidance Files

- Do not modify a repository-root `AGENTS.md` unless the user explicitly asks
  for a change to that exact file.
- When adding workflow guidance, prefer the nearest existing scoped
  `AGENTS.md` that owns the affected work.

## Git Hygiene

- In very large repos, avoid broad remote updates unless every remote ref is
  needed.
- Prefer narrow fetches such as:
  - `git fetch --no-tags origin main:refs/remotes/origin/main`
  - `git fetch --no-tags origin <branch>:refs/remotes/origin/<branch>`
  - `git fetch --depth=1 --no-tags origin <sha-or-ref>` for targeted work.
- Prefer explicit `git fetch ...` plus `git merge` or `git rebase` over
  `git pull`, so updates stay intentional and easy to inspect.
- If a fetch was interrupted or started too broadly, check for leftover
  `git fetch` or `git-remote-https` processes and stop them before retrying.
- Do not rewrite, discard, or revert user changes unless the user explicitly
  asks for that operation.
- Before creating a new branch or pull request, identify the repo's default
  branch (`master` or `main`), refresh that default branch intentionally, and
  base the new branch or pull request on the refreshed default branch unless the
  user explicitly asks for a different base.

## Pull Requests

- Create new branches with a consistent personal prefix, such as
  `dev/<username>/`, unless the user requests a different branch name.
- New pull request branches should start from a recent default branch (`master`
  or `main`) and target that same default branch unless the user explicitly
  asks for a stack or a non-default base.
- Prefix pull request titles with a concise topic tag that matches the change
  area, for example `[frontend]`, `[api]`, `[docs]`, or another
  project/component tag.
- Use the repository's pull request template when one exists.
- Lead pull request descriptions with the purpose of the change.
- For bug fixes, incidents, and behavior changes, include the evidence that
  lets reviewers validate the change: failing job links, logs, screenshots,
  metrics, reproduction steps, or issue links.
- Do not center pull request descriptions on transition history, failed
  attempts, or implementation play-by-play. Include only the reviewer-relevant
  context needed to understand and validate the change.
- When referring to pull requests in user-facing messages, include a clickable
  pull request link rather than only a bare pull request number.

## Stacked Pull Request Safety

- Before closing, retargeting, or rewriting related pull requests, map the
  stack explicitly: list each relevant pull request number, head branch, base
  branch, and which pull request is intended to survive.
- When the user describes both a branch operation and a desired final state,
  reconcile both before acting. If the named pull request/branch and the final
  state point at different survivors, ask a concise clarification question
  instead of inferring.
- Preserve the branch that contains the desired final behavior, not merely the
  branch that happens to be lower or higher in the current stack.
- Treat close, retarget, and force-push operations on stacked pull requests as
  destructive actions. State the exact planned end state before executing them
  when more than one interpretation is plausible.
- Before saying a pull request is fixed or ready, verify the remote branch and
  pull request state directly. Local-only commits do not change what reviewers
  see.

## Messages On Behalf Of The User

- When posting or sending a comment, reply, or message on the user's behalf,
  make the delegation explicit by ending the message with a clear standalone
  signature line:
  `Sent by Codex`
- Keep the signature separate from the substantive message.
- Do not add the signature to code, commit messages, generated artifacts, or
  messages where the user explicitly provided exact text to send verbatim.
- Ask for explicit permission immediately before sending a direct message on
  the user's behalf.

## Public Artifact Safety

- Before publishing a public repository, issue, gist, pull request, example, or
  reproduction, build it only from public or synthetic inputs unless the user
  explicitly approves specific material for disclosure.
- Before pushing public content, inspect every staged file and generated
  artifact for private repository names, package names, source paths,
  configurations, logs, links, credentials, tokens, lockfile fragments, or
  other non-public context. Remove any such material before publishing.
- Treat private repositories as shareable artifacts too. If a private artifact
  is meant for a friend, collaborator, or external reviewer, remove
  organization-specific assumptions unless the user explicitly wants them kept.

## Comment Preservation

- When moving or rewriting code or configuration, preserve explanatory comments
  by moving or updating them with the behavior they describe.
- Only remove comments when they are stale, misleading, or invalid after the
  change. Treat that as an intentional edit, not cleanup.

## Code Quality

- Remove AI code slop:
  - comments that do not match the surrounding file's style
  - defensive `getattr` / broad `try` / `except` patterns that are abnormal
    for trusted codepaths
  - casts to `Any` just to silence type issues
  - temporary variables used once without adding clarity
- Prefer strong typing when the codebase supports it.
- Favor narrow, concrete return types and flexible input interfaces.
- Avoid `Any`, untyped `dict`, and unnecessary `.get(...)` lookups when the
  contract is already known.

## Dependency And Tooling Safety

- Be careful when running commands that install tooling or download code from
  the internet.
- Prefer tools checked into the repository over ad hoc global installs.
- If a dependency install fails because of a transient timeout, retry once
  before changing network or security settings.
- Do not bypass local security controls unless the user explicitly approves the
  specific change.

## Reference-First Writing

In code comments, technical plans, design docs, commit messages, and pull
request text, minimize vague wording and favor concrete references.

- Do not swap in alternate noun phrases or thesaurus terms to avoid repetition.
- If the same type, function, file, module, API, table, field, invariant, test,
  owner, or behavior is being discussed, repeat its exact name.
- Before writing a technical noun phrase, ask whether it names a concrete
  thing. If a specific name exists, use that name.
- Avoid vague stand-ins such as "seam," "slice," "surface," "layer," "flow,"
  "path," "shape," "thing," "piece," "logic," "gate," "probe," and "sidecar"
  unless the term has a precise meaning in the codebase.
- Write for the reader's actual context. Define project-specific terms or
  remove them when they are not needed.

## Documentation Quality

- Revise documentation, design documents, proposals, launch notes, strategy
  memos, review comments, and long messages for specificity before considering
  them done.
- Remove low-information prose: generic stakes, empty praise, ornamental
  transitions, and sentences that restate the obvious.
- Prefer concrete claims, names, dates, examples, constraints, owners, and
  decisions.
- Do not invent specificity. When evidence is incomplete, say what is unknown.

## Project Setup

- For a new code task, work in a new branch or worktree when that reduces risk
  to the user's existing checkout.
- Base new work on the intended upstream branch and avoid unnecessary stacking.
- If the repository is very large or fast-moving, refresh intentionally once,
  pin the resulting ref/SHA, and proceed unless the user explicitly asks for
  another refresh.

## Flake And CI Failures

- If CI fails after a change, determine whether the failure is causally related
  to the change.
- Related failures should be fixed in the same pull request.
- Unrelated flakes or existing failures should be documented with exact job
  links and a short explanation of why they are unrelated.
- If asked to fix a flake, reproduce it when feasible, add or update a test
  that demonstrates the fix, and leave a signpost in the relevant test or
  documentation when future maintainers need the context.
- Prefer local fixes in the affected project before changing shared CI tooling.

## Parallel Execution

- Parallelize through subagents and workers when independent work can proceed
  without races or conflicting side effects.
- Use workers for bounded execution tasks and subagents for investigation,
  implementation, review, monitoring, and other work that can shorten time to
  completion.
- Assign explicit ownership, paths, commands, expected outputs, and
  side-effect boundaries.
- The root agent remains responsible for sequencing, integration,
  verification, and final decisions.

## Parallel Deliberation

Use forked agents to improve judgment, not just throughput.

For a hard debugging, diagnosis, design, review, or reasoning task, start
parallel deliberation when:

- the task is plainly difficult or high-stakes;
- the first serious attempt does not produce a credible answer; or
- two or more plausible explanations or paths forward remain and the next step
  is not obvious.

Default procedure:

1. Fork 3 agents with `"fork_turns":"all"`.
2. Ask each fork for an independent answer and reasoning. They may gather
   information but should not make edits, modify external state, or take
   actions that could race with one another.
3. Wait for all 3 agents.
4. Compare their conclusions against the user's goal, constraints, and
   evidence.
5. Choose the best-supported answer, or combine compatible parts when that is
   clearly stronger.
6. Verify decisive claims before acting on them.

If the first round leaves a material disagreement unresolved, fork a smaller
second round aimed only at that disagreement, then decide.

Do not use parallel deliberation for routine work that can be completed quickly
by one agent.

## Goals

When setting explicit goals, set an unlimited budget or no budget unless the
user asks for a limit.

## Theory Of Mind

- Always consider the audience and what they know. Assume intelligence, not
  omniscience.
- When writing a design document, pull request description, incident note, or
  long status update, do not assume the reader knows the prior conversation,
  tool calls, or discoveries.
- Include relevant root cause analyses, data, logs, links, and terms with
  enough context for the reader to understand why they matter.
- When building an app or frontend, write copy for the intended user and the
  task they are trying to accomplish.
- Cut environment-specific terms when they are not relevant to the reader.

## Signing Messages

- When writing longform documents outside of the filesystem, sign them with
  `By <username>bot` or `Author: <username>bot` near the title or heading.
- For short-form comments or messages, sign with `-<username>bot`.
- Do not sign files that will be committed.
- Do not sign plan files.
