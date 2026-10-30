# Prompt Log

A running record of meaningful AI sessions in this repository: what I asked, what the AI got wrong, and how I caught it.

## 2026-10-29 — Stage 1.2 audit: corrected inputs, Solver re-run, tolerance bands, audit findings

**Asked:** Continue last night's work after the workspace move, then walk me to a stopping point for Stage 1.2 so I can push for feedback.

**What it did:** Found last night's session in the archive and reviewed it. Edited the saved `model.xlsx` file directly (so my Solver setups survived) to enter carrot hours as 50 ÷ 60 and the wages as season pay ÷ paid hours. Walked me through re-running Solver one step at a time: Setup 1 from two starting points, Setup 2, and the What if tab. Checked the Checks tab and the Schedules tab against the results, added `Tomato_LastPossibleProfitableBed`, `Carrot_LastPossibleProfitableBed` and `Mesclun_LastPossibleProfitableBed` to the Schedules tab (row 3), put my six audit findings into the spec as I wrote them, rewrote the capability README, and started a tally file of times I said I didn't understand and times the AI was wrong.

**Who did what:** I decided that the model is the farmer's reusable tool, so it uses the exact wages (decision 2b); that bed counts must match exactly and profit within ±$500; and that the report gives both the last profitable bed and the last one inside the cap. I ran Solver myself and wrote all six audit findings. Claude edited the workbook, explained the numbers, and placed my words in the spec.

**Results on the corrected inputs:** Setup 1 gives 10 tomato, 20 carrot and 30 mesclun beds and $42,762 from both starting points. Setup 2 gives 7 tomato beds and $1,626, and the Schedules tab's `Tomato_BreakEvenBeds` also says 7. The What if tab keeps the same mix, with slightly lower profit. The Checks tab shows no FAIL.

**What I caught / changed:**

- Claude asked me decision 2b as if it were new; I said we had already discussed it, and it found the archived session.
- Claude's spec draft said the exact wage rate "pays me exactly $50,000." The model charges my 720 hours, which is $25,000.00. Corrected.
- Claude's explanations of the check figures and of the carrot bed that costs less than the one before it (bed 17) lost me several times; it re-explained with the Schedules numbers until I followed.
- Claude mislabeled the best mix as 10 carrot / 20 mesclun / 30 tomato beds and used that to say the Optimizer contradicted my reason for reporting beds 26 and 37. That was wrong. The mix is 10 tomato / 20 carrot / 30 mesclun. Checking it showed that 26 and 37 are past the caps (20 and 30), so I changed what the report says.
- Both rounding changes (carrot hours and wages) were made after I saw the professor's check figure, and the audit says so.

## 2026-10-27 — Stage 1.2 spec: Professor Stauffer's 2026-10-17 review

**Asked:** Pull, help me address Professor Stauffer's feedback, then work through the next session.

**What it did:** Found his review (PR #6, answering my PR #5), merged both PRs with merge commits at my request, and pulled. Fetched the case README from the course repository so I could read the labor-function sentence he pointed to. Then took his spec fixes one at a time: placed my resolution of the labor-function disagreement in the spec, wrote marginal cost in named notation (`Tomato_MC(q)` and the rest), suggested named result cells for every Output and a second Solver setup for `BreakEvenBeds`, added a memo note, and fixed "five tabs" to "six".

**Who did what:** I resolved the disagreement: all beds are planted at the start of the season, so "one more bed" means planning a season with one more bed. I approved the result-cell names and the second Solver setup. Claude wrote the spec paragraph from my words and my PR #5 description, wrote the marginal cost notation and the cell names, and wrote the memo note's wording.

**What I caught / changed:**

- Claude first asked me Stauffer's question without explaining it, and I didn't understand it. It explained that a bed's 99 hours are spread over 36 weeks, so a second bed changes the weeks still to come, not the past.
- Claude's marginal cost draft said each crop is costed "as if it were the only crop planted," which I thought contradicted the bug logic. Claude reworded it: that rule only decides which wage each hour is paid on the Schedules tab, and Solver costs the whole farm together.
- I asked whether diminishing returns should cross crops. The case's formula counts each crop separately, so the main model keeps it; the point went into the Stage 3 memo notes.
- I left the width of each check-figure band (item 5) for when the workbook runs.

**Build (same session, into 2026-10-28):** Claude built `model.xlsx` from the spec and checked it in Excel (no error cells; hand checks and an independent recalculation matched). I ran Solver myself: Setup 1 from 0/0/0 and from 20/0/0 (same mix both times), Setup 2, and the What if tab, plus the zero-fatigue test and the Farm Profit Lab check on tomato bed 8. Excel's Solver misread a constraint pointing at the Inputs tab (`Mesclun_Cap` came back as `LaborCost`), so Claude copied the limits onto the Optimizer and What if tabs, pre-loaded the Solver setups, and wrote the reason into the spec.

**Decisions so far:** report each crop's last profitable bed, not the first crossing (decision 1); enter carrot labor as 50 minutes a week instead of 0.833 hours (decision 2a). Still open: the wages (2b), which depends on whether the model is the farmer's reusable tool or only this case's answer.

**What I caught / changed (build):**

- Claude slipped back into "planting one bed at a time" language after we had settled that all beds are planted at the start of the season. It corrected the wording.
- Claude gave me three decisions at once; I asked for one at a time with context, as we had agreed.
- Claude told me my wage was $25,000 for 720 hours; the case says $50,000 for 1,440 paid hours, 720 of them in the field. Same hourly rate, wrong basis; it corrected itself from the case page.

## 2026-10-14 — Stage 1.2 spec: calculation logic, validation rules, What-if tab

**Asked:** A status update on my projects, and help understanding the push/pull/PR workflow with Professor Stauffer. Then merge his 2026-10-12 review (PR #4) and interview me to finish the Stage 1.2 spec.

**What it did:** Explained the two directions of pull requests: mine to him for feedback, and his review PRs to me, which I merge and then pull. He had answered PR #3's questions inside PR #4. Merged PR #4 with a merge commit and pulled it. Read the Stage 2 case page for the labor formula, then asked me questions and placed my answers into `capabilities/marginal-analysis/spec.md`: labor hours, the hours one extra bed adds, labor cost (my hours first, then the blended rate), revenue, fertilizer, profit, the Solver rules, marginal cost, the Schedules tab, and validation rules. At my request, it added a README tab and a "What if workers got tired throughout the week?" tab, a side interest project using my own fatigue rule (1% less efficient each hour, and 1% more each day until the weekend). It also started `docs/decisions/perfect-competition-memo-notes.md` for Stage 3 ideas.

**Who did what:** I made every decision in the spec: the assumptions, the printed labor rates, whole beds, the 10% tolerance, and the What-if rule. I did the hand checks on a calculator (one tomato bed is 99 hours, two are 217.8). Claude explained the math, wrote the formula notation and most of the connecting sentences, and placed my answers. The wording doesn't sound exactly like me, but I approved it.

**What I caught / changed:**

- Claude's studio comparison for diminishing returns was wrong: in my studio, more students make me more efficient. I explained it with soil fatigue and walking further instead.
- I disagree with Claude that adding a bed raises the hours of the beds already planted ("time travel isn't possible"). We're asking Professor Stauffer to resolve it. The numbers come out the same either way.
- Claude kept bringing up the published check figures (10/20/30, $42,762), and wrote about them in a way I read as my prediction. I had wanted not to know the answers. I asked for a reset, and we separated my prediction (14/20/30), my falsification test, and the check figures, which only test whether the workbook was built correctly.
- Claude asked too many questions at once and mixed up terms. For example, I read its fertilizer question as being about crops' diminishing returns. It now asks one question at a time.
- Claude recommended an exact match for the bed mix; I kept 10% for every check figure (±1, ±2, ±3 beds).
- Claude changed my "labor hours will be cut in half" to "the cost of a labor hour is cut in half," which is what I meant.

## 2026-10-08 — Review feedback, Stage 1.2 spec start, paper prewriting

**Asked:** Plan a session to take in Professor Stauffer's feedback, start Stage 1.2, and work on the paper. Set up the feedback workflow he asked for (open PR tagged @adamwstauffer). I don't have a math background, so I asked Claude to explain the math and handle the build, while I do the thinking.

**What it did:** Pulled his 2026-09-28 review, which was on GitHub but not on my computer. Installed the GitHub CLI and added the feedback workflow to AGENTS.md. Made the mechanical brief fixes (front matter, heading, engagement index). Explained falsifiability, tipping point versus bed count, and what a spec is, then placed my answers into the brief, `capabilities/marginal-analysis/spec.md` (Purpose, Inputs, Structure, Conventions, Outputs), and `stauffer-ai-go-here/paper-prewriting.md`. Wrote `my-work-process-is-it-right.md` at my request.

**What I caught / changed:** The process doc credited Claude with arithmetic I did (20% of 14 is about 3), and I think it undersells how much of the writing is mine; that section is marked draft. Claude caught its own errors: it told me the Case 1 check figures before I set my falsification threshold, and it presented temp-worker pay (whole workers versus hourly) as my decision when the Farm Profit Lab already fixes it as hourly. Both are disclosed in the process doc. Claude also caught me writing "60/64" beds and a 1,440-hour temp limit that should be 5,760.

## 2026-10-01 — Paper-topics folder for Professor Stauffer

**Asked:** My professor said I could send a short description of some paper topics via the repo. Asked Claude to set up a folder for it. Paper must be micro or macro; my first idea is pricing structures of business coaching for musicians.

**What it did:** Created `stauffer-ai-go-here/` with a stub `README.md` and `paper-topics.md`. Then interviewed me with questions about my interests instead of writing the topic itself, offered five ways to frame the idea as economics, and drafted the Topic 1 text from my chat answers, keeping my wording.

**What I caught / changed:** Claude kept trying to combine all the options into one big paper; I pushed back and narrowed it to one question ("Are we preying on artist desperation, and is there a better way?"). Topic 2 (how a big local event affects small and arts businesses, using Charlottesville, Virginia) and Topic 3 (why small businesses fail) were drafted from my chat answers, and I tied all three to one question about demand uncertainty. I dropped a "how I would know I was wrong" section Claude had added, because it didn't fit a short topic description. Remaining checks before relying on it: confirm the Rolling Stones/Charlottesville detail and that the Field label for Topic 2 is right.

## 2026-09-27 — Perfect-competition brief critique (Stage 1)

**Asked:** Had Claude critique `docs/briefs/perfect-competition-brief.md` using the Stage 1 critique protocol: do not rewrite and do not suggest replacement wording; (1) name every implicit assumption, (2) name every unsupported claim, (3) ask three client questions, (4) say whether the hypothesis is falsifiable.

**What it did:** Pulled the latest `main` (the brief was on GitHub but not yet local), read the brief, and checked its numbers against the case facts page. Returned the critique in chat without editing the brief. Main findings: the stated assumptions are about the data being reliable, while the economic assumptions stay implicit (price-taking, whether my own 720 hours cost anything, whether temp labor is hired by the hour or by the worker, whether every bed has to be planted); the rationale contradicts itself (it says "mostly carrots" and "fewer than 7 tomato beds" but hypothesizes 30 mesclun and 14 tomatoes); a few figures don't match the case formula; and the headline mix is falsifiable, but the "How I would know I was wrong" section gives no threshold and doesn't name a model output to check.

**What I caught / changed:** Nothing changed yet — critique received, brief not yet revised.

## 2026-09-21 — AGENTS.md rewrite against the baseline template

**Asked:** Had Claude rewrite AGENTS.md to match the course's AI-conventions baseline structure — I said to set it up and I'd go check it, and to ask if anything needed my own words.

**What it did:** Restructured AGENTS.md into the baseline's sections: About this repository, Who I am, Where things are, Naming (kept from the baseline word for word, per the standard's requirement), How I work, What you may and may not draft, Documentation, Scope, Commits, Prompt log (standing rule kept exactly, per the standard's requirement), Never include (kept the secondary-student data specifics), What I check before committing (carried over from the prior version — not part of the baseline template, but kept since it was useful), and Mistakes to avoid (new, starts empty). Did not ask me to write anything new — everything was either baseline boilerplate or content already established in earlier sessions.

## 2026-09-21 — Rubric evaluation and FormalBio.md split

**Asked:** Had Claude evaluate the repo against the course's actual portfolio-repo standard and AGENTS.md baseline (not just the generic onboarding page). Then asked Claude to create `FormalBio.md` holding the full performer bio, and rewrote `README.md`'s bio to a few sentences (written by me) pointing to it.

**What it did:** Evaluation surfaced several gaps: RESUME.md vs CV.md naming conflicts with the standard, repo name isn't lowercase-hyphenated, README's engagement index doesn't match the standard's definition (links to briefs/analysis/memos, not a performance table), and AGENTS.md is missing several sections the baseline requires verbatim (Naming, prompt-log standing rule, Mistakes to avoid, others). None of those are fixed yet — pending my decisions. Separately, created `FormalBio.md` with the original full bio text, trimmed `README.md`'s bio to the short version I wrote, added the "See FormalBio.md" pointer, and added FormalBio.md to the Repository Guide list.

## 2026-09-21 — AGENTS.md rewrite and CV naming reversal

**Asked:** Told Claude what I actually let AI write: this AGENTS.md file itself, plus formatting and code/mechanical things — nothing else. Also asked to rename `RESUME.md` back to `CV.md`.

**What it did:** Rewrote `AGENTS.md`'s "What I let AI draft" / "What I write myself" sections to state only those two things, renamed `RESUME.md` to `CV.md`, and updated the links in `README.md` that pointed to it.

## 2026-09-21 — Repo mechanics cleanup (instructor feedback)

**Asked:** Got instructor feedback that the repo's content was strong but scoring low on mechanics — tab-indented `CV.md` rendering as an unformatted code block, no `RESUME.md`/`AGENTS.md`/`CLAUDE.md`/`prompt-log.md`, no folder skeleton, no `.gitignore`. Asked Claude to fix the mechanics without touching the writing.

**What it did:** Rewrote `CV.md` as `RESUME.md` with proper markdown headings and spacing (no content changes — same dates, same wording), moved `bio.md`'s text into `README.md` with an engagement-index table added beneath it, and created `AGENTS.md`, `CLAUDE.md`, `.gitignore`, and the `analysis/`, `capabilities/`, `data/`, `docs/` folder skeleton with one-line stub READMEs.

**What I caught / changed:** Claude's first draft of `AGENTS.md` claimed I let AI draft "first passes" on bios and outreach copy, and that an AI draft is "a starting point" for my résumé — neither is true, I don't use AI as a starting point for my own writing. Also caught the same problem in `README.md`'s disclosure line, which implied AI helped draft the bio when it only assembled the engagement-index table. Had Claude correct both to state only what actually happened: AI's role here is mechanical (formatting, structure, scaffolding), not composition.
