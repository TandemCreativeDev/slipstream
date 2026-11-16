# Slipstream

How Tandem plans and delivers projects.

A project is cut into releases. Each release gives the client one outcome they can test themselves. Stories are written for the whole project up front; technical tasks are written only for the release about to start. What the contract promises and the budget both hold; how far each outcome is taken is what flexes. There are no sprints, stand-ups, story points or velocity.

The name is from cycling: each accepted release clears the air for the next, and detailed planning rides just behind what has shipped.

## Principles

1. **Outcomes before tasks.** A release is defined by something the client can test, not by a list of work.
2. **Plain language above the line, technical detail below it.** The client reads stories. The team and its coding agents work in tasks.
3. **Outline far ahead, detail close up.** Stories for the project; tasks for the next release only.
4. **Outcomes and budget hold; depth flexes.** What the contract promises is delivered within the days agreed. How far each outcome is taken is what gives way. Dates are targets.
5. **Acceptance criteria are the contract.** What is in them is required and proven. What is not in them is not required.
6. **People steer; agents investigate and build.** Agents draft plans, break stories into tasks and write code. People set priorities, approve plans and merge.
7. **Decisions are written down once.** A material, non-obvious decision gets a decision record.

## Structure

| Level | What it is | Read by | Written when | Size |
| --- | --- | --- | --- | --- |
| Epic | Something a person does, in plain words | Client and team | Project start | Spans releases |
| Release | A slice across the epics with one testable outcome, a budget in days and a target date | Client and team | At contract; re-planned after each review | Two to six weeks |
| Story | One thing a user can do, with acceptance criteria | Client and team | Project start, for every release | Half a day to two days |
| Task | A technical step towards a story | Team and agents | Release start, for that release only, by the technical PM | Up to a day |

Epics run across; releases cut down through them. A story belongs to one epic and one release. The first release is thin and end to end: it touches most epics lightly so the whole thing works before any part is deep.

A small story needs no tasks.

### Commitments

The contract does not list stories. It promises outcomes, and it sets the releases with their budgets. Each promised outcome is a commitment.

Commitments are not a level in the table above. They cut across it: one commitment is usually met by several stories, sometimes in more than one epic or release. Every commitment traces to at least one Must story, and that link is checked when each release is planned.

## The release loop

| Step | What happens |
| --- | --- |
| Plan | Agree the outcome and the decisions the client must make. Refine the release's stories, set priorities, write the tasks |
| Build | Pull tasks from the ready set. The client follows the stories in the tracker at any time |
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

Leave out any empty section except acceptance criteria. Epic, release, priority, estimate (in days) and assignee are fields in the tracker, not text in the body.

Acceptance criteria:

- Each is one observable outcome that a person or a command can check.
- They are numbered so tasks can cite them.
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

## Tasks

Tasks hold the technical detail. A task is a working note for the team, not a contract: it adds only what the story leaves out. Outcome, scope, priority and release are inherited from the story, and so are the acceptance criteria: a task points at the ones it proves.

The technical PM sets out every task for a release before building begins:

1. A coding agent reads the code as it is that day and drafts the tasks and their dependencies for the release's stories.
2. The technical PM corrects the direction: approach, order, what to reuse, which decisions need a record first.
3. The technical PM publishes the task graph. Developers pull from it.

Developers are free within a task and may add or split tasks inside a story. Anything that changes the approach or the dependencies comes back to the technical PM.

Use the [task template](templates/task.md). See a [worked example](examples/task.md).

- The title names the change, not the user outcome.
- File and function names are fine here; a task is short-lived.
- "Proves" names the story's criteria this task answers for, by number. A task has no acceptance criteria of its own. Every story criterion must be claimed by at least one task; one that nobody claims is a gap found at planning.
- "Done when" is usually empty. It holds only technical conditions the client would not recognise as something they asked for. Everything else is a story criterion, cited under Proves.
- Upkeep with no story, such as dependency upgrades or pipeline fixes, sits as tasks under a standing maintenance epic.

## Dependencies and the ready set

There is one dependency graph per release. It is drawn between tasks and runs across stories: a task may depend on a task in another story, or on a client decision.

The graph is built from each task's "Depends on". Record it as a dependency link where the tracker has one, so the ready set can be computed instead of read off the ticket text.

- **Nobody works out what to start.** The ready set is computed from the graph: every task with nothing in its way.
- **A story's state is read off its tasks.** It is in play when one of its tasks is ready or claimed, waiting when none are, done when all are. It is never kept separately.
- **Stories carry rough links first.** Before a release has tasks, a story's "Depends on" orders the work and helps decide which release it lands in. Once the tasks exist, they are the source of truth and the story-level view is a roll-up of them.
- **Each developer sees their own stories first.** A ready set lists ready tasks in the stories that developer owns, then everyone else's, each group ordered by how much it unblocks.

## Ownership

A developer can own a story, or a whole epic. The owner holds its design: how it should be built and why.

- The technical PM names owners when a release's tasks are set out, or earlier.
- Owning is not doing. Anyone may claim a ready task in any story, most usefully when their own stories are blocked or finished.
- The owner approves the plan for any task in their story and is the first call for changes inside it.
- A change that crosses stories, or alters the dependencies between them, goes to the technical PM.

How tasks are run:

- **Status is computed** from dependencies, not set by hand.
- **Work is pulled** from the ready set: every unblocked, unclaimed task, the one that unblocks most first.
- **Starting is a claim** by a named person. Nobody is given a start they did not make.
- **A client decision the work waits on is a gate**, visible alongside the tasks it blocks.
- **Done is confirmed by a person.** An agent may propose it, with evidence.

## Decision records

A decision that is material and not obvious from the code gets an Architecture Decision Record: a short, numbered file in the repository.

Write one when a choice is costly to reverse, when a reasonable person would have chosen differently, or when the client's decision shapes the design. Do not write one for anything a reader can see by reading the code.

```markdown
# ADR-<number>: <Decision in plain words>

Status: Proposed | Accepted | Superseded by ADR-<number>
Date: <date>

## Context

<The situation and the constraint that forced a choice.>

## Decision

<What was chosen.>

## Alternatives

<What else was considered, and why not.>

## Consequences

<What this makes easier, harder or impossible.>
```

- A record is never edited after it is accepted. A change of mind is a new record that supersedes the old one.
- A story or task shaped by a decision points to its record instead of restating it.
- A spike that ends in a decision ends in a record.

## Roles

| Role | Does |
| --- | --- |
| Technical PM | Owns the plan above and below the line. Writes epics and stories, agrees priorities with the client, signs off criteria and estimates, sets out the tasks for each release, sets technical direction, runs the release review |
| Story owner | A developer named on a story, or a whole epic, who holds its design. Approves plans for tasks in it and is the first call for changes inside it. Does not have to build all of it |
| Developer | Reviews the stories before each release starts, claims ready tasks in any story, approves agent plans, reviews, merges, writes decision records that arise mid-task |
| Coding agent | Investigates, drafts stories and tasks, implements, proves each criterion with real output |
| Client | Follows stories in the tracker, makes the decisions each release needs, tests the outcome, accepts in writing |

On larger projects the client-facing half (stories, priorities, chasing client decisions, the release review) may be handed to a client lead. Criteria, estimates and tasks stay with the technical PM.

## In any tracker

Slipstream is defined by its concepts, not a tool.

| Concept | Jira | GitHub | Monday |
| --- | --- | --- | --- |
| Epic | Epic | Parent issue | Item on an epics board |
| Release | Fix version | Milestone | Group |
| Story | Story | Sub-issue of the epic | Item |
| Task | Sub-task | Sub-issue of the story | Subitem |
| Priority | Label or custom field | Project field or label | Status column |
| Estimate | Original estimate | Project number field | Numbers column |
| Dependency | "Blocks" link | "Blocked by" relation | Dependency column |
| Claim | Assignee and In Progress | Assignee | Person column |
| Client gate | Linked issue on the client | Labelled issue | Item in a client group |

Each level has one source of truth. Stories live in the tracker the client reads. Tasks live either there or in the repository, with the other as a mirror. Decision records always live in the repository.
