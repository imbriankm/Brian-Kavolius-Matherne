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

This model will help us decide which ratio of crops to plant and how many of those crops we should plant. It will also tell us if there is a "ceiling" where planting less than 64 beds makes sense. It might also suggest a floor, where the fixed costs won't be covered by what we've planted. I guess in that sense, a model will tell us if our business is even possible. If the model says there is no profitable amount of crops we can plant that will cover our fixed costs, then that will be valuable information.

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

## Structure

_Not written yet._

## Calculation logic

_Not written yet._

## Conventions

- **Beds:** 64 is the most we can plant, not a requirement. This is a blend of my assumptions from the brief. I think we will end up planting all 64. However, the model may show we should plant fewer.
- **Carrot labor:** use `Carrot_HoursPerWeek` = 0.833 exactly as the case prints it, not 5/6.
- **Labor:** My own first 720 hours are valued at $34.72 per hour. Every hour of labor after 720 costs $17.36. (I can do this for up to 5,760 hours after the initial 720: 4 temp workers × 1,440 hours each.)

## Validation rules

_Not written yet._

## Outputs

_Not written yet._

## Audit findings

_Added after the build._
