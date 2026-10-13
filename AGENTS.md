# Agent & AI Working Preferences — Brian KM

This file tells any AI assistant working in this repository how I work, what I want it to do, and where the lines are. `CLAUDE.md` just points here so every tool finds it.

## Who I am

Performer-composer, music instructor, and french horn player, Honolulu-based (formerly Melbourne). MBA candidate at Shidler College of Business (2026–2028). I run a private horn studio and perform university residencies. I am also a substitute horn player for the Hawaii Symphony Orchestra.

## What I let AI draft

- This file — AGENTS.md itself.
- Formatting and code things: markdown structure, tab/whitespace cleanup, folder scaffolding, `.gitignore` and stub-README boilerplate.

## What I write myself

- Nearly everything else: my bio, my résumé/CV, and any substantive, public, or evaluative writing.
- Anything evaluative about a specific student's playing or progress.

Often the words start as me typing straight into the chat, and I have the AI place them into the right file and format them. That's still my writing — the AI is doing placement and formatting, not composition.

## What I check before committing

- That a formatting or structural change didn't alter dates, facts, or meaning.
- That nothing AI-touched invents a credential, award, or performance that didn't happen.
- That markdown actually renders correctly on github.com, not just in a preview.

## What never gets committed

- Any secondary student's name, grade, assessment, or other identifying detail — feedback to students and parents stays in school systems, not in this repo.
- Credentials, API keys, or personal contact information (enforced by `.gitignore`).
- Anything I haven't personally reviewed and wouldn't be comfortable with a future employer, presenter, or student's parent reading.

## Getting feedback from Professor Stauffer

- When I want him to look at something, open a pull request and **leave it open**, or open an issue, with `@adamwstauffer` in the description. That mention is what notifies him. Don't merge it until he has replied.
- One PR per working session is fine. The description should say what I want him to look at.
- Merge with **"Create a merge commit,"** never squash, so the commit order stays visible (the spec has to be committed before the workbook).
- Graded work has to be on `main` by its deadline, so merge before then even if he hasn't replied.
- His reviews arrive as PRs (e.g. `review/2026-09-28`). After I merge one on GitHub, run `git pull` so the review is on my computer too.
