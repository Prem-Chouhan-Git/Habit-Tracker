# Habit Tracker Analytics Dashboard — Power BI

A reusable habit-tracking project built with **Excel, Power Query, DAX and Power BI**. The Excel workbook is the data-entry/configuration layer; Power BI transforms the entries into a star-schema model and analyses consistency, progress toward targets, trends, streaks and habit-level performance.

## What the dashboard answers

- What percentage of my habit targets am I meeting?
- How close am I to targets when I miss them?
- Am I improving or declining over time?
- Which habits/categories are strongest or weakest?
- What changed compared with the previous month?
- What do my 7-day and 30-day trends look like?
- What is my current/best streak for a selected habit?
- How does actual performance compare with the target?
- Are there days of the week where a habit performs better or worse?

## Report pages

### 1. Overview
Provides the high-level view of performance using:

- Target Hit Rate
- Target Achievement %
- 30-Day Rolling Target Hit Rate
- Month-over-Month change
- Days Tracked
- Overall performance trend
- Performance by habit
- Change vs previous month by category
- Category performance over time

### 2. Habit Detail
A drill-through page for one selected habit showing:

- Configured target
- Hit Rate and Achievement %
- Current streak and best streak
- Days since last target hit
- Actual vs Target over time
- Monthly performance
- Performance by day of week
- Monthly summary

## Files

```text
Habit-Tracker-PowerBI/
├── README.md
├── Habit_Tracker_Dashboard.pbix
├── Habit_Tracker_PowerBI_Template.xlsx
└── screenshots/
    ├── overview.png
    └── habit-detail.png
```

## Quick start

1. Download `Habit_Tracker_Dashboard.pbix` and `Habit_Tracker_PowerBI_Template.xlsx`.
2. Open the Excel file and replace the seven example rows in `Daily_Entry` with your own data.
3. Keep entering new dates underneath the existing Excel table. Use the same table across months and years.
4. Save the workbook.
5. Open the PBIX in Power BI Desktop.
6. If required, go to **Home > Transform data > Data source settings > Change Source** and point Power BI to your downloaded Excel template.
7. Select **Home > Refresh**.

Do not refresh the PBIX while `DailyHabitTable` is completely empty. Add at least one valid data row first.

## Customising the habits

The safest customisation is to change the **display name** in `Habit_Config` while keeping the technical mapping unchanged.

| Change | Supported? | Notes |
|---|---|---|
| `Habit_Name` | Yes | Change this to rename what appears in Power BI. Keep `Habit_ID` unchanged. Refresh the PBIX to see the new label. |
| `Target` | Yes, with limitation | The new target is applied on refresh, including to historical rows. Target history is not yet implemented. |
| `Unit` | Yes | Safe as a display label as long as the underlying entered values still use that unit. |
| `Include_in_Overall` | Yes | TRUE includes the habit in the overall KPI; FALSE excludes it. |
| `Target_Operator` | Yes | Supports `>=`, `<=` and `=`. |
| `Category` | Mostly | Generic category visuals update automatically. Some specialised DAX uses the exact `Exercise` and `Guitar` category names. |
| `Habit_ID` | No | Treat this as a stable technical key. |
| `Source_Column` | Do not change alone | It must exactly match the corresponding `Daily_Entry` column header. |
| `Daily_Entry` habit header | Advanced | If renamed, update the matching `Source_Column` too. A Power Query step may also need updating if it references the old header. |
| Add/remove a habit | Not fully automatic in V1 | May require Power Query/model changes as well as an Excel column/config row. |
| `Metric_Type` | Advanced | Changing it can affect how values such as Yes/No are converted in Power Query. |

### Example: rename a habit safely

If you want `Theory Practice` to appear as `Music Theory` in the report:

1. Open `Habit_Config`.
2. Find the existing row for that habit.
3. Change only `Habit_Name` from `Theory Practice` to `Music Theory`.
4. Leave `Habit_ID` and `Source_Column` unchanged.
5. Save Excel and refresh Power BI.

The slicers, habit rankings and Habit Detail page will use the new display name.

## Excel workbook structure

### `Daily_Entry`
One row per calendar day. The included file contains seven example rows.

### `Habit_Config`
Controls the habit metadata and mapping used by Power BI, including:

- `Habit_ID`
- `Category`
- `Habit_Name`
- `Metric_Type`
- `Unit`
- `Target`
- `Active`
- `Source_Column`
- `Target_Operator`
- `Include_in_Overall`

## Current limitations

- **Historical target changes:** changing a target can cause earlier records to be re-evaluated against the new target. A future version should use an effective-dated target-history table.
- **Exercise rest days:** zero-rep exercise days are currently treated as missed targets; planned rest days are not modelled separately.
- **Streaks:** streaks use calendar days, so a rest day currently breaks an exercise streak.
- **Adding/removing habits:** V1 is not fully metadata-driven for completely new or removed habit columns and may require Power Query/model edits.
- **Category-specific measures:** some specialised measures depend on the exact `Exercise` and `Guitar` category names.
- **Target Achievement is capped at 100%:** exceeding a target does not raise the achievement score above 100%.

## Publishing your own copy

Before making the repository public, create a **public/demo PBIX**:

1. Point the PBIX at `Habit_Tracker_PowerBI_Template.xlsx`.
2. Refresh so the PBIX contains only the example data.
3. Save it as `Habit_Tracker_Dashboard.pbix`.
4. Upload that public copy rather than a PBIX containing personal habit history.

Power BI import-mode PBIX files contain imported model data, so changing only the Excel file does not remove data already stored inside a saved PBIX.

## Tech stack

- Microsoft Excel
- Power Query
- Power BI
- DAX
- Star-schema modelling
- Time intelligence
- Data-quality checks
- Drill-through analysis

