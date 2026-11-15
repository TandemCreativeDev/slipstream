# Slipstream

How Tandem plans and delivers projects.

A project is cut into releases. Each release gives the client one outcome they can test themselves. Stories are written for the whole project up front; technical tasks are written only for the release about to start. Dates hold and scope flexes. There are no sprints, stand-ups, story points or velocity.

The name is from cycling: each accepted release clears the air for the next, and detailed planning rides just behind what has shipped.

## Principles

1. **Outcomes before tasks.** A release is defined by something the client can test, not by a list of work.
2. **Plain language above the line, technical detail below it.** The client reads stories. The team and its coding agents work in tasks.
3. **Outline far ahead, detail close up.** Stories for the project; tasks for the next release only.
4. **Dates hold; scope flexes.** Priorities decide what gives way.
5. **Acceptance criteria are the contract.** What is in them is required and proven. What is not in them is not required.
6. **People steer; agents investigate and build.** Agents draft plans, break stories into tasks and write code. People set priorities, approve plans and merge.
7. **Decisions are written down once.** A material, non-obvious decision gets a decision record.

## Structure

| Level | What it is | Read by | Written when | Size |
| --- | --- | --- | --- | --- |
| Epic | Something a person does, in plain words | Client and team | Project start | Spans releases |
| Release | A slice across the epics with one testable outcome and a date | Client and team | Project start; re-planned after each review | Two to six weeks |
| Story | One thing a user can do, with acceptance criteria | Client and team | Project start, for every release | Half a day to two days |
| Task | A technical step towards a story | Team and agents | Release start, for that release only | Up to a day |

Epics run across; releases cut down through them. A story belongs to one epic and one release. The first release is thin and end to end: it touches most epics lightly so the whole thing works before any part is deep.

A small story needs no tasks.

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

Every story has a MoSCoW priority, set per release.

| Priority | Meaning | Guide |
| --- | --- | --- |
| Must | The release outcome fails without it | At most about 60% of the release's days |
| Should | Painful to leave out, but the outcome survives | |
| Could | Wanted if time allows; first to go | About 20% of the release's days |
| Won't this time | Agreed as out of this release | |

When a release is at risk, Could-haves move to a later release first, then Should-haves, by agreement with the client. The date moves only if a Must cannot be met.

## Stories

A story states the outcome first and says nothing about files or implementation.

```markdown
<Outcome in plain words>

<Who gets what, and why it matters. One or two sentences.>

## Requirements

| Need | Detail |
| ---- | ------ |

## Not included

- <Item, and the release where it arrives if known>

## Acceptance criteria

- <One checkable outcome, and how it is proved>

## Depends on

<Stories that must land first>

## Blocked on

<Who supplies what, in which format, by when>
```

Leave out any empty section except acceptance criteria. Epic, release, priority, estimate (in days) and assignee are fields in the tracker, not text in the body.

Acceptance criteria:

- Each is one observable outcome that a person or a command can check.
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

Tasks hold the technical detail. At the start of a release an agent reads the code as it is that day and proposes the tasks for that release's stories; a developer approves them.

A task has a one-sentence description, the story it serves, what it depends on, and notes from the investigation.

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
| Technical PM | Writes epics and stories, sets priorities with the client, runs release planning and review |
| Client lead | Owns communication with the client, chases client decisions, sends release updates |
| Developer | Approves task breakdowns and agent plans, reviews, merges, writes decision records |
| Coding agent | Investigates, drafts stories and tasks, implements, proves each criterion with real output |
| Client | Follows stories in the tracker, makes the decisions each release needs, tests the outcome, accepts in writing |

One person can hold more than one role.

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
