# Worked example — Challenge 4, SCEN-01

A completed run of the [30-minute quick start](../../resources/04-homelessness-handoff.md#30-minute-quick-start)
for Challenge 4, using scenario `SCEN-01` (adult sleeping outdoors during a heat
advisory; needs shade and water; resource type is a cooling center).

**Artifact:** [`heat-handoff-card.html`](heat-handoff-card.html) — open it in a browser,
or view the [published version](https://claude.ai/code/artifact/c48e86a6-7370-434a-82e1-ad798942fb7b).

This is a reference, not a model answer. It exists so teams can see what "shows
source and freshness, eligibility uncertainty, and human takeover" looks like
when it is built rather than described. Your artifact should look different.

## What it does

The hard part of this challenge is not looking up a resource. It is that **no
Birmingham source publishes live cooling-center capacity** — HUD PIT/HIC counts
are historical and system-level, and the city and 211 directories are listings,
not availability feeds. Any page that just displays a resource list is quietly
claiming something it cannot know.

So the card inverts the usual shape. It produces no handoff at all until a person
has called the center and recorded what they were told. The phone call is the
artifact; everything else is the confirmed facts, written out twice:

| For | Contents |
|---|---|
| The responder's log | Structured summary, copy-to-clipboard. Owner, follow-up deadline, what was confirmed, what is still unknown, escalation path. |
| The person being handed off | Plain language, large type, print to a handout. Where to go, until when, what to do if the door is locked. |

Both are generated from the same confirmed fields, so the person-facing card
cannot state anything the call did not establish. Unconfirmed items become "we
could not check", never silence.

## How it maps to the quick start

| Quick-start step | In the artifact |
|---|---|
| 1. Pick a scenario | Stage 1 renders `SCEN-01` verbatim, labeled with its CSV column names |
| 2. Choose a primary user and moment | Two users, one moment: the responder deciding "what happens now?" and the person who has to walk there |
| 3. Build a safe handoff summary | Stage 3, from `public_resource_type`, `information_to_confirm`, `safe_next_step` |
| 4. Source, freshness, capacity flag, owner, follow-up | Freshness strip, persistent capacity banner, owner derived from initials, `follow_up_window` of 2 hours |
| 5. Produce a first artifact | A handoff card, with the confirmation workflow built into it |

## Design decisions worth stealing

- **The gate is the product.** Stage 3 is visible but locked, and lists exactly
  what is still missing. A hidden section looks broken; a locked one teaches the
  rule.
- **"No answer" is not a confirmation.** Answering *No* or *No answer* produces
  an escalation path instead of a handoff card. Sending someone to walk to a
  closed building during a heat advisory is the specific harm the workflow
  exists to prevent, so the card refuses to be produced.
- **Freshness expires.** The card ages visibly at 60 minutes and marks itself
  expired at the 2-hour `follow_up_window`, so a stale card is not quietly
  reused on a later shift.
- **No PII by construction.** There is no field for name, age, or story — the
  constraint is enforced by the form's shape, not by a policy sentence. Nothing
  is written to `localStorage`; closing the tab discards everything.
- **It will not invent a phone number.** Fields start empty and link out to the
  real public directories. The demo button fills clearly fictional values
  (`Fictional Cooling Center A`, `555-0100`) so the page never prints a
  Birmingham address or number someone might actually dial.

## Rules compliance

Checked against [`RULES.md`](../../RULES.md):

| Rule | How it is met |
|---|---|
| Synthetic data labeled | Persistent banner, `is_synthetic`/`capacity_is_not_live` shown as dataset fields, and a synthetic notice inside both output cards — including the printed handout |
| No live capacity claimed | "What is still unknown" names seat availability explicitly; the banner is non-dismissible |
| Human review point named | The entire artifact is gated on a human phone call, attributed to the responder's initials |
| No vulnerability or risk scoring | Nothing ranks, triages, or rates a person |
| Does not replace Coordinated Entry | Stated in the banner, both cards, and the escalation path, which routes shelter needs to One Roof |
| No writes to live systems | Static page, no network calls, no storage |

## Limits

- `SCEN-01` is fictional and so are the demo values. Real use means typing in a
  real listing from the [city directory](https://cobcd.com/communityresources/)
  or 211.
- Heat relief only. Shelter, housing, and placement run through
  [One Roof Coordinated Entry](https://www.oneroofonline.org/coordinated-entry).
- One scenario. The other five rows in
  [`homelessness-handoff-scenarios.csv`](../../resources/data/homelessness-handoff-scenarios.csv)
  have different confirmation questions and different follow-up windows; the
  gate would need different fields for each.

## Next step toward a pilot

Ask one Birmingham outreach team to use the printed handout for a week of heat
advisories and report back on one question: **did the person arrive, and was the
building open when they got there?** That is the only measure that tells you
whether the confirmation gate is doing anything. Everything else about this card
is a guess until then.
