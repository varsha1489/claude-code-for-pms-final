# 1:1 with Ravi Menon — agenda

Written 14 October 2026, ahead of the team regroup on 4.2.

**Who:** Ravi Menon, Data Analyst (Dispatch & Supply), Singapore. Requests
normally go through #data.

**Purpose:** Understand the 4.2 acceptance numbers before the regroup —
not to confirm a conclusion, to understand the data itself.

---

## 1. Intro and what you're trying to do (3 min)
- You're the new Dispatch PM, getting ready for the 4.2 regroup.
- You aren't asking him to confirm a conclusion. You want to understand
  the numbers first.

## 2. Where `callout-history.csv` came from (5 min)
- Is this the official weekly export, or a rougher pull? Who ran it, and when?
- What exactly does `pings_sent` count?
  - One per offer, or one per responder asked?
  - Does it include re-offers? 4.2 fixed a duplicate push on re-offer,
    which could inflate counts before the fix.
- Does `pings_taken` mean accepted? Where do declines and timeouts go?
- **Why it matters:** this decides whether the ticket mismatch is a data
  problem or a perception problem. Nine responders whose handlers filed
  "gone quiet" tickets show steady or rising offers in the file. Nightwell
  is the clearest case: her ticket says nothing in ten days, and the file
  shows 16–21 offers a week.

## 3. How the headline gets computed (5 min)
- Is weekly acceptance rate pooled (all accepts ÷ all offers) or an
  average of per-responder rates?
  - Pooled gives 0.73 for the week of 31 Aug; the average gives 0.59.
  - Which one goes in the weekly report, and which one has Helen seen?
- Four responders (Farlight, Meteor Mite, The Undertow, Vesper) went
  from about 12 offers a week to 0–1. Does the report show them? In the
  pooled number they've dropped out of the denominator.
- Could he add a view of how many responders got zero offers that week?

## 4. What the file can't tell us (7 min)
Ask whether these exist and whether he can pull them:
- **Declines vs. timeouts**, separately. The glossary says the data
  distinguishes them, but the CSV doesn't.
- **Time-to-accept**, the second metric, by week.
- **Rank position** each responder held when asked, or at least how
  often they were asked first.
- **Region or travel-time bands**, to test whether the four are simply
  far from incidents. No names or identities, only bands.
- **Incident volume**, as opposed to offers sent. Offers fell only
  slightly, but offers aren't incidents.
- **Coverage gaps** — incidents with no available responder carrying
  the needed tag.

## 5. The seasonality question (4 min)
- Is there data for August in previous years? Priya said "August is
  always soft", but our file starts on 29 June.
- If it exists, what did acceptance and offer volume look like in
  August and September of past years?

## 6. What's missing and what's next (4 min)
- The file ends at the week of 31 Aug. Can he share September and October?
- Can the weekly series be re-run the same way after any fix, so we can
  compare before and after?
- Agree on what he'll send and by when, and how long #data requests
  typically take.

---

## Ready-to-send data request (16 Oct 2026)
This is the single highest-priority ask from the full investigation — ask
for this even if there's no time for anything else in item 4.

> Hi Ravi — ahead of the 4.2 regroup, I'd like to test one specific theory.
> The 4.2 routing weights lean harder on proximity now (60% of the ranking
> score), and anyone 45+ minutes from an incident scores zero on that part
> — a hard cutoff, not a gradual one. Four responders have seen their
> offer volume collapse to near-zero since the release and never recovered.
>
> Could you pull average travel-time (or distance/region bands — no need
> for exact locations) to assigned incidents, for each of these 16
> responders, from before the 12 Aug release? I'm especially looking at:
> Farlight, Meteor Mite, The Undertow, and Vesper (the four who collapsed),
> plus Corporal Ashgrove and Halfmoon (declining, possibly heading the same
> way) — compared against the other 10 as a baseline.
>
> If the four collapsed responders are consistently farther out than the
> rest, that confirms proximity as the mechanism. If they're not, it rules
> it out and points us back toward the acceptance-history scoring instead.
> Either answer moves this forward — happy to talk through format, bands
> are completely fine if exact figures raise any concerns.

**Why this one matters most:** every other open question in the
investigation either depends on this answer or is secondary to it (see the
ranked hypothesis table in `notes/responder-priority-and-pov.md`).

## Keep in mind
- **Don't lead him.** Ask what the numbers are, not whether the timeout
  broke things.
- **No identities.** The confidentiality rule applies here too — ask for
  aggregates and bands, never anything that points back to a person.
  Check Security Policy 4.1 before requesting new cuts; it isn't in this
  repo.
- **Bring the one table that makes the case** — the four responders vs.
  the other twelve, week by week.

---
*Source: [CLAUDE.md](../CLAUDE.md) working context, `00-rook/data/callout-history.csv`,
and the 4.2 ticket/interview review. See CLAUDE.md for the fuller picture
of where Dispatch stands.*
