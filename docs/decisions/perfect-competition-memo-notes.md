# Things we think should go in the Stage 3 memo

Notes toward the Stage 3 analysis and memo, collected while writing the Stage 1.2 spec. These are not the memo. Claude placed them from our chat; lines marked _(Claude)_ are Claude's wording, not mine.

## Assumptions in the model that might not hold on a real farm

- **Beds planted in waves.** The labor formula treats `q` beds of a crop as sharing the whole season, so adding a bed raises the hours of every bed of that crop. My argument: "If you plant one bed, that will take 99 hours. If you then decide to plant a second bed, it then doesn't retroactively make the 99 hours larger." _(Claude) If a real farm planted in waves, the formula would overstate the hours. The numbers in the model don't change either way._ This is the disagreement we've asked Professor Stauffer to resolve.
- **Fatigue may not be crop-specific.** The model counts each crop's diminishing returns separately. It's reasonable to assume that worker diminishing effort/fatigue is possible across all crops, not just one.

## Things to check in the model's results

- **Worker fatigue.** The What-if tab runs the model again with workers losing 1% an hour, and 1% more each day until the weekend. How much do the best mix and profit change?

- **Where my 720 hours run out.** I predicted the cost of the next bed would drop there, because the cost of a labor hour is cut in half. Did it?
- **Average versus marginal.** _(Claude) In a two-tomato-bed plan, each bed takes 108.9 hours, but the second bed adds 118.8. When the marginal number is above the average, the average is rising._
