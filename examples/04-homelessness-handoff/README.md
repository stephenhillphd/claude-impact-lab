# Worked example — Challenge 4, SCEN-01

A completed run of the [30-minute quick start](../../resources/04-homelessness-handoff.md#30-minute-quick-start)
for Challenge 4, using scenario `SCEN-01` (adult sleeping outdoors during a heat
advisory; needs shade and water; resource type is a cooling center).

**Artifact:** [`birmingham-heat-handoff.html`](birmingham-heat-handoff.html) — open it in a browser,
or view the [published version](https://claude.ai/code/artifact/94ccf7a6-f9ac-4ce8-a9db-f4c2c8cfb091).

> **The person is fictional. The cooling stations are real.**
> `SCEN-01` comes from the event's synthetic dataset and describes nobody. The
> eleven stations, their addresses, and their hours are transcribed from the
> City of Birmingham. Keep that distinction straight when showing this to anyone.

This is a reference, not a model answer. It exists so teams can see what "shows
source and freshness, eligibility uncertainty, and human takeover" looks like
when it is built rather than described. Your artifact should look different.

## What the research turned up

The findings are the useful part of this example — more so than the card.

1. **Birmingham runs a real cooling-station program.** Eleven stations across
   five quadrants, published at
   [birminghamal.gov/homelessness](https://www.birminghamal.gov/homelessness).
   The same list was re-published unchanged in June and August 2026, so it has
   been stable through the summer.
2. **The stations are activated, not standing.** They open only when the
   temperature or heat index reaches 95°F. Being on the list and being open
   today are two different facts.
3. **The city publishes no phone number for any station.** Addresses and hours
   only. A workflow built on "call before anyone travels" gets nothing to dial
   from the canonical source.
4. **211 is not wired into the city's activations.** United Way's own Central
   Alabama heat guidance lists zero Birmingham addresses and never uses the
   city's term "cooling station". The county chain is circular: United Way points
   to Jefferson County EMA and JCDH; EMA lists no locations and points to "your
   local health department"; JCDH's site contains no occurrence of "heat" or
   "cooling" at all. **The authoritative list is municipal; 211 is the human
   fallback.** The card says this rather than implying a data link that does not
   exist.
5. **Pathways serves women and children only.** The city list gives no
   eligibility information for any station, so this had to come from
   [pathwayshome.org](https://pathwayshome.org/what-we-do-emergency-shelter/).
   For a scenario about an unhoused adult, sending the wrong person to a
   restricted site is a real harm, so eligibility travels with the address —
   into the dropdown, the responder's log, and the first line of the person's
   card.
6. **There is no 211 API.** The public directory is an embedded iCarol
   ASP.NET WebForms search: no GET endpoint, no deep-linkable query, no bulk
   download, no HSDS feed. The resource pack's "no API has been verified" claim
   is still accurate.

Finding 3 drove the design. Rather than source eleven phone numbers from eleven
other websites — any of which could be wrong, and a wrong number sends a real
person to a dead line — **everything routes through 211**, which is free,
confidential, 24/7, and serves Jefferson County. The only numbers on the page
are 211, 888-421-1266, the 898-211 text line, and 7-1-1 for relay.

### Bad data encountered

Worth knowing if you build on the same sources:

- **Firehouse Ministries: `1501 3rd Ave N` is stale.** It still circulates on
  findhelp.org and similar directories. The city and
  [firehouseshelter.com](https://firehouseshelter.com/) both give
  **626 2nd Ave. North**. Do not publish the old one.
- **`bplonline.org` is dead** (DNS does not resolve). The library's current
  official domain is `cobpl.org`, linked from the city site.
- **`uwca.org/need-help/211-call-center/` 404s.** The live path is
  `/programs/211-call-center/` — which is what the event resource pack already
  links to, so that entry is correct as-is.
- **The city and the library publish conflicting hours** for Five Points West
  and North Birmingham (the library shows Saturday hours the city does not).
  This card uses the **city's** hours throughout, since the cooling-station
  designation is the city's. The two are not silently merged.

## What it does

The card produces no handoff until someone has called 211 and recorded what they
were told. The phone call is the artifact; everything else is the confirmed
facts, written out three ways:

| For | Contents |
|---|---|
| The responder's log | Structured summary, copy-to-clipboard. Owner, follow-up deadline, what was confirmed, what is still unknown, escalation path. |
| The person being handed off | Plain language, large type. Where to go, until when, what to do if the door is locked. |
| **The field template** | A blank, printed in advance and filled in by hand on the street. See below. |

The first two are generated from the same confirmed fields, so the person-facing
card cannot state anything the call did not establish. Unconfirmed items become
"we could not check", never silence.

## The field template

There is no printer on a sidewalk. The screen version above assumes an office —
which is where the confirming phone call often happens, but not where the
handoff does. So the page also produces a **blank template designed to be
printed in a stack beforehand and filled in with a pen** while standing with
someone.

It is one sheet with a tear line across the middle:

- **Top half — the volunteer keeps it.** The 211 numbers, the 95°F activation
  rule, all eleven stations with addresses and hours in two columns, and
  write-in blanks for open-until, checked-at, initials, and the follow-up time.
  Stations are **circled, not transcribed** — faster, and an address cannot be
  copied down wrong.
- **Bottom half — torn off and handed over.** Three large ruled lines (go to,
  address, open until) and, pre-printed, what to do if the door is locked.

The part that matters: because the addresses and the 211 numbers are already
printed on it, **the template works with no signal and a dead battery** — the
two conditions the digital version cannot survive, and the two most likely to
apply to a volunteer at the end of a shift in August.

`Print blank templates` prints only the blank; the person card's `Print` button
prints only that card. A plain <kbd>⌘P</kbd> defaults to the blank template.

## How it maps to the quick start

| Quick-start step | In the artifact |
|---|---|
| 1. Pick a scenario | Stage 1 renders `SCEN-01` verbatim, labeled with its CSV column names |
| 2. Choose a primary user and moment | Two users, one moment: the responder deciding "what happens now?" and the person who has to walk there |
| 3. Build a safe handoff summary | Stage 3, from `public_resource_type`, `information_to_confirm`, `safe_next_step` |
| 4. Source, freshness, capacity flag, owner, follow-up | Retrieval provenance on the station list, freshness strip on the call, persistent banner, owner from initials, `follow_up_window` of 2 hours |
| 5. Produce a first artifact | A handoff card, with the confirmation workflow built into it |

## Design decisions worth stealing

- **A list is not a schedule.** Showing eleven stations without the 95°F
  activation rule would imply eleven open doors. The rule sits above the list,
  and confirming activation is a required field.
- **The gate is the product.** Stage 3 is visible but locked, and lists exactly
  what is still missing. A hidden section looks broken; a locked one teaches the
  rule.
- **A dead end is a real outcome.** "Not activated today" and "211 could not
  name an open station" both produce an escalation path instead of a handoff
  card. Sending someone to walk to a closed building during a heat advisory is
  the specific harm the workflow exists to prevent.
- **"They couldn't say" is not "yes".** If 211 names a station but cannot
  confirm activation, the cards are still produced — carrying an explicit
  caveat, and telling the person to call 211 before walking there.
- **Freshness expires.** The card ages visibly at 60 minutes and marks itself
  expired at the 2-hour `follow_up_window`, so a stale card is not quietly
  reused on a later shift.
- **No PII by construction.** There is no field for name, age, or story — the
  constraint is enforced by the form's shape, not by a policy sentence. Nothing
  is written to `localStorage`; closing the tab discards everything.
- **It will not print a number it cannot source.** The only numbers on the page
  are 211 and 1-888-421-1266. No station number is shown, because the city
  publishes none.

## Sources

| Fact | Source |
|---|---|
| Eleven cooling stations: names, addresses, published hours | [birminghamal.gov/homelessness](https://www.birminghamal.gov/homelessness), retrieved 28 August 2026 |
| 95°F activation rule | Same page, and the City of Birmingham release of 16 June 2026 |
| 211 numbers, text line, TTY relay, free and confidential | [uwca.org/programs/211-call-center](https://www.uwca.org/programs/211-call-center/) |
| 24/7 operation, Jefferson County coverage | [211connectsalabama.org](https://www.211connectsalabama.org/) — **not** the UWCA page, which publishes no hours |
| Pathways eligibility restriction | [pathwayshome.org](https://pathwayshome.org/what-we-do-emergency-shelter/) |
| One Roof is the Continuum of Care body | [oneroofonline.org](https://www.oneroofonline.org/) |
| `SCEN-01` scenario fields | [`homelessness-handoff-scenarios.csv`](../../resources/data/homelessness-handoff-scenarios.csv) — synthetic |

The station data is transcribed verbatim and lives in one commented array near
the top of the script in `birmingham-heat-handoff.html`. Nothing is fetched at
runtime; the page makes no network calls at all.

## Rules compliance

Checked against [`RULES.md`](../../RULES.md):

| Rule | How it is met |
|---|---|
| Synthetic data labeled | Persistent banner separates the fictional person from the real stations; `is_synthetic`/`capacity_is_not_live` shown as dataset fields; the synthetic notice appears inside both output cards, including the printed handout |
| No live capacity claimed | The 95°F rule and "a listing, not a schedule" sit above the station list; published hours are labeled as the city's hours, not today's; "what is still unknown" names capacity explicitly |
| Human review point named | The entire artifact is gated on a human calling 211, attributed to the responder's initials |
| No vulnerability or risk scoring | Nothing ranks, triages, or rates a person |
| Does not replace Coordinated Entry | Stated in the banner, both cards, and the escalation path, which routes shelter needs to One Roof |
| No writes to live systems, no scraping | Static page, no network calls, no storage. The station list was read once, by hand, from one page |

## Limits

- The station list is a snapshot taken 28 August 2026, not a feed. It will go
  stale, and nothing in the page can tell you when it has.
- **No overnight option exists.** Every listed station closes by 8 p.m. at the
  latest, most by 5 or 6. Sunday coverage is thinnest: only Harrison Recreation
  Center and Firehouse Ministries publish Sunday hours. Heat does not stop at
  closing time, and this card has nothing to offer after it.
- **Entry requirements are not published**, in either direction. No source
  states an ID, residency, or registration rule for the stations — and none
  states that there isn't one. The card never asserts "no ID needed" on its own
  authority; only a responder who asked can record that.
- Eligibility is only known for Pathways. The other ten stations may well have
  restrictions that simply are not published anywhere.
- Heat relief only. Shelter, housing, and placement run through
  [One Roof Coordinated Entry](https://www.oneroofonline.org/coordinated-entry)
  — which is also, itself, one of the eleven listed stations.
- One scenario. The other five rows in
  [`homelessness-handoff-scenarios.csv`](../../resources/data/homelessness-handoff-scenarios.csv)
  have different confirmation questions and different follow-up windows.

## Next step toward a pilot

Ask one Birmingham outreach team to use the printed handout for a week of heat
advisories and report back on one question: **did the person arrive, and was the
building open when they got there?** That is the only measure that tells you
whether the confirmation gate is doing anything. Everything else about this card
is a guess until then.

Two cheaper next steps, both of which are just asks:

- **Ask the city to publish a phone number for each station.** Their absence is
  the single biggest piece of friction this example ran into, and it is the
  reason the whole workflow has to detour through 211.
- **Ask whether 211 receives the city's activation notices.** If it does not,
  the referral that United Way's own heat page recommends — "call or text 211 to
  find the closest cooling location" — cannot actually be answered on the days
  it matters most.
