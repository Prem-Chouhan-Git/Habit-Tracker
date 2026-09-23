# Habit Tracker - Setup & User Guide

A reusable habit-tracking system built with a macro-enabled Excel workbook and a four-page Power BI report. The Excel workbook is the data-entry/configuration layer; Power BI is the analysis layer.

## Quick setup

### Excel
1. Save the `.xlsm` workbook somewhere writable.
2. If Windows blocks macros, close Excel, right-click the file, choose **Properties**, tick **Unblock** if shown, and reopen it.
3. Choose **Enable Content / Enable Macros** when prompted.
4. Use the visible Home-page buttons; the hidden technical sheets are not intended for normal editing.
5. Run **CHECK DATA** before Power BI refreshes after structural changes.

The VBA project and button assignments are already inside the workbook. The recipient does not need to import VBA; Excel simply needs permission to run it.

### Power BI
1. Open the `.pbix`.
2. Go to **File > Options and settings > Data source settings**.
3. Change the Excel source to the correct `.xlsm` file.
4. Choose **Home > Refresh**.

## Sample vs starter
- **Sample-data workbook:** keeps the historical example tracking data used to demonstrate the dashboard.
- **Starter workbook:** same structure, habits, targets, schedules, macros and Power BI table names, but tracking history is reset for a new user.

## Daily Excel workflow
1. Home > **LOG TODAY**.
2. Check the date.
3. Enter each habit result.
4. Add optional notes.
5. Choose **SAVE TODAY**.

**Entry meanings**
- Numeric value: compared with the target in force on that date.
- Yes/No: binary result.
- **Rest:** planned non-opportunity; not a miss.
- **Skip:** expected opportunity deliberately not completed; counts against strict adherence.

Use **Missing Days** or **Tracked Days** > **OPEN SELECTED DAY** to backfill/edit a date. Tracking overrides correct tracking status only; they do not create habit performance.

## Manage habits
Use **MANAGE HABITS** to add, change or stop habits. The form controls name, category, measure, unit, goal type, goal, optional upper goal, inclusion in the dashboard, effective date, and schedule. Goal/schedule changes create effective-dated history, so old results retain the rule that applied at the time. **STOP TRACKING** end-dates a habit without deleting history.

## Missing, tracked and paused days
- **Tracked:** the day counts as tracked.
- **Missing:** the day was expected but has no qualifying saved log.
- **Paused:** a deliberate break such as holiday or illness; excluded from expected tracking days.
- **Override:** manual correction to tracking status only.

## Data Check
Run **CHECK DATA** before Power BI refreshes after structural changes. It checks duplicate Date + Habit rows, duplicate Habit IDs, missing targets/schedules, overlapping target/schedule periods, and invalid effective-date ranges.

## Main measures
| Dashboard label | Measure | Meaning |
|---|---|---|
| Tracking Rate | `[Tracking Rate]` | Days Tracked / (Days Tracked + Missing Days); Paused days are excluded. |
| Target Hit Rate | `[Target Hit Rate]` | Strict adherence across scheduled/evaluable opportunities. |
| Logged Target Hit Rate | `[Logged Target Hit Rate]` | Hit rate for scheduled opportunities actually logged as Track. |
| Achievement | `[Opportunity Achievement %]` | Average capped progress toward target; skipped/untracked opportunities contribute zero. |
| Targets Met | `[Targets Met]` | Scheduled opportunities that achieved the target. |
| Recorded Misses | `[Recorded Misses]` | Scheduled Track attempts that did not meet the target. |
| Skipped | `[Skipped Opportunities]` | Scheduled opportunities marked Skip. |
| Untracked | `[Untracked Opportunities]` | Scheduled opportunities with no outcome. |
| Tracking Streak | `[Current Tracking Streak]` | Current run of tracked calendar days since the last Missing day. |
| Target Streak | `[Current Target Streak]` | Consecutive scheduled opportunities met for the selected habit. |
| Best Target Streak | `[Longest Target Streak]` | Longest run of target hits for the selected habit. |
| Days Since Hit | `[Days Since Last Target Hit]` | Calendar days since the selected habit last hit its target. |

Validation: `Scheduled Opportunities = Targets Met + Recorded Misses + Skipped Opportunities + Untracked Opportunities`.

## Navigation and slicers
- Overview / Weekly / Monthly / Yearly: Page Navigator.
- Category: `DimHabit[Category]` (synced across pages).
- Habit Name: `DimHabit[Habit_Name]` (synced across pages).
- Week: `DimDate[Week Label]` (Weekly only).
- Month: `DimDate[Year_Month]` (Monthly only).
- Year: `DimDate[Year]` (Yearly only).

## Overview page
- Tracking Rate card -> `[Tracking Rate]`
- Target Hit Rate card -> `[Target Hit Rate]`
- Achievement card -> `[Opportunity Achievement %]`
- Tracking Streak card -> `[Current Tracking Streak]`
- Missing Days card -> `[Missing Days]`
- Habit summary -> Target Hit Rate, Achievement, Current/Best target streak and Days Since Hit
- Target Hit Rate by Category -> `DimHabit[Category]` + `[Target Hit Rate]`
- Habit Completion Breakdown -> Targets Met, Recorded Misses, Skipped, Untracked; centre = Scheduled Opportunities
- Logged Target Hit Rate: 7-Day Rolling -> `DimDate[Date]` + `[Logged Target Hit Rate - 7D]`

## Weekly page
- Week slicer -> `DimDate[Week Label]`
- Cards -> Tracking Rate, Targets Met, Opportunity Achievement %, Target Hit Rate
- Habit Outcomes by Day -> Habit x Day + `[Daily Habit Outcome]`
- Daily Target Hit Rate -> Day + `[Target Hit Rate]`
- Weekly Habit Summary -> Opportunities, Met, Missed, Untracked, Hit Rate, Achievement
- Daily Habit Outcomes -> Met, Recorded Misses, Skipped, Untracked by weekday

## Monthly page
- Month slicer -> `DimDate[Year_Month]`
- Cards -> Tracking Rate, Targets Met, Opportunity Achievement %, `[Target Hit Rate MTD]`, `[Target Hit Rate MTD Change (pp)]`
- Target Hit Rate by Weekday -> Day + `[Target Hit Rate]`
- Target Hit Rate by Habit -> Habit + `[Target Hit Rate]`
- Monthly Habit Summary -> Opportunities/outcomes/Hit Rate/Achievement by habit
- Daily Target Hit Rate -> Date + `[Target Hit Rate]`

## Yearly page
- Year slicer -> `DimDate[Year]`
- Cards -> Tracking Rate, Targets Met, Opportunity Achievement %, `[Target Hit Rate YTD]`, `[Target Hit Rate YTD Change (pp)]`
- Target Hit Rate by Category -> Category + `[Target Hit Rate]`
- Monthly Tracking Rate -> Month + `[Tracking Rate]`
- Yearly Habit Summary -> Opportunities/outcomes/Hit Rate/Achievement by habit
- Monthly Performance Trend -> Month + `[Target Hit Rate]` + `[Opportunity Achievement %]`

## Power BI source tables
Power BI reads these hidden Excel tables: `LogDataV32`, `TrackingCalendarV32`, `HabitConfig`, `TargetHistory`, `HabitSchedule`.

Core relationships:
- `DimDate[Date] 1 -> * FactHabitDaily[Date]`
- `DimDate[Date] 1 -> * FactTrackingDay[Date]`
- `DimHabit[Habit_ID] 1 -> * FactHabitDaily[Habit_ID]`

## Maintenance and handoff
- Use Manage Habits rather than editing technical keys.
- Never delete historical configuration simply to retire a habit; use STOP TRACKING.
- Keep the technical Excel table names stable unless Power Query is updated too.
- Keep backups of the `.xlsm`, `.pbix`, DAX exports and VBA module.

## Improvements over earlier versions
- Effective-dated target history preserves old target logic.
- Effective-dated schedules and proper rest/non-opportunity handling.
- Habit start/end lifecycle instead of deleting history.
- Separate tracking consistency from habit performance.
- Pause tracking for holidays/illness without fake results.
- Dedicated Missing Days and Tracked Days workflows.
- Logged/updated timestamps for same-day, backfill and edit analysis.
- User-friendly Manage Habits form for adding/changing/stopping habits.
- Built-in data-quality checks.
- Normalized V3.2 Power BI-ready log and tracking-calendar tables.
- Strict Target Hit Rate, Logged Target Hit Rate and Opportunity Achievement are separated.
- Opportunity-based target streaks handle rest/paused days more appropriately.
- Four report levels: Overview, Weekly, Monthly and Yearly.
- Sample-data and reset-starter versions for reusable delivery.
