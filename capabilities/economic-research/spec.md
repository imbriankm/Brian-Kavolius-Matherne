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
   **Decision 2 (decided 2026-11-09):** Claude drafted the list and I chose and approved it. Terms built two ways, as is standard for a fixed search: from the words the six seed programs use for themselves (taken from their program titles and web addresses in `scratch/coaching-key.md`; no sales page was opened), and from plain words for what an artist needs, so the search can also find coaches who are not like the six.

   - **From the seed list:**
     1. audition coaching program
     2. music career coaching program
     3. musician mastermind group
     4. music entrepreneurship course
     5. accelerator program for musicians
     6. musician launchpad program
   - **From the need (neutral):**
     7. music career coach
     8. how to book more gigs coaching
     9. freelance musician business coaching
     10. music career consultation
     11. artist career coach
     12. creative entrepreneur coaching
   - **Other kinds of artists** (Decision 1 covers artists in general):
     13. actor career coaching
     14. visual artist business coach
     15. dancer career coaching
   - **Added by me (2026-11-09):**
     16. DJ career coach
     17. EDM career coach
     18. DJ business coach
     19. EDM business coach
   - **Second round (2026-11-09):** Claude suggested these and I chose them. Composers and film composers are my additions to that list.
     - By kind of musician:
       20. classical musician career coaching
       21. jazz musician career coaching
       22. singer-songwriter career coach
       23. composer career coaching
       24. film composer career coaching
       25. film and game composer career course
       26. independent artist coaching program
     - By career stage:
       27. music graduate career coaching
       28. portfolio career musician coaching
       29. full-time musician coaching program
     - By what the program sells:
       30. music teacher studio business coaching
       31. music marketing coaching program
       32. musician social media coaching
       33. music booking agent course
     - By format:
       34. group coaching program for musicians
       35. online course for musicians career
       36. musician business mastermind
     - Other artists:
       37. photographer business coach
       38. author career coach
   - **Considered and dropped (2026-11-09):** mindset coach for musicians, actors, visual artists and dancers. Mindset coaching is often about confidence or performance, not careers, so it falls outside Decision 1. Also music licensing course.

   **Where:** Google, in a private window while logged out so past searches don't shape the results, and the Meta Ad Library (it shows who is actively advertising, which matters for sales tactics).

   **How deep:** the first 20 results for each term on Google, leaving out paid ads, and the first 20 advertisers for each term in the Meta Ad Library. Every result is written in the search log, included or excluded, with the reason.

   **When:** every term on the same day.

   **Seed and pitch programs:** counted in the main share, and the share is also reported without them (from `FoundVia`), so the result doesn't rest on programs I already suspected.

   **The list is closed (2026-11-09):** no term is added or removed after the search runs.
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
| `Field` | music (including DJ/EDM and composers), acting, visual art, dance, photography or writing (decided 2026-11-09) | — |
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
| `RefundTerms` | the refund's claim window and conditions as the page states them, or "not stated" (decided 2026-11-09, for my reason 4: "Refund terms are vague"). **Proposal (Claude):** written as short phrases (e.g. "14 days; must complete modules 1–3"), with the exact wording left in the snapshot in `scratch/`, because a long quote can be searched back to its coach | days, conditions |
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

**Decided 2026-11-09:**

- **Hot seat** (one person coached while the others watch): let's call a hot seat the same as a group call. _(I first set watching at 25% of being coached, but that let a hot-seat call earn more contact time than a plain group call of the same length, so I dropped it.)_
- **Recorded videos** count as materials. Not coach time.
- **Office hours** should be divided across the group members.
- **Messaging:** any messaging access counts as one 1-1 session per week, an hour a week unless it says otherwise. _(First set for unlimited messaging only; expanded 2026-11-09 to any messaging access.)_
- **Feedback** on submitted work: 15 minutes per piece.
- **Guest speakers** should be treated the same as the coach.
- **Group size not stated:** we'll have to either look at pics on the page, or look for them explaining the size of the cohort. **Proposal (Claude):** record where each group size came from (stated, or counted from a photo), so the per-hour figures can also be run without the photo counts. If neither gives a number, the "not stated" rule under Conventions applies.

### Expensive or not

**Measure (decided 2026-11-01):** the sticker price per hour of coach time, using the coach contact time relationships above, is how we measure expensive.

**Self-paced courses (decided 2026-11-01):** we'll use flat rates for programs that are in line with textbook pricing. Perhaps 1.5 the cost of an average university business entrepreneurship textbook (I'm guessing $200, so about $300). A course is only worth $150-$300 (decided 2026-11-01). _(Claude's reading: a course with no coach time counts as overpriced if its sticker price is above $300, the top of that range. The $200 is a guess, to be checked against real textbook prices.)_

**Programs with coach time (decided 2026-11-01):** if coaching programs have inflated beyond $150 per contact hour, then we can think they're overpriced. $150 comes from university lessons. My understanding of industry standards is that a lesson with a great player (music), is between $85 and $130. I just, as an industry member, feel confident in that call. We'll find industry standard lesson pricing later (there's a lesson-rate calculator I know of; source to confirm).

### Desperation language

**Decision 6:** the codebook. The list of sales tactics that count as desperation or predatory language, written before any page is read for language. My examples so far: high prices that include things like "Invest in yourself," prices that aren't listed, early-bird pricing used as a sales tactic, and "applying" (decided 2026-11-01: "a marketing tactic to induce the feeling of scarcity").

**Price stays out of the codebook (decided 2026-11-09):** high price itself is not the tactic; it's what step 2 measures. The code is the "invest in yourself" language, wherever it appears. Prices that aren't listed and early-bird pricing are features of the price, so they're recorded as fields in step 2 (`PriceListed`, `EarlyBird`), not as language codes. That keeps step 3 independent of step 2. Whether unlisted or early-bird programs use more of the language is then compared in the analysis.

**Decision 6 (decided 2026-11-09): the codebook.** Claude drafted it and I agreed to every code. Codes 1, 2 and 11 are mine, and I chose 12 after the source check; the rest are common sales-pressure categories, checked on 2026-11-09 against two published lists: the taxonomy in Mathur et al., "Dark Patterns at Scale" (CSCW 2019; [paper](https://arxiv.org/abs/1907.07032), [category list](https://webtransparency.cs.princeton.edu/dark-patterns)), and the FTC's business-coaching case against Vision Solution Marketing ([FTC blog, 2018-05-25](https://www.ftc.gov/business-guidance/blog/2018/05/ftc-says-consumers-struck-out-deceptive-business-coaching-pitches)). The FTC's staff report "Bringing Dark Patterns to Light" (September 2022, [PDF](https://www.ftc.gov/system/files/ftc_gov/pdf/P214800%20Dark%20Patterns%20Report%209.14.2022%20-%20FINAL.pdf)) also covers false scarcity and countdown timers; its full category list is not yet checked. The example phrases are invented to show the idea. None is quoted from a real page, and no sales page was read to write them.

| # | Code | Counts when the page… | Example phrase (invented) |
|---|---|---|---|
| 1 | `Invest` | recasts the price as an investment in yourself or your career | "the best investment you'll ever make in your career" |
| 2 | `Apply` | makes you apply, qualify or book a call before you can buy | "apply now to see if you're a fit" |
| 3 | `Scarcity` | says places are limited or in high demand (widened 2026-11-09, from the "high-demand message" in Mathur et al.) | "only 12 seats this round", "spots are filling fast" |
| 4 | `Urgency` | sets a deadline or countdown to join, apart from any early-bird price | "doors close Friday at midnight" |
| 5 | `ValueStack` | lists a "total value" far above what you pay | "a [AMOUNT] value, yours today for [AMOUNT]" |
| 6 | `OutcomePromise` | promises an income or career result | "book your dream gigs in 90 days" |
| 7 | `Pain` | leads with fear, struggle or shame about where you are now | "tired of being a starving artist?" |
| 8 | `Exclusivity` | says the program is only for the serious or chosen few | "this isn't for everyone" |
| 9 | `Pressure` | treats hesitating as a lack of commitment | "don't you want to succeed?" |
| 10 | `RiskFree` | calls the purchase risk-free or guaranteed (checked against `RefundTerms`) | "zero risk: you're fully protected" |
| 11 | `OriginStory` *(mine, 2026-11-09)* | tells the coach's own story of using their method to pull themselves out of a bad place, so the buyer can expect the same. A plain bio or list of credentials doesn't count | "I was broke and playing to empty rooms until I built this system" |
| 12 | `Upsell` *(decided 2026-11-09)* | pitches a higher tier, or presents this program as a step toward a pricier one (the "advanced" program ladder in the FTC case) | "ready for more? join the VIP inner circle" |

**Rules for coding:**

- Each code is yes or no for each page. A page can have any number.
- A program "uses the tactics" if it has at least one code (suspicion 2a in the brief). The count of codes per program is also reported.
- Code what the page says, not what the coach might mean. When unsure, code no and write a note.
- Code from the Language tab, where every price and dollar figure is replaced with [AMOUNT].
- **Other (decided 2026-11-09):** any tactic on the page that isn't in the codebook is written in an `OtherTactic` note. Notes don't count in the test, because the codebook is fixed. They're reported as tactics the codebook didn't cover.
- **Not coded**, because they're ordinary marketing or are already recorded as fields: testimonials (`PublishedResults`), the coach's credentials, plain descriptions of what's included, payment plans (`PlanPrice`), early-bird prices (`EarlyBird`) and unlisted prices (`PriceListed`).
- **Pilot:** before coding the full sample, both coders code the same 5 pages and compare. Definitions can be sharpened then, but no code is added or removed after the pilot.

**Proposal (Claude):** code the language from the Language tab, where the prices have been taken out, so knowing the price can't sway how a page is coded.

**Sharpened after the pilot (2026-11-21).** Both coders coded the same 5 pilot pages and agreed on 46 of 60 code decisions (Cohen's kappa 0.52). These sharpen the definitions once, as the pilot rule allows; no code is added or removed. The pilot codes stay as they were for the agreement report.

**How disagreements are settled (decided 2026-11-21):** I think this is the right method: we'll discuss the disagreements until we reach alignment. **Proposal (Claude):** the agreement figure is reported as it was before discussion (46 of 60), and the codes agreed in discussion are the codes of record for the 5 pilot pages.

- **`Apply`:** I felt that encouragement to call counted, because that opens up an avenue for more sales. So the code counts when the page makes you apply, qualify or book a call before you can buy, **and also when it encourages you to book a call or apply, even if you could buy without one.**
- **`Pain`:** "leads with" isn't right because it's not clear. We need "anywhere in the page." So the code counts when the page states fear, struggle or shame about where the buyer is now, anywhere in the page. We don't mind where each code appears.
- **`OriginStory`:** **Proposal (Claude), from the L-P125 discussion:** the "bad place" includes doubt or struggle in the coach's own career (feeling like the odd one out, not knowing if the work will add up), not only a crisis such as debt. The code needs the whole arc: the coach's struggle, what got them out, and the offer that the buyer can do the same. A success story with no struggle before it is a bio.
  - **Round 2 (2026-11-21), loosened:** naming the past struggle counts as origin story I think. I understand that this code blends with pain a little bit, but this isn't an exact science. So the code counts when the coach names their own past struggle of the kind the buyer is in (for example "self-doubt, all of it," L-P186), even without telling how they got out. A success story with no struggle in it is still a bio.
- **`ValueStack`:** I think the big thing is that it has to be a bundle with a PRODUCT. Teachers who offer a number of lessons for a discount over one lesson aren't being predatory. So the code counts when the page claims a "total value" above the price, or sells a bundle that includes a product (a course, materials, an eBook) as worth more together. A package of sessions or lessons sold at a discount over one session does not count.
- **`Scarcity` (round 2, 2026-11-21):** a group-size cap stated as a feature of the format ("a maximum of 12 participants," so you get more attention) does not count. A statement or counter that places are limited or running out does, including a booking page's automatic "spots left" counter. I think the word "left" is the thing that's setting me off. _(Claude's wording of the reason, which I agreed with: "left" tells the buyer places are being used up, the "low-stock message" in Mathur et al.'s Scarcity category; the counter counts even if the booking software shows it automatically, because we code what the page says, not what the coach might mean.)_
- **`Exclusivity`:** "high performing" and "isn't for beginners" is the most exclusive. So an entry bar the buyer must meet (a level of success or years of experience) counts, as well as "this isn't for everyone" language.
- **`Urgency`:** "act fast" and "limited time" were plenty for urgency for me. So undated wording that tells the buyer to act fast, or that the offer is for a limited time, counts as well as a dated deadline or countdown, even when it is tied to a discount. The early-bird price is still recorded in the `EarlyBird` field.
- **`Pressure`:** choosing to continue failing is enough for pressure. So the code counts when the page frames not buying, or waiting, as choosing to keep failing or stay stuck, as well as when it treats hesitating as a lack of commitment.
  - **Round 2 (2026-11-21):** yes, I think saying it's impossible to reach the next level on your own is pressure (L-P84). A claim that the buyer can't succeed without the program counts.
- **`OutcomePromise`:** settled on L-P270 that a general claim about what the program gives you ("the tools you need to start or re-start your business") is not a promise of an income or career result. I was being far too firm.
  - **Round 2 (2026-11-21):** "aims to" and a "chance" at a collab aren't promises (L-P84, no). A page teaching "a reliable way to make consistent income" does promise an income result (L-P149, yes). I think the disclaimers being in several places outweigh a single claim of selling more (L-P186, no). "Every time" is enough to make an offer of help a promise (L-P274, yes: being paid on time, every time). **Close-call review (2026-11-22):** I think the issue is the number and prominence of the disclaimers. I viewed one of these as a very minor disclaimer meant to just cover liability. Having many changes my view on that. So a single small, liability-style disclaimer does not cancel a promise (L-P216, L-P317: yes); disclaimers repeated in several places in the main copy outweigh a single claim (L-P186: no).
  - **Reliability (decided 2026-11-21):** OutcomePromise agreed on only 5 of 10 pages across the two rounds, the lowest of any code. Yes, we'll flag it. Claude proposed and I accepted: the code stays (no code is removed after the pilot), but it is flagged in the paper as the least reliable; the main test (a program uses at least one tactic) is also run without it; and OutcomePromise close calls come to me first in the full run.
- **`RiskFree`:** unchanged. A money-back guarantee, including a "double money-back guarantee," counts. A free call or free first session before buying does not.
- **`Upsell` (close-call review, 2026-11-22):** let's keep the original rule: code what the page says. Knowing that the coach sells a higher tier elsewhere does not make this page an upsell (L-P201: no).
- **`Upsell`:** settled on L-P348 that paid add-on services ("add on done-for-you services") and a sister company selling more services count. **Proposal (Claude):** a price-blind coder judges by the wording, not the price: an add-on, a "next level" or a bigger package counts; a sibling product offered side by side, with nothing saying it is more or further, does not.

**Head-coder rulings on the round-3 close calls (2026-11-21).** The AI coder passed these up as close calls; my ruling is the code. Each also clarifies a definition for the full run.

- **`Exclusivity`:** a soft "who may benefit" or "works best if" statement is not exclusive (L-P303, L-P111: no). Only a firm requirement counts.
- **`RiskFree`:** Fable is right that "nothing to lose" wording counts even when the page offers no guarantee (L-P365: yes).
- **`OutcomePromise`:** fine, yes: a stated result under "what's in it for you" ("see increased bookings and income") counts (L-P303). I'll have to make sure to discuss the limitations of these codes.
- **`Pain`:** I'm right, this is a yes: naming the buyer's struggles, even briefly ("old sabotaging habits," overwhelm), counts (L-P303).
- **`Apply`:** yes, I'm right: having to send a registration request with work samples before buying counts (L-P115).
- **`Upsell`:** Fable is right: a described ladder of programs or levels ("Level 1" up to "high-level coaching") counts even without a named higher tier being pitched (L-P208).
- **`OriginStory`:** no, that's a struggle, I'm right: the coach's half-empty rooms count (L-P111).

All 14 pilot disagreements were settled this way: 8 to my original code, 6 to Claude's coder's.

**Rounds (decided 2026-11-21):** what if we called this "one round" of testing/improvement/training, and then we do another 5? If the agreement number increases, we know we're directionally right. Things that I "missed," like the guarantee, we know are aligned, because the AI should be more thorough at text searching than me.

**Proposal (Claude):**

- **Round 2:** the next 5 pages of my blind order (rows 6–10). Claude's coder (the model that will run the full coding) codes them from the sharpened definitions, sealed, before it sees my codes.
- **Misses vs. judgment calls:** each disagreement is sorted as a miss (one coder overlooked text that fits the definition) or a judgment call (both saw it and read it differently). For every yes, the coder gives its evidence, and a script checks that the quoted words are really on the page, so a claimed miss can be verified, not assumed. Agreement is reported raw and with misses set aside.
- **Measuring a round (decided 2026-11-21):** we might need a new way to measure success here than kappa. Claude proposed and I accepted: the training measure is the number of judgment disagreements per round (disagreements that are not misses or rule errors), because that is what sharpening the definitions should shrink. Raw agreement, kappa and agreement on yes codes are still reported, with raw agreement shown beside what a coder would get by marking every code "no," so a reader can judge it.
- **When the coder is aligned (decided 2026-11-21):** if we can get to a point where I'm saying "Yes, you're right, I missed that" rather than pointing something out to the model during disagreements, then it's right. We should assume that the AI is more thorough, but that I know more about what I'm talking about. This is why we'll have the close call method.
- **Head coder (2026-11-21):** I think the difference is that the Fable coder does have the option to opt out of a choice by saying it's a budget [close] call and letting me do it. There really is only one head coder: me. **Proposal (Claude):** so the agreement that matters is on the codes the AI coder was sure of (pilot 32 of 37, 86%; round 2 29 of 33, 88%); its close calls come to me. Because it flagged 23 and 27 close calls in the two rounds (about 5 per page), the full run would produce far more than I can review, so the coder also rates each close call "close" or "very close," and I review the very close ones first, in the priority order below, up to the cap.
- **Wrong or incomplete pages (raised 2026-11-21):** I think this one is unfinished. This has happened a number of times. We may need a way to let the agent find more language if it's landed on the wrong page. **Proposal (Claude):** four of the first 13 coded pages had a capture or page-choice problem (FAQ answers not saved, packages described off-site twice, and a short "program specifics" page coded instead of the coach's main coaching page). Before the full run: (1) a page-choice audit of every site where another saved page on the same site names the same program with more text; (2) the 14 near-empty saves and a random 10 of the off-site cases, as planned. In the full run, the coder flags any page that looks incomplete or like the wrong page, with its reason, and that page is fixed and re-coded before its codes are used.
- **Close calls both coders flag (raised 2026-11-21):** if we agree on a close call, that's an agree? (I see a few problems with this.) **Proposal (Claude):** agreement is still counted on the codes themselves. When both coders mark the same code on the same page as a close call, that is recorded separately as a sign the code is grey there, and it feeds the grey-area score, not the agreement figure.
- **Stopping rule (set 2026-11-21, before round 3 was coded):** Claude proposed it and I went ahead with it: round 3 is the last blind round. Leaving OutcomePromise aside, if I correct the coder on no more disagreements than I concede ("yes, I missed that"), the coder is aligned. Either way the rounds stop there, and remaining grey cases go to the close-call review.
- **Result of round 3 (2026-11-21):** raw agreement 82% (pilot 77%, round 2 80%), kappa 0.57, and agreement on the codes the AI coder was sure of 92% (86%, 88%). Of 11 disagreements, 8 were the coder's close calls and 3 were confident calls; on those three, I think Fable is right on all of these. By the stopping rule, the coder is aligned and the blind rounds are done.
- **Reading the result:** directional improvement means our training process works, not that we're "there." The agreement of the latest round is the figure the paper reports for the coding. If a round sharpens a definition again, another round of 5 pages follows on the same terms.
- **Page length (raised 2026-11-21):** I wonder if length of these pages should count? **Proposal (Claude):** record each page's length in words (from the price-blind text, so no price is seen) and report it in the analysis. A longer page has more room for tactics, so the tactic count partly measures length. The test of suspicion 2b is also run with tactics per 1,000 words, and the correlation between page length and price per contact hour is reported.
- **Pressure measures (decided 2026-11-21, prices still sealed; the high-pressure line was later dropped, see "Figure 2 changed"):** I'd like to see the tactic count, the high pressure page, and the tactic density. The tactic count (0–12) and tactics per 1,000 words are compared with price per contact hour as a ranking; that comparison is fixed now. A high-pressure page starts at 4 tactics. If we set the bar for high pressure at 4, and then the evidence is overwhelming that 7 is a better line, then that should be considered. So the 4+ result is always reported; a different line found by looking at the data is reported beside it and labeled as chosen after seeing the data. "At least one tactic" (suspicion 2a) is still reported as pre-registered, and the paper calls it a low bar.
- **Close calls in the full run:** reviewed in this order: first those that would change whether a page uses any tactic at all, then those on the codes with the lowest agreement. How many I review is decided once the count is known; the rest keep the coder's answer, and the number is reported.

## Conventions

- **Planning for the data (decided 2026-11-01):** we don't know what data we'll have yet, so we shouldn't count on it. We just need to build a system and idea that works for the data that we HOPE to get, with plans to adjust later.

- **No names:** I'll need to keep individual coaches' names out of the repo. Program IDs only; names, URLs and snapshots stay in `scratch/`, which `.gitignore` keeps out of every commit. A local pre-commit check refuses any commit that contains a name or site from the private list in `scratch/`, including inside spreadsheets. Every new coach goes on that list when they get a program ID. **Proposal (Claude):** quotes in the repo are kept to short, common phrases, because a long quote can be searched back to its coach.
- **Dates:** every page gets a `CaptureDate`. Sales pages change, so a figure is only true for the day it was read.
- **Unlisted prices (decided 2026-11-01):** programs that only say "apply" or "book a call" are still collected, but they are a separate category. Their sales language is still coded.
  - **Archived pages:** if a program has ever listed its price, an older copy of its page (for example on the Internet Archive's Wayback Machine) can supply it. **Proposal (Claude):** record the archive date beside the price, because an old price may not be today's, and keep it apart from current sticker prices.
  - **Research emails:** I'll decide about emailing them later (I'm leaning not). **Proposal (Claude), if I do:** I write and send them myself, and say it's for research, not pose as a buyer.
  - A price that's never found is recorded as unlisted, not guessed.
- **Unclear numbers:** **Proposal (Claude):** when a page gives a range (a group "of 8 to 12," calls "up to 90 minutes"), record both ends and run the analysis both ways. If the answer only holds at one end, that's a finding. When a page gives no number at all, the program is left out of the per-hour figures and counted as "not stated," which is itself an answer to question 2.

## Run rules added before the search (2026-11-10)

_Added after the green light and before the first search or sales page._

**Decided 2026-11-10** (Claude offered two options for each; I chose):

- **Who sells it:** independent coaches only. Career courses sold by universities, course marketplaces (for example Udemy or Skillshare) and nonprofits are written in the search log as excluded, with the reason.
- **Tiers:** each package is a program. One row per priced package, with a `CoachID` (C01, C02, …) on every row, so the share can also be counted once per coach.
- **My order:** I code the language first and confirm the live pages after. Claude holds back every price result until my coding is done.

**Decided 2026-11-21:** how the run fills gaps the spec leaves open. Claude proposed these; I accepted all eight. Found twice, the home page/blog rule and snowball were unclear to me, but I think they're OK.

- **Independent coach:** a person or a coaching business selling from its own site. A coach's course that is sold only through a marketplace is excluded as marketplace.
- **Google:** run in Claude's built-in browser, which is logged out and has no search history, in English, from Honolulu. "The first 20 results" means the unpaid results on Google's first two pages. Google hides each result's full address behind a redirect, so the log records the address as Google displays it.
- **Meta Ad Library:** country United States, active ads, all ad types, in Meta's default order, taking the first 20 different advertisers for each term.
- **Found twice:** one row. `FoundVia` is the first place it was found, and every sighting stays in the search log.
- **Not a sales page:** when a result is a coach's home page or blog post, the run follows it to the sales page on the same site.
- **Snowball:** one step only, from included program pages. Programs named in "best of" articles are not followed, because the search list is closed.
- **Currency:** a price is recorded in the currency on the page and converted to US dollars at the capture date's rate, with the rate's source noted.
- **What is committed:** the full sales text stays in `scratch/`, because any sentence of it can be searched back to its coach. The committed Language tab holds the codes and short notes. The committed Search log holds a result number, the kind of site, included or excluded, the reason and the `ProgramID`; the log with addresses stays in `scratch/`.

**How the run was done (Claude, 2026-11-10):**

- **Group size from photos:** not counted in this run. Those rows are marked "needs photo check" for my confirm pass.
- **Two agents:** the Claude agent that records prices and the Claude agent that codes language are separate. The coder is given only the text with prices taken out.

## Division of work (decided 2026-11-21)

I want AI to help me with:

- Data collection
- Data analysis
- Correlations
- Excel work (graphs/charts)

I want to write the rest.

**Proposal (Claude):** what that changes in this spec. These replace "My order" in the run rules above and the "Second coder" validation rule.

- **My coding:** I code 30 pages blind, before I see any price or any of Claude's codes: the 5 pilot pages first, then 25 drawn at random (`scratch/coding/blind-draw.txt`), in that order. I may stop after 20; the pages done are still a random sample.
- **Names hidden too (2026-11-21):** I think we need to find a way to blind the names from testimonials. **Proposal (Claude):** every person's name on the page (testimonial givers, clients, guests and the coach) and any business name or web address built from one is replaced with [NAME], the same way prices are replaced with [AMOUNT]. Both coders code from the same name-masked text. Claude's sealed pilot coding of the 5 pilot pages was done before names were masked, which the agreement report says.
- **Claude's coding:** Claude's coder, a separate agent that sees only the text with prices taken out, codes all 256 pages. My codes are the codes of record on the pages I coded, and Claude's on the rest. The agreement between us on my pages is reported. The pilot rule still holds: definitions can be sharpened once after the 5 pilot pages, and no code is added or removed.
- **Close calls:** on the pages I don't code, Claude's coder marks the codes it was unsure of, with the sentence that triggered each, and I decide them.
- **Prices:** unsealed to me once my blind coding is done.
- **Group size from photos:** if there's no photo, we can't get the data. If there are photos, Claude shows them to me and I say how many people are in the group. This covers every program whose group size isn't stated; the source is recorded as "photo," so the per-hour figures can be run with and without them. **Proposal (Claude):** Claude shows only photos cropped from the page, with no price in view, and I decide whether a photo shows the session at all.
- **Confirming the data:** I check 10 random rows against the live pages, check every number the paper uses against the workbook, and hand-check one `CoachContactTime`. Each check is logged in `prompt-log.md`.
- **What stays mine:** what the numbers mean, the implications, the recommendation and all the prose.

## Method as run

_Record (Claude), for me to check: each step actually taken, in order, with its date. The paper's method section draws on this list. New steps are added as they happen._

1. **Search (2026-11-10):** all 38 terms run on Google and the Meta Ad Library on the same day. 1,032 results logged, each included or excluded with a reason.
2. **Collection (2026-11-10):** 245 candidate sites read by Claude agents against written rules. 150 coaches included, 95 sites excluded with a reason, giving 365 packages. A copy of every page read was saved in `scratch/`.
3. **Price audit (2026-11-10):** every recorded price checked against the saved page text, and 15 random packages re-checked by a separate agent. Three records corrected.
4. **Archive lookup (2026-11-10):** the Wayback Machine checked for the 88 packages with no listed price. 4 had a dated archived price, kept apart from current prices.
5. **Price masking (2026-11-10):** every price and money figure in the sales text replaced with [AMOUNT], giving 256 price-blind pages. A scan finds no currency figure left.
6. **Name masking (2026-11-21):** at my request, every person's name on the 30 pages I code blind (testimonial givers, clients, guests and the coach), and any business name or web address built from one, replaced with [NAME]. A Claude agent listed the names, a script replaced them (720 replacements), and a scan found none of the 928 private names or sites left and no dollar figure. Famous people mentioned only as references were left in. The other 226 pages are masked the same way before Claude's coder codes them.
7. **FAQ answers re-captured (2026-11-21):** while coding blind, I found that one page's FAQ answers were missing from its text. The saved copies held only what was on the page when it loaded, so FAQ answers that load only when a question is clicked open came out empty. Claude re-opened that page and the 5 other blind pages flagged as possibly affected, clicked every FAQ open, and kept only text not already in the saved copy. One page gained 8 FAQ answers; the other 5 had nothing missing (their answers were already saved, or the "questions" were rhetorical). The added text is masked for prices and names like the rest, marked with its own capture date, and appended to the page. Empty FAQ answers of this kind are on 3 saved pages, all from one site; the others are re-captured the same way before the full coding run.
8. **Off-site package pages (2026-11-21):** while coding blind, I questioned whether the right sales page was selected for one coach, whose packages are each described on a booking service rather than on the coach's own site. The run rule followed links only within the same site, so those pages were never saved. Claude opened the booking pages for the page's recorded packages and appended their descriptions to the coding text, masked and marked with their capture date. The coach's group workshop page, which had saved empty on 2026-11-10, now says the workshop is no longer available, so it stays unrecorded. The same was done on 2026-11-21 for a second coach whose workshops are booked off-site: 2 of 4 booking pages were still live and their descriptions were appended; the other 2 were no longer bookable. On 2026-11-21 a third case: the page coded for one coach was a short "program specifics" page, while the coach's main coaching page for the same program had been saved the same day (2026-11-10) but filed as a secondary page. Its text was masked and appended, with no new visit to the site. Before the full coding run, Claude checks the 14 saved pages that came out nearly empty and a random 10 of the 73 included sites whose sales page links to a booking, checkout or enroll page elsewhere, to see whether this is a one-off.

9. **Page-choice audit (2026-11-21):** before the full coding run, every language page was checked for a better saved page on the same site, and for failed or very short saves. Of 70 pages flagged by a script (one, already fixed, was skipped), review agents kept 60 as they were, judged 3 short but complete, found 2 where the coach's main page for the same program had been saved but not used (its text was masked and appended), and found 4 failed saves (menus only). Three of those were re-captured live on 2026-11-21 (curriculum and long lists condensed; FAQ answers would not open); the fourth is still a login page live and stays as saved. A random 10 of the 75 sites whose sales page links to an off-site booking or checkout page were also checked: 4 linked only to a free-call scheduler, 1 to a payment script, 1 to course lesson pages, and 4 to booking or checkout pages with at most a few sentences of description and no material sales copy. So off-site package pages with real sales copy (two cases found while coding blind) are rare, and no rule changed.
10. **Name masking for the full run (2026-11-21):** the other 226 pages (and the one blind page re-captured above) were name-masked the same way as the blind pages.
11. **Masking gap found during the full run (2026-11-21):** two of the AI coder's batches flagged prices the masking had missed, written as words or as "## US dollars." A scan for those forms found and masked them on 7 pages (none of them among my blind pages). The AI coder had already read 2 of those pages with the price visible; that is a small breach of its price-blindness, recorded here.
    A second sweep after the full run found prices split across lines (a currency sign on one line, the number on the next) and "sale price" lines: masked on 2 pages. One of them, L-P270, is a pilot page I coded blind: a per-session price was visible on it when I coded it (Claude's masking error). My L-P270 codes are kept and this is disclosed. A final scan finds no currency sign followed by a number on any of the 256 pages.
12. **Full coding run (2026-11-21):** the AI coder (Claude Fable 5.2) coded all 256 pages in 24 sealed batches from the final codebook. Every yes has an evidence quote, and a script found all 895 quotes word for word on their pages. It marked 800 close calls (148 "very close," 652 "close") and flagged 70 pages as incomplete, a catalog or home page, a short booking listing, or mostly not sales copy.
13. **Close-call review and final codes (2026-11-22):** I ruled on all 146 "very close" calls on pages I had not coded, with the AI coder's decision hidden and its reason visible. My ruling matched its hidden answer on 79 of 144 (2 I first marked unknown, then settled). Three of my notes refined rules (disclaimers, outside knowledge for Upsell, the free call), and the AI coder re-checked OutcomePromise on the 9 pages the disclaimer rule could affect (1 changed). The final codes are: my codes on the 15 pages I coded blind, as agreed in discussion; my rulings on the 146 calls; and the AI coder's codes everywhere else (652 "close" calls keep its answer).

## Scope and page decisions before prices are revealed (2026-11-22)

- **Audition preparation stays in.** Let's leave audition prep programs in. This might be bias: one of the seed programs, an audition-prep program, is the main motivator for this research. The 12 programs typed "professional audition" are all kept.
- **Thin pages (rule set 2026-11-22):** a page is thin if the AI coder flagged it as having little sales text **and** it is under 500 words: 14 pages (L-P238, L-P49, L-P85, L-P203 to L-P207, L-P24, L-P34, L-P234, L-P166, L-P83, L-P159). Thin pages are left out of the sales-language analysis. They stay in the price-only questions (hypothesis 1, what a buyer can know, refunds and guarantees), which do not depend on the language codes. Claude proposed this; I was unsure, then decided: keep them.
- **Incomplete captures:** fixed where possible (the FAQ answers that did not open, a description cut off mid-sentence), then re-coded.
- **All other flagged pages** (several offers on one page, mixed-in text, closed enrollment) stay in, and the main results are also run without them as a check.

## Expectations before prices are revealed

_Written before any price, per-hour figure or result was seen. Each entry is dated._

**Decided 2026-11-22:** we'll have to do a round of expectations and analysis before prices are revealed.

1. **Origin story (2026-11-22):** origin story might not be that high pressure, and it might be extremely common in these types of products. _(Noticed while reviewing close calls, which over-represent hard cases; the looser OriginStory rule from round 2 also makes the code more common.)_
2. **Share above $150 (2026-11-22):** my views on this have not changed, and I have already given reasons: more than 50% of programs above the $150 line, tested on the median (see the brief, "How I would know I was wrong").
3. **Density and price (2026-11-22):** I think that higher priced programs will have a generally higher density. However, I think the shortest programs will have the lowest price. Since they are so short (in sales page length), they will naturally have a higher density. An inverse bell curve will develop. _(Claude's reading, for testing: plotted against price per contact hour, tactic density is high at the cheapest end (short pages) and at the most expensive end, and lowest in the middle: a U shape.)_
4. **Origin story is common (2026-11-22):** I think most programs will have an origin story. _(Tested as: OriginStory on more than half of the coded pages.)_
5. **Urgency on dense pages (2026-11-22):** I think the highest density programs will have a lot of urgency. _(Tested as: Urgency is more common among the densest pages than among the rest.)_
6. **Apply and Exclusivity together (2026-11-22):** I think applications and exclusivity will often be seen together. _(Tested as: a page with Apply is more likely to have Exclusivity than a page without it.)_

**Tactics-only "before" look (2026-11-22, still no prices seen):** 242 pages after removing the 14 thin pages. 95% use at least one tactic; median 3.5 tactics; tactic count and page length rank-correlate at about 0.6. E4 (most programs have an origin story): not supported, OriginStory on 26%. E1 (extremely common): not supported. E5 (urgency on the densest pages): not supported, 15% in the densest quarter and 15% elsewhere. E6 (Apply and Exclusivity together): supported, Exclusivity on 47% of pages with Apply and 13% of pages without.

## Planned figures

**Course rule:** at least one graph, chart or diagram that the paper's text actually uses, labeled well enough to read on its own. AI may build figures from a specification I write.

**Decision 8:** which figure or figures, and what each one shows.

**Decided 2026-11-09:** two figures. Both use price per contact hour on a log scale (so $100, $1,000 and $10,000 sit evenly apart and the outliers don't squash everything else), and both draw the $150 line.

1. **Dot plot (Claude's idea; I chose it).** One dot per program along the price-per-contact-hour line. The $150 line is marked, and so is the median. Filled dots use at least one tactic; hollow dots use none. The share of dots right of the line is suspicion 1; the share of filled dots right of the line is suspicion 2a. Self-paced courses sit in a small separate strip against their $300 line.
2. **Box-and-whisker plots by tactic count (my idea).** One box per group: **0 | 1–3 | 4–6 | 7 or more** tactics. Each box shows the median, the middle half of the programs and the spread, with a small mark for the mean and one dot per program laid over it. The $150 line runs across the chart. This tests suspicion 2b. **Proposal (Claude):** a group with fewer than 5 programs shows its dots only, without a box, because a box drawn from so few points is misleading.

**Figure 2 changed (2026-11-21, prices still sealed):** yeah, the box plot has to go. Figure 2 becomes a scatter plot: tactic count (0–12) across, price per contact hour up on a log scale, one dot per program, with the $150 line. The test of tactics against price is a rank correlation (with tactics per 1,000 words as a check), not a comparison of groups. No bins and no "high-pressure" line are needed. Since the limitation of the "lines" is so strong, can we "fuzzy up" the edges? Claude proposed these and I accepted them ("Those are great ideas"): (1) instead of group boundaries, a smoothed running median of price across tactic count, with a shaded band showing its uncertainty; (2) each program's dot drawn as a short horizontal bar from its sure tactics to its sure tactics plus any close calls not ruled on, so coding uncertainty is visible. The hypothesis in the brief (1, 2a, 2b) is unchanged.

Programs with an unlisted price, and self-paced courses in figure 2, have no price per contact hour, so they're left out of the figures. Each caption says how many were left out.

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
  - **By field (decided 2026-11-09):** the main test uses all fields against the $150 line. The share is also reported for music alone.
  - `MedianPerContactHour` and `MeanPerContactHour`. The test uses the median, because I anticipate many outliers; the mean is still calculated, and outliers are not removed (decided 2026-11-09).
- Do the overpriced programs use desperation language?
  - The share of overpriced and of not-overpriced programs that use each tactic in the codebook.
  - The median price per contact hour of the programs that use at least one tactic, and of the programs that use none (decided 2026-11-09).
- What can a buyer know before paying? (Question 2)
  - The share of programs that list their price, their contact hours and their group size.
- Why haven't refunds, outcome-based fees or published results pushed the expensive programs out? (Question 3)
  - How many programs offer each, and whether they are the cheaper or the more expensive ones.
- What are clients actually paying for: skills, success, or a network?
  - The count of each `PromisedOutcome`.
- What a better way might look like: I don't know yet, and figuring that out is what the paper is about. The data won't answer this on its own.
  - **Course rule:** the paper recommends a policy or strategy and defends it against its strongest objection. The recommendation is mine to write.
