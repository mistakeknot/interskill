# interskill

Skill authoring toolkit for Claude Code plugins.

## What this does

interskill guides you through creating Claude Code skills that actually trigger reliably and do what they promise. It combines two phases: spec-driven creation (structure, frontmatter, invocation control, description engineering for Claude Search Optimization) and TDD-adapted pressure testing (invoke the skill in edge cases, verify it handles them).

The audit skill checks existing skills against the Claude Code spec — structure, frontmatter validity, CSO compliance, and whether the skill's description is actually triggering it in the right contexts.

## Installation

First, add the [interagency marketplace](https://github.com/mistakeknot/interagency-marketplace) (one-time setup):

```bash
/plugin marketplace add mistakeknot/interagency-marketplace
```

Then install the plugin:

```bash
/plugin install interskill
```

## Usage

Create a new skill:

```
/interskill:create
```

Or ask naturally:

```
"create a skill for database migration safety"
"write a new skill that handles deployment"
```

Audit an existing skill:

```
/interskill:audit
```

Or:

```
"check if my skill description triggers correctly"
"audit the flux-drive skill"
```

## Architecture

```
skills/
  create/
    SKILL.md                  Unified creation workflow (spec + TDD phases)
    references/               17 reference docs (best practices, CSO, patterns, etc.)
    templates/                Starter templates (simple, router)
    workflows/                Sub-workflows (add reference, add script, upgrade to router, etc.)
  audit/
    SKILL.md                  Skill verification workflow
```

The `create` skill ships with a full knowledge cluster: 17 reference documents covering Anthropic's official skill spec, persuasion principles, CSO optimization, common patterns, and testing strategies. The `audit` skill verifies structure, frontmatter, and invocation control against these references.

## Ecosystem

interskill was extracted from [interdev](https://github.com/mistakeknot/interdev), which consolidated two earlier skills (`create-agent-skills` and `writing-skills`) into the unified create + audit pair. See also [interplug](https://github.com/mistakeknot/interplug) for plugin-level development tooling.
