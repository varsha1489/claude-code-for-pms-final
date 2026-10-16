# 04 · X-Ray Vision — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session the numbers showed you what happened: a
ping used to wait ninety seconds and now it waits sixty, people
missed pings they used to catch, and missing one counts the same as
turning one down — so four responders stopped hearing from us
altogether.

At the end of the session, ask Claude Code to save the prompts you
wrote yourself below — not the starter prompt. The closing slide has
the exact prompt to paste. By Module 6 this file is a prompt library
built from your own questions.

---

### 1.

Open the folder 00-rook/code/dispatch-routing/. This is the part of our software that decides who gets asked to take a job. I have never read code before and I am not going to start now. Walk me through what happens from the moment something goes wrong somewhere to the moment a responder's phone buzzes, in plain English, no jargon. Then tell me which file each step lives in.

### 2.

Interesting, how do you think this could infer what we saw from some of the data analysis?

### 3.

add this to synthesis file, but before you do so, give me the quick tl;dr on what we need to do next?

### 4.

do we have the real location/travel time data?

### 5.

do we have any information in the repo that can help us

### 6.

give me the tour for the .py files you mentioned

### 7.

Based on what we now know after looking at the code, help me create hypothesis' statements that we should go test. Rank the hypothesis based on how likely we are able to identify the root cause.

### 8.

Put this into a table. Add another column that will give me a brief 1 sentance explaination on why we recommend #1 and #2 over the others. Give me some data that will support the recommendation

### 9.

Rewrite the hypothesis statement using scientific method framing

### 10.

Yes, do both

### 11.

marcus, our engineering manager is asking,

Marcus
Aug 14, 2:47 PM
Quick one, did the change to who gets asked first apply to responders who'd already been turning jobs down, or just new ones? Can't tell from the code and don't want to guess on this. Anyone know?

### 12.

what hypothesis would otherwise confirm this?

### 13.

yes, add it to the synthesis file, re-draft the slack message to marcus with this info

### 14.

can we confirm nothing changed in the code pre 4.2? is it really just configs?

### 15.

from your point of view, what should we do next?

### 16.

Let's send the message to Marcus on this Slack channel https://product-school.slack.com/archives/C0A933JBW

### 17.

No, wrong channel

### 18.

save this recommended nest steps to the synthesis file

### 19.

Find me the part of this code that takes points off somebody when they miss a ping or turn one down. Show it to me and explain it in plain English. Then find me every single thing in this code that puts points back on.

### 20.

add this to the synthesis file

### 21.

According to my findings in the code, for someone who's gone quiet, they would need to ___.

fill in the blank

### 22.

looking at the repo, what were the specific requirements for 4.2, and was it implemented per spec?
