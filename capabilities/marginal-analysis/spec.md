---
type: spec
capability: marginal-analysis
engagement: perfect-competition
date: 2026-10-08
status: draft
built_with:
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
| `Carrot_HoursPerWeek` | 0.833 | labor hours per week per bed | Case facts, crop table |
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
| `OwnRate` | 34.72 | $ per hour (implied) | Case facts, farm inputs |
| `TempRate` | 17.36 | $ per hour | Case facts, farm inputs |
| `TempHoursEach` | 1,440 | hours per worker per season | Case facts, farm inputs |
| `MaxTemps` | 4 | workers | Case facts, farm inputs |

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

One workbook file (`model.xlsx`) with five tabs:

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

### Marginal cost

The cost one extra bed adds, using the same subtraction rule as for hours: the total cost (labor plus fertilizer) with `q` beds, minus the total cost with `q − 1` beds.

What I expect: where my 720 hours run out, the cost of the next bed will likely drop, because the cost of a labor hour is cut in half ($34.72 to $17.36). I don't think diminishing returns will have that large of an effect at that point, but the model will show that.

### Schedules tab

Each crop's schedule is worked out on its own, as if it were the only crop planted: my 720 hours go to that crop first, then temporary hours. For each bed `q`, the schedule shows that crop's hours, the extra hours bed `q` adds, the marginal cost of bed `q`, and the crop's revenue per bed, so I can see where marginal cost meets revenue. Each schedule runs past the crop's cap until marginal cost is above revenue per bed.

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
WorkHours     = TotalHours, from the main model
OwnWork       = the smaller of WorkHours and OwnHours × OwnEfficiency
OwnPaidHours  = OwnWork ÷ OwnEfficiency
TempPaidHours = (WorkHours − OwnWork) ÷ TempEfficiency   (no more than MaxTemps × TempHoursEach)
LaborCost     = OwnPaidHours × OwnRate + TempPaidHours × TempRate
```

Revenue, fertilizer, profit and the blended rate work as in the main model, and Solver follows the same rules, run separately on this tab.

## Conventions

- **Beds:** 64 is the most we can plant, not a requirement. This is a blend of my assumptions from the brief. I think we will end up planting all 64. However, the model may show we should plant fewer.
- **Whole beds:** you can't plant part of a bed, so every bed count is a whole number.
- **Carrot labor:** use `Carrot_HoursPerWeek` = 0.833 exactly as the case prints it, not 5/6.
- **Labor:** My own first 720 hours are valued at $34.72 per hour. Every hour of labor after 720 costs $17.36. (I can do this for up to 5,760 hours after the initial 720: 4 temp workers × 1,440 hours each.)
- **Labor rates:** use `OwnRate` = 34.72 and `TempRate` = 17.36 exactly as the case prints them. Both work out to about $25,000 for the season (720 × 34.72 and 1,440 × 17.36 are each $24,998.40), but I'm not using the unrounded $25,000 ÷ hours.

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

- Which ratio of crops to plant, and how many of each.
- Whether there is a "ceiling" where planting less than 64 beds makes sense.
- Whether the fixed costs are covered — whether our business is even possible.

And also:

- At what point each crop becomes not-profitable (in theory, not just capped at the bed cap). Each crop's schedule keeps going past its cap until an extra bed costs more than it earns.
- The minimum amount I can plant and still manage my fixed costs. I want both answers: the smallest total number of beds where the best mix still covers the $20,000, and the fewest beds of each crop that would cover it on its own.
- What if workers got tired throughout the week? The best mix, profit and paid labor hours, side by side with the main model.
- What percentage of my costs are labor, fertilizer, and the fixed cost — all three, so I know which is most expensive for me. (If I find big discrepancies, I might be able to focus my negotiation efforts on lower prices elsewhere.)

## Audit findings

_Added after the build._
