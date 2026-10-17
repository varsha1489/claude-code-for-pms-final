# Supply requisition queue — brief

## Part 1: What was actually asked

- **The literal request:** "Supply's requisition approval queue is way
  slower than usual and Halloran's complaining — can you figure out what's
  going on and put together a recommendation?"
- **Framing worth flagging back:** the request assumes two things that
  this repo cannot confirm — that the queue has gotten *slower* (vs. being
  chronically slow), and that "usual" is a known baseline. Those are
  reasonable things to believe from a handler complaint, but they're not
  yet facts. See synthesis.md for why.
- **What they didn't ask for:** they didn't ask me to design or ship a
  Supply fix — Supply is not my product (I'm the Dispatch PM) and this
  repo's own working notes already say this one is "not mine to fix; worth
  passing along." I'm reading that as: diagnose what's knowable, and
  produce a recommendation that's realistic about who acts on it.

## Part 2: The answer

**Bottom line:** There's exactly one piece of evidence behind this
complaint in the entire repo — Halloran's 5 Sep interview, where he
described an 11-day wait for a cracked vest plate and said the priority
field doesn't change queue order because everything sits in one
undifferentiated queue. There's no Supply ticket data, no Supply queue
metrics, and no Supply code to check any of it against. Separately,
"Requisition approval chains" is already a **committed** roadmap item for
Supply 4.3 — so the fix this complaint is asking for is already in motion;
what's missing is confirmation that it actually addresses triage-by-urgency,
and real data to know if "slower than usual" is true.

**Recommendation:**
1. Send Halloran's full account to whoever owns Supply product (route
   through Helen — no Supply PM appears in the team directory) now, while
   4.3 is still being scoped, instead of waiting for a future regroup.
2. Request Supply's equivalent of a queue-time export (submitted →
   quartermaster decision, with priority flag and timestamp) from Supply's
   data owner, so "way slower than usual" can be checked instead of assumed.
3. Don't propose a specific fix design as a final answer — use the attached
   prototype only as an illustration of the problem to make Halloran's case
   vivid, not as a spec, since 4.3 may already cover this differently.
4. Loop in Halloran's other two unprompted Supply complaints (no feedback
   on field failure reports; weak catalog search) in the same hand-off —
   same source, same moment, easy to lose if split out.

**Confidence: 25/100** that the specific mechanism ("single queue + a
priority field with no real effect") is the actual cause of what Halloran
experienced — the account itself is credible, but it's one person,
unverified against data or code. See synthesis.md for the full history of
that number and what would move it.

**What this deliberately doesn't settle:** whether Supply's 4.3 item will
actually fix this, whether the queue has genuinely slowed vs. always being
slow, and what Supply's broader backlog priorities are. Those are Supply
product decisions, not something to resolve from a Dispatch vantage point
with no access to Supply's own data.

**Simulated feedback (round 1, in character as Halloran — see
`simulated-feedback-round-1.md`):** generally positive on the tiering idea
but skeptical it's real unless it's proven to change actual signature
speed, not just display order, and flagged it could be gamed if urgency is
self-reported. His line — "flagged isn't fixed" — echoes a gap already
seen in the separate Dispatch prototype work in this repo, suggesting it's
a recurring failure mode worth watching for generally, not just here.
Two concrete questions worth attaching to the hand-off: (1) who/what
assigns the urgency tier, and can it be gamed; (2) does tiering actually
change approval time or only sort order. This is simulated feedback on an
illustrative mockup — it does not change the 25/100 confidence number.

**Prototype attached:** `prototype-requisition-triage.html` — an
illustrative mockup of a priority-tiered queue (vs. today's single FIFO
queue), built only from Halloran's one account. Every number in it is
marked as assumed. It exists to make the problem concrete in a
conversation, not as a validated design — building it does not raise the
25/100 confidence above, per the skill's own rule that illustrative
prototypes with made-up inputs don't move confidence.
