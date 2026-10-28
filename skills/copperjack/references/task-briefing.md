---
name: copperjack-task-briefing
description: Use when the user asks what to work on next in CopperJack, requests a task briefing, or asks about tasks due in a date window.
---

# CopperJack task briefing

Give a concise briefing from saved, unfinished tasks. Follow the user's explicit scope and method when they differ from this default.

## Read the tasks

For an ordinary “what's next?”, fetch `list_open_tasks` for the whole connected organization. Previous discussion of one property does not narrow the scope. This matches the top-level Tasks tab: unfinished tasks on nonarchived deals, including closed deals. Only an explicit property request selects `list_deal_tasks`; resolve an ambiguous property before selecting it, and exclude completed tasks.

Read every page until `next_cursor` is null, keeping the other inputs unchanged. Reassemble fragmented groups. Omit `timezone` unless the user explicitly requests a timezone; use the returned viewer timezone and calendar dates, not the property location or agent clock. Keep the first page’s `briefing_as_of` reference for the whole briefing; later `read_at` values only report when each page was fetched. If `timezone_source` is `explicit` or `app-default`, name the effective timezone.

On `invalid_cursor`, discard the partial collection and restart once. After a second invalidation, say “Tasks changed while I was reading them, so I couldn't complete the briefing.” Label any displayed records “Partial results.” Failed or incomplete reads cannot establish an empty window.

## Select and order

Default briefing, in this order:

1. Every overdue task.
2. Every remaining task due today.
3. Every task due tomorrow, including when tomorrow starts a new week.

Exclude undated tasks. Never cap these three groups at five. Only when all three are empty, show the earliest one to five tasks later in the current Monday-through-Sunday calendar week. If none qualify, say so.

Use `viewer_today` and `viewer_tomorrow` for the default groups. An explicit “this week” replaces those buckets with dates from `viewer_week_start` through, but excluding, `viewer_week_end_exclusive`. “Next seven days” uses dates from `viewer_today` through, but excluding, `viewer_next_seven_end_exclusive`. Include unfinished overdue tasks whose due dates fall inside an explicit window.

Use returned urgency for overdue/today; an overdue timed task from earlier today appears only in overdue. Keep literal date-only deadlines unchanged. Use `viewer_due_date` and `viewer_due_time` for timed calendar placement and display; if those are unavailable, disclose the affected timing limitation instead of guessing a timezone conversion. Sort chronologically within each group, timed deadlines before date-only deadlines on the same date, preserving tool order for ties.

## Deliver

Show each selected task once, with its property, due date or time, and a short useful description when available. Label the default groups with relative day and calendar date, such as “Today, September 14”; omit weekday names. Omit empty headings. A count refers to the tasks actually shown, not every open task fetched.

Report saved work only. Do not invent priorities, dependencies, task sequences, suggested new work, or offers to research, dig in, or edit anything. Do not pull unrelated property details or documents to elaborate a task.
