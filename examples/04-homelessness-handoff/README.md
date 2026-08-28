# Worked example — Challenge 4, SCEN-01

A completed run of the [30-minute quick start](../../resources/04-homelessness-handoff.md#30-minute-quick-start)
for Challenge 4, using scenario `SCEN-01` (adult sleeping outdoors during a heat
advisory; needs shade and water; resource type is a cooling center).

**Artifact:** [`birmingham-cooling-handoff.html`](birmingham-cooling-handoff.html), or the
[published version](https://claude.ai/code/artifact/923e8ee4-00b4-48bf-b834-44d50b1eb14f).

**To hand round:** [`birmingham-cooling-field-card.pdf`](birmingham-cooling-field-card.pdf)
is the blank template, ready to print in a stack.
[`what-happens-now.pdf`](what-happens-now.pdf) is a six-slide setup for a live
walkthrough: who it is for, the three things the city's list does not tell you,
then straight into the artifact itself.

> **The person is fictional. The cooling stations are real.**
> `SCEN-01` comes from the event's synthetic dataset and describes nobody. The
> eleven stations, their addresses, and their hours are transcribed from the
> City of Birmingham. Keep that straight when showing this to anyone.

This is a reference, not a model answer. It exists so teams can see what "shows
source and freshness, eligibility uncertainty, and human takeover" looks like
once someone has actually built it. Your artifact should look different.

## What the research turned up

Honestly, the research was more useful than the card.

1. **Birmingham runs a real cooling-station program.** Eleven stations across
   five quadrants, published at
   [birminghamal.gov/homelessness](https://www.birminghamal.gov/homelessness).
   The same list went out unchanged in June and again in August 2026, so it has
   held steady all summer.
2. **The stations are activated, not standing.** They open only when the
   temperature or heat index reaches 95°F. So a station can be on the list and
   still be shut.
3. **The city publishes no phone number for any station.** Addresses and hours,
   nothing else. A workflow built on "call before anyone travels" gets nothing
   to dial from the source that matters.
4. **211 is not wired into the city's activations.** United Way's own Central
   Alabama heat guidance lists zero Birmingham addresses and never uses the
   city's term "cooling station". Following the county trail just loops: United
   Way points to Jefferson County EMA and JCDH, EMA lists no locations and says
   to contact your local health department, and JCDH's site does not contain the
   word "heat" or "cooling" anywhere. The real list is municipal. 211 is the
   human you call when the list is not enough. The card says so plainly instead
   of implying a data link that nobody has built.
5. **Pathways serves women and children only.** The city list says nothing about
   eligibility for any station, so this had to come from
   [pathwayshome.org](https://pathwayshome.org/what-we-do-emergency-shelter/).
   In a scenario about an unhoused adult, routing the wrong person to a
   restricted site does real damage, so the restriction travels with the address
   into the dropdown, the responder's log, and the top of the person's card.
6. **There is no 211 API.** The public directory is an embedded iCarol
   ASP.NET WebForms search with no GET endpoint, no linkable query, no bulk
   download, no HSDS feed. The resource pack's "no API has been verified" claim
   is still accurate.

Finding 3 is what shaped the build. Sourcing eleven phone numbers from eleven
other websites would mean eleven chances to publish a wrong one, and a wrong
number sends a real person to a dead line. So everything routes through 211,
which is free, confidential, staffed around the clock, and covers Jefferson
County. The only numbers anywhere on the page are 211, 888-421-1266, the
898-211 text line, and 7-1-1 for relay.

### Bad data encountered

If you build on the same sources, watch for these.

Firehouse Ministries still shows up as `1501 3rd Ave N` on findhelp.org and
similar directories. That address is stale. The city and
[firehouseshelter.com](https://firehouseshelter.com/) both give **626 2nd Ave.
North**.

`bplonline.org` no longer resolves. The library's current domain is `cobpl.org`,
linked from the city site.

`uwca.org/need-help/211-call-center/` returns a 404. The live path is
`/programs/211-call-center/`, which is what the event resource pack already
links to, so that entry needs no change.

The city and the library publish different hours for Five Points West and North
Birmingham, with the library showing Saturday hours the city does not. This card
uses the city's hours throughout, since the cooling-station designation is the
city's to make. The two sets are not quietly merged.

## What it does

The card produces no handoff at all until someone has called 211 and written
down what they were told. Everything else on the page is just those confirmed
facts, rendered three ways.

| For | Contents |
|---|---|
| The responder's log | Structured summary, copy-to-clipboard. Owner, follow-up deadline, what was confirmed, what is still unknown, escalation path. |
| The person being handed off | Plain language, large type. Where to go, until when, what to do if the door is locked. |
| The field template | A blank, printed in advance and filled in by hand on the street. See below. |

The first two are built from the same confirmed fields, so the person-facing
card cannot claim anything the call did not establish. Where something went
unconfirmed it says "we could not check" rather than going quiet.

## The field template

There is no printer on a sidewalk. The screen version assumes an office, which
is often where the confirming phone call happens, but never where the handoff
does. So the page also produces a blank template you print in a stack
beforehand and fill in with a pen while standing with someone.

It is one sheet with a tear line across the middle.

The volunteer keeps the top half: the 211 numbers, the 95°F activation rule, all
eleven stations with addresses and hours in two columns, and write-in blanks for
open-until, checked-at, initials, and the follow-up time. Stations get circled
rather than copied out, which is quicker with a pen and takes transcription
errors off the table.

The bottom half tears off and goes to the person. Three large ruled lines (go
to, address, open until), and what to do if the door is locked, already printed.

Because the addresses and the numbers are pre-printed, the template still works
with no signal and a dead battery. Those are the two conditions the digital
version cannot survive, and they are exactly what a volunteer hits at the end of
a shift in August.

`Print blank templates` prints only the blank. The person card's `Print` button
prints only that card. A plain <kbd>⌘P</kbd> gives you the blank. If you just
want paper without opening the page,
[`birmingham-cooling-field-card.pdf`](birmingham-cooling-field-card.pdf) is the
same blank, already rendered.

## How it maps to the quick start

| Quick-start step | In the artifact |
|---|---|
| 1. Pick a scenario | Stage 1 renders `SCEN-01` verbatim, labeled with its CSV column names |
| 2. Choose a primary user and moment | Two users, one moment: the responder deciding "what happens now?" and the person who has to walk there |
| 3. Build a safe handoff summary | Stage 3, from `public_resource_type`, `information_to_confirm`, `safe_next_step` |
| 4. Source, freshness, capacity flag, owner, follow-up | Retrieval provenance on the station list, freshness strip on the call, persistent banner, owner from initials, `follow_up_window` of 2 hours |
| 5. Produce a first artifact | A handoff card, with the confirmation workflow built into it |

## Design decisions

Showing eleven stations without the 95°F rule attached would imply eleven open
doors, so the rule sits above the list and confirming it is a required field.

Stage 3 stays visible but locked, and spells out what is still missing. Hiding
it would just look broken; locking it teaches the rule instead.

Two answers end the workflow rather than degrading it. If 211 says stations are
not activated, or cannot name one that is open, the card produces an escalation
path and no handoff. Walking someone to a locked building during a heat advisory
is the exact harm this thing exists to prevent.

"They couldn't say" gets treated as its own answer, not as a yes. The cards
still appear, but they carry a caveat and tell the person to call 211 before
setting out.

The card ages visibly at 60 minutes and marks itself expired at the two-hour
`follow_up_window`, so nobody quietly reuses a stale one on a later shift.

There is no field for a name, an age, or a story. The form's shape is what
prevents PII, not a line of policy text. Nothing goes to `localStorage` either,
so closing the tab discards the lot.

The page will not print a number it cannot source. No station number appears,
because the city publishes none.

## Sources

| Fact | Source |
|---|---|
| Eleven cooling stations: names, addresses, published hours | [birminghamal.gov/homelessness](https://www.birminghamal.gov/homelessness), retrieved 28 August 2026 |
| 95°F activation rule | Same page, and the City of Birmingham release of 16 June 2026 |
| 211 numbers, text line, TTY relay, free and confidential | [uwca.org/programs/211-call-center](https://www.uwca.org/programs/211-call-center/) |
| Round-the-clock operation, Jefferson County coverage | [211connectsalabama.org](https://www.211connectsalabama.org/), **not** the UWCA page, which publishes no hours |
| Pathways eligibility restriction | [pathwayshome.org](https://pathwayshome.org/what-we-do-emergency-shelter/) |
| One Roof is the Continuum of Care body | [oneroofonline.org](https://www.oneroofonline.org/) |
| `SCEN-01` scenario fields | [`homelessness-handoff-scenarios.csv`](../../resources/data/homelessness-handoff-scenarios.csv), synthetic |

The station data is transcribed verbatim into one commented array near the top
of the script in `birmingham-cooling-handoff.html`. Nothing is fetched at runtime.
The page makes no network calls at all.

## Rules compliance

Checked against [`RULES.md`](../../RULES.md):

| Rule | How it is met |
|---|---|
| Synthetic data labeled | Persistent banner separates the fictional person from the real stations; `is_synthetic`/`capacity_is_not_live` shown as dataset fields; the synthetic notice appears inside both output cards, including the printed handout |
| No live capacity claimed | The 95°F rule and "a listing, not a schedule" sit above the station list; published hours are labeled as the city's hours, not today's; "what is still unknown" names capacity explicitly |
| Human review point named | The whole artifact is gated on a human calling 211, attributed to the responder's initials |
| No vulnerability or risk scoring | Nothing ranks, triages, or rates a person |
| Does not replace Coordinated Entry | Stated in the banner, both cards, and the escalation path, which routes shelter needs to One Roof |
| No writes to live systems, no scraping | Static page, no network calls, no storage. The station list was read once, by hand, from one page |

## Limits

The station list is a snapshot from 28 August 2026, not a feed. It will go
stale, and nothing in the page can tell you when that has happened.

There is no overnight option. Every listed station shuts by 8 p.m. at the
latest and most by 5 or 6. Sundays are worst: only Harrison Recreation Center
and Firehouse Ministries publish Sunday hours at all. Heat does not stop when
the doors lock, and this card has nothing to offer once they do.

Entry requirements are not published either way. No source states an ID,
residency, or registration rule for the stations, and no source rules one out.
The card never asserts "no ID needed" on its own authority. Only a responder who
asked can record that.

Eligibility is known for exactly one station. The other ten may well have
restrictions that simply are not published anywhere.

This is heat relief and nothing more. Shelter and housing placement run through
[One Roof Coordinated Entry](https://www.oneroofonline.org/coordinated-entry),
which is also one of the eleven listed stations.

It covers one scenario. The other five rows in
[`homelessness-handoff-scenarios.csv`](../../resources/data/homelessness-handoff-scenarios.csv)
ask different confirmation questions on different follow-up windows.

## Next step toward a pilot

Give the printed handout to one Birmingham outreach team for a week of heat
advisories and ask them one question afterwards: did the person arrive, and was
the building open when they got there? Nothing else tells you whether the
confirmation gate does any good. Until someone runs that week, the rest of this
is a guess.

Two cheaper steps, both just asks:

Ask the city to publish a phone number for each station. Their absence caused
more friction than anything else here, and it is the only reason the workflow
detours through 211 at all.

Ask whether 211 receives the city's activation notices. If it does not, then the
referral United Way's own heat page recommends, "call or text 211 to find the
closest cooling location", cannot be answered on the days it counts.
