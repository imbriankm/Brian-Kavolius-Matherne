---
type: spec
capability: economic-research
engagement: research-paper
date: 2026-11-01
status: draft
built_with: not built yet
---

# Economic research — coaching sales page study spec

_Draft. Text in plain type is mine, taken from `stauffer-ai-go-here/paper-prewriting.md`. Text marked **Proposal (Claude)** is Claude's suggestion and isn't decided yet. Text marked **Decision N** is a question I still have to answer. Text marked **Course rule** comes from the [research paper page](https://adamwstauffer.github.io/ai-lms/research-paper.html). Nothing here gets collected until every decision is made, because the rules have to be fixed before I read the sales pages._

**Course rule:** the brief (`docs/briefs/YYYY-MM-DD-research-brief.md`) is committed before research starts, and this spec is committed with its data sources, planned figures and success criteria. Writing the brief and the spec is reserved for me.

## Purpose

Are society, the market, and the coaching world preying on the desperation of artists trying to work as professionals, and is there a better way? By "preying" I mean the price depends on how badly the buyer wants a result instead of what is actually delivered, and the buyer can't check beforehand whether the result will come.

**Proposal (Claude):** this study finds the sales pages of coaching programs for artists, records what each one costs and what it delivers, and then reads the sales language. It should tell me whether the programs that use desperation language charge more for the same coach time. It should also tell me what a buyer can know before paying, and why refund guarantees, outcome-based fees or published client results haven't pushed the expensive programs out.

## Hypothesis

My hypothesis is that a high percentage of the programs (>50%) will have pricing that exceeds the $150 line. I think, that out of those programs >50% will use the sales tactics as described.

**Measure (decided 2026-11-01):** the sticker price per hour of coach time.

**Line (decided 2026-11-01):** $150 per contact hour, and the textbook line for self-paced courses. See Success criteria.

## Inputs: the named contract

### Where the programs come from

1. **Seed list:** the six programs in my pre-research. The list names coaches, so it stays in `scratch/` (never committed) and is entered here only as program IDs.
2. **Searches:** a fixed list of search terms, run on the same day, with the results looked at recorded in the search log.
   **Proposal (Claude):** Google and the Meta Ad Library (it shows who is actively advertising, which matters for sales tactics), with terms like "music career coaching program", "musician business mastermind", "audition coaching program", "artist entrepreneurship course" and "creative career coach cohort". **Decision 2** sets the final list.
3. **Snowball:** programs named on pages already found, and pitches that I or people I know have received.

4. **Outside research (decided 2026-11-01):** external research on typical customer behavior on refunds for expensive service-based products, for question 3. Not a source of programs.

**Decision 1:** what counts as "this type of coaching"?

- **Who it's for (decided 2026-11-01):** artists in general, specifically talking about careers. "Book your next gig/client" is a key phrase. Seed program P05 is a good example.
- **Formats (decided 2026-11-01):** I think mostly group/cohort programs, masterminds, and self-paced courses are the main offenders here, but we can include 1-1 also if it's too expensive. We'll keep self-paced courses, because the 1-1 time can be calculated. It's zero.
  - **One-to-one (decided 2026-11-01):** include 1-1 programs fully, at any price. If it turns out that there's enough data to compare two different classes of coaches, I think that would be a great thing. If we can't find enough programs in each class, that's fine, we won't use the class.

**How many (decided 2026-11-01):** the research will work if we find 30 programs at least.

### What gets recorded for each program

One row per program. **Proposal (Claude)** for the names; the tangibles are from my answer to Professor Stauffer's question 2.

| Name | What it records | Unit |
|---|---|---|
| `ProgramID` | P01, P02, … in the order found | — |
| `FoundVia` | seed list, search term, ad, snowball or pitch | — |
| `CaptureDate` | the day the page was read | date |
| `Snapshot` | a saved copy of the page (PDF in `scratch/`) | — |
| `Audience` | who the page says it's for | — |
| `Format` | one-to-one, group, cohort, mastermind, or self-paced with calls | — |
| `PriceListed` | yes, no, or "apply / book a call" | — |
| `Price` | the sticker price: pay-in-full, before any early-bird, scholarship, payment plan or negotiation (decided 2026-11-01: "What is the sticker price before modifications?") | USD |
| `PlanPrice` | the total paid on the payment plan, if there is one | USD |
| `EarlyBird` | whether there is early-bird or deadline pricing, and the price | USD |
| `LengthWeeks` | how long the program runs | weeks |
| `Sessions` | each kind of live session: how many, how long, how many clients in it | hours, clients |
| `Community` | "Facebook groups" or other things like that | yes / no |
| `Materials` | course materials or modules | yes / no |
| `Refund` | none, conditional, or full | — |
| `OutcomeFee` | any part of the fee that depends on the result | yes / no |
| `PublishedResults` | none, testimonials only, or numbers | — |
| `PromisedOutcome` | what clients are paying for: skills, success or a network | — |

Language codes are added to each row only in step 3 (see Calculation logic).

## Structure

**Proposal (Claude):**

1. **`scratch/coaching-key.xlsx`** (never committed): program ID, coach name, URL, and the snapshot file.
2. **`data/coaching-programs.xlsx`** (committed, no names), with tabs:
   - **README:** what the workbook is for and how to read it.
   - **Search log:** every search, its date, and every page it turned up, included or excluded, with the reason.
   - **Programs:** the named fields above, one row per program.
   - **Language:** the sales text of each page with the prices taken out, and the language codes.
   - **Analysis:** the outputs below.
   - **Checks:** pass/fail cells, green when met.

## Calculation logic

The process has three steps, in this order:

1. Gather a number of sales pages for these programs.
2. Analyze the pricing (separate expensive from not expensive).
3. THEN, analyze the language with a predefined understanding of what predatory sales tactics are.

### Coach contact time

Coach contact time is defined as (contact hours) / (number of clients in the group). So a single 1-1 session is worth 1 hour, and a 2-hour session with 4 people is worth 1/2 hour. For a program with more than one kind of session, each kind is worked out and added up:

```
CoachContactTime    = Σ (sessions × hours per session ÷ clients in the session)
PricePerContactHour = Price ÷ CoachContactTime
```

**Proposal (Claude):** a self-paced course has a `CoachContactTime` of 0, and a price can't be divided by 0, so its `PricePerContactHour` is shown as "no coach time" instead of a number. Self-paced courses are judged against the textbook line instead (see Expensive or not).

**Decision 4:** what counts as a contact hour? For example: live group calls, recorded videos, office hours, messaging access ("unlimited Voxer"), and guest speakers.

### Expensive or not

**Measure (decided 2026-11-01):** the sticker price per hour of coach time, using the coach contact time relationships above, is how we measure expensive.

**Self-paced courses (decided 2026-11-01):** we'll use flat rates for programs that are in line with textbook pricing. Perhaps 1.5 the cost of an average university business entrepreneurship textbook (I'm guessing $200, so about $300). A course is only worth $150-$300 (decided 2026-11-01). _(Claude's reading: a course with no coach time counts as overpriced if its sticker price is above $300, the top of that range. The $200 is a guess, to be checked against real textbook prices.)_

**Programs with coach time (decided 2026-11-01):** if coaching programs have inflated beyond $150 per contact hour, then we can think they're overpriced. $150 comes from university lessons. My understanding of industry standards is that a lesson with a great player (music), is between $85 and $130. I just, as an industry member, feel confident in that call. We'll find industry standard lesson pricing later (there's a lesson-rate calculator I know of; source to confirm).

### Desperation language

**Decision 6:** the codebook. The list of sales tactics that count as desperation or predatory language, written before any page is read for language. My examples so far: high prices that include things like "Invest in yourself," prices that aren't listed, early-bird pricing used as a sales tactic, and "applying" (decided 2026-11-01: "a marketing tactic to induce the feeling of scarcity").

**Proposal (Claude):** code the language from the Language tab, where the prices have been taken out, so knowing the price can't sway how a page is coded.

## Conventions

- **Planning for the data (decided 2026-11-01):** we don't know what data we'll have yet, so we shouldn't count on it. We just need to build a system and idea that works for the data that we HOPE to get, with plans to adjust later.

- **No names:** I'll need to keep individual coaches' names out of the repo. Program IDs only; names, URLs and snapshots stay in `scratch/`, which `.gitignore` keeps out of every commit. A local pre-commit check refuses any commit that contains a name or site from the private list in `scratch/`, including inside spreadsheets. Every new coach goes on that list when they get a program ID. **Proposal (Claude):** quotes in the repo are kept to short, common phrases, because a long quote can be searched back to its coach.
- **Dates:** every page gets a `CaptureDate`. Sales pages change, so a figure is only true for the day it was read.
- **Unlisted prices (decided 2026-11-01):** programs that only say "apply" or "book a call" are still collected, but they are a separate category. Their sales language is still coded.
  - **Archived pages:** if a program has ever listed its price, an older copy of its page (for example on the Internet Archive's Wayback Machine) can supply it. **Proposal (Claude):** record the archive date beside the price, because an old price may not be today's, and keep it apart from current sticker prices.
  - **Research emails:** I'll decide about emailing them later (I'm leaning not). **Proposal (Claude), if I do:** I write and send them myself, and say it's for research, not pose as a buyer.
  - A price that's never found is recorded as unlisted, not guessed.
- **Unclear numbers:** **Proposal (Claude):** when a page gives a range (a group "of 8 to 12," calls "up to 90 minutes"), record both ends and run the analysis both ways. If the answer only holds at one end, that's a finding. When a page gives no number at all, the program is left out of the per-hour figures and counted as "not stated," which is itself an answer to question 2.

## Planned figures

**Course rule:** at least one graph, chart or diagram that the paper's text actually uses, labeled well enough to read on its own. AI may build figures from a specification I write.

**Decision 8:** which figure or figures, and what each one shows.

## Success criteria

**Course rule:** the spec states its success criteria.

1. **The research worked:** we find 30 programs at least, and the validation rules below pass.
2. **Reading the result:** if coaching programs have inflated beyond $150 per contact hour, then we can think they're overpriced. For self-paced courses, the line is about 1.5 times the cost of an average university business entrepreneurship textbook.

## Validation rules

These test whether the data was collected and coded correctly, not whether my hypothesis was right.

**Course rule:** every number or source an AI supplies is checked against the real source, and the check is logged in `prompt-log.md`. Any sales page Claude finds, I open and confirm myself before it goes into the data.

**Proposal (Claude):**

- **Hand check:** one program's `CoachContactTime` worked out on a calculator, matching the workbook.
- **Second coder:** Claude codes the language of every page on its own, from the same codebook. Where we disagree, that's a finding, and the disagreements are counted.
- **Search log is complete:** every page found is in the log, included or excluded with a reason, so nothing was left out because it didn't fit.
- **Every row is dated and saved:** each program has a `CaptureDate` and a `Snapshot`.
- **No names:** no coach name or URL in anything committed.
- **Formulas, not typed numbers:** `CoachContactTime` and `PricePerContactHour` are formulas.

## Outputs

From my Purpose and Professor Stauffer's questions. **Proposal (Claude)** for the named results:

- Are the programs overpriced?
  - `OverpricedShare`: the share of programs above $150 per contact hour, or above the textbook line for self-paced courses.
- Do the overpriced programs use desperation language?
  - The share of overpriced and of not-overpriced programs that use each tactic in the codebook.
- What can a buyer know before paying? (Question 2)
  - The share of programs that list their price, their contact hours and their group size.
- Why haven't refunds, outcome-based fees or published results pushed the expensive programs out? (Question 3)
  - How many programs offer each, and whether they are the cheaper or the more expensive ones.
- What are clients actually paying for: skills, success, or a network?
  - The count of each `PromisedOutcome`.
- What a better way might look like: I don't know yet, and figuring that out is what the paper is about. The data won't answer this on its own.
  - **Course rule:** the paper recommends a policy or strategy and defends it against its strongest objection. The recommendation is mine to write.
