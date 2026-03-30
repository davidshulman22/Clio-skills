---
name: clio-time-entry
description: "Creates a billable time entry in Clio. Trigger on ANY of these: /time, 'log time', 'bill time', 'add a time entry', 'log X hours', 'I spent X hours on', 'bill X hours to', 'enter time for', 'record time on', or any plain-language description of time spent on a matter that should be billed. The user does not need to use a slash command — plain English like 'log half an hour on Smith for reviewing the trust draft' is enough to trigger this skill. Always billable. Always today's date unless stated otherwise. Uses clio_find_matter to resolve matter by partial name before posting."
---

# Clio Time Entry Skill

Creates a billable time entry in Clio. Always billable. Always today's date unless explicitly overridden.

## Input Format
```
/time [matter search terms] [hours] [description]
```

Examples:
- `/time smith 0.5 reviewed draft trust`
- `/time jones 1.2 telephone conference re estate plan`
- `/time johnson 0.3 reviewed paralegal correspondence`

Optional date override (ISO format, prepended after matter search):
- `/time smith 2026-03-27 0.5 reviewed draft trust`

## Workflow

### Step 1 — Search for the matter

Call `clio_find_matter` with the matter search terms. Request: matter_id, display_number, description, client_name, status.

### Step 2 — Confirm matter selection

**If one result returned:** Show it and ask for confirmation before posting.
> Found: **01234-Smith — Estate of Jane Smith** (John Smith, Open). Post 0.5 hrs — "reviewed draft trust"? [yes/no]

**If multiple results returned:** Show a numbered list; ask the user to pick by number.
> Multiple matters found:
> 1. 01234-Smith — Estate of Jane Smith (John Smith, Open)
> 2. 01235-Smith — Smith Family Trust (John Smith, Open)
> Which matter? (1/2)

**If no results:** Report no match found. Ask the user to try a different search term.

**Never auto-select.** Always require explicit confirmation or selection before posting.

### Step 3 — Post the time entry

Once confirmed, call `clio_create_time_entry_activity` with:
- `matter`: the matter ID (integer) from Step 1 — always use the numeric ID, never the display name
- `quantity`: convert hours to whole minutes before passing. Examples: 0.1 = "6", 0.2 = "12", 0.3 = "18", 0.5 = "30", 1.0 = "60", 1.5 = "90", 2.0 = "120". Always pass as a whole number string.
- `note`: description text
- `date`: today's date in YYYY-MM-DD format, or override date if provided
- `non_billable`: "false"
- `price`: DO NOT PASS — omit entirely so Clio uses the matter's default rate
- `output_hint`: "time entry id, date, quantity, note"

### Step 4 — Confirm success

Report back concisely:
> Logged: 0.3 hrs on Estate of Jane Smith — "reviewed draft trust" (2026-03-28)

If the API call fails, report the error and stop.

## Rules

- Always billable — set non_billable to "false"
- Always default to today's date
- Never post without explicit user confirmation of the matter
- If the search returns ambiguous results, always show the list — never guess
- Always convert hours to minutes (multiply by 60, round to nearest whole number)
- Never pass a price/rate — let Clio use the matter default
- Always pass matter as numeric ID, not display name
