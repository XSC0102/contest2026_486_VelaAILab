# VelaStudy Task Manager

Manage persistent research and study tasks for VelaStudy AI.

## When to use

Use when the user asks to:
- add a research or study task
- view pending tasks
- mark a task complete
- arrange tasks from a research or study plan
- set a reminder for a task

## Task File

Use:
`/data/ai_agent/VELASTUDY_TASKS.md`

Task format:
`- [ ] [YYYY-MM-DD] task description`

Completed format:
`- [x] [YYYY-MM-DD] task description`

## How to use

### Add
1. Use `get_current_time`.
2. Read the task file if it exists.
3. Use `write_file` or `edit_file` to add the new task.
4. Confirm the task that was added.

### View
1. Use `read_file`.
2. Show pending tasks first.
3. Keep completed tasks separate.

### Complete
1. Use `read_file` to read the entire task file.
2. Find the matching task in the Pending section.
3. Remove the pending line from the Pending section.
4. Add the same task to the Completed section with `[x]`.
5. Preserve the original task date and description.
6. Use `write_file` to save the complete updated task file.
7. Confirm completion only after the file update succeeds.

### Reminder
If the user explicitly asks for a timed reminder:
1. Use `get_current_time`.
2. Use `cron_add` with the requested time.
3. Confirm the human-readable reminder time.

## Rules

- Never claim a task was saved before the file tool succeeds.
- Do not delete completed tasks unless the user explicitly asks.
- Do not create reminders unless requested.
