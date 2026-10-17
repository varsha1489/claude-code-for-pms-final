---
name: review-checklist
description: Use this skill whenever the user points at a brief, proposal, PRD, one-pager, or similar stakeholder-facing doc and asks you to check, review, vet, or sanity-check it before it goes out. Make sure to trigger on phrases like "check this brief", "does this pass the checklist", "review this against the checklist", "is this ready to send", "run the checklist on X", or any request to validate a doc's structure — even if the user doesn't name all four checks explicitly. Runs four structural checks: does it name an owner, does it say how success will be measured, does the scope stay the same from the opening framing to the closing ask, and does it explain the problem before proposing the fix.
---

# Review checklist

A fast structural pass over a brief — not a review of whether the idea is
good, but whether the doc is built so a reader can actually act on it. All
four checks exist because each one is a specific, common way a brief stalls
in review: nobody picks it up, nobody can tell later if it worked, the ask
quietly changed shape partway through, or the reader is asked to approve a
fix before they understand what it's fixing.

## How to run it

Read the whole doc first, then go through all four checks. For each one,
mark it **Pass**, **Fail**, or **Partial**, quote the specific line that
makes your case either way, and — for anything short of a clean Pass — give
one concrete line the author could add or change. Don't rewrite the doc
yourself; the point is to hand back a diagnosis, not a new draft.

### 1. Named owner

Look for a specific person, not a team or a pronoun ("we'll figure this
out"), attached to this doc as the one accountable for it moving forward.
A role label is fine if it's unambiguous in context (e.g. "the EM for
Dispatch") — what fails this check is no accountable person at all, or an
owner named for the *problem* but not for *this specific ask*.

Why it matters: a brief with no owner doesn't get picked up — it gets read,
nodded at, and forgotten, because nobody's job is to make it happen next.

### 2. Defined success criteria

Look for a concrete way to tell, later, whether this worked — a metric, a
threshold, a before/after comparison, a specific observable outcome. "Improve
the experience" or "reduce friction" without a number or a check-back point
fails this. A deadline alone ("ship by Q2") isn't success criteria either —
that's a schedule, not a measure of whether the thing worked once shipped.

Why it matters: without this, there's no way to close the loop later and
say whether the brief's premise held up — it just quietly stops being
checked on.

### 3. Consistent scope start to finish

Compare what the doc says it's about in its opening framing against what
it's actually asking for by the end. Flag the specific point where scope
widens (a request to fix one thing turns into a platform redesign) or
narrows (a broad problem statement resolves into a fix for only the loudest
complainer) without the doc itself acknowledging the shift.

Why it matters: scope drift between the opening and the ask is one of the
most common reasons a brief bounces back for another round — the reader
approved one thing and is being asked to sign off on another.

### 4. Problem explained before the fix

Check the order of ideas, not just whether the problem is mentioned
somewhere. The doc should establish what's happening, what evidence shows
it, and why it matters *before* it proposes what to do about it. A fix
named in the opening line or title, with the problem backfilled afterward
as justification, fails this — even if the problem section is thorough once
you get to it.

Why it matters: a reader asked to approve a fix before they understand the
problem either rubber-stamps it without real buy-in, or rejects it on gut
feel — neither is a real review. Problem-first gives them something to
actually evaluate the fix against.

## Output format

```markdown
# Review checklist: <doc name>

## 1. Named owner — Pass / Fail / Partial
<quoted evidence>
<fix suggestion, if not a clean Pass>

## 2. Defined success criteria — Pass / Fail / Partial
...

## 3. Consistent scope start to finish — Pass / Fail / Partial
...

## 4. Problem explained before the fix — Pass / Fail / Partial
...

## Overall
<one or two sentences — is this ready to send as-is, or what's the one
thing to fix first>
```

If the doc is genuinely strong on all four, say so plainly rather than
manufacturing a nitpick to fill the template.
