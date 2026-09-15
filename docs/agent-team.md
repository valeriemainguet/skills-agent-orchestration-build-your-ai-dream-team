# Project Pulse agent team

I will use a four-agent team from `.github/agents/` to coordinate the build of Mona's Project Pulse dashboard.

## 1. Orchestrator

- Source file: `.github/agents/orchestrator.agent.md`
- Model: Claude Opus 4.7 (copilot)
- Responsibility: break the request into phases, assign work to the right specialist, keep file ownership clear, and review whether the final result fits together.
- Role in Project Pulse: this agent coordinates the dashboard work from start to finish and decides when to delegate to the Planner, Designer, and Coder.

## 2. Planner

- Source file: `.github/agents/planner.agent.md`
- Model: Claude Opus 4.7 (copilot)
- Responsibility: inspect the repo, identify dependencies and risks, and create an implementation plan with ordered steps, file assignments, parallel work, and validation guidance.
- Role in Project Pulse: this agent researches the dashboard brief and defines the execution plan before the implementation work starts.

## 3. Designer

- Source file: `.github/agents/designer.agent.md`
- Model: Gemini 3.1 Pro (copilot)
- Responsibility: shape the user experience, layout, information hierarchy, accessibility, and visual polish for the dashboard.
- Role in Project Pulse: this agent focuses on the dashboard look and feel, including clear project cards, readable spacing, status badges, priority treatment, and a polished visual style.

## 4. Coder

- Source file: `.github/agents/coder.agent.md`
- Model: GPT-5.5 (copilot)
- Responsibility: implement the assigned code and support files in a clear, deterministic, testable way, staying within the Orchestrator's scope.
- Role in Project Pulse: this agent builds the static app files and can create `.vscode/launch.json` so the dashboard can run with a launch configuration named `Run Project Pulse Dashboard`.

## How the team works together

1. The Orchestrator receives the Project Pulse request and delegates tasks.
2. The Planner researches the repository and creates the implementation plan, including phases and file ownership.
3. The Designer focuses on the dashboard experience and visual structure.
4. The Coder implements the static dashboard files, data, and launch support.
5. The Orchestrator reviews the integrated result to confirm the app is coherent, runnable, and ready for handoff.

This keeps the work organized, prevents overlapping edits, and matches the practice of using GitHub Copilot CLI orchestration instead of one undifferentiated prompt.
