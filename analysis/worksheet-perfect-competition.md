# Stage 3 worksheet: perfect competition

This is where I answer the Stage 3 questions. I write under each **My answer**. The final analysis, `analysis/YYYY-MM-DD-perfect-competition-analysis.md`, is built from my answers here and nothing else.

_Claude set up the questions and pulled the numbers from `capabilities/marginal-analysis/model.xlsx` on 2026-11-01. Each number gives its cell, e.g. `Schedules!F18`. Numbers marked **(recalculated)** are not in the workbook: Claude worked them out from the workbook's own formulas, and they need checking in Excel before I cite them. The words under **My answer** are mine._

How to read the Schedules tab: the row is the bed number + 8 (bed 10 is row 18). Tomato columns are A–I, carrot K–S, mesclun U–AC. Each crop there is worked out as if it were planted alone, with my 720 hours going to it first.

---

## Question 1: Where does the money crop stop, and why there?

**The numbers**

| | Tomato | Carrot | Mesclun |
|---|---|---|---|
| Revenue per bed | $8,800 (`Inputs!B6`) | $2,094 (`Inputs!B11`) | $2,700 (`Inputs!B16`) |
| Cap | 20 (`Inputs!B5`) | 20 (`Inputs!B10`) | 30 (`Inputs!B15`) |
| Beds the model plants | 10 (`Optimizer!B6`) | 20 (`Optimizer!B7`) | 30 (`Optimizer!B8`) |

Tomato, either side of where it stops:

| Tomato bed | Extra hours it adds | Marginal cost | Revenue per bed | Revenue − MC |
|---|---|---|---|---|
| 9 | 366.6 (`Schedules!C17`) | $7,243.78 (`Schedules!F17`) | $8,800 | +$1,556.22 |
| 10 | 424.4 (`Schedules!C18`) | $8,248.59 (`Schedules!F18`) | $8,800 | +$551.41 |
| 11 | 490.2 (`Schedules!C19`) | $9,390.72 (`Schedules!F19`) | $8,800 | −$590.72 |

- On the whole farm: an 11th tomato bed would lower SeasonProfit by $590.72, and dropping to 9 would lower it by $551.41 **(recalculated)**. Those are the same two numbers as the tomato schedule, because from tomato bed 6 on, every extra hour is a temp hour on both tabs.

**Questions to answer**

1. Which crop earns the most revenue per bed? Does the model plant it all the way to its cap?
2. Where does it stop, and what are the marginal costs on either side of that bed?
3. What actually decides how many beds to plant: revenue per bed, or something else?

**My answer**

The tomato beds by far earn the most revenue per bed. However, the model does not plant it all of the way to the cap, due to how the diminishing returns and labor costs affect those plants in particular.

The relationship between revenue, diminishing returns, and labor costs all work together to determine how many of one crop to plant.


---

## Question 2: Which constraints bind, and what would relaxing one be worth?

**The numbers at 10 / 20 / 30**

| Constraint | Limit | Used | Left over |
|---|---|---|---|
| Tomato cap | 20 (`Optimizer!F13`) | 10 (`Optimizer!B6`) | 10 |
| Carrot cap | 20 (`Optimizer!F14`) | 20 (`Optimizer!B7`) | 0 |
| Mesclun cap | 30 (`Optimizer!F15`) | 30 (`Optimizer!B8`) | 0 |
| Total beds | 64 (`Optimizer!F16`) | 60 (`Optimizer!B17`) | 4 (`Optimizer!B18`) |
| Temp hours | 5,760 (`Optimizer!B14`) | 4,557.2 (`Optimizer!B13`) | 1,202.8 |
| Whole beds only | — | — | — |

Not a Solver constraint, but worth knowing: my own 720 hours are all used (`Optimizer!B12` = 720 out of `Optimizer!B11` = 5,277.2 total hours).

**⚠ Check before citing Solver's report.** Answer Reports 4 and 5 (the 10/29 runs) list the tomato cap as "Binding" with 0 slack. That suggests the tomato cap was 10, not 20, when those runs were made. Answer Reports 6 (Setup 2) and 7 (What if) show it at 20. The best mix is the same either way, because tomato stops at 10 on its own (see Question 1). But the report you'd cite for this question has the wrong cap in it. Rerunning Setup 1 once now would give a clean report, and that rerun counts as a check for your reflection. Answer Reports 1 and 2 are older still, from before the inputs changed.

What one more bed is worth at each cap (revenue per bed − marginal cost of the next bed):

| One more bed of | Marginal cost of that bed | Revenue per bed | Adds to SeasonProfit |
|---|---|---|---|
| Carrot (bed 21) | $1,741.51 (`Schedules!P29`) | $2,094 | +$352.49 |
| Mesclun (bed 31) | $2,453.53 (`Schedules!Z39`) | $2,700 | +$246.47 |

- At the caps themselves: carrot bed 20 costs $1,688.95 (`Schedules!P28`), and mesclun bed 30 costs $2,420.10 (`Schedules!Z38`).
- These match the whole farm: 10 / 21 / 30 gives $43,114.16, and 10 / 20 / 31 gives $43,008.14 **(recalculated)**. You can check this in Excel by raising `Carrot_Cap` to 21 on Inputs, rerunning Solver, then setting it back.
- Excel won't make a Sensitivity Report when there are whole-number constraints, so the value of one more bed can't come from Solver here. It comes from the schedules or from a rerun like the one above.

If the caps kept rising, each further bed would be worth **(recalculated)**:

| Bed | 21 | 22 | 23 | 24 | 25 | 26 | 27 |
|---|---|---|---|---|---|---|---|
| Carrot | +$352.49 | +$298.09 | +$241.77 | +$183.49 | +$123.18 | +$60.77 | −$3.81 |

| Bed | 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 |
|---|---|---|---|---|---|---|---|---|
| Mesclun | +$246.47 | +$212.45 | +$177.82 | +$142.58 | +$106.72 | +$70.22 | +$33.07 | −$4.73 |

- That's where the last profitable beds of 26 carrot and 37 mesclun come from (`Schedules!N5`, `Schedules!X5`).
- With both caps lifted, there are only 4 spare beds. The best four extra beds give 10 / 23 / 31 = 64 beds and $43,900.49, using 4,882.3 temp hours **(recalculated)**. At that point the 64-bed limit is the one that binds.

**Questions to answer**

1. List each constraint and mark it binding or slack at 10 / 20 / 30.
2. For a crop that stops at its cap: is its marginal cost still below its price there? Is it the economics that stops production, or a fence?
3. What would one more unit of each binding constraint add to profit, and where in the model do you read that number?
4. Is any slack constraint worth spending a dollar on?

**My answer**



---

## Question 3: Does any marginal cost curve ever fall, and what would explain it?

**The numbers**

All three curves dip once, then climb again:

| Crop | Bed | Marginal cost | Extra hours that bed adds | Total hours after that bed |
|---|---|---|---|---|
| Tomato | 5 | $7,660.86 (`Schedules!F13`) | 197.7 (`Schedules!C13`) | 724.7 (`Schedules!B13`) |
| | 6 | $4,906.28 (`Schedules!F14`) | 231.9 (`Schedules!C14`) | 956.6 (`Schedules!B14`) |
| Carrot | 16 | $2,552.10 (`Schedules!P24`) | 60.8 (`Schedules!M24`) | 712.6 (`Schedules!L24`) |
| | 17 | $1,670.90 (`Schedules!P25`) | 63.5 (`Schedules!M25`) | 776.0 (`Schedules!L25`) |
| | 18 | $1,589.14 (`Schedules!P26`) | 66.2 (`Schedules!M26`) | 842.2 (`Schedules!L26`) |
| Mesclun | 13 | $2,988.40 (`Schedules!Z21`) | 60.7 (`Schedules!W21`) | 687.5 (`Schedules!V21`) |
| | 14 | $2,522.58 (`Schedules!Z22`) | 62.1 (`Schedules!W22`) | 749.7 (`Schedules!V22`) |
| | 15 | $1,983.96 (`Schedules!Z23`) | 63.6 (`Schedules!W23`) | 813.3 (`Schedules!V23`) |

- My hours: 720 (`Inputs!B23`) at $34.72 an hour (`Inputs!B24`). Temp hours: $17.36 an hour (`Inputs!B25`).
- Each crop's extra hours per bed keep rising the whole way (column C, M or W). None of them falls.
- On the carrot and mesclun schedules, the drop pulls marginal cost back under price. Carrot is above price at beds 11–16 (`Schedules!R19:R24` say "No") and below it again from bed 17 to 26. Mesclun is above price at beds 7–13 (`Schedules!AB15:AB21`) and below it from bed 14 to 37.
- On the Optimizer, the three crops share the 720 hours, and all 720 are used (`Optimizer!B12`).

**Chart for this question:** marginal cost against bed number for each crop, with price as a flat line (see Figures below).

**Questions to answer**

1. Where does each curve dip, and what changes in the labor calculation at that bed?
2. Did the physical returns change (hours per bed), or did the cost of the next hour of work change?
3. What does that say about what a marginal cost curve measures?

**My answer**



---

## Question 4: Would each crop make money on its own, and does that matter?

**The numbers**

Each crop planted alone, carrying the whole $20,000 fixed cost (`Inputs!B22`):

| Crop alone | Best profit within its cap | Best profit at any size | Fewest beds to cover the fixed cost |
|---|---|---|---|
| Tomato | $6,172.77 at 10 beds (`Schedules!I18`) | same | 7 (`Schedules!D6`) |
| Carrot | −$16,488.92 at 20 beds (`Schedules!S28`) | −$15,229.13 at 26 beds (`Schedules!S34`) | None (`Schedules!N6`) |
| Mesclun | −$11,922.19 at 30 beds (`Schedules!AC38`) | −$10,932.85 at 37 beds (`Schedules!AC45`) | None (`Schedules!X6`) |

In the best mix, each crop's profit before the fixed cost (`Optimizer!G6:G8`): tomato $33,143.42, carrot $13,682.27, mesclun $15,935.98. SeasonProfit is $42,761.66 (`Optimizer!B19`).

Price against marginal cost, average variable cost and average total cost, at the bed counts the model plants. The workbook has no AVC or ATC column. These are worked out as total cost for q beds ÷ q, and ATC adds $20,000 ÷ q **(recalculated)**:

| Crop, beds | Price | MC of last bed | AVC, crop alone | ATC, crop alone | AVC, whole farm |
|---|---|---|---|---|---|
| Tomato, 10 | $8,800 | $8,248.59 (`Schedules!F18`) | $6,182.72 (`Schedules!E18` ÷ 10) | $8,182.72 | $5,485.66 |
| Carrot, 20 | $2,094 | $1,688.95 (`Schedules!P28`) | $1,918.45 (`Schedules!O28` ÷ 20) | $2,918.45 | $1,409.89 |
| Mesclun, 30 | $2,700 | $2,420.10 (`Schedules!Z38`) | $2,430.74 (`Schedules!Y38` ÷ 30) | $3,097.41 | $2,168.80 |

- "Whole farm" is that crop's fertilizer plus labor on the Optimizer, divided by its beds, e.g. (`Optimizer!E7` + `Optimizer!F7`) ÷ 20. The Optimizer charges labor to each crop at one blended rate, $19.73 an hour (`Optimizer!B16`).
- Costs that change when one more bed is planted: fertilizer and labor. The cost owed whether or not anything is planted: the $20,000 fixed cost.

**Questions to answer**

1. Charge the whole fixed cost to one crop at a time. Does it ever make a profit?
2. Compare that with what the best mix plants.
3. Which costs belong in the decision to plant one more bed, and which are owed either way?
4. Which comparison settles it: price against marginal cost, against average variable cost, or against average total cost?

**My answer**



---

## Against my Stage 1 hypothesis

**The numbers**

- My hypothesis (`docs/briefs/perfect-competition-brief.md`): 14 tomato, 20 carrot, 30 mesclun. I expected tomatoes to be cut first, and wrote that I'd be wrong if the model showed tomato inefficiencies were overcome, for example tomatoes only becoming too costly at 16 beds.
- The model: 10 / 20 / 30, $42,761.66 (`Optimizer!B19`). Tomato marginal cost passes price at bed 11 (`Schedules!F19`).
- 14 / 20 / 30 would need 7,727.7 total hours, so 7,007.7 temp hours. The limit is 5,760, so the farm couldn't staff that plan. If it could, profit would be $31,898.61, which is $10,863.05 less **(recalculated)**.
- Carrot 20 and mesclun 30, both at their caps, matched my guess.

**Question to answer**

One honest paragraph: what I predicted, what the model found, and what my prior got wrong, and why.

**My answer**



---

## Figures

At least two, made by me from the workbook, saved in `analysis/figures/`, and each one shown inline in the analysis where the text first points at it. Lower-case file names make the paths easier to get right.

Data for marginal cost against price:

- Tomato: `Schedules!A9:A58` (bed), `Schedules!F9:F58` (MC), `Schedules!G9:G58` (price).
- Carrot: `Schedules!K9:K58`, `Schedules!P9:P58`, `Schedules!Q9:Q58`.
- Mesclun: `Schedules!U9:U58`, `Schedules!Z9:Z58`, `Schedules!AA9:AA58`.
- Tomato MC reaches $1,001,370 by bed 50 (`Schedules!F58`), which squashes the rest of the chart flat. Charting tomato beds 1–20 (rows 9–28) keeps the dip at bed 6 and the crossing at bed 11 readable.

**Which figures I'm making, and their file names**


