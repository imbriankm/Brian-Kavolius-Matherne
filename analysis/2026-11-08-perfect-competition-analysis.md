# Perfect competition — Stage 3 analysis

Workbook: [`capabilities/marginal-analysis/model.xlsx`](../capabilities/marginal-analysis/model.xlsx). Cells are cited as `Tab!Cell`.

## 1. Where does the money crop stop, and why there?

The tomato beds by far earn the most revenue per bed ($8,800 against $2,094 for carrot and $2,700 for mesclun, `Inputs!B6`, `Inputs!B11`, `Inputs!B16`). However, the model does not plant it all of the way to the cap, due to how the diminishing returns and labor costs affect those plants in particular.

The relationship between revenue, diminishing returns, and labor costs all work together to determine how many of one crop to plant.

The extra cost of planting that bed exceeds the revenue it would bring in (bed 10 costs $8,248.59 against $8,800 revenue, `Schedules!F18`; bed 11 costs $9,390.72, `Schedules!F19`).

<!-- Figure 1 goes here: ![Figure 1: ...](figures/<file-name>.png) -->

## 2. Which constraints bind, and what would relaxing one be worth?

The limits defined by the case are the hard limits. You are not allowed to plant more than 20 tomato or carrot beds, and 30 mesclun beds, or total beds (`Optimizer!F13:F16`). The temp-worker hours are also hard limits (`Optimizer!B14`). If those limits did not exist, we would plant more carrots and mesclun beds (at its cap, carrot bed 20 costs $1,688.95 against its $2,094 price, `Schedules!P28`; mesclun bed 30 costs $2,420.10 against $2,700, `Schedules!Z38`). (A fence, not economics).

The tomato plant's 'limit' is the economics. The 11th bed doesn't make enough money to cover its costs, so we don't plant it ($9,390.72 marginal cost against $8,800 revenue, `Schedules!F19`). That's not so much a limit as it is a choice to not continue for more.

The limit, or bottleneck for this particular case is the bed limits on carrots and mesclun. Workers still had availability and we still had 4 total beds available for planting (4,557.2 of 5,760 temp hours used, `Optimizer!B13` and `Optimizer!B14`; 60 of 64 beds planted, `Optimizer!B17` and `Optimizer!F16`). Raising the carrot and mesclun caps would allow us to get a little more profit (one more carrot bed adds $352.49, `Schedules!P29`; one more mesclun bed adds $246.47, `Schedules!Z39`). At that point, more calculations would have to be run to determine if the bed limits are the bottleneck or if worker hours are. We can calculate how much these individual limits cost by running the solver without those limits and compare the profits.

## 3. Does any marginal cost curve fall, and why?

There is a drop in the cost of beds due to the switchover of labor. The temp worker's hours are about half as expensive as the farmer's (temp $17.36 an hour against $34.72, `Inputs!B25` and `Inputs!B24`), so when the farmer times out, the labor cost drops. (This might be a reason to hire the temps first, or more temps in general). What I'm suggesting about the temp workers is in the same style as adding beds. This is a business limitation worth discussing and maybe it can be changed.

- Tomato beds have this drop between 5 and 6 ($7,660.86 to $4,906.28, `Schedules!F13:F14`).
- Carrots have this drop between 16 and 17 ($2,552.10 to $1,670.90, `Schedules!P24:P25`).
- Mesclun has the drop between 13 and 14 ($2,988.40 to $2,522.58, `Schedules!Z21:Z22`).

<!-- Figure 2 goes here: ![Figure 2: ...](figures/<file-name>.png) -->

The work hours didn't drop, they rose (tomato 197.7 to 231.9 hours, `Schedules!C13:C14`; carrot 60.8 to 63.5, `Schedules!M24:M25`; mesclun 60.7 to 62.1, `Schedules!W21:W22`). However, since additional hours are half the cost, the total effect is a lower cost. The marginal cost that's being tracked is the cost of all the labor put together — not just the labor cost of a single hour.

In this case, I was correct that the costs of beds would drop when the farmer's (my) hours ran out.

## 4. Would each crop make money on its own, and does that matter?

I don't think it matters to distinguish each crop in this scenario. You have the beds laid aside for these crops, and I don't see a practical reason to only focus on one crop. Carrots and mesclun will never, with the limited beds, cover the $20,000 (`Schedules!N6`, `Schedules!X6`). Tomatoes will at 7 beds (`Schedules!D6`).

Even though a single crop doesn't cover the $20,000 alone, having any amount of profit does help to close the gap (in the best mix, tomato, carrot and mesclun earn $33,143.42, $13,682.27 and $15,935.98 before the fixed cost, `Optimizer!G6:G8`). The $20,000 is a fixed cost (`Inputs!B22`), so it shouldn't be used to evaluate the plan itself- it is simply failure line. If there is no combination to reach $20,000, then you consider stopping operations. If you can cover the $20,000, it becomes irrelevant for determining the most profitable combination, because the $20,000 is owed no matter what.

For this particular season, the farmer should pay attention to the price against AVC (carrot at 20 beds: $2,094 price against $1,918.45 AVC, worked out as `Schedules!O28` ÷ 20). The next season will have to be checked against the new costs, assuming they change. When considering to plant another bed (meaning plant N+1 beds instead of N beds), the MC of the last bed when compared to the revenue of the bed being considered is how that decision is made.

## Against my Stage 1 hypothesis

My hypothesis represents awareness of the majority of economic factors at play in this case. I just did mental math wrong (tomato bed 11 costs $9,390.72 against $8,800, `Schedules!F19`, so the model stops at 10). I was focused, in my mental math, on the diminishing returns of tomato plants. Even if I was right, not accounting for the labor in my prediction is what created the +4 estimation for tomatoes (I predicted 14; the model plants 10, `Optimizer!B6`). I also correctly estimated that I would want to max the other two crops. Having a correct understanding of the shape of the problem (knowing that max non-tomato crops will be planted) is helpful for the construction of a hypothesis, allowing an analysis to more effectively build a spec for a workbook. It is in this sense that the hypothesis' reasoning is what's important more than the resulting figures. I think that the main reason that this exercise is helpful because of directional understanding of concepts, rather than mental math. If you could sufficiently do mental math, you'd leave the hypothesis stage behind and just start running your own solver.
