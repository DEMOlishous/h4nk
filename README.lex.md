# git-lex — soul kit

This repo is managed by [git-lex](https://github.com/repolex-ai/git-lex) — git extensions for knowledge graphs.

## Quick Start

```bash
git lex create <type>    # Create a new document (types: Interest, Creation, Resource, Habit, Mantra, Subagent, Skill, Note, Journal, Friend, Exploration, Task, Decision, Memory, Soul)
git lex save "message"   # Add + commit (extracts automatically)
git lex sync              # Build/update the knowledge graph
git lex query "SPARQL..." # Query the knowledge graph
git lex status            # Show extraction status
```

## Commands

| Command | What it does |
|---|---|
| `git lex create <type>` | Scaffold a new document. Valid types: Interest, Creation, Resource, Habit, Mantra, Subagent, Skill, Note, Journal, Friend, Exploration, Task, Decision, Memory, Soul |
| `git lex save "msg"` | Stage all changes, commit, extract frontmatter |
| `git lex sync` | Build the SPARQL knowledge graph from git + extractions |
| `git lex query "..."` | Run a SPARQL query against the knowledge graph |
| `git lex status` | Show which files have been extracted |
| `git lex log` | Show commit history |
| `git lex llm list` | Show files needing LLM extraction |
| `git lex llm extract <file> --model <id>` | Extract entities via LLM |

## Writing Documents

Documents use YAML frontmatter with flat dot notation: `kit.class.property`

```yaml
---
soul.memory.confidence: "certain"
soul.memory.source: "observation"
soul.memory.category: "fact"
---

Your content here. Use [[wikilinks]] for relationships between documents.
```

See `__ClassName.md` files in each folder for available properties and SHACL-derived constraints.

## [[wikilinks]]

Reference other documents naturally in your text:

- `[[Class/id]]` — creates a `lex:linksTo` relationship to that document
- bare `[[some-doc]]` is also accepted (resolved via slug)

Wikilinks are extracted automatically from document bodies and commit messages.

## soul Kit — Document Types

### Interest

Create: `git lex create Interest`

| Property | Type | Description |
|---|---|---|
| interestId | string | Unique identifier for this interest. |

### Creation

Create: `git lex create Creation`

| Property | Type | Description |
|---|---|---|
| creationId | string | Unique identifier for this creation. |
| creationType | string | Type of creation. |

### Resource

Create: `git lex create Resource`

| Property | Type | Description |
|---|---|---|
| resourceId | string | Unique identifier for this resource. |
| resourceType | string | Type of resource: 'paper', 'link', 'book', 'video', 'document'. |
| url | string | Where to find this resource. |

### Habit

Create: `git lex create Habit`

| Property | Type | Description |
|---|---|---|
| frequency | string | How often: 'per-session', 'daily', 'weekly', 'on-trigger'. |
| habitId | string | Unique identifier for this habit. |
| habitStatus | string | Status: 'active', 'paused', 'retired'. |
| lastRun | string | When this habit last executed. |
| trigger | string | What initiates this habit (session-start, idle, schedule, event). |

### Mantra

Create: `git lex create Mantra`

| Property | Type | Description |
|---|---|---|
| mantraId | string | Unique identifier for this mantra. |

### Subagent

Create: `git lex create Subagent`

| Property | Type | Description |
|---|---|---|
| subagentDescription | string | When to delegate to this subagent. Used for auto-delegation matching. |
| subagentId | string | Unique identifier for this subagent. |
| subagentMaxTurns | string | Maximum agentic turns before the subagent stops. |
| subagentModel | string | Model to use: 'sonnet', 'opus', 'haiku', or 'inherit'. |
| subagentTools | string | Comma-separated tools the subagent can use. |

### Skill

Create: `git lex create Skill`

| Property | Type | Description |
|---|---|---|
| skillAllowedTools | string | Space-separated list of tools the skill may use without confirmation. |
| skillArgumentHint | string | Hint shown during autocomplete (e.g. '[read|write|status]'). |
| skillDescription | string | What this skill does and when to use it. |
| skillDomain | string | What domain this skill belongs to (e.g. 'rust', 'sparql', 'ontology design'). |
| skillId | string | Unique identifier for this skill. |
| skillInvocability | string | Who can invoke this skill: 'user' (slash command), 'agent' (auto-triggered), 'both'. |
| skillLevel | string | How well-developed: 'learning', 'competent', 'fluent', 'expert'. |

### Note

Create: `git lex create Note`

| Property | Type | Description |
|---|---|---|
| noteId | string | Unique identifier for this note. |
| relatedTo | reference | Other documents this note references. |
| topic | string | What this note is about. |

### Journal

Create: `git lex create Journal`

| Property | Type | Description |
|---|---|---|
| earthDate | string | The calendar date this journal entry covers. |
| emojimood | string | The day's vibe as emoji. Compact, expressive, queryable. |
| journalId | string | Unique identifier for this journal. |
| soulDay | string | The soul day number (compaction cycle count), starting from 1. Model-agnostic version of 'Claude Day'. Starts from 1. |

### Friend

Create: `git lex create Friend`

| Property | Type | Description |
|---|---|---|
| email | string | Friend's email. |
| friendId | string | Unique identifier for this friend. |
| friendType | string | Type: 'human', 'agent', 'team', 'organization'. |
| notes | string | Personal notes about this friend. |
| relationship | string | How this agent relates to the friend. |
| squad | string | Which squad this friend belongs to, if any. |

### Exploration

Create: `git lex create Exploration`

| Property | Type | Description |
|---|---|---|
| explorationId | string | Unique identifier for this exploration. |
| explorationStatus | string | Status: 'exploring', 'active', 'paused', 'concluded'. |
| findings | string | What you've discovered so far. |
| hypothesis | string | What you're trying to figure out. |

### Task

Create: `git lex create Task`

| Property | Type | Description |
|---|---|---|
| blockedBy | string | What's blocking this task. |
| priority | string | Priority. |
| taskId | string | Unique identifier for this task. |
| taskStatus | string | Task status. |

### Decision

Create: `git lex create Decision`

| Property | Type | Description |
|---|---|---|
| alternatives | string | Other options that were considered. |
| decisionId | string | Unique identifier for this decision. |
| outcome | string | What happened. |
| rationale | string | Why this option was chosen. |
| supersededBy | reference | A later decision that replaces this one. |

### Memory

Create: `git lex create Memory`

| Property | Type | Description |
|---|---|---|
| category | string | Memory category. |
| confidence | string | How sure: 'certain', 'likely', 'hypothesis', 'hunch'. |
| memoryId | string | Unique identifier for this memory. |
| source | string | Where this memory came from. |

### Soul

Create: `git lex create Soul`

| Property | Type | Description |
|---|---|---|
| alignment | string | The agent's guiding principles or values. |
| expertise | string | Areas of expertise. |
| role | string | The agent's primary role or specialty. |
| soulEmail | string | Git author email. Used to attribute commits across soul and squad repos. |
| soulId | string | Primary identifier. Lowercase, alphanumeric + hyphens. Must be consistent across all repos. |
| substrate | string | What the agent runs on: 'carbon' (human) or 'silicon' (AI). |

## Querying

Auto-injected prefixes: `git:`, `fm:`, `lex:`, `soul:`

```sparql
# List all documents by type
SELECT ?name ?type WHERE {
  GRAPH ?g { ?doc fm:soul.type ?type ; fm:title ?name }
}
```
