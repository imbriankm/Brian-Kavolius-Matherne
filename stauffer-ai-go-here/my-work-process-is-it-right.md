# My work process: is it right?

I'd like feedback on **how** I'm using AI, not only on what I produce. Claude (Claude Code) wrote this summary at my request, from our 2026-10-08 session. I read it before committing.

## How a session works

1. **I set the goals and the order.** This session: take in your feedback, start Stage 2, work on the paper.
2. **Claude explains, I decide.** I don't have a math background. When a concept stops me (falsifiability, a tipping point versus a bed count, what a spec is), I ask Claude to explain it at a lower level, often with a comparison from my teaching studio. Then I make the call.
3. **I answer in chat, and Claude places my words.** For anything substantive, Claude asks me questions. I answer in my own words, and Claude puts the answers into the file and formats them. When Claude adds a connecting phrase or a label of its own, it tells me, and I keep it, change it, or remove it. I've encouraged Claude to use my own language as much as possible. This is my writing; Claude is formatting it.
4. **Claude does the mechanical work.** Front matter, headings, links, folder setup, git commits, and now opening PRs.
5. **I check before committing.** Claude shows me each change before it is committed.

In my words: "You're doing the organization, I'm doing the thinking, and you're refining the math." Working in small chunks, where I answer questions and do short pieces of work that Claude tells me about, is working well for me.

## Example from this session: my Stage 1.1 falsification

- Your review said my falsification had no threshold and no model output.
- Claude explained what a threshold is, and the difference between a tipping point (where one more tomato bed stops paying) and a bed count (how many tomato beds the Solver picks).
- I chose a 20% line on my guess of 14 tomato beds, then refined it in stages:
  - My guesses are directional, so 11 or fewer means directionally right and 16 or more means directionally wrong.
  - My claim is comparative: tomatoes are the best crop at low numbers and worse than carrots and mesclun at high numbers.
- I did the arithmetic (20% of 14 is about 3 beds). Claude checked it, pointed out where my new line clashed with older text, and placed my wording.
- Claude also caught that I'd written "60/64" beds. 60 is the published answer, so it shouldn't go into a pre-model brief. I had meant 64 beds against caps that add up to 70.
- **A process error to flag:** earlier in the session, while summarizing the Stage 2 page, Claude told me the published check figures (10 tomato / 20 carrot / 30 mesclun). That was **before** I set my threshold. My 14-bed prediction was committed weeks earlier, but I did know the answer when I chose the 12–15 / 11-or-fewer / 16-or-more lines. From now on, Claude should not tell me check figures before I've committed a prediction.

## Where I'm unsure the line is right

_Draft — I'll revise this section at the end of this block of work. I think it undersells how much of the writing is mine._

1. **Stage 2 says "you write the spec."** I can write the Purpose. Without a math background, I don't think I can write the Inputs, Structure or Conventions from a blank page. My plan is to have Claude ask me plain-language questions, so that each choice in the spec is my decision. For example: must every bed be planted, how far past each cap the schedules should run, and which outputs I need to test my own prediction? Claude would lay the case's given numbers out in the inputs table and propose names for me to approve. **Is that acceptable, or does it cross into Claude writing the spec?**
2. **Claude builds the workbook and explains the math to me.** I'll run Solver and do the audit checks myself, with Claude walking me through them. Is that the right division?

## Getting your feedback

As you asked, I'll open a PR with you tagged and leave it open, instead of emailing. This file is part of that PR.
