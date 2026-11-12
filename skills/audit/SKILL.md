---
name: audit
description: Audit an existing skill against best practices — checks structure, frontmatter, invocation control, description quality, instruction writing, and anti-patterns. Use when verifying a skill before deployment.
---

# Skill Audit

Verify a skill follows best practices and the official specification.

## Instructions

Given a skill path (SKILL.md or skill directory), check each item below. Report findings as PASS/WARN/FAIL, or SKIP when a required behavioral check was not run. Count skipped checks separately from passes.

Done when: the report records each check's result, evidence, and any untested candidates.

## Audit Checklist

### Structure
- [ ] Valid YAML frontmatter with `---` delimiters
- [ ] `name` field present (lowercase, hyphens, max 64 chars)
- [ ] `description` field present and specific (includes trigger keywords)
- [ ] SKILL.md under 500 lines
- [ ] References one level deep from SKILL.md (no nested chains)
- [ ] SKILL-compact.md exists if SKILL.md > 200 lines (run `gen-skill-compact.sh --check <dir>` to verify freshness)

### Invocation Control
- [ ] `disable-model-invocation: true` set if skill has side effects (deploy, commit, send)
- [ ] `user-invocable: false` set if skill is background knowledge only
- [ ] `allowed-tools` declared if specific tools needed
- [ ] `context: fork` used for isolation where appropriate

### Content Quality
- [ ] Uses standard markdown headings (NOT XML tags)
- [ ] Description includes both "what" and "when" triggers
- [ ] Examples are concrete, not abstract
- [ ] Has Quick Start or Instructions section
- [ ] No vague guidance ("use best judgment") — specific steps

### Anti-Patterns
- [ ] No XML tags in body
- [ ] No deep reference nesting
- [ ] No "punting" patterns (scripts handle errors, not "ask Claude to fix")
- [ ] No overly broad descriptions ("helps with things")

### Instruction Writing
- [ ] Delete test: preserve safety rules and hard authority gates, which PASS this check without a deletion trial. For other sentences that seem unnecessary, rerun the same pressure scenario with and without each candidate: FAIL if omission preserves the required behavior (recommend deletion); PASS if omission loses it. Without that comparison, report SKIP and list candidates for testing, without recommending removal.
- [ ] Action wording: PASS if each prohibition is paired with a concrete required action or protects a safety or authority boundary (such as destructive operations or approval gates); preserve explicit boundary prohibitions. Otherwise FAIL and request an action ("do X") in place of the ban ("avoid Y"), which puts the unwanted behavior in focus. See [replacement-action guidance](../skill/references/testing-skills-with-subagents.md#1-replacement-action-in-rules) for testing a rewrite.
- [ ] Every procedural step ends with a "Done when:" line naming an observable result (a file, command result, or recorded decision), rather than "reviewed" or "understood". A step is an executable action or workflow stage with its own outcome, whether numbered or headed; supporting bullets, audit criteria, and illustrative snippets are not separate steps. A single-action Instructions section counts as one step.
- [ ] The description names each distinct trigger case once; combine synonymous trigger clauses that route the same request. Compact search keywords within that case may include synonyms (such as "timeout/hang/freeze"); these are discovery terms, not additional cases.
- [ ] Configuration values and command options stay in their config file or `--help`; link to that source and explain only the skill-specific decision.
- [ ] Each rule has one authoritative home; other sections link to it instead of repeating it.

### Optional Intermesh Manifest

- [ ] If `intermesh.yaml` exists and `command -v intermesh >/dev/null 2>&1` succeeds, run `intermesh manifest validate <path>/intermesh.yaml`; report its diagnostics as normal audit findings.
- [ ] If Intermesh is unavailable, mark manifest validation `SKIP` rather than WARN/FAIL and continue the existing audit unchanged.
- [ ] The absence of `intermesh.yaml` is not an audit finding unless the user explicitly requires Intermesh routing metadata.

## Output Format

```
Skill Audit: {skill-name}
──────────────────────────────
Structure:           {N}/6 pass
Invocation Control:  {N}/4 pass
Content Quality:     {N}/5 pass
Anti-Patterns:       {N}/4 pass
Instruction Writing: {N}/6 pass
Skipped:             {N} (list checks and reasons below)
──────────────────────────────
Overall: {PASS | WARN (N issues) | FAIL (N issues)}

Issues:
- [WARN] Description could be more specific
- [FAIL] Missing disable-model-invocation for deploy workflow
```
