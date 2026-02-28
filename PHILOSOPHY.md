# interskill Philosophy

## Purpose
Skill authoring toolkit — unified skill creation with TDD-adapted testing and audit. Consolidated from interdev's create-agent-skills and writing-skills into a single spec-driven workflow.

## North Star
Every skill created through interskill should pass audit on first review.

## Working Priorities
- Creation quality (spec-driven, not freeform)
- Audit coverage (catch problems before publish)
- TDD pressure testing effectiveness

## Brainstorming Doctrine
1. Start from outcomes and failure modes, not implementation details.
2. Generate at least three options: conservative, balanced, and aggressive.
3. Explicitly call out assumptions, unknowns, and dependency risk across modules.
4. Prefer ideas that improve clarity, reversibility, and operational visibility.

## Planning Doctrine
1. Convert selected direction into small, testable, reversible slices.
2. Define acceptance criteria, verification steps, and rollback path for each slice.
3. Sequence dependencies explicitly and keep integration contracts narrow.
4. Reserve optimization work until correctness and reliability are proven.

## Decision Filters
- Does this reduce skill defects at creation time?
- Does this make audit results more actionable?
- Is the feedback fast enough to stay in flow?
- Does this prevent bad skills from reaching users?
