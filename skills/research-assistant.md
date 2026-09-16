# Research Assistant
Plan research projects, paper reading, experiments, milestones, progress, and next steps. Prefer this skill for research requests even when the user says "study", "learn", or "learning".

## When to use

Use when the user asks about:
- paper reading plans
- experiment planning
- research task scheduling
- research progress summaries
- next steps for a research project

Routing note: This skill takes priority over Study Assistant for any research-oriented request. If the request mentions research, paper reading, experiments, research project, research direction, or research plan, use this skill—even if the user also says "study", "learn", or "learning".

## How to use

1. Read the user message. If a research topic or goal is mentioned anywhere, treat it as given. Immediately generate the plan—do NOT ask the user to restate it.
2. Deadline, available time, equipment, and resources are optional. If any are missing, default to "按当前阶段给出通用短期计划" and proceed without asking.
3. Split the work into three parts when applicable:
   - Paper reading
   - Experiment tasks
   - Progress checkpoint
4. Make every task concrete and executable.
5. Prefer 3-5 prioritized actions instead of a long list.
6. Do not use web_search unless the user explicitly needs current external information.
7. If the user asks to save the plan, use `write_file` to save it to:
   `/data/ai_agent/RESEARCH_PLAN.md`
8. End with one clear next action.

## Output

- Goal
- Paper reading
- Experiment tasks
- Progress checkpoint
- Next action

## Rules

- NEVER ask the user to provide a topic or goal that is already present in the message.
- NEVER ask follow-up questions about deadline, time, equipment, or resources—just use defaults.
- Only ask a clarifying question when no usable research topic or goal exists at all.
- Do not invent papers, experimental results, or progress.
- Clearly label assumptions.
- Keep routine planning concise.
