# Slipstream

How Tandem plans and delivers projects.

A project is cut into releases. Each release gives the client one outcome they can test themselves. Stories are written for the whole project up front. The story is the unit of work: it is what a developer claims and finishes, and what the client tests as it lands. Each story's plan of technical steps is written only when its release starts, or when the story is claimed. What the contract promises and the budget both hold; how far each outcome is taken is what flexes. There are no sprints, stand-ups, story points or velocity.

The name is from cycling: each accepted release clears the air for the next, and detailed planning rides just behind what has shipped.

## Principles

1. **Outcomes before tasks.** A release is defined by something the client can test, not by a list of work.
2. **Finish stories one at a time.** A developer claims a story, works through its plan and finishes it. Stories land whole, so the client can test each as it lands and effort does not spread across many half-done stories.
3. **Plain language above the line, technical detail below it.** The client reads stories. The team and its coding agents work through each story's plan.
4. **Outline far ahead, detail close up.** Stories for the project; plans for the next release only.
5. **Outcomes and budget hold; depth flexes.** What the contract promises is delivered within the days agreed. How far each outcome is taken is what gives way. Dates are targets.
6. **Acceptance criteria are the contract.** What is in them is required and proven. What is not in them is not required.
7. **People steer; agents investigate and build.** Agents draft each story's plan and write code. People set priorities, approve plans and merge.
8. **Decisions are written down once.** A material, non-obvious decision gets a decision record.

## Structure

| Level | What it is | Read by | Written when | Size |
| --- | --- | --- | --- | --- |
| Epic | Something a person does, in plain words | Client and team | Project start | Spans releases |
| Release | A slice across the epics with one testable outcome, a budget in days and a target date | Client and team | At contract; re-planned after each review | Two to six weeks |
| Story | One thing a user can do, with acceptance criteria. The smallest unit that is claimed, blocked or ready | Client and team | Project start, for every release | Half a day to two days |
| Task | A step in a story's plan | Team and agents | Release start, or when the story is claimed | None of its own |

Epics run across; releases cut down through them. A story belongs to one epic and one release. The first release is thin and end to end: it touches most epics lightly so the whole thing works before any part is deep.

A small story needs no steps.

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

Leave out any empty section except acceptance criteria. Epic, release, priority, estimate (in days), owner and claim are fields in the tracker, not text in the body.

"Depends on" names the stories and inputs that must be done first. The release's dependency graph is built from it.

Acceptance criteria:

- Each is one observable outcome that a person or a command can check.
- They are numbered so the steps in the story's plan can cite them.
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

Split, too, when another story needs only part of this one. Dependencies run between whole stories, never between steps, so the part that is needed becomes a story of its own.

## The story is the unit of work

A story is what is claimed, what depends on other work, what is blocked or ready, what is pulled from the ready set and what is confirmed done. A developer claims a story, works through its plan and finishes it. The team finishes stories one at a time, and the client can test each one as it lands.

Tasks remain, but only as the plan inside a story.

### What this gives up

- **A dependency waits for a whole story.** A story that needs only the first half of another waits for all of it, unless that story is split.
- **Two people cannot independently pull separate parts of one story.** Others can help the person who claimed it, but there is one claim.

Both cost little, because no story is over two days.

## A story's plan

A story's plan is an optional, ordered list of tasks, called steps. Each step names a change and the story's criteria it proves. A step is a working note for the team, not a contract: outcome, scope, priority and release are inherited from the story, and so are the acceptance criteria.

Steps are never claimed, blocked or pulled. Whoever claimed the story works through them in order and ticks each off as it lands. A small story needs no steps.

The plan is written from the code as it is that day, at release start or when the story is claimed:

1. A coding agent reads the code and drafts the steps.
2. The story's owner approves the plan: approach, order, what to reuse, which decisions need a record first.
3. Whoever claimed the story works through it, and may add, split or reorder steps. Anything that changes the story's scope or its dependencies goes to the technical PM.

Use the [task template](templates/task.md). See a [worked example](examples/task.md).

- The title names the change, not the user outcome.
- File and function names are fine here; a step is short-lived.
- "Proves" names the story's criteria this step answers for, by number. A step has no acceptance criteria of its own. Every story criterion must be proved by at least one step; one that no step proves is a gap found when the plan is approved.
- "Done when" is usually empty. It holds only technical conditions the client would not recognise as something they asked for. Everything else is a story criterion, cited under Proves.
- Upkeep with no user outcome, such as dependency upgrades or pipeline fixes, is a story under a standing maintenance epic. It is claimed and finished like any other.

## Dependencies and the ready set

There is one dependency graph per release, drawn between stories and inputs. "B depends on A" means story B normally waits for story or input A to be done.

The graph is built from each story's "Depends on". Record it as a dependency link where the tracker has one, so the ready set can be computed instead of read off the ticket text.

A dependency is information, not a lock, as in Jira, GitHub and Linear. A story with an open dependency is blocked and stays out of the ready set, but a person can still start it knowingly: for example by stacking a branch on the unmerged work it needs. If a story only needs part of another, split that story (SPIDR) rather than linking its steps.

A story's status is worked out from its dependencies and its claim, never set by hand:

| Status | When |
| --- | --- |
| Blocked | Any of its dependencies is open. When only inputs hold it, it shows as waiting on their owner, such as "waiting on the client" |
| Ready | Every dependency is done and nobody has claimed it |
| In progress | A named person has claimed it, even if they started it knowingly while it was blocked |
| Done | A person has confirmed it: every step is ticked and every criterion is proved |

The ready set is every ready story in the current release. Each developer sees their own stories first, then the rest, each group ordered by how many other stories it unblocks. Nobody works out what to start.

## Ownership

A developer can own a story, or a whole epic. The owner holds its design, how it should be built and why, and approves the plan.

- The technical PM names owners when a release's graph is drawn, or earlier.
- Owning is not doing. Anyone may claim a ready story, most usefully when their own stories are blocked or finished.
- The owner approves the story's plan and is the first call for changes inside it.
- A change that crosses stories, or alters the dependencies between them, goes to the technical PM.

How stories are run:

- **Status is computed** from dependencies and the claim, not set by hand.
- **Work is pulled** from the ready set: every ready story, the one that unblocks most first.
- **Starting is a claim** on a story by a named person. Nobody is given a start they did not make. Others can help without claiming it.
- **Anything the work needs but nobody builds is an input**: a decision, an approval, information or a deliverable. It is owned by the client or a third party, never by the team, and its owner closes it. It sits in the graph alongside the stories it holds up, and is never claimed or in the ready set. A decision the team must make is work in a story, not an input.
- **Done is confirmed by a person**, only when every step is ticked and every criterion is proved. An agent may propose it, with evidence.

## Decision records

A decision that is material and not obvious from the code gets an Architecture Decision Record: a short, numbered file in the repository.

Write one when a choice is costly to reverse, when a reasonable person would have chosen differently, or when the client's decision shapes the design. Do not write one for anything a reader can see by reading the code.

Use the [ADR template](templates/adr.md). See a [worked example](examples/adr.md).

- A record is never edited after it is accepted. A change of mind is a new record that supersedes the old one.
- A story or step shaped by a decision points to its record instead of restating it.
- A spike that ends in a decision ends in a record.

## Roles

| Role | Does |
| --- | --- |
| Technical PM | Owns the plan above and below the line. Writes epics and stories, agrees priorities with the client, signs off criteria and estimates, draws each release's dependency graph and names owners, sets technical direction, runs the release review |
| Story owner | A developer named on a story, or a whole epic, who holds its design. Approves the story's plan and is the first call for changes inside it. Does not have to build it |
| Developer | Reviews the stories before each release starts, claims ready stories, works through each story's plan with agents, reviews, merges, writes decision records that arise mid-story |
| Coding agent | Investigates, drafts stories and their plans, implements, proves each criterion with real output |
| Client | Follows stories in the tracker, makes the decisions each release needs, tests the outcome, accepts in writing |

On larger projects the client-facing half (stories, priorities, chasing client decisions, the release review) may be handed to a client lead. Criteria, estimates and the dependency graph stay with the technical PM.

## In any tracker

Slipstream is defined by its concepts, not a tool.

| Concept | Jira | GitHub | Monday |
| --- | --- | --- | --- |
| Epic | Epic | Parent issue | Item on an epics board |
| Release | Fix version | Milestone | Group |
| Story | Story | Sub-issue of the epic | Item |
| Task | Sub-task: a step in the plan, never assigned or transitioned on its own | Sub-issue of the story: a step in the plan, never assigned or closed on its own | Subitem: a step in the plan, never assigned |
| Priority | Label or custom field | Project field or label | Status column |
| Estimate | Original estimate | Project number field | Numbers column |
| Dependency | "Blocks" link between stories | "Blocked by" relation between stories | Dependency column on stories |
| Claim | Assignee and In Progress, on the story | Assignee, on the story | Person column, on the story |
| Input | Linked issue, labelled with its type | Issue labelled with its type | Item in an inputs group |

Each level has one source of truth. Stories live in the tracker the client reads. A story's plan lives either there or in the repository, with the other as a mirror. Decision records always live in the repository.

## Terms

Words that are easy to confuse. Slipstream uses each in this sense only.

| Term | Kind | Meaning |
| --- | --- | --- |
| Dependency | Link | "B depends on A" means story B normally waits for A to be done. A may be a story or an input. Information, not a lock: B can still be started knowingly |
| Blocked | Status | Computed. A story is blocked while any of its dependencies is open, and stays out of the ready set. When only inputs hold it, it shows as waiting on their owner, such as "waiting on the client" |
| Blocker | Derived label | An open story or input that is stopping a story which would otherwise be ready. Never an item type, and not a priority as in Jira |
| Ready | Status | Computed. A story in the current release whose dependencies are all done and that nobody has claimed. Only stories are ever ready |
| Input | Item | Something the work needs that nobody builds: a decision, an approval, information or a deliverable. Has a type and an owner outside the team: the client or a third party. Never claimed or pulled |
| Task | Item | A step in a story's plan: never claimed, blocked or pulled |
| Release acceptance | Event | The client's written sign-off at the release review. The only point where the client approves the release as a whole |
