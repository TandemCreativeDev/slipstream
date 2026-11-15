# Tandem tooling: design for review

15 November 2026. A proposal to mark up before anything is built.

## Summary

The method is backed by two kinds of tooling. Packages hold everything with one right answer and run anywhere. Plugins hold the judgement and run in Claude Code.

| Layer | Form | Runs in | Holds |
| --- | --- | --- | --- |
| Method | Document | Nowhere | The written framework, tool-neutral |
| Engine, adapters, actions | npm packages | Terminal, git hooks, CI, any coding agent | Status, the ready set, claims, tracker updates, project checks |
| Skills | Claude Code plugins | Claude Code | Wording, splitting, direction, review |

Three plugins, in one marketplace named `tandem`:

| Plugin | For | Does |
| --- | --- | --- |
| `slipstream` | Whoever plans a project | Draws up the stories and the task graph for each release |
| `dev` | Every developer | Shows what is ready, records claims, makes small edits to the graph, and covers commit, pull request, review and decision records |
| `docs` | Anyone | House-style documents |

A developer needs `dev` only. Neither plugin depends on the other; both call the same package.

Everything works against Jira, GitHub or markdown files in the repository, through one adapter per tracker. Monday is parked.

## 1. Repositories

| Repository | Holds | Visibility |
| --- | --- | --- |
| `slipstream` | The method and its explainer | Could be public |
| Packages | The engine, the adapters, the sync action and small project tools, published as scoped npm packages | Public, so any project in any organisation can install and call them |
| `tandem` marketplace | The three plugins | Private |

## 2. The package

`@tandemcreativedev/slipstream`: one core that speaks to every tracker in the same language, offered three ways.

| Surface | Used by |
| --- | --- |
| Command line, every command able to print JSON | People, git hooks, CI |
| Library | The plugins and other packages |
| MCP server | Any coding agent, in any tool, with no plugin installed |

All three are the same operations over the same core. An agent asking for the ready set gets the same answer whichever tracker the project uses, and never calls a tracker's own interface.

| Command | Does |
| --- | --- |
| `init` | Writes the project's configuration file and the workflow that calls the sync action, and lists the secrets to add |
| `ready [--for NAME]` | Lists ready tasks, with what each unblocks and whose story it is in |
| `claim ID --by NAME`, `release ID`, `done ID` | Records a start, drops it, or finishes a task |
| `add`, `split`, `link`, `unlink` | Edits the graph |
| `status` | Each story's state, read off its tasks |
| `validate` | Finds cycles, links to missing items and stories with no owner |
| `graph` | Draws the graph |
| `sync` | Applies a repository event to the tracker (see section 4) |

Rules it enforces:

- Status is computed from links and is never set by hand.
- A claim always names a person; a name is never inferred.
- A client decision blocks until the technical PM settles it.
- Finishing a task is confirmed by a person.

There is one implementation of these rules. Anything else that shows task status, such as a personal dashboard, reads this package's output instead of working status out again. Two programs computing status separately would disagree in edge cases.

## 3. Tracker adapters

The tracker is the single source of truth. The package reads through an adapter, computes, and writes back. Nothing is kept in a second copy, so nothing has to be synchronised.

Each tracker has its own interface. The adapter is the only place that knows it; everything above the adapter speaks in stories, tasks, links, claims and owners.

Each adapter implements the same operations:

| Operation | Meaning |
| --- | --- |
| Read items | Epics, stories and tasks with their fields |
| Write item | Create or update one |
| Read and write links | Which task depends on which |
| Read and write claim | Who started a task and when |
| Read and write owner | Who owns a story |
| Comment | Post a proposal on a story |

Each adapter declares what its tracker supports. Where a tracker has no real dependency link, the adapter falls back to reading and writing the "Depends on" section of the ticket.

| Concept | Jira | GitHub | Markdown |
| --- | --- | --- | --- |
| Epic | Epic | Parent issue | Folder with `epic.md` |
| Story | Story | Sub-issue of the epic | Folder with `story.md` |
| Task | Subtask | Sub-issue of the story | One file in the story's folder |
| Link | "Blocks" link | "Blocked by" relation | Frontmatter list of ids |
| Release | Fix version | Milestone | Frontmatter field on the story |
| Priority | Label or field | Project field | Frontmatter field |
| Owner | Custom field | Project field | Frontmatter field |
| Claim | Assignee and status | Assignee | Frontmatter fields |
| Client decision | Linked issue | Labelled issue | One file |

A project names its adapter, and how its fields are named, in one configuration file in the repository. Adding a tracker means writing one adapter.

What each tracker supports, as checked on 15 November 2026:

| Tracker | Dependency links | Tasks under stories | Checked how |
| --- | --- | --- | --- |
| Jira | Yes: a "Blocks" link type | Yes: Subtask type | Read from a live Jira site in use |
| GitHub | Yes: "blocked by" relation | Yes: sub-issues | Called on a live repository |
| Markdown | Yes: by design | Yes: by design | Not applicable |

### Markdown layout

Every task is its own file. Epics and stories are folders. A story's release is a field, because releases cut across epics.

```text
tickets/
  <epic>/
    epic.md
    <story>/
      story.md
      <task>.md
      <task>.md
  decisions/
    <client decision>.md
  archive/
    <release>/
      <epic>/
        <story>/
```

When a release is accepted, its stories move, with their tasks, to `archive/<release>/`. The live tree then holds only work that is still ahead.

### Monday: parked

No Monday adapter is planned. It is written only if a project uses Monday again.

What is known, should that happen: Monday has a dependency column that its interface can read and write. Whether it works on subitems, and on which pricing plans, is not confirmed. An earlier project ran a working Monday sync that moved an item's status as its pull request progressed; its rules are folded into section 4.

## 4. Keeping the tracker current automatically

A package cannot run a workflow by itself: an event only fires a workflow file in the repository where it happens. So the work is split three ways.

| Part | Where | Size |
| --- | --- | --- |
| The logic | `sync` command in the package | All of it |
| A reusable workflow | Packages repository, public | Installs the package and runs `sync` |
| The trigger | Each project, written by `init` | About five lines and a version pin |

What `sync` does with each event:

| Event | Effect on the tracker |
| --- | --- |
| Branch pushed whose name carries a ticket id | Task shown as in progress. A start only: closing a ticket still needs the explicit `Closes` line below |
| Pull request opened | Task shown as in review |
| Pull request merged | Task proposed as done; newly unblocked tasks become ready |
| Branch with no recent commits | Claim flagged as stale; nothing changed |

The same command runs by hand when CI is unavailable.

Each project supplies its tracker token as a secret. Runner time is billed to the project's repository.

Rules carried over from two tracker syncs already run on real projects:

| Rule | Why |
| --- | --- |
| A pull request links to a ticket only by an explicit `Closes <id>` line in its title or description | A branch name or a passing mention is not an instruction |
| An id inside backticks or a code block is ignored | Documentation describing the syntax once moved live tickets |
| A pull request with no ticket fails a check, with one comment saying how to fix it; a `no-ticket` label clears it | Chores and CI changes have no ticket, and should not need an invented one |
| A status the sync does not recognise was set by a person and is left alone, with a warning | The board is the client's view; a person's move outranks the automation |
| Runs for the same pull request queue, never race | Two runs writing one ticket corrupt it |
| A repository variable switches the sync between off, dry run and apply | A new project watches a dry run before trusting it |
| A ticket that no longer matches anything is reported, never deleted | Deleting is not the automation's call |
| Bot pull requests are skipped | Dependency updates have no ticket |

One of those syncs mirrored a file in the repository onto the tracker, and most of its size went on working out what had changed. With the tracker as the single source of truth that problem disappears: `sync` reacts to an event and writes one change.

## 5. Small project tools

Checks that more than one project needs become their own small package, each with a configuration file, an `init` and a `--check` that fails CI when something is out of date.

A check that runs in CI or a git hook is a package. A guard on what a coding agent may run is a Claude Code hook, so it ships in the `dev` plugin.

Candidates, read in a project that uses them today:

| Tool | Does | General enough | Ships as |
| --- | --- | --- | --- |
| Migration restamp | Renames a branch's new migrations so they sort after the newest on the main branch; `--check` fails CI when a rename is needed | Yes, for any project whose host applies migrations in merge order | Package |
| Documentation link check | Every relative link resolves and every page is reachable from the root README | Yes; one house rule about README depth becomes an option | Package |
| Tracker sync | Section 4 | Yes | Package |
| Branch gate | Refuses a new branch whose name does not follow the convention below | Yes; ships with the skill it guards | Hook in `dev` |
| Commit gate | Sends every agent commit through the `commit` skill; refuses staging everything at once, and staging and committing in one command | Yes; ships with the skill it guards. Written for one shell, so it needs making portable | Hook in `dev` |
| No AI attribution | Refuses a commit message that credits an AI tool | Yes, as it is | Hook in `dev` |
| Database reset guard | Never resets a hosted database; asks before resetting a shared local one | Yes, for projects on the same database platform | Hook in `dev` |
| Default-branch guard | Refuses feature commits on the main branch; asks before a push that would trigger a release | Partly; its exceptions are specific to one project and need to become configuration | Hook in `dev`, later |

A tool is lifted out of a project when a second project needs it, never before. Each is separate, so a project installs only what it uses.

### Branch names

With a ticket: `<type>/<ticket id>-<short description>`. Without one: `<type>/<short description>-<DDMMYYYY>`. All lower case, words joined by hyphens.

| Part | Rule | Example |
| --- | --- | --- |
| Type | The same list as commit types: `feat`, `fix`, `refactor`, `perf`, `docs`, `test`, `chore`, `build`, `ci` | `feat` |
| Ticket id | The task's id as the tracker gives it. Required for `feat`, `fix`, `refactor` and `perf`; optional for the rest, which may have no ticket | `t2dv-74`, `107` |
| Description | A few words from the task's title | `cancel-endpoint` |
| Date | Only when there is no ticket id: the day the branch is cut, as `DDMMYYYY`, so the name is unique | `15112026` |

`feat/t2dv-74-cancel-endpoint`, `fix/107-refund-rounding`, `chore/upgrade-dependencies-15112026`.

The ticket id format is set per project in its configuration file, since each tracker numbers differently. The gate checks the name only when a coding agent creates the branch; it does not rename or reject branches that already exist.

## 6. Who may change what

The technical PM draws the graph. Developers adjust it inside a story. Changes that cross stories go back to the technical PM.

| Change | `slipstream` | `dev` |
| --- | --- | --- |
| Create epics, releases, stories | Yes | No |
| Change a story's criteria, priority or release | Yes | No |
| Create the task graph for a release | Yes | No |
| Name or change a story's owner | Yes | No |
| Add or settle a client decision | Yes | No |
| Add a task inside a story | Yes | Yes |
| Split a task | Yes | Yes |
| Add or remove a link between tasks in one story | Yes | Yes, story owner |
| Add or remove a link between tasks in different stories | Yes | Proposes only |
| Claim, release, finish a task | | Yes |

Where `dev` may not make a change, it writes the proposal as a comment on the story for the technical PM and makes no edit. This is a working agreement the skills follow, not access control.

## 7. Skills

Skills are thin. Each calls the package and applies judgement to what comes back.

### `slipstream`

| Skill | Input | Output |
| --- | --- | --- |
| `map` | Contract documents and scope | Epics, releases with outcomes and dates, and every story with an estimate and a first priority, shown for approval and then written to the tracker |
| `story` | An idea, or an existing story | One story in the standard format: outcome, requirements table, not included, acceptance criteria, depends on, blocked on |
| `split` | A story over two days or hiding an unknown | Two or more stories, split on one axis, with the reason |
| `plan-release` | A release whose stories are signed off | Tasks, links across stories, client decisions and owners, drafted from the code as it is, corrected by the technical PM, then written |
| `review-release` | A release ready for the client | The outcome and each story's criteria laid out as a test script for the client, then a record of the result |
| `replan` | What the client found | Proposed changes to later stories and releases, for approval |

### `dev`

| Skill | Input | Output |
| --- | --- | --- |
| `ready` | Nothing, or a developer's name | The ready set: own stories first, then others, each by how much it unblocks |
| `claim` | A task | The task marked as started by a named person, or the claim dropped |
| `task` | A story and a change | A task added or split, or a link changed, within the limits in section 6 |
| `branch` | A task, usually the one just claimed | A branch named `<type>/<ticket id>-<short description>`, cut from the main branch, or stacked on the branch of a dependency that has not merged yet. Paired with the branch gate hook in section 5 |
| `commit` | Staged or pending changes | Single-line commits, split by concern, each staged by exact path and checked before it is made. Paired with the commit gate hook in section 5 |
| `pr` | A finished branch | A pull request whose body proves each acceptance criterion with real output |
| `review` | A pull request or branch | Findings on content, each probed before it is reported |
| `address-review` | Unresolved review comments | Each claim reproduced, fixed and proved, one at a time |
| `stack` | Multi-part work | Dependent branches and pull requests |
| `adr` | A material, non-obvious decision | A numbered decision record in the repository |

### `docs`

House-style documents: proposal, statement of work, report. Carried over as they are.

## 8. Extending it

| Extension point | Use |
| --- | --- |
| The package's JSON output | Personal dashboards, panes, status lines and session tools read `ready`, `status` and `graph` |
| A new adapter | Any other store of tasks, including an existing roadmap file in a repository, can be a backend |
| Wrapping a skill | Skills are namespaced by plugin, so a personal skill can call a shared one and add to it without a name clash |

The existing roadmap plugin fits as both: its file format becomes an adapter, and its dashboard and ready pane read the package's output.

What stays personal: voice and persona, output styles, hooks that prompt or nag, stack-specific helpers, permissions.

## 9. Rules for what goes in

- A skill is shared when its output lands in the tracker, the repository or in front of a client.
- A skill or tool is promoted once two people or two projects use it. Nothing is added on speculation.
- Anything with one right answer goes in a package, not a skill.
- Shared hooks are rare. One that blocks work needs everyone's agreement.
- Shared skills write in a neutral voice.
- Each package and plugin has one owner. Changes arrive by pull request, with a changelog entry.

## 10. Decisions

| Decision | Settled as |
| --- | --- |
| Source of truth | The tracker. No second copy, no cache |
| Engine | Built out from the existing roadmap script by its author, as a piece of work in its own right. Its rules are the starting point; the language is his to propose |
| Names | Marketplace `tandem`; plugins `slipstream`, `dev`, `docs` |
| Who uses what | `slipstream` for whoever plans; `dev` for every developer |
| Trackers | Jira and GitHub first, then markdown. Monday parked |
| Markdown layout | One file per task, folders for epics and stories, archived by release |
| Acceptance criteria style | Scenarios or tagged one-liners, chosen per project in its configuration file |
| Tracker credentials | A set-up step per project: a secret in the repository, listed by `init` |

Not planned: a hosted service that updates trackers with nothing in each repository. It would hold credentials for every client and is not needed at this size.

Nothing open blocks a start.
