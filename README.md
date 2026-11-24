# Slipstream

How Tandem plans and delivers projects.

A project is cut into releases. Each release gives the client one outcome they can test themselves. Stories are written for the whole project up front. The story is the unit of work: it is what waits on other work, what is ready to start, and what the client tests as it lands. Each story's technical tasks are written only when its release starts, or when the story is started. What the contract promises and the budget both hold; how far each outcome is taken is what flexes. There are no sprints, stand-ups, story points or velocity.

The name is from cycling: each accepted release clears the air for the next, and detailed planning rides just behind what has shipped.

## Principles

1. **Outcomes before tasks.** A release is defined by something the client can test, not by a list of work.
2. **Finish stories one at a time.** A developer starts a story, works through its tasks, with help where someone is free, and finishes it. Stories land whole, so the client can test each as it lands and effort does not spread across many half-done stories.
3. **Plain language above the line, technical detail below it.** The client reads stories. The team and its coding agents work in each story's tasks.
4. **Outline far ahead, detail close up.** Stories for the project; tasks for the next release only.
5. **Outcomes and budget hold; depth flexes.** What the contract promises is delivered within the days agreed. How far each outcome is taken is what gives way. Dates are targets.
6. **Acceptance criteria are the contract.** What is in them is required and proven. What is not in them is not required.
7. **People steer; agents investigate and build.** Agents draft each story's tasks and write code. People set priorities, approve plans and merge.
8. **Decisions are written down once.** A material, non-obvious decision gets a decision record.

## Structure

| Level | What it is | Read by | Written when | Size |
| --- | --- | --- | --- | --- |
| Epic | Something a person does, in plain words | Client and team | Project start | Spans releases |
| Release | A slice across the epics with one testable outcome, a budget in days and a target date | Client and team | At contract; re-planned after each review | Two to six weeks |
| Story | One thing a user can do, with acceptance criteria. Has one assignee. The smallest unit that is blocked or ready | Client and team | Project start, for every release | Half a day to two days |
| Task | A piece of technical work inside a story, like a Jira subtask, with its own optional assignee and status | Team and agents | Release start, or when the story is started | None of its own |

Epics run across; releases cut down through them. A story belongs to one epic and one release. The first release is thin and end to end: it touches most epics lightly so the whole thing works before any part is deep.

A small story needs no tasks.

### Commitments

The contract does not list stories. It promises outcomes, and it sets the releases with their budgets. Each promised outcome is a commitment.

Commitments are not a level in the table above. They cut across it: one commitment is usually met by several stories, sometimes in more than one epic or release. Every commitment traces to at least one Must story, and that link is checked when each release is planned.

## The release loop

| Step | What happens |
| --- | --- |
| Plan | Agree the outcome and the decisions the client must make. Refine the release's stories, set priorities, draw the dependencies between stories and inputs |
| Build | Pull stories from the ready set. The client follows the same stories in the tracker |
| Review | The client tests the outcome themselves |
| Accept or adjust | The outcome is accepted in writing, or the stories are adjusted and building continues |
| Re-plan | Later releases are reshaped by what the client found |

The release review is the only fixed meeting.

## Priorities

Every story has a MoSCoW priority, agreed with the client before its release starts.

| Priority | Meaning |
| --- | --- |
| Must | Needed to meet a commitment at its simplest. Committed |
| Should | Depth on a commitment. Painful to leave out, but the promise is still kept |
| Could | Depth that is wanted if the days allow. First to go |
| Won't this time | Agreed as out of this release |

### Core and depth

A story that bundles a promise with its refinements cannot be prioritised: all of it becomes a Must. So stories are cut in two kinds.

- **Core:** the commitment at its simplest. A Must.
- **Depth:** everything that takes it further. Separate Should or Could stories.

A story's "Not included" list is the first place to look for depth stories. Cut this way, every release holds something that can be removed without breaking the contract. A release that is all Musts has nothing to give.

### What gives way, in order

A release has a budget in days. The budget is what a release is measured against, not its date: a release estimated at 20 days is on track while it has used 20 days or fewer, whenever it lands.

When a release is forecast to exceed its days:

1. Could-haves are removed.
2. Should-haves are removed.
3. Contingency is drawn, on notice to the client. Contingency is a separate allowance of days in the contract, outside every release's budget.
4. The fee is raised, only by written agreement.

Removed means removed. A removed story may return in a later release or a later contract, but that is never assumed.

Dates are targets. A date moves by agreement when the calendar is the problem: a client decision arrives late, or people are unavailable. Moving a date does not create days, so it never fixes an overrun in effort. Where a client does have a date that matters more than the budget, say so in the contract and plan that release against the date.

## Stories

A story states the outcome first and says nothing about files or implementation.

Use the [story template](templates/story.md). See a [worked example](examples/story.md).

Leave out any empty section except acceptance criteria. Epic, release, priority, estimate (in days), assignee and status are fields in the tracker, not text in the body.

"Depends on" names the stories and inputs that must be done first. The release's dependency graph is built from it.

Acceptance criteria:

- Each is one observable outcome that a person or a command can check.
- They are numbered so the story's tasks can cite them.
- Each says how it is proved: a Given/When/Then scenario, or a one-line criterion tagged with its method (`api`, `db`, `browser`, `cli`). Pick one style per project.
- A criterion that forbids something is paired with one showing the allowed path still works.
- A figure the client must decide is marked as theirs and listed among the release's client decisions.
- Constraints are stated in words ("reuses the existing access roles").

## Splitting stories

Split a story when it is over two days, hides an unknown, or part of it could ship earlier. Otherwise leave it whole. SPIDR gives five ways:

| Split by | Use when |
| --- | --- |
| Spike | Something must be learned before the work can be estimated |
| Path | Users can take more than one route |
| Interface | The same thing is delivered through more than one surface |
| Data | Some kinds of data are harder than others |
| Rule | Some business rules can be relaxed at first |

Split on one axis, then look again. A spike produces a decision and its evidence, not code to keep.

Split, too, when another story needs only part of this one. Dependencies run between whole stories, never between tasks, so the part that is needed becomes a story of its own.

## The story is the unit of work

A story is what depends on other work, what is blocked or ready, what is pulled from the ready set and what is confirmed done. Its assignee starts it, works through its tasks and finishes it, with help from anyone who is free. The team finishes stories one at a time, and the client can test each one as it lands.

Tasks remain, inside a story, and work like Jira subtasks.

### What this gives up

- **A dependency waits for a whole story.** A story that needs only the first half of another waits for all of it, unless that story is split.
- **Readiness and dependencies only work at story level.** Two people can work on separate parts of one story by assigning themselves its tasks, but no task is ever blocked or ready, so nothing points anyone at a task: a developer who is free looks inside the stories already in progress.

Both cost little, because no story is over two days.

## Tasks

A story's tasks are an optional list of the technical work inside it. Each names a change and the story's criteria it proves. A task is a working note for the team, not a contract: outcome, scope, priority, release and dependencies are inherited from the story, and so are the acceptance criteria.

A task works like a Jira subtask. It has an optional assignee, who may differ from the story's, and its own status of To do, In progress or Done, set by whoever works on it. Tasks are never blocked, never linked, never in the ready set and never picked up through it. A small story needs no tasks.

Tasks are written from the code as it is that day, at release start or when the story is started:

1. A coding agent reads the code and drafts the tasks.
2. The story's assignee approves them: approach, what to reuse, which decisions need a record first.
3. Once the story is in progress and its tasks are approved, any developer can assign themselves a task and work on it. That is how someone free helps on another person's story. Tasks may be added or split along the way; anything that changes the story's scope or its dependencies goes to the technical PM.

Use the [task template](templates/task.md). See a [worked example](examples/task.md).

- The title names the change, not the user outcome.
- File and function names are fine here; a task is short-lived.
- "Proves" names the story's criteria this task answers for, by number. A task has no acceptance criteria of its own. Every story criterion must be proved by at least one task; one that no task proves is a gap found when the tasks are approved.
- "Done when" is usually empty. It holds only technical conditions the client would not recognise as something they asked for. Everything else is a story criterion, cited under Proves.
- Upkeep with no user outcome, such as dependency upgrades or pipeline fixes, is a story under a standing maintenance epic. It is started and finished like any other.

## Dependencies and the ready set

There is one dependency graph per release, drawn between stories and inputs. "B depends on A" means story B normally waits for story or input A to be done. Tasks are never in it.

The graph is built from each story's "Depends on". Record it as a dependency link where the tracker has one, so the ready set can be computed instead of read off the ticket text.

A dependency is information, not a lock, as in Jira, GitHub and Linear. A story with an open dependency is blocked and stays out of the ready set, but a person can still start it knowingly: for example by stacking a branch on the unmerged work it needs. If a story only needs part of another, split that story (SPIDR) rather than linking its tasks.

A story's status:

| Status | When |
| --- | --- |
| Blocked | Not started, and any of its dependencies is open. Computed. When only inputs hold it, it shows as waiting on their owner, such as "waiting on the client" |
| Ready | Not started, and every dependency is done. Computed |
| In progress | Started by its assignee, even if they started it knowingly while it was blocked |
| Done | Confirmed by a person: every task is done and every criterion is proved |

The ready set is every ready story in the current release. Each developer sees the stories assigned to them first, then the rest, each group ordered by how many other stories it unblocks. Nobody works out what to start.

## Assignees

Every story has one assignee: the developer the technical PM assigns when a release's graph is drawn, or earlier. The assignee holds the story's design, how it should be built and why, approves its tasks and starts it, which moves the story to In progress.

- Anyone may start a ready story, most usefully when their own stories are blocked or finished. Whoever starts someone else's story becomes its assignee.
- The assignee is the first call for changes inside the story.
- Anyone may assign themselves a task in a story that is in progress, once its tasks are approved.
- A change that crosses stories, or alters the dependencies between them, goes to the technical PM.

How stories are run:

- **Readiness is computed** from dependencies, not set by hand.
- **Work is pulled** from the ready set: every ready story, the one that unblocks most first.
- **Starting is done by a named person**, who is then the story's assignee. Nobody is given a start they did not make. Others help by assigning themselves its tasks.
- **Anything the work needs but nobody builds is an input**: a decision, an approval, information or a deliverable. It is owned by the client or a third party, never by the team, and its owner closes it. It sits in the graph alongside the stories it holds up, and is never assigned to the team or in the ready set. A decision the team must make is work in a story, not an input.
- **Done is confirmed by a person**, only when every task is done and every criterion is proved. An agent may propose it, with evidence.

## Decision records

A decision that is material and not obvious from the code gets an Architecture Decision Record: a short, numbered file in the repository.

Write one when a choice is costly to reverse, when a reasonable person would have chosen differently, or when the client's decision shapes the design. Do not write one for anything a reader can see by reading the code.

Use the [ADR template](templates/adr.md). See a [worked example](examples/adr.md).

- A record is never edited after it is accepted. A change of mind is a new record that supersedes the old one.
- A story or task shaped by a decision points to its record instead of restating it.
- A spike that ends in a decision ends in a record.

## Roles

| Role | Does |
| --- | --- |
| Technical PM | Owns the plan above and below the line. Writes epics and stories, agrees priorities with the client, signs off criteria and estimates, draws each release's dependency graph and assigns its stories, sets technical direction, runs the release review |
| Story assignee | The one developer on a story. Holds its design, approves its tasks, starts it and is the first call for changes inside it. Does not have to build every task |
| Developer | Reviews the stories before each release starts, starts ready stories, assigns themselves tasks in stories already in progress, works through tasks with agents, reviews, merges, writes decision records that arise mid-story |
| Coding agent | Investigates, drafts stories and their tasks, implements, proves each criterion with real output |
| Client | Follows stories in the tracker, makes the decisions each release needs, tests the outcome, accepts in writing |

On larger projects the client-facing half (stories, priorities, chasing client decisions, the release review) may be handed to a client lead. Criteria, estimates and the dependency graph stay with the technical PM.

## In any tracker

Slipstream is defined by its concepts, not a tool.

| Concept | Jira | GitHub | Monday |
| --- | --- | --- | --- |
| Epic | Epic | Parent issue | Item on an epics board |
| Release | Fix version | Milestone | Group |
| Story | Story | Sub-issue of the epic | Item |
| Task | Sub-task, with its own assignee and status | Sub-issue of the story, with its own assignee and open or closed state | Subitem, with its own person and status columns |
| Priority | Label or custom field | Project field or label | Status column |
| Estimate | Original estimate | Project number field | Numbers column |
| Dependency | "Blocks" link between stories | "Blocked by" relation between stories | Dependency column on stories |
| Assignee | Assignee, on the story and optionally on each sub-task | Assignee, on the story and optionally on each sub-issue | Person column, on the item and optionally on each subitem |
| Input | Linked issue, labelled with its type | Issue labelled with its type | Item in an inputs group |

Each level has one source of truth. Stories live in the tracker the client reads. A story's tasks live either there or in the repository, with the other as a mirror. Decision records always live in the repository.

## Terms

Words that are easy to confuse. Slipstream uses each in this sense only.

| Term | Kind | Meaning |
| --- | --- | --- |
| Dependency | Link | "B depends on A" means story B normally waits for A to be done. A may be a story or an input. Information, not a lock: B can still be started knowingly |
| Blocked | Status | Computed. A story that has not been started is blocked while any of its dependencies is open, and stays out of the ready set. When only inputs hold it, it shows as waiting on their owner, such as "waiting on the client" |
| Blocker | Derived label | An open story or input that is stopping a story which would otherwise be ready. Never an item type, and not a priority as in Jira |
| Ready | Status | Computed. A story in the current release that has not been started and whose dependencies are all done. Only stories are ever ready |
| Input | Item | Something the work needs that nobody builds: a decision, an approval, information or a deliverable. Has a type and an owner outside the team: the client or a third party. Never assigned to the team or pulled |
| Assignee | Field | The one developer on a story, who holds its design, approves its tasks and starts it. A task may have its own, different assignee |
| Task | Item | A piece of work inside a story, like a Jira subtask, with its own optional assignee and its own status: To do, In progress or Done. Never blocked, never in the ready set |
| Release acceptance | Event | The client's written sign-off at the release review. The only point where the client approves the release as a whole |
