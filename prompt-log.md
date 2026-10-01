# Prompt Log

A running record of meaningful AI sessions in this repository: what I asked, what the AI got wrong, and how I caught it.

## 2026-09-21 — Repo mechanics cleanup (instructor feedback)

**Asked:** Got instructor feedback that the repo's content was strong but scoring low on mechanics — tab-indented `CV.md` rendering as an unformatted code block, no `RESUME.md`/`AGENTS.md`/`CLAUDE.md`/`prompt-log.md`, no folder skeleton, no `.gitignore`. Asked Claude to fix the mechanics without touching the writing.

**What it did:** Rewrote `CV.md` as `RESUME.md` with proper markdown headings and spacing (no content changes — same dates, same wording), moved `bio.md`'s text into `README.md` with an engagement-index table added beneath it, and created `AGENTS.md`, `CLAUDE.md`, `.gitignore`, and the `analysis/`, `capabilities/`, `data/`, `docs/` folder skeleton with one-line stub READMEs.

**What I caught / changed:** Claude's first draft of `AGENTS.md` claimed I let AI draft "first passes" on bios and outreach copy, and that an AI draft is "a starting point" for my résumé — neither is true, I don't use AI as a starting point for my own writing. Also caught the same problem in `README.md`'s disclosure line, which implied AI helped draft the bio when it only assembled the engagement-index table. Had Claude correct both to state only what actually happened: AI's role here is mechanical (formatting, structure, scaffolding), not composition.

## 2026-09-21 — AGENTS.md rewrite and CV naming reversal

**Asked:** Told Claude what I actually let AI write: this AGENTS.md file itself, plus formatting and code/mechanical things — nothing else. Also asked to rename `RESUME.md` back to `CV.md`.

**What it did:** Rewrote `AGENTS.md`'s "What I let AI draft" / "What I write myself" sections to state only those two things, renamed `RESUME.md` to `CV.md`, and updated the links in `README.md` that pointed to it.

## 2026-09-21 — Rubric evaluation and FormalBio.md split

**Asked:** Had Claude evaluate the repo against the course's actual portfolio-repo standard and AGENTS.md baseline (not just the generic onboarding page). Then asked Claude to create `FormalBio.md` holding the full performer bio, and rewrote `README.md`'s bio to a few sentences (written by me) pointing to it.

**What it did:** Evaluation surfaced several gaps: RESUME.md vs CV.md naming conflicts with the standard, repo name isn't lowercase-hyphenated, README's engagement index doesn't match the standard's definition (links to briefs/analysis/memos, not a performance table), and AGENTS.md is missing several sections the baseline requires verbatim (Naming, prompt-log standing rule, Mistakes to avoid, others). None of those are fixed yet — pending my decisions. Separately, created `FormalBio.md` with the original full bio text, trimmed `README.md`'s bio to the short version I wrote, added the "See FormalBio.md" pointer, and added FormalBio.md to the Repository Guide list.

## 2026-09-21 — AGENTS.md rewrite against the baseline template

**Asked:** Had Claude rewrite AGENTS.md to match the course's AI-conventions baseline structure — I said to set it up and I'd go check it, and to ask if anything needed my own words.

**What it did:** Restructured AGENTS.md into the baseline's sections: About this repository, Who I am, Where things are, Naming (kept from the baseline word for word, per the standard's requirement), How I work, What you may and may not draft, Documentation, Scope, Commits, Prompt log (standing rule kept exactly, per the standard's requirement), Never include (kept the secondary-student data specifics), What I check before committing (carried over from the prior version — not part of the baseline template, but kept since it was useful), and Mistakes to avoid (new, starts empty). Did not ask me to write anything new — everything was either baseline boilerplate or content already established in earlier sessions.
## 2026-09-27 — Perfect-competition brief critique (Stage 1)

**Asked:** Had Claude critique `docs/briefs/perfect-competition-brief.md` using the Stage 1 critique protocol: do not rewrite and do not suggest replacement wording; (1) name every implicit assumption, (2) name every unsupported claim, (3) ask three client questions, (4) say whether the hypothesis is falsifiable.

**What it did:** Pulled the latest `main` (the brief was on GitHub but not yet local), read the brief, and checked its numbers against the case facts page. Returned the critique in chat without editing the brief. Main findings: the stated assumptions are about the data being reliable, while the economic assumptions stay implicit (price-taking, whether my own 720 hours cost anything, whether temp labor is hired by the hour or by the worker, whether every bed has to be planted); the rationale contradicts itself (it says "mostly carrots" and "fewer than 7 tomato beds" but hypothesizes 30 mesclun and 14 tomatoes); a few figures don't match the case formula; and the headline mix is falsifiable, but the "How I would know I was wrong" section gives no threshold and doesn't name a model output to check.

**What I caught / changed:** Nothing changed yet — critique received, brief not yet revised.

## 2026-10-01 — Paper-topics folder for Professor Stauffer

**Asked:** My professor said I could send a short description of some paper topics via the repo. Asked Claude to set up a folder for it. Paper must be micro or macro; my first idea is pricing structures of business coaching for musicians.

**What it did:** Created `stauffer-ai-go-here/` with a stub `README.md` and `paper-topics.md`. Then interviewed me with questions about my interests instead of writing the topic itself, offered five ways to frame the idea as economics, and drafted the Topic 1 text from my chat answers, keeping my wording.

**What I caught / changed:** Claude kept trying to combine all the options into one big paper; I pushed back and narrowed it to one question (\"Are we preying on artist desperation, and is there a better way?\"). Topic 2 (how a big local event affects small and arts businesses, using Charlottesville, Virginia) and Topic 3 (why small businesses fail) were drafted from my chat answers, and I tied all three to one question about demand uncertainty. I dropped a \"how I would know I was wrong\" section Claude had added, because it didn't fit a short topic description. Remaining checks before relying on it: confirm the Rolling Stones/Charlottesville detail and that the Field label for Topic 2 is right.
