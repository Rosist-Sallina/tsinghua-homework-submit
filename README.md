# Tsinghua Homework Submit

An Alma skill for handling Tsinghua Learn homework workflows safely and repeatably.

It is designed for a very specific but common failure mode: when an agent or user is already logged into Tsinghua Learn, needs to inspect an assignment, download the prompt, read the instructions, submit the correct file, and verify the recorded result without mixing up courses or assignments.

## What this skill does

This skill teaches Alma to:

- locate the correct course and assignment page in a logged-in browser session
- read assignment instructions and due dates from Tsinghua Learn
- find and download assignment attachments using the existing login session
- submit the intended local file through the site UI or, if necessary, through a browser-context multipart request
- verify success from the assignment list instead of trusting fragile frontend messages
- avoid cross-course submission mistakes by requiring explicit pre-submit checks

## Why this exists

Tsinghua Learn submission flows are easy to get wrong in automation:

- the same assignment title can exist across different courses
- “most recent PDF” is an unreliable heuristic
- upload widgets may report stale or misleading frontend errors
- browser upload success does not guarantee the file actually entered `input.files`
- true confirmation usually comes only from the assignment list after submission

This skill encodes those lessons into a repeatable workflow.

## Scope

This skill is:

- specific to Tsinghua Learn
- intended for Alma agents using a logged-in Chrome session
- optimized for assignment workflows rather than generic LMS automation

This skill is not a universal LMS abstraction.

If you want to generalize it later, the most reusable ideas are:

- always confirm course + assignment + local file path before submitting
- prefer DOM inspection over UI guessing
- verify upload state from the form object, not from toast messages
- verify submission from the destination list page, not from the transient submit page

## Repository layout

- `SKILL.md` — the skill itself
- `README.md` — project overview and publishing notes
- `LICENSE` — license for reuse

## Requirements

- Alma CLI
- browser skill or Chrome Relay access
- a live logged-in Tsinghua Learn session in Chrome
- a local file already prepared for submission

## Installation

If you publish this skill through a skill-compatible repo, users can install it with the normal Alma/skills workflow.

For local use, place it under:

`~/.config/alma/skills/tsinghua-homework-submit/`

with `SKILL.md` inside that directory.

## Usage notes

The intended order is always:

1. Find the correct course and assignment.
2. Read the assignment details and prompt.
3. Confirm the exact local file path.
4. Upload or submit.
5. Verify from the assignment list.

Do not skip the final verification step.

## Generality review

Current portability is moderate:

- the workflow principles are reusable
- the selectors, page routes, and field names are Tsinghua Learn-specific
- direct POST fallback depends on the site’s current form fields and token behavior

So this is best published as a site-specific operational skill, not as a generic “homework submit” skill.

## Recommended future improvements

- split site-specific constants into a dedicated section
- add a short troubleshooting matrix with observed server responses
- add example flows for PDF, ZIP, and text-only submission
- add an optional verification snippet for extracting attachment names after submission
- factor out the “safe submission checklist” into a more reusable helper skill later

## License

MIT
