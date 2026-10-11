# Production & Engineering Agents

Reusable AI agent profiles for the Production & Engineering Department, with clear responsibilities, explicit boundaries, and project-specific memory.

The collection currently includes five specialists: **Beck** for back-end development, **Faye** for front-end development, **Uma** for UX/UI design, **Sophie** for internal technical writing, and **Paige** for product technical writing. Together, they support the department’s design, development, and documentation work.

## Meet the Agents

| Agent | Role | Focus |
| --- | --- | --- |
| **Beck** | Back-end Developer | APIs, business logic, database schemas and migrations, service integrations, server-side authorization, and developer tests |
| **Faye** | Front-end Developer | User interfaces, responsive layouts, accessibility implementation, client-side state, API integration, and developer tests |
| **Uma** | UX/UI Agent | User journeys, information architecture, wireframes, prototypes, interface copy, design-system guidance, and usability reviews |
| **Sophie** | Internal Technical Writer | Internal system references, employee onboarding, standard operating procedures (SOPs), operational and troubleshooting guides for an internal knowledge platform such as Confluence |
| **Paige** | Product Technical Writer | Help-center and knowledge-base articles, user manuals, tutorials, release notes, reseller and partner playbooks, and developer-portal documentation |

Each agent follows the project’s existing conventions, asks when a consequential decision is unresolved, and hands off work outside its responsibilities.

## Repository Layout

```
.claude/
├── agents/
│   ├── be-dev/beck.md
│   ├── fe-dev/faye.md
│   ├── ux-ui/uma.md
│   ├── int-tech-writer/sophie.md
│   └── prod-tech-writer/paige.md
└── agent-memory/
    ├── Beck/MEMORY.md
    ├── Faye/MEMORY.md
    ├── Uma/MEMORY.md
    ├── Sophie/MEMORY.md
    └── Paige/MEMORY.md
```

## Getting Started

1. Select the agent whose responsibilities match your task.
2. Load its Markdown profile into your AI environment using the environment’s supported agent configuration or instruction-loading mechanism.
3. Provide project context, relevant files, requirements, and acceptance criteria.
4. Configure the tools and permissions needed for the assignment.
5. Review the deliverables, verification evidence, and unresolved dependencies before integrating the work.

The profiles contain YAML frontmatter specifying their names, descriptions, intended handoff partners, model preferences, memory scope, and version metadata. All five currently declare `model: sonnet` and `memory: project`.

Support for configuration fields varies by environment. Verify how your runner handles the frontmatter, particularly `allowed_handoffs`. Automatic delegation requires support from the environment running the agents.

## Example Assignments

### Beck — Back-end Development

> Implement the agreed project-members API using the existing service and database patterns. Enforce server-side authorization, test relevant success and failure cases, and document the contract for Faye.

### Faye — Front-end Development

> Implement the project-members screen from Uma’s approved design using the documented API. Include loading, empty, error, and success states, verify keyboard behavior, and report the checks performed.

### Uma — UX/UI Design

> Design the project-members management flow using our existing design system. Define relevant interaction states, identify unresolved product decisions, and provide an implementation handoff for Faye and Beck.

### Sophie — Internal Technical Writing

> Write an SOP for rotating the project-members service credentials, based on Beck’s implementation notes. Identify who performs each step, the prerequisites, expected results, and escalation path, and flag any steps the system owner still needs to verify.

### Paige — Product Technical Writing

> Write a help-center article and release note for the new project-members feature. Explain who can manage members, the required permissions, and the steps involved, and identify any behavior that still needs confirmation from Beck or Faye.

## Working Together

For a feature involving all five roles:

1. **Uma** defines the experience and communicates the intended behavior.
2. **Beck and Faye** agree on the API contract and implementation dependencies.
3. **Beck** implements the supporting services and data behavior.
4. **Faye** implements the interface and integrates it with those services.
5. **Uma** reviews the implemented experience, while Beck and Faye verify their changes and address findings within their scope.
6. **Sophie** documents internal operation of the feature—system references, SOPs, and onboarding notes—verified with Beck and Faye.
7. **Paige** documents the released behavior for customers, partners, and developers, verified against the implementation and Uma’s design.

Work can proceed in parallel when ownership and shared contracts are clear. If delegation is unavailable, the agent asks the user or coordinator how to proceed.

Each profile lists its intended handoff partners in `allowed_handoffs`:

| Agent | Hands off to |
| --- | --- |
| Beck | Faye, Uma, Sophie, Paige |
| Faye | Beck, Uma, Sophie, Paige |
| Uma | Faye, Beck, Paige |
| Sophie | Faye, Beck, Paige |
| Paige | Faye, Beck, Uma, Sophie |

A useful handoff includes:

- The action or decision needed.
- The reason it is needed.
- Relevant requirements, evidence, and constraints.
- Unresolved questions and dependencies.
- What has already been completed or verified.

## Profile Structure

Each profile follows a shared structure:

| Section | Purpose |
| --- | --- |
| Frontmatter | Agent identity, description, and configuration metadata |
| Who Are You | The role’s purpose |
| What You Do | Responsibilities the agent owns |
| Rules | Scope boundaries and actions to avoid |
| What Good Output Looks Like | Expectations for deliverables and verification |
| Memory | Instructions for preserving project knowledge |

## Project Memory

Each agent keeps its own memory folder within the project, declared by `memory: project` in its frontmatter:

| Agent | Memory folder |
| --- | --- |
| Beck | `.claude/agent-memory/Beck/` |
| Faye | `.claude/agent-memory/Faye/` |
| Uma | `.claude/agent-memory/Uma/` |
| Sophie | `.claude/agent-memory/Sophie/` |
| Paige | `.claude/agent-memory/Paige/` |

### How memory is organized

- `MEMORY.md` in each folder is an **index**, with one short line per note.
- Each note is its own small file in the same folder, named to match its content.
- An agent reads and writes only its own folder. It never creates memory files elsewhere (including the repository root) and never edits another role’s memory.

### Session lifecycle

- **Start of session:** The agent reads its `MEMORY.md` index, then any notes relevant to the task, before beginning work.
- **End of session:** The agent records new learnings and decisions, revises or removes entries the session made untrue rather than only appending, and keeps the index in sync.
- **Retirement:** Once a note’s subject is resolved and recorded in `CLAUDE.md`, the agent deletes the note and its index line.

### What each agent records

| Agent | Typical notes |
| --- | --- |
| Beck | Back-end conventions, API contracts, data and migration constraints, unresolved dependencies |
| Faye | Front-end conventions, integration constraints, unresolved dependencies |
| Uma | Design decisions and rulings, design-system patterns, usability findings and their status, unresolved dependencies |
| Sophie | Documentation conventions, source locations, document ownership, review requirements, unresolved questions |
| Paige | Audience needs, product terminology, documentation conventions, version constraints, publication requirements, unresolved questions |

### Rules for memory content

- `CLAUDE.md` is authoritative project guidance. A note that disagrees with it or with the current code is stale; the agent verifies and corrects it before relying on it.
- Entries stay concise and specific to the project, and do not duplicate what `CLAUDE.md` already records.
- Agents distinguish confirmed decisions from assumptions and open questions.
- Memory never contains secrets or sensitive user data.

### Version control

`.gitignore` excludes `.claude/agent-memory/`, so new note files agents write stay local to each working copy. The empty `MEMORY.md` index files are committed to seed the folder structure; because they are already tracked, Git still reports changes to them. To share memory across a team, remove that `.gitignore` entry and review memory changes like any other contribution.

## Scope and Quality

The current collection covers design, development, and documentation. Dedicated QA, security-review, operations, and release-management profiles are not yet included.

Each agent is expected to:

- Stay within its assigned responsibilities.
- Preserve existing project conventions and other agents’ work.
- Identify mocks, unresolved dependencies, and verification limitations.
- Report checks as passed only when they were actually run and passed.
- Provide evidence and context for the next person or agent.

Developer tests, design reviews, and documentation reviews support—but do not replace—independent QA, security review, or release approval. The writers document verified behavior; they do not claim technical, security, legal, or release approval on behalf of other roles.

Role instructions guide behavior. Access permissions, repository protections, publication destinations, and deployment controls must be configured separately in the execution environment.

## Contributing

Contributions should preserve the shared profile structure and keep responsibilities clear.

- Define explicit ownership and practical handoff boundaries.
- Describe observable expectations for quality.
- Avoid unnecessary overlap between roles.
- Keep general instructions independent of specific application frameworks.
- Distinguish runtime-supported settings from descriptive metadata.
- Give each new agent its own `.claude/agent-memory/<Name>/` folder with a seeded `MEMORY.md` index, and follow the shared Memory section wording.
- Base new rules on demonstrated needs and practical experience.
