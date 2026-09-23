# Habit Tracker V2

This README is the practical setup and Excel-use guide for **Habit Tracker V2**.

The package is designed around three files:

- **Habit_Tracker_V2_Starter_Empty.xlsm** - a clean workbook with no habits and no tracking history. Use this when starting your own tracker.
- **Habit_Tracker_V2_Sample_Data.xlsm** - the same tracker with sample habits and historical data. Use this to see how the system looks when it is populated.
- **Habit_Tracker_Dashboard-V2.pbix** - the Power BI report.

The Excel workbook is where you create habits and enter data. Power BI reads that workbook and turns the data into the Overview, Weekly, Monthly and Yearly report pages.

---

## 1. First-time setup

### 1.1 Choose the workbook you want to use

Use **Habit_Tracker_V2_Starter_Empty.xlsm** if this is your own tracker. It starts with no configured habits, so the first thing you will do is create them on **Manage Habits**.

Use **Habit_Tracker_V2_Sample_Data.xlsm** if you want to explore the finished system before entering your own data.

### 1.2 Allow the Excel automation to run

The workbook already contains the VBA used by the buttons and automation. You do not need to install or paste any code.

Windows may block macros in a downloaded file. If Excel says the macros are blocked:

1. Close Excel.
2. Right-click the `.xlsm` file in File Explorer.
3. Choose **Properties**.
4. If you see **Unblock**, tick it and press **Apply**.
5. Open the workbook again.
6. Click **Enable Content** if Excel shows the security warning.

Only enable macros for a copy of the tracker that you trust.

### 1.3 Connect Power BI to the workbook

1. Open **Habit_Tracker_Dashboard-V2.pbix**.
2. Go to **File > Options and settings > Data source settings**.
3. Select the Excel source and choose **Change Source**.
4. Browse to the `.xlsm` workbook you want the dashboard to use.
5. Press **Refresh**.

Power BI does not need the macros to run. It only reads the workbook tables. The macros are needed for the Excel buttons and automatic maintenance of the tracker.

If you are using the empty starter, the dashboard will naturally be blank or show zeroes until you create habits and start logging data.

---

## 2. The normal day-to-day routine

For most days the workflow is simply:

1. Open Excel and enable content if prompted.
2. On **Home**, press **LOG TODAY**.
3. Check the date.
4. Enter the result for each habit that needs an entry.
5. Add a note if you want one.
6. Press **SAVE TODAY**.
7. Save the workbook.
8. Refresh Power BI when you want the dashboard updated.

You should normally enter the actual result, not whether you think the day was good or bad. The workbook compares the result with the correct target for that date.

---

## 3. What to enter on Log Today

The **Entry** column accepts the following types of input:

- **Number** - use the actual value, for example `35` push-ups, `42` minutes of reading, or `7.5` hours of sleep.
- **Yes / No** - used for habits that are measured as a yes/no result.
- **Rest** - use this when the habit was deliberately not an opportunity that day. A rest day is not treated as a target miss.
- **Skip** - use this when the habit was expected that day but you deliberately did not do it. A skip is an opportunity that was not completed.

A blank scheduled opportunity is different from a skip. If an expected habit is left without an entry, it can become **Untracked**. This lets the report distinguish between "I chose not to do it" and "I did not record anything".

The **Notes** column is optional and is for anything useful to remember later.

### Rest vs Skip

Use **Rest** for a planned non-opportunity. Use **Skip** when the habit was expected but you did not do it.

That distinction matters because the Power BI report treats them differently.

---

## 4. Creating your first habit

The empty starter contains no habits. Create them on **Manage Habits**.

1. From Home, press **MANAGE HABITS**.
2. Press **NEW HABIT**.
3. Complete the form.
4. Press **SAVE CHANGES**.
5. Repeat for each habit you want to track.

### Habit fields

**Name**  
The name shown in Excel and Power BI, for example `Reading` or `Push-ups`.

**Category**  
A grouping used by Power BI, for example `Exercise`, `Health`, `Reading` or `Music`. You can use your own category names.

**Measure**  
Choose **Number** or **Yes / No**.

**Unit**  
The unit shown beside the habit, for example `Reps`, `Minutes`, `Hours`, `Pages`, or leave it blank if it is not needed.

**Goal type**  
Choose how the result should be judged:

- **At least** - target is met when the actual value is greater than or equal to the goal.
- **At most** - target is met when the actual value is less than or equal to the goal.
- **Exactly** - target is met only when the actual value equals the goal.
- **Between** - target is met when the value falls inside the lower and upper goal.

**Goal / Upper goal**  
Enter the target. Upper goal is only used for **Between**.

**Include in dashboard**  
Choose Yes if this habit should be included in overall dashboard calculations.

**Changes from**  
The date from which the new habit, goal or schedule should apply. When changing an existing habit later, this is important because the tracker keeps the old history rather than rewriting the past.

**Schedule**  
Choose:

- **Every day**
- **Weekdays**
- **Custom**

For **Custom**, set Monday to Sunday to Yes or No in the custom-days section.

The workbook creates the technical Habit ID, goal history and schedule history automatically.

---

## 5. Editing a habit without changing old results

1. Open **Manage Habits**.
2. Choose the habit from the Habit dropdown.
3. Press **LOAD HABIT**.
4. Change the goal, schedule or other settings.
5. Set **Changes from** to the date on which the change should take effect.
6. Press **SAVE CHANGES**.

The old target and old schedule are kept in history. This means a result from an earlier month is still judged against the target that applied at that time.

Do not manually delete old history records just because a target has changed.

---

## 6. Stopping a habit

If you no longer want to track a habit:

1. Load it on **Manage Habits**.
2. Press **STOP TRACKING**.

The habit is ended rather than deleted. Historical results remain available in the workbook and Power BI.

---

## 7. Excel pages

### Home

The main menu. It shows the current date, last logged date, number of active habits and the number of missing tracking days.

Buttons open the main parts of the workbook: Log Today, Missing Days, Tracked Days, Manage Habits, Pause Tracking, Data Check and Help.

### Log Today

The main data-entry page. Enter the actual result, Yes/No, Rest or Skip and then press **SAVE TODAY**.

### Manage Habits

Used to create habits, change targets, change schedules and stop tracking habits. Goal and schedule changes are effective-dated so older results keep their original rules.

### Missing Days

Shows days currently counted as missing. You can:

- apply a tracking-status override when the calculated day status is wrong;
- use **OPEN SELECTED DAY** to enter or correct the actual habit results for that date.

An override changes tracking consistency only. It does not invent habit results.

### Tracked Days

Shows days currently counted as tracked. Use it to review tracked dates, correct an unusual status, or open an old day for editing.

### Pause Tracking

Used for a holiday, illness or another deliberate break.

Enter a start date, end date and optional reason, then press **APPLY PAUSE**. Paused days stay in the calendar but are not counted as missing tracking days. Existing habit entries on a paused date are not deleted.

Use **CLEAR PAUSE RANGE** if the pause needs to be removed.

### Data Check

Run this before an important Power BI refresh or after making structural changes. It checks for:

- duplicate Date + Habit records;
- duplicate Habit IDs;
- active habits with no target;
- active habits with no schedule;
- overlapping target periods;
- overlapping schedule periods;
- invalid history date ranges.

A clean result should show **OK / 0** for every check.

### Help

A short in-workbook reminder of the main workflows and the meaning of Rest, Skip, Missing, Tracked and Pause.

---

## 8. Tracking status vs habit performance

These are deliberately separate.

A day can be **Tracked** even if several targets were missed. Tracking answers: "Did I record the day?"

Habit performance answers: "Did I meet the scheduled targets?"

A **Paused** day is excluded from tracking consistency. A manual Tracked override does not create fake habit successes.

---

## 9. Refreshing Power BI

After changing Excel:

1. Save the workbook.
2. Open Power BI.
3. Press **Refresh**.

If the PBIX has been moved to another computer or the Excel filename/path has changed, use **Data source settings > Change Source** and point it at the correct workbook.

---

## 10. Quick troubleshooting

**The Excel buttons do nothing**  
Macros are probably disabled. Close Excel, unblock the downloaded file if necessary, reopen it and enable content.

**Power BI says it cannot find the Excel file**  
Change the source in Power BI Data source settings.

**The dashboard is blank after switching to the empty starter**  
This is normal until habits have been created and data has been logged. Save Excel and refresh Power BI after adding data.

**A day is marked Missing but I actually logged it**  
Open Missing Days, use OPEN SELECTED DAY to check the log, correct the day if needed, save, then refresh Power BI.

**I changed a goal and old results changed**  
Use Manage Habits and make the change with the correct **Changes from** date. Do not overwrite history manually.

**Data Check reports a problem**  
Read the Details column first. Fix the configuration issue before relying on the dashboard refresh.

---

## 11. Files to keep together

For a normal handoff, keep these files together:

```text
Habit Tracker V2/
    Habit_Tracker_V2_Starter_Empty.xlsm
    Habit_Tracker_V2_Sample_Data.xlsm
    Habit_Tracker_Dashboard-V2.pbix
    README_Habit_Tracker_V2.md
    Habit_Tracker_V2_Detailed_User_Guide.docx
```

The README is the practical setup and Excel-use guide. The Word document contains the full reference, including Power BI pages, measures and technical notes.
