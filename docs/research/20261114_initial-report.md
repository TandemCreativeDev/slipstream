# Slipstream: research report

14 November 2026. Research behind a shared project management framework for Tandem.

## Summary

Tandem already works in a consistent way across its recent projects; it has not been written down in one place. This report describes that way of working, compares it with the published methods it borrows from, and proposes a framework called Slipstream.

The main findings:

1. **Our tickets carry too much implementation detail.** The `/plan-tickets` skill asks for `file:line` evidence, a files list and developer notes in every ticket. The tickets we were happiest with this month have none of that: a one-line outcome, a requirements table, what is left out, and acceptance criteria. The technical detail belongs one level down, in tasks written at the start of a release.
2. **The Tranche 2 plan is a user story map.** Epics run across, releases cut down through them, each release ends with the client testing one stated outcome. This matches Jeff Patton's story mapping closely and is the right backbone for every project.
3. **The roadmap plugin is a dependency graph, not a backlog.** Status is computed, the next task is picked by how much it unblocks, and "in progress" is a claim by a named person. It fits the task level of the framework well and is already tracker-neutral in design.
4. **The planning horizon should be rolling.** Stories for the whole project up front, tasks only for the release about to start. This has a name in the literature (rolling-wave planning) and is what we already did by instinct.
5. **Two gaps in the current Tranche 2 set-up.** MoSCoW priorities are not recorded in the tracker (every story is priority Medium with no labels), and no story has been split into sub-tasks yet. Neither is urgent, but the framework should say where each lives.

The name comes from cycling, in keeping with Tandem: each accepted release clears the air for the next, and detailed planning rides just behind what has shipped.

## 1. What was reviewed

| Source | What it shows |
| --- | --- |
| `/plan-tickets` skill, its ticket template and worked examples | How we currently tell an agent to write tickets |
| Ticket instructions given during the Corbi planning sessions, 11 to 12 November 2026, and the tickets that resulted (#106 to #109) | How we want tickets to read |
| Tranche 2 planning deck, "How we'll run Tranche 2", 13 November 2026 | The epics, releases, loop and roles agreed with the client |
| Tranche 2 tracker project: 8 epics and the stories under them | Story format in practice |
| Story-map repository (`stories.json`, tracker sync and build scripts) | The data model behind the map: epics, releases, stories, estimates, dates, blockers |
| Roadmap plugin from goblin-mode: README, conventions, skills | How implementation work is divided and picked up |
| The Tranche 1C roadmap file | The plugin at scale: 19 milestones, 112 tasks |
| Published methods (section 4) | Where each practice comes from and what its authors recommend |

## 2. How we write tickets, and how we want to

### 2a. What `/plan-tickets` asks for

The skill turns a contract phase into parent and child tickets. Each child ticket has, in order: a paragraph on what exists today with `file:line` evidence, deliverables that name the pattern to follow, open questions, an indicative files list, acceptance criteria, dependencies, blockers, an estimate, and notes for the developer.

Its worked examples set the tone: "every sentence carries a number, a name or a `file:line`".

That density was right for the job the skill was written for: one technical person planning a contract extension on a codebase they know, for agents to implement.

### 2b. What we asked for instead

On 12 November, while writing tickets for a mixed team, the instruction was:

> keep the copy in it minimal, use BLUF, no fluff, prefer tables to explain things and dont get too technical or indicate exactly which files need to be edited. just outline requirements and acceptance criteria basically

Earlier the same day a ticket drafted from the standard template was questioned as "heavily embellished" next to a teammate's short original. The diagnosis at the time: the template had been applied by habit, and every agreed point had been restated to make it checkable. The short version, which pointed at the pull request where the points were agreed, scoped the work better and would not go stale as that discussion moved. The ticket was trimmed back to 665 characters.

The tickets written after that follow one shape:

```markdown
Chats are saved per session

Every message in a session is stored and can be read back in order,
so a conversation survives a restart.

## Requirements

| Need                  | Detail                                   |
| --------------------- | ---------------------------------------- |
| Store a message       | Role, content, optional tool calls       |
| Read the conversation | Oldest first                             |
| Unknown session       | 404                                      |

## Out of scope

The agent writing chats itself.

## Acceptance criteria

- Adding a message returns 201 and it then appears in the conversation (`api`).
- Messages are still there after the API restarts (`api`).
- Deleting the project leaves no chats behind (`db`).

## Depends on

#31.
```

The Tranche 2 stories reach the same place from the client's side. Each has a user story sentence, two or three sentences of context, acceptance criteria as named Given/When/Then scenarios, and a "Not included" list that names the release where each excluded item arrives. None names a file.

### 2c. The difference, side by side

| Aspect | `/plan-tickets` today | What we now want |
| --- | --- | --- |
| Opening | Current state of the code, with `file:line` | The outcome, in one or two sentences (BLUF) |
| Requirements | Deliverables list naming migrations, endpoints, patterns | A table of needs and their detail |
| Files | "Files (indicative)" section | None |
| Developer notes | "Notes for dev" section | None in the story; in the task if needed |
| Acceptance criteria | Present, among eight other sections | The centre of the ticket |
| Scope limits | Optional, mostly in spikes | Always: "Out of scope" or "Not included" |
| Reader | The implementer | The client, the team and the implementer |
| Title | Project code plus outcome | Outcome in plain words |

### 2d. Why less technical detail is better here

- **It goes stale.** A `file:line` reference is wrong after the next merge. A ticket written weeks before it is picked up describes a codebase that no longer exists.
- **Agents read the code anyway.** A coding agent investigates the repository when it starts a ticket, with the code as it is that day. Detail copied in at planning time duplicates that work with older facts.
- **The client reads the tracker.** A lesson from the Tranche 1 retrospective was that the work was "a black box" to the client. Stories they can read fix that; a files list does not help them.
- **Acceptance criteria are the contract.** They are what the reviewer checks and what the client signs off. Everything else in a ticket is commentary on them.

### 2e. What to keep from `/plan-tickets`

The skill's investigation discipline is sound and should survive. It moves, it does not disappear.

| Practice | Where it goes in Slipstream |
| --- | --- |
| Investigate the code before deciding what to build | Still done at planning time; findings go in the release plan, not the story |
| Criteria in pairs: "cannot do X" with "can still do Y" | Story acceptance criteria |
| Name the tool or method that proves each criterion | Story or task acceptance criteria |
| "Check production first" as the first criterion | Task acceptance criteria |
| Client hand-offs: who supplies what, in which format | Story "Blocked on" section and the release's list of client decisions |
| Spikes produce a decision and its evidence | Unchanged; this is the S in SPIDR |
| Estimates in days, in quarter-day steps, adding up to the budget | Unchanged |
| Security bugs filed separately, outside the budget | Unchanged |
| Exact names and counts ("all 14 public functions") | Task level |

## 3. What we already do

### 3a. The Tranche 2 plan

| Element | In Tranche 2 |
| --- | --- |
| Epics | 8, each named as something a person does ("Ask a question", "Check the evidence", "Run the service safely"), ordered as people do them |
| Releases | 5, each with dates, an estimate in days and a target outcome the client can test |
| Stories | 64 in the tracker export (the deck says 65), each under one epic and in one release, half a day to two days of work |
| Estimate | 94.5 days of stories plus 11.5 days not yet split, 106 build days |
| Splits | SPIDR sub-tasks inside a story, "as needed" |
| First release | Thin and end to end: it touches five of the eight epics |
| Client inputs | Listed per release as "decides first", with dates from the Statement of Work |

The release loop: plan the outcome and the client's decisions, build while the client follows the stories in the tracker, the client tests the outcome themselves, then either the outcome is accepted in writing or the stories are adjusted and building continues. Acceptance starts the warranty. The next release is re-planned and agreed by email.

The plan is read at two levels. The client reads epics, releases and stories. The team works in sub-tasks and pull requests. One person handles client communication; one acts as technical project manager, writing stories and setting priorities.

The changes it made after the Tranche 1 retrospective are worth keeping as principles: one testable outcome per release, data checked early with spikes for unknowns, a check against the funding bid every release, stories in plain language, and a definition of done backed by the client's own test sets.

### 3b. The roadmap plugin

The plugin, written by Jason as part of his goblin-mode configuration and developed through Tranche 1, holds implementation work as a dependency graph in one file in the repository.

| Concept | How it works |
| --- | --- |
| Phase | One contract tranche; old phases are archived |
| Milestone | A group of tasks with a one-sentence goal |
| Task | An id (`1ID.1`), a description and the ids it depends on |
| Status | Computed from the graph, never set by hand: `todo`, `blocked`, `paused`, `deferred`, `done`, `out_of_scope` |
| In progress | Not a status. A claim: a start date and a named person, asked for and never inferred |
| Ready set | Every unblocked, unclaimed task, ordered by how many other tasks finishing it would unblock |
| External gate | Something outside the team's control that blocks tasks, such as a client decision |
| Release tier | Core, Secondary, Tertiary: a later tier waits until every earlier one is done |
| Soft dependency | A link worth drawing that blocks nothing |
| Reconcile | An agent checks the code for work already finished and proposes status changes, with evidence, for approval |

Tranche 1C shows it at scale: 19 milestones, 112 tasks, 94 done, one external gate, 13 milestones in a later tier.

Three design choices matter for a wider framework:

- **Scripts do the part with one right answer.** A script computes status, the ready set and the diagram; the model only judges the script's output. The goblin-mode documentation calls this the deterministic-half pattern.
- **The tracker is a projection.** The file in the repository is the source of truth. A tracker sync mirrors it. The documentation describes the whole configuration as "tracker-agnostic", with the same four status transitions (branch checked out, pull request opened, merged, gone quiet) whichever tool is in use.
- **Nothing is marked done or created automatically.** The agent proposes; a person confirms.

### 3c. Where the roadmap plugin's ideas come from

The goblin-mode repository names no outside methodology. What it does state about its own origins:

- Every skill "traces back to a specific friction point"; nothing is built ahead of a problem.
- It was shaped to reduce task-initiation difficulty: its design notes describe "staring at the screen wondering what to do" as the problem the session tools solve.
- It replaced an earlier set of commands tied to one tracker, one of which was named `crit-path-to`. Generated roadmap files still open with a "Critical path" line.

The closest published relatives, by behaviour, are below. These are comparisons made for this report, not sources Jason cites, and are worth confirming with him.

| Roadmap plugin feature | Closest published idea | Origin |
| --- | --- | --- |
| Dependency graph, critical path line | Critical Path Method | Kelley and Walker, 1950s |
| Ready set ordered by tasks unblocked | Theory of Constraints: work on what releases the most flow | Goldratt, *The Goal*, 1984 |
| Pull the next task from a ready set, no assignment up front | Kanban pull systems | Anderson, 2010 |
| Git-held task graph, a `ready` command, explicit claims | Beads, an issue tracker built for coding agents | Yegge, 2025 |
| Core, Secondary, Tertiary tiers | MoSCoW: Must, Should, Could | DSDM, 1994 |
| External gates | Dependencies on the client, as a named node | Standard in critical-path planning |
| Script computes, model judges | Deterministic tools around a language model | Common agent-engineering practice |

Beads is the nearest match. It also keeps issues in the repository, links them with blocking dependencies, computes which are ready rather than asking the model to work it out, and has agents claim work before starting.

## 4. Published methods, and what we take from each

| Method | Core idea | What Slipstream takes | What it leaves |
| --- | --- | --- | --- |
| User story mapping (Patton, 2005 onwards) | Activities across the top in the order users do them; releases as horizontal slices | The whole structure: epics across, releases down | The workshop with sticky notes; an agent drafts the map from the contract documents |
| Walking skeleton (Cockburn) | The first slice is the thinnest thing that works end to end | Release 1 touches most epics thinly | Nothing |
| MoSCoW (DSDM, Clegg, 1994) | Must, Should, Could, Won't this time; time is fixed and scope flexes | Priority on every story, set per release | DSDM's wider process and roles |
| SPIDR (Cohn) | Split a large story by Spike, Path, Interface, Data or Rule | Used when a story is too big or has an unknown | Applying it to every story |
| INVEST (Wake, 2003) | A good story is independent, negotiable, valuable, estimable, small, testable | A checklist for reviewing stories | Nothing |
| Behaviour-driven development (North, 2006) | Acceptance criteria as Given/When/Then scenarios | Scenario format for stories the client reads | Tooling that executes the scenarios |
| Shape Up (Basecamp, 2019) | Decide how much time a problem is worth, fix the time, vary the scope; no sprints, no stand-ups | Fixed release dates with flexible scope; no ceremonies | Six-week cycles, the betting table, having no backlog at all |
| Kanban (Anderson, 2010) | Pull work when there is capacity; limit work in progress | Tasks are pulled from a ready set and claimed | Formal work-in-progress limits, for now |
| Rolling-wave planning (PMI) | Plan near-term work in detail and later work in outline | Stories for the project, tasks for the next release only | Nothing |
| Spec-driven development (2025 onwards) | Give an agent behaviour, constraints and acceptance criteria; let it choose the implementation | The story format in section 2 | Specifications as the permanent source of truth for the code |
| BLUF | State the conclusion first | Every story, every update to the client | Nothing |

Two points from the reading that sharpen our practice:

- **DSDM's guidance on proportions.** It recommends that Must-have work is no more than about 60% of a release's effort, with around 20% in Could-haves as contingency. If every story in a release is a Must, there is nothing to drop when something overruns and the date moves instead.
- **Spec-driven development is not criteria alone.** Writers on the subject are consistent that agents fill gaps with plausible defaults. A story still needs its constraints (use the existing roles, reuse the existing scope filter) stated in words. What it does not need is a list of files.

## 5. The proposed framework

### 5a. Principles

1. **Outcomes before tasks.** A release is defined by something the client can test, not by a list of work.
2. **Plain language above the line, technical detail below it.** The client reads stories. The team and its agents work in tasks.
3. **Plan in outline far ahead and in detail close up.** Stories for the whole project; tasks only for the release that is starting.
4. **Dates hold; scope flexes.** MoSCoW decides what gives way.
5. **Acceptance criteria are the contract.** If it is not in the criteria, it is not required. If it is, it is proven with real output.
6. **People steer; agents investigate and build.** Agents draft plans, break stories into tasks and write code. People set priorities, approve plans, and merge.
7. **No ceremonies.** No sprints, stand-ups, story points or velocity. The release review is the one fixed meeting.

### 5b. The hierarchy

| Level | What it is | Written by | Read by | Defined when | Size |
| --- | --- | --- | --- | --- | --- |
| Project | One contract or tranche | Signed scope | Everyone | At contract | Weeks to months |
| Epic | Something a person does, in plain words | Technical PM | Client and team | Project start | Spans releases |
| Release | A slice across the epics with one testable outcome and a date | Technical PM with the client | Client and team | Project start; re-planned after each review | Two to six weeks |
| Story | One thing a user can do, with acceptance criteria | Technical PM, drafted with an agent | Client and team | Project start, for all releases; refined at release start | Half a day to two days |
| Task | A technical step towards a story | Agent investigation, approved by a developer | Team and agents | Release start, for that release only | Up to a day |

A story belongs to one epic and one release. Releases are not a level in the tree; they cut across it.

Not every story needs tasks. A half-day story is its own task.

### 5c. Planning horizons

| When | What is planned | Output |
| --- | --- | --- |
| Project start | Epics, releases with outcomes and dates, every story with an estimate and a first MoSCoW priority | Story map; stories in the tracker |
| Release start | Stories refined against what the client found; MoSCoW set for this release; stories split where needed; tasks and their dependencies written from an investigation of the code as it is now | Task graph for the release; list of client decisions due |
| During the release | Tasks pulled from the ready set and claimed; stories adjusted with the client when something is learned | Pull requests; tracker kept current |
| Release end | The client tests the outcome; acceptance in writing or adjustment; next release re-planned | Acceptance record; changes to later stories |

Later releases keep their stories but not their tasks. Client feedback after a release is expected to change them, so detailed planning before then is wasted.

### 5d. Prioritising with MoSCoW

Set per release, on stories.

| Priority | Meaning | Guide |
| --- | --- | --- |
| Must | The release outcome fails without it | No more than about 60% of the release's days |
| Should | Important, and painful to leave out, but the outcome survives | |
| Could | Wanted if time allows; the first to go | About 20% of the release's days |
| Won't this time | Agreed as out for this release | Recorded in the story's "Not included", with the release where it lands if known |

When a release is at risk, Could-haves move to a later release first, then Should-haves, by agreement with the client. The date moves only if a Must cannot be met.

### 5e. Splitting with SPIDR

Split a story when it is estimated at more than two days, when it hides an unknown, or when part of it could ship earlier. Otherwise leave it alone.

| Split by | Use when | Example |
| --- | --- | --- |
| Spike | Something must be learned before the work can be estimated | Compare two retrieval approaches against a test set, then build the chosen one |
| Path | Users can take more than one route | Sign in with a password first; with a magic link later |
| Interface | The same thing is delivered through more than one surface | API endpoint first; page later |
| Data | Some kinds of data are harder than others | One country's reports first; the rest later |
| Rule | Some business rules can be relaxed at first | Count every report first; count each accident once later |

Split on one axis, then look again. A spike's output is a decision with its evidence, not code to keep.

### 5f. The story

```markdown
<Outcome in plain words>

<Who gets what, and why it matters. One or two sentences.>

## Requirements

| Need | Detail |
| ---- | ------ |

## Not included

- <Item>, <and the release where it arrives, if known>

## Acceptance criteria

- <Checkable outcome> (`<method>`)

## Depends on

<Stories that must land first>

## Blocked on

<Who supplies what, in which format, by when>
```

Fields held by the tracker, not the body: epic, release, MoSCoW priority, estimate in days, assignee.

Rules for acceptance criteria:

- Each is one observable outcome that a person or a command can check.
- Each names how it is proved. Tranche 2 uses named Given/When/Then scenarios, which suit stories a client reads. Corbi uses one-line criteria tagged with a method (`api`, `db`, `browser`, `cli`), which suit a technical team. Either is fine; pick one per project.
- A criterion that forbids something comes with one that shows the allowed path still works.
- Numbers the client must decide are marked as such ("within [time agreed with the client]") and listed among the release's client decisions.
- State constraints in words ("reuses the existing access roles"). Never list files.

Leave out any empty section except acceptance criteria.

### 5g. The task

Tasks are where the technical detail lives. They are written at the start of a release by an agent that reads the code as it is, and approved by a developer.

| Field | Content |
| --- | --- |
| Id and description | One sentence saying what changes |
| Story | The story it serves |
| Depends on | Other tasks, stories or external gates |
| Notes | What the investigation found: the pattern to follow, the thing to check in production first, exact names and counts |
| Acceptance | Only where the story's criteria do not already cover it |

How tasks are run, taken from the roadmap plugin:

- Status is computed from dependencies, not set by hand.
- The next task is pulled from the ready set, most-unblocking first.
- Starting a task is a claim by a named person. Nobody is assigned a start they did not make.
- A client decision the work waits on is an external gate, visible in the graph.
- An agent may propose that a task is done, with evidence. A person confirms.

### 5h. Tool mapping

The framework is defined by its concepts. Each tracker carries them differently.

| Concept | Jira | GitHub | Monday | Repository only |
| --- | --- | --- | --- | --- |
| Epic | Epic | Issue of type Epic, or a parent issue | Item on an epics board, or a column value | Section of the story-map file |
| Release | Fix version | Milestone | Group | Field on the story |
| Story | Story | Issue, as a sub-issue of the epic | Item | Markdown file or entry in the map file |
| Task | Sub-task | Sub-issue | Subitem | Entry in the roadmap file |
| MoSCoW | Label or custom field | Project field or label | Status column | Field |
| Estimate | Original estimate | Project number field | Numbers column | Field |
| Dependency | "Blocks" link | "Blocked by" relation, or a "Depends on" section | Dependency column | `dependsOn` |
| Claim | Assignee plus In Progress | Assignee | Person column | `started` and `assignee` |
| Client gate | Linked issue on the client | Issue with a "blocked on client" label | Item in a client group | External gate |

Whichever tool is used, one place is the source of truth for each level. Stories live in the tracker the client reads. Tasks may live in the same tracker or in the repository, with the other as a mirror.

### 5i. Roles

| Role | Does |
| --- | --- |
| Technical PM | Writes epics and stories, sets priorities with the client, runs release planning and review |
| Client lead | Owns communication with the client, chases client decisions, sends release updates |
| Developer | Approves task breakdowns and agent plans, reviews, merges |
| Coding agent | Investigates, drafts stories and tasks, implements, proves each criterion with real output |
| Client | Follows stories in the tracker, makes the decisions each release needs, tests the outcome, accepts in writing |

One person can hold more than one role.

## 6. Changes this implies for `/plan-tickets`

1. Replace the ticket template with the story template in 5f. Remove "Files (indicative)" and "Notes for dev".
2. Open every ticket with the outcome. Move the current-state paragraph and its `file:line` evidence to the plan document.
3. Add epic, release and MoSCoW as outputs, mapped per destination as in 5h.
4. Keep the investigation step, the questions step and the budget arithmetic as they are.
5. Add a second mode, or a second skill, that runs at release start: it reads the release's stories, investigates the code, and writes the tasks and their dependencies.
6. Keep the guidance on paired criteria, proving methods, spikes, client hand-offs and separate security bugs.
7. Add a size rule: a story over two days is split with SPIDR or justified.

## 7. Open questions

1. **Where do tasks live?** In the tracker as sub-tasks, in the repository as a roadmap file with the tracker as a mirror, or either per project. The roadmap plugin assumes the repository.
2. **Where is MoSCoW recorded in Tranche 2?** It is in the method but not yet in the tracker.
3. **One acceptance-criteria format or two?** Scenarios for client-read stories and tagged one-liners for technical teams, or one format everywhere.
4. **Does every project use the roadmap plugin?** It needs Python and Claude Code. A team on other tools needs the same concepts by another route.
5. **Jason's own account of his influences.** Section 3c is inferred from behaviour.
6. **How much of this is public?** The framework repository is intended for Tandem's GitHub page. This report names client projects and should be anonymised, or kept out of the public repository, before then.

## 8. Sources

Internal:

- `/plan-tickets` skill: `SKILL.md`, `ticket-template.md`, `examples.md`
- Corbi issues #83, #106, #107 and the session instructions of 11 to 12 November 2026
- "How we'll run Tranche 2" deck, 13 November 2026
- Tranche 2 tracker project and story-map export, 11 November 2026
- Roadmap plugin: `README.md`, `references/roadmap-conventions.md`, skills; goblin-mode `docs/architecture.md`, `docs/guides/building-your-own.md`, `docs/reference/task-trackers/README.md`, `docs/design-history/agent-workflow-design.md`

Published:

- Jeff Patton, *User Story Mapping* (2014), and "The New User Story Backlog is a Map" (2008): <https://jpattonassociates.com/the-new-backlog/>
- Mike Cohn, "Five Simple but Powerful Ways to Split User Stories" (SPIDR): <https://www.mountaingoatsoftware.com/blog/five-simple-but-powerful-ways-to-split-user-stories>
- Agile Business Consortium, DSDM handbook, MoSCoW prioritisation: <https://www.agilebusiness.org/dsdm-project-framework/moscow-prioririsation.html>
- Bill Wake, "INVEST in Good Stories, and SMART Tasks" (2003): <https://xp123.com/invest-in-good-stories-and-smart-tasks/>
- Dan North, "Introducing BDD" (2006): <https://dannorth.net/introducing-bdd/>
- Ryan Singer, *Shape Up* (Basecamp, 2019): <https://basecamp.com/shapeup>
- David J. Anderson, *Kanban: Successful Evolutionary Change for Your Technology Business* (2010)
- Eliyahu Goldratt, *The Goal* (1984)
- Project Management Institute, *PMBOK Guide*: rolling-wave planning and progressive elaboration
- Steve Yegge, Beads: <https://github.com/steveyegge/beads>
- Birgitta Böckeler, "Understanding Spec-Driven Development" (Thoughtworks / martinfowler.com, 2025): <https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html>
- Jason Warren, goblin-mode: <https://github.com/JasonWarrenUK/goblin-mode>
