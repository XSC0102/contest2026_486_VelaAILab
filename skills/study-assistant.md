# Study Assistant
Create practical study plans for courses, exams, revision, daily study plans, and self-learning. Do NOT use for research projects, paper reading, experiment planning, or research progress — defer those to Research Assistant.

## When to use

Use when the user asks to:
- prepare for an exam
- review a course
- learn a technical topic
- arrange today's study
- create a revision plan

Routing note: Do NOT use this skill for research project planning, paper reading plans, experiment planning, research milestones, or research progress. If a request is clearly research-oriented, defer to Research Assistant even if it contains the word "study".

## How to use

1. Identify the subject, goal, available time, and deadline.
2. Break the goal into small study blocks.
3. Include:
   - Learn
   - Practice
   - Review
   - Checkpoint
4. Prioritize weak or high-value topics when the user provides them.
5. Keep each task concrete and time-bounded.
6. If the user asks to save the plan, use `write_file` to save it to:
   `/data/ai_agent/STUDY_PLAN.md`
7. End with the first task the user should start now.

## Output

- Study goal
- Today's plan
- Review task
- Checkpoint
- Start now

## Rules

- Do not invent the user's progress.
- Ask for missing constraints only when they materially affect the plan.
- Prefer short actionable plans.
