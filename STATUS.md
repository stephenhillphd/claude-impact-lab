# Status

Working record for this fork. One place to look when picking work back up, so
nothing depends on remembering a session. Update it rather than adding new
per-session files.

**Updated:** 28 August 2026, ~14:00 CDT
**`main` at:** `8350518` before this doc landed
**Upstream:** this fork is ~17 commits ahead of `Birmingham-AI/claude-impact-lab`

---

## What this fork holds

Two things now live here that upstream does not have:

- `examples/04-homelessness-handoff/` — a worked example for Challenge 4, built
  on lab day. Originally an organizer's reference, later also used as the basis
  of a Team 4B submission.
- Two additions to `resources/04-homelessness-handoff.md` — the city's
  homelessness page, and a note that Jefferson County publishes nothing useful.
  These are the only edits to a file upstream also owns, so they are where a
  future `git pull upstream main` will conflict if it ever conflicts.

## Shipped

Seventeen commits between 10:13 and 13:26 CDT, in four phases.

**The example itself** (`1ee81de`, `251a860`). First as a synthetic-placeholder
demo, then rebuilt on real Birmingham data once research came back.

**The field template** (`791f06c`, `960afb4`). A blank sheet to print in advance
and fill in with a pen, since the handoff happens on a sidewalk where the screen
version is useless. `960afb4` also fixed a real print bug: printing the blank
emitted a wasted first page carrying the lock notice.

**Prose and copy passes** (`2a763b2`, `ddcab1a`, `a8c7ef0`). Removed machine-
writing habits from the README and deck. `a8c7ef0` fixed a regression: cutting
the deck to six slides orphaned the term "the gate", which then appeared twice
having never been defined.

**The deck** (`392a2c7` through `8350518`). Eleven slides cut to six once the
plan changed to demoing the artifact live rather than describing it.

Separately, a standalone submission repo was created at
`stephenhillphd/birmingham-cooling-handoff` (`ead3c4c`, `336dec8`, `6d2c66e`).
It is public, verified by anonymous fetch.

## Work with no diff

Everything in this section is invisible to `git log` and is the easiest to lose.

**Four artifacts were published to claude.ai.** Only the last of each pair is
current:

| Artifact | URL id | State |
|---|---|---|
| Card (1st) | `c48e86a6-7370-434a-82e1-ad798942fb7b` | **Shared, badly outdated.** Should be unshared. |
| Card (2nd) | `94ccf7a6-f9ac-4ce8-a9db-f4c2c8cfb091` | **Shared, pinned mid-way.** Should be unshared. |
| Card (current) | `923e8ee4-00b4-48bf-b834-44d50b1eb14f` | Shared, anyone with the link, Version 1 = current |
| Deck | `00372ae9-ef0e-4f76-9492-66106dffa8f8` | Private |

The card was republished to a fresh URL **twice** because an artifact's share
link pins the version live at share time, and later republishes do not move it.
Viewers were getting stale cards while the owner's own view showed current — the
failure is invisible from the owner's side. The working rule: share only once
content has settled, and if you republish afterwards, go back and update the
shared version.

**Sharing was set through the browser**, not the API. The Artifact tooling has
no share action; the artifact's own page does. Verified on 28 Aug: the share
panel reads "Anyone with the link", shared version "Version 1 · Heat Handoff
Card". Not verified from a logged-out session.

**A submission form was filled but not submitted**, at
`Birmingham-AI/claude-impact-lab/issues/new?template=project-submission.yml`.
It is filled with Team 4B, the standalone repo URL, and all four
acknowledgements ticked. It was deliberately left unsent — see Outstanding.

**Team 4B had already submitted.** Issue **#2** on the upstream repo, filed
12:54 PM CDT by **@mhancock537**, titled "[Submission] Team 4B — Help the
Homeless", pointing at `github.com/mhancock537/confirm-first-handoff`. Found by
listing the repo's issues, not by anything in this session's own work.

**Deck images were generated locally** with the Gemini `nanobanana` extension
(three risograph illustrations kept, three photographic alternates discarded).
The QR was generated locally with `segno` in a throwaway venv rather than a web
QR service, so the URL was not handed to a third party, and decoded back with
OpenCV to confirm it resolves to the intended artifact.

## Decisions and their reasons

**Everything routes through 211; no per-station phone numbers.** The city
publishes none. Sourcing eleven numbers from eleven other sites would have meant
eleven chances to print a wrong one, and a wrong number sends a real person to a
dead line. One number on the page, and it is correct.

**211 is described as a referral line, not the city's activation system.** The
research found no documented link between them: United Way's own Central Alabama
heat guidance lists zero Birmingham addresses and never uses the city's term
"cooling station". Stating otherwise would have implied a data connection nobody
has built.

**Eligibility travels with the address.** Pathways serves women and children
only, which the city's list does not say. For a scenario about an unhoused adult
this is a routing error waiting to happen, so the restriction appears in the
dropdown, the responder's log, and the first line of the card handed over.

**Claude does not appear in the artifact at runtime.** Deliberate. Putting a
model between a volunteer and a person in a heat emergency adds a failure mode
without removing one. The card is deterministic.

**Illustrated imagery over photographic, and no depiction of a person.** Both
styles were generated and compared. The risograph set won on palette continuity
with the artifact, and drawn hands avoid the question of whose hands those are.
A generated photo of an unhoused person would have been the one thing in the
deck that could not be sourced.

**The example's README framing changed** when it became a submission. As an
organizer's reference it said "this is a reference, not a model answer"; that
line was dropped from the submission copy, where it would read oddly.

## Outstanding

Ordered by what will bite first.

1. **Unshare the two stale card artifacts** (`c48e86a6…`, `94ccf7a6…`). Both are
   live and serving outdated versions to anyone holding a link from earlier
   today. Done from each artifact's own share menu.

2. **Resolve the team name disagreement.** This fork and the submission repo say
   the team is **4B**; upstream issue #2 says **Help the Homeless**. Both are
   public and they contradict each other. Issue #2 belongs to @mhancock537 and
   was deliberately not edited from here.

3. **Decide whether a second submission is wanted at all.** `SUBMISSIONS.md`
   says exactly one per team, and #2 already covers 4B with a different project
   (a family navigator, different primary user, own repo and live demo — built
   on the same confirm-first idea). The filled form was left unsent for this
   reason. If 4B wants this repo to be the entry instead, editing #2 is cleaner
   than filing a second issue.

4. **The deck artifact is private**, and because it declares PDF export it can
   only ever be link-shared **within the org**. Anyone outside needs the PDF.
   The PDF is committed and the GitHub links are public, so this only matters if
   someone wants the interactive canvas.

5. **The station list is a dated snapshot** (28 Aug 2026) hard-coded in the
   HTML. Nothing in the page can detect that it has gone stale. Re-check
   `birminghamal.gov/homelessness` before any reuse next summer.

6. **Nothing verifies the shared artifact from a logged-out view.** The share
   panel was read while signed in as the owner, which always shows current.
   Worth one scan of the slide-4 QR from a phone in a private tab.

7. **This fork's `main` has diverged from upstream.** A future
   `git pull upstream main` produces a merge, not a fast-forward, and
   `resources/04-homelessness-handoff.md` is the file most likely to conflict.

## Deliberately local

Nothing from this session is left uncommitted. The deck's editable source —
thirteen `.dc.html` artboards, `canvas.json`, and the images — was rescued from
the session scratchpad into `examples/04-homelessness-handoff/deck-source/`,
because the committed PDF had no source and the deck would otherwise have been
unmaintainable. The three discarded photographic alternates were not kept.
