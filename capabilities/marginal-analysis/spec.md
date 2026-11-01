---
type: spec
capability: marginal-analysis
engagement: perfect-competition
date: 2026-10-08
status: committed
built_with: model.xlsx, built by Claude from this spec on 2026-10-27
---

# Marginal analysis — farm planting model spec

## Purpose

This model will help us decide which ratio of crops to plant and how many of those crops we should plant. It will also tell us if there is a "ceiling" where planting less than 64 beds makes sense. It might also suggest a floor, where the fixed costs won't be covered by what we've planted. In that sense, a model will tell us if our business is even possible. If the model says there is no profitable amount of crops we can plant that will cover our fixed costs, then that will be valuable (although sad) information.

## Inputs: the named contract

All values come from the case facts page (`case-perfect-competition.html`).

| Name | Value | Unit | Source |
|---|---|---|---|
| `Tomato_Cap` | 20 | beds | Case facts, crop table |
| `Tomato_Revenue` | 8,800 | $ per bed | Case facts, crop table |
| `Tomato_HoursPerWeek` | 2.5 | labor hours per week per bed | Case facts, crop table |
| `Tomato_Fertilizer` | 880 | $ per bed | Case facts, crop table |
| `Tomato_DimRate` | 10% | added labor per bed planted | Case facts, crop table |
| `Carrot_Cap` | 20 | beds | Case facts, crop table |
| `Carrot_Revenue` | 2,094 | $ per bed | Case facts, crop table |
| `Carrot_HoursPerWeek` | 50 ÷ 60 (0.8333…) | labor hours per week per bed | Case facts, crop table (printed as 0.833, which is 50 minutes rounded) |
| `Carrot_Fertilizer` | 440 | $ per bed | Case facts, crop table |
| `Carrot_DimRate` | 2.5% | added labor per bed planted | Case facts, crop table |
| `Mesclun_Cap` | 30 | beds | Case facts, crop table |
| `Mesclun_Revenue` | 2,700 | $ per bed | Case facts, crop table |
| `Mesclun_HoursPerWeek` | 1.25 | labor hours per week per bed | Case facts, crop table |
| `Mesclun_Fertilizer` | 880 | $ per bed | Case facts, crop table |
| `Mesclun_DimRate` | 1.25% | added labor per bed planted | Case facts, crop table |
| `TotalBeds` | 64 | beds (maximum) | Case facts, farm inputs |
| `SeasonWeeks` | 36 | weeks | Case facts, farm inputs |
| `FixedCost` | 20,000 | $ per season | Case facts, farm inputs |
| `OwnHours` | 720 | hours per season | Case facts, farm inputs |
| `OwnSeasonPay` | 50,000 | $ per season | Case facts, farm inputs (the given behind the printed $34.72) |
| `OwnPayBasisHours` | 1,440 | paid hours per season (720 of them in the field) | Case facts, farm inputs |
| `TempSeasonPay` | 25,000 | $ per temp worker per season | Case facts, farm inputs (the given behind the printed $17.36) |
| `TempHoursEach` | 1,440 | hours per worker per season | Case facts, farm inputs |
| `MaxTemps` | 4 | workers | Case facts, farm inputs |

`OwnRate` and `TempRate` are not typed in. The workbook works them out from the givens above, and everything else uses them by name:

```
OwnRate  = OwnSeasonPay ÷ OwnPayBasisHours   (50,000 ÷ 1,440 = 34.7222…, printed in the case as 34.72)
TempRate = TempSeasonPay ÷ TempHoursEach  (25,000 ÷ 1,440 = 17.3611…, printed in the case as 17.36)
```

### What-if inputs

These are my own assumptions, not the case's, for the worker fatigue side project. They are used only on the What-if tab.

| Name | Value | Unit | Source |
|---|---|---|---|
| `HourlyLoss` | 1% | efficiency lost for each hour already worked that day | My assumption |
| `DailyPenalty` | 1% | efficiency lost at the start of each day, carried until the weekend | My assumption |
| `TempDayHours` | 8 | hours per day | 1,440 hours ÷ 36 weeks = 40 hours a week |
| `TempDaysPerWeek` | 5 | days | 1,440 hours ÷ 36 weeks = 40 hours a week |
| `OwnDayHours` | 4 | hours per day | My choice: 720 hours ÷ 36 weeks = 20 hours a week |
| `OwnDaysPerWeek` | 5 | days | My choice: 720 hours ÷ 36 weeks = 20 hours a week |

## Structure

One workbook file (`model.xlsx`) with six tabs:

1. **README:** how to use the workbook, for someone opening it for the first time: what the model is for, what each tab does, how to change an input, how to run Solver, and how to read the Checks tab.
2. **Inputs:** the named inputs above, and nothing else.
3. **Schedules:** one table per crop, one row per bed number, showing the hours and cost that one extra bed adds (the marginal cost schedule).
4. **Optimizer:** the three bed counts Solver changes, the resulting profit and loss, and the Solver setup.
5. **Checks:** pass/fail cells for every constraint and validation rule, green when met.
6. **What if:** titled "What if workers got tired throughout the week?" A side interest project of mine, not part of the case's model. The same model run in parallel, with workers who tire through the day and the week. It has its own bed counts and its own Solver setup, and it never changes the other tabs.

## Calculation logic

### Labor hours

Labor hours for `q` beds of one crop, as the case gives it, for each crop separately:

```
Tomato_Hours(q)  = q × Tomato_HoursPerWeek  × SeasonWeeks × (1 + Tomato_DimRate)^q
Carrot_Hours(q)  = q × Carrot_HoursPerWeek  × SeasonWeeks × (1 + Carrot_DimRate)^q
Mesclun_Hours(q) = q × Mesclun_HoursPerWeek × SeasonWeeks × (1 + Mesclun_DimRate)^q
```

The `(1 + DimRate)^q` term is the diminishing returns. On the farm, I think it's explained by soil fatigue and walking further. Each crop counts separately: more tomato beds don't change carrot or mesclun hours. This is an assumption, because it's reasonable to assume that worker diminishing effort/fatigue is possible (not crop specific).

All the beds are planted at the start of the season, so "one more bed" means planning a season with one more bed, not adding a bed later. With more beds, every bed takes a little more work all season: more bugs, a busier harvest, more walking. Either way, the hours one extra bed adds are the same (118.8 for the second tomato bed), and that is the only number P = MC and Solver use.

### Hours one extra bed adds

The formula gives the total hours for `q` beds. The hours the `q`th bed adds is the total for `q` beds minus the total for `q − 1` beds, with zero beds taking zero hours:

```
Tomato_ExtraHours(q)  = Tomato_Hours(q)  − Tomato_Hours(q − 1)
Carrot_ExtraHours(q)  = Carrot_Hours(q)  − Carrot_Hours(q − 1)
Mesclun_ExtraHours(q) = Mesclun_Hours(q) − Mesclun_Hours(q − 1)
```

For example, one tomato bed takes 99 hours and two take 217.8, so the second bed adds about 119 hours instead of 99.

### Labor cost

My own hours are used first, then temporary hours. Labor cost is worked out for the whole farm, then charged back to each crop at the blended rate:

```
TotalHours      = Tomato_Hours + Carrot_Hours + Mesclun_Hours
OwnHoursUsed    = the smaller of TotalHours and OwnHours
TempHours       = TotalHours − OwnHoursUsed   (no more than MaxTemps × TempHoursEach)
LaborCost       = OwnHoursUsed × OwnRate + TempHours × TempRate
BlendedRate     = LaborCost ÷ TotalHours      (LaborCost is 0 when TotalHours is 0)
Crop labor cost = that crop's hours × BlendedRate
```

Every crop is charged the same blended rate. Labor all matters the same in the end, and I can't deploy it differently: I have to use all my hours first. If the plan uses 720 hours or fewer, every hour is mine and the blended rate is `OwnRate`. Past 720, temporary hours pull the average down.

### Revenue, fertilizer and profit

Revenue and fertilizer are a flat amount per bed for each crop, no matter how many beds I plant. I'm a price-taker, and diminishing returns only affect labor hours.

```
Crop revenue    = q × Crop_Revenue
Crop fertilizer = q × Crop_Fertilizer
Crop profit     = Crop revenue − Crop fertilizer − Crop labor cost
Season profit   = (sum of the three crop profits) − FixedCost
```

The `FixedCost` of $20,000 is always subtracted, even if I plant nothing.

### What Solver does

Solver changes the three bed counts to make `Season profit` as large as possible, with these rules:

- Beds are whole numbers: no partial beds.
- Each crop is between 0 and its cap (`Tomato_Cap`, `Carrot_Cap`, `Mesclun_Cap`).
- Total beds are no more than `TotalBeds`.
- `TempHours` is no more than `MaxTemps × TempHoursEach`.

A second Solver setup on the Optimizer tab finds `BreakEvenBeds`. It has its own three bed counts and its own copy of the profit calculation (names starting `BreakEven_`), so it never overwrites the best mix. It makes `BreakEven_BedsPlanted` as small as possible, with the same rules plus `BreakEven_SeasonProfit` of 0 or more.

The temp-hours limit, `MaxTemps × TempHoursEach`, sits in its own cell (`TempHoursMax`) so Solver can use it as a constraint. The caps and `TotalBeds` are copied from Inputs into a "Limits Solver uses" block on the Optimizer and What if tabs, because Excel's Solver misread a constraint pointing at another tab (`Inputs!$B$15`, `Mesclun_Cap`, came back as `LaborCost`, the cell at B15 on the Optimizer tab). The Solver setups for the Optimizer and What if tabs are saved in the workbook.

### Marginal cost

The cost one extra bed adds, using the same subtraction rule as for hours: the total cost (labor plus fertilizer) with `q` beds, minus the total cost with `q − 1` beds.

For the Schedules tab, each crop's cost is worked out as if it were the only crop planted: my 720 hours go to that crop first, then temporary hours. This only decides which wage each hour is paid; the hours come from that crop's own labor formula, bugs and all. Solver doesn't use these schedules. It costs the whole farm together, as in Labor cost above.

```
Tomato_LaborCost(q) = (the smaller of Tomato_Hours(q) and OwnHours) × OwnRate
                    + (Tomato_Hours(q) − OwnHours, or 0 if that is negative) × TempRate
Tomato_Cost(q)      = Tomato_LaborCost(q) + q × Tomato_Fertilizer
Tomato_MC(q)        = Tomato_Cost(q) − Tomato_Cost(q − 1)
```

Carrots and mesclun work the same way, with their own hours and fertilizer:

```
Carrot_MC(q)  = Carrot_Cost(q)  − Carrot_Cost(q − 1)
Mesclun_MC(q) = Mesclun_Cost(q) − Mesclun_Cost(q − 1)
```

Zero beds cost zero, so `Tomato_MC(1)` is the whole cost of the first bed: 99 hours × `OwnRate` + `Tomato_Fertilizer`.

What I expect: where my 720 hours run out, the cost of the next bed will likely drop, because the cost of a labor hour is cut in half ($34.72 to $17.36). I don't think diminishing returns will have that large of an effect at that point, but the model will show that.

### Schedules tab

Each crop's schedule is worked out on its own, as if it were the only crop planted: my 720 hours go to that crop first, then temporary hours. For each bed `q`, the schedule shows that crop's hours, the extra hours bed `q` adds, the marginal cost of bed `q`, and the crop's revenue per bed, so I can see where marginal cost meets revenue. Each schedule runs past the crop's cap until marginal cost is above revenue per bed. In the workbook, every schedule runs to bed 50, and the Checks tab confirms marginal cost is above revenue on that last row. Each schedule also shows profit after `FixedCost` for that crop alone (`q × Crop_Revenue − total cost − FixedCost`), which is where the `BreakEvenBeds` for each crop comes from.

### What if workers got tired throughout the week?

The worker fatigue question is one I'm running on the side as an interest project. It is not part of the case's model, and the main model's results and checks don't depend on it.

On top of each crop's diminishing returns, workers get less efficient the longer they work. Every hour is 1% less efficient than the hour before. Each day starts 1% lower than the day before, until the weekend resets everyone to 100%:

```
Efficiency(day d, hour h) = 1 − DailyPenalty × (d − 1) − HourlyLoss × (h − 1)
```

For a temp, Monday runs from 100% down to 93%, and Friday from 96% down to 89%. For me, with four-hour days, Monday runs from 100% down to 97%.

The tab shows one week hour by hour for a temp (5 days × 8 hours) and for me (5 days × 4 hours), and averages each:

```
TempEfficiency = average of Efficiency over the temp's week
OwnEfficiency  = average of Efficiency over my week
```

The field hours needed come from the main labor formula, unchanged. Paid hours are higher, because a tired hour gets less done:

```
WorkHours     = the main model's TotalHours formula, using this tab's own bed counts
OwnWork       = the smaller of WorkHours and OwnHours × OwnEfficiency
OwnPaidHours  = OwnWork ÷ OwnEfficiency
TempPaidHours = (WorkHours − OwnWork) ÷ TempEfficiency   (no more than MaxTemps × TempHoursEach)
LaborCost     = OwnPaidHours × OwnRate + TempPaidHours × TempRate
```

Revenue, fertilizer, profit and the blended rate work as in the main model, and Solver follows the same rules, run separately on this tab.

## Conventions

- **Beds:** 64 is the most we can plant, not a requirement. This is a blend of my assumptions from the brief. I think we will end up planting all 64. However, the model may show we should plant fewer.
- **Whole beds:** you can't plant part of a bed, so every bed count is a whole number.
- **Carrot labor:** carrots take 50 minutes a week per bed, so `Carrot_HoursPerWeek` is entered as 50 ÷ 60. The case prints 0.833, which is 50 minutes rounded. Changed on 2026-10-28, after the audit (see Audit findings). When I know the exact number behind a rounded one, I use it; when I don't, I round in the direction that lowers profit.
- **Labor:** My own first 720 hours are valued at $34.72 per hour. Every hour of labor after 720 costs $17.36. (I can do this for up to 5,760 hours after the initial 720: 4 temp workers × 1,440 hours each.)
- **Labor rates:** the case prints $34.72 and $17.36, which are rounded from the season pay and hours behind them: $50,000 ÷ 1,440 paid hours for me, $25,000 ÷ 1,440 hours for a temp. The model is a tool the farmer can reuse when the numbers change, so the inputs are the season pay and the hours, and the workbook works out `OwnRate` and `TempRate` (see Inputs). Changed on 2026-10-29, after the audit (see Audit findings). The exact rates are slightly higher than the printed ones, so this also follows my rule of rounding the way that lowers profit. At 10 / 20 / 30, my 720 hours are all used, and the model charges them at the exact rate, 720 × 34.7222… = $25,000.00, where the printed rate would charge $24,998.40. The temps use fewer hours than the 5,760 available, so their season pay is not a cap here, only the way their hourly rate was worked out.

## Validation rules

These test whether the workbook was built correctly, not whether my prediction was right. The workbook is built only from the named inputs. The published check figures below are not used anywhere in the build; they are compared against its results afterward, in the audit.

### Published check figures

Built from the case's numbers, the workbook should reproduce the results published on the Stage 2 case page. A result passes if it is within 10% of the published figure:

| Check | Published | Passes if within 10% |
|---|---|---|
| Optimal mix: tomatoes | 10 beds | ±1 bed (9–11) |
| Optimal mix: carrots | 20 beds | ±2 beds (18–22) |
| Optimal mix: mesclun | 30 beds | ±3 beds (27–33) |
| Season profit | $42,762 | ±$4,276 |
| Tomatoes alone: where revenue per bed meets marginal cost | about 10 beds | ±1 bed |
| Carrots alone: where revenue per bed meets marginal cost | about 10 beds | ±1 bed |
| Mesclun alone: where revenue per bed meets marginal cost | about 6 beds | ±1 bed |

### Other checks

- **Hand check:** one tomato bed (`q` = 1) takes 1 × 2.5 × 36 × 1.10 = 99 hours. The workbook must show 99.
- **Farm Profit Lab:** at least one marginal cost from the middle of a schedule matches the Farm Profit Lab.
- **Two Solver starts:** Solver run from 0/0/0 and from 20/0/0 lands on the same mix. If not, that is a finding.
- **Formulas, not typed numbers:** every calculated cell is a formula that refers to the named inputs.
- **No error cells:** no `#REF!`, `#DIV/0!` or `#NAME?` anywhere.

### Checks tab

Pass/fail cells, green when met:

- Every bed count is a whole number.
- Each crop is within its cap.
- Total beds are no more than `TotalBeds`.
- `TempHours` is no more than `MaxTemps × TempHoursEach` (5,760).
- Shown for information, not pass/fail: are my 720 hours used up?

### What-if checks

Nobody has published answers for the What-if tab, so these are how I'll know it's built right:

- **Zero fatigue:** with `HourlyLoss` and `DailyPenalty` set to 0%, the What-if tab gives exactly the same mix and profit as the main model.
- **Hand check:** a temp's week averages 94.5%, and mine averages 96.5%.
- **Same checks as the main model:** whole beds, caps, 64 beds, temp paid hours within 5,760, no error cells.

If a check fails, fix the spec and rebuild the workbook from it, rather than patching the workbook by hand.

## Outputs

From my Purpose:

Each output is answered in a named result cell, shown after it.

- Which ratio of crops to plant, and how many of each.
  - Optimizer: `Tomato_Beds`, `Carrot_Beds`, `Mesclun_Beds`.
- Whether there is a "ceiling" where planting less than 64 beds makes sense.
  - Optimizer: `BedsPlanted` (the three bed counts added up) and `BedsUnused` (`TotalBeds` − `BedsPlanted`).
- Whether the fixed costs are covered — whether our business is even possible.
  - Optimizer: `SeasonProfit`. Covered if it is 0 or more.

And also:

- At what point each crop becomes not-profitable (in theory, not just capped at the bed cap). Each crop's schedule keeps going past its cap until an extra bed costs more than it earns.
  - Schedules: `Tomato_LastProfitableBed`, `Carrot_LastProfitableBed`, `Mesclun_LastProfitableBed`, the last bed where marginal cost is at or below revenue per bed. These can pass the cap (26 carrot, 37 mesclun), which shows the farmer what expansion would be worth.
  - Schedules: `Tomato_LastPossibleProfitableBed`, `Carrot_LastPossibleProfitableBed`, `Mesclun_LastPossibleProfitableBed`, the last bed at or below the crop's cap where marginal cost is at or below revenue per bed. This is the figure the farmer can actually plant (10 tomato, 20 carrot, 30 mesclun). The report states both.
- The minimum amount I can plant and still manage my fixed costs. I want both answers: the smallest total number of beds where the best mix still covers the $20,000, and the fewest beds of each crop that would cover it on its own.
  - Optimizer: `BreakEvenBeds`, from the second Solver setup. Schedules: `Tomato_BreakEvenBeds`, `Carrot_BreakEvenBeds`, `Mesclun_BreakEvenBeds`.
- What if workers got tired throughout the week? The best mix, profit and paid labor hours, side by side with the main model.
  - What if: `WhatIf_Tomato_Beds`, `WhatIf_Carrot_Beds`, `WhatIf_Mesclun_Beds`, `WhatIf_SeasonProfit`, `WhatIf_PaidHours`, next to `TotalHours` from the main model.
- What percentage of my costs are labor, fertilizer, and the fixed cost — all three, so I know which is most expensive for me. (If I find big discrepancies, I might be able to focus my negotiation efforts on lower prices elsewhere.)
  - Optimizer: `LaborShare`, `FertilizerShare`, `FixedShare`, each one's share of total cost.

## Audit findings

_Added after the build. Each finding is in my own words._

### Finding 1: the cost of a carrot bed drops at bed 17

The cost drop at bed 17 is when the farmer's working hours are exhausted and the labor is switched to temporary workers.

### Finding 2: the last profitable beds (26 carrot, 37 mesclun) are beyond the caps

There is a profit changing event that occurs when the farmer exhausts his/her her labor hours and the temp workers are employed. The most profitable plan for each crop planted alone would involve 26 carrot beds and 37 mesclun beds, but the caps (20 and 30) prevent those figures. The most profitable result, because of the labor swap, is 10, 20, and 30.

### Finding 3: entering the Solver rules

I had problems with writing the solver, entering the cells correctly. Occasionally, I wouldn't select the second box before selecting my next cell, which would overwrite the first cell. This created an error where the same cell would be entered twice. AI helpers had to spot this error and direct the user (me) to fix it.

### Finding 4: the two rounding decisions

There are two rounding decisions in this model. The first, is the amount of hours it takes to work a carrot bed. The case prints .833 which is a repeating number. However, this is closest to 50 minutes, which is what I used for the model.

Wages were calculated by dividing the number of hours divided by the budget for that labor. ($25k/1440)

These decisions closed a $13 gap from the original model. We made this adjustment after seeing the professor's check figure.

### Finding 5: the fatigue What-if

The what if tab was written as a question of curiosity about workers getting fatigued. I reasoned that diminishing returns may be involved in workers themselves as well. However, after a guess at the rates of fatigue, I learned that it did not have an impact on the beds planted. (Although profit fell slightly due to the increased hours).

### Finding 6: which check I trust most

I think the number of beds is the decision that is being made. Therefore, if a model fits the professor's bed counts, then it is the most trustworthy measure.
