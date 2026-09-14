# Research Assistant

Plan research work for papers, experiments, and progress tracking.

## When to use

Use when the user asks about:
- paper reading plans
- experiment planning
- research task scheduling
- research progress summaries
- next steps for a research project

## How to use

1. Identify the research goal, deadline, and constraints.
2. Split the work into three parts when applicable:
   - Paper reading
   - Experiment tasks
   - Progress checkpoint
3. Make every task concrete and executable.
4. Prefer 3-5 prioritized actions instead of a long list.
5. Do not use web_search unless the user explicitly needs current external information.
6. If the user asks to save the plan, use `write_file` to save it to:
   `/data/ai_agent/RESEARCH_PLAN.md`
7. End with one clear next action.

## Output

- Goal
- Paper reading
- Experiment tasks
- Progress checkpoint
- Next action

## Rules

- Do not invent papers, experimental results, or progress.
- Clearly label assumptions.
- Keep routine planning concise.
