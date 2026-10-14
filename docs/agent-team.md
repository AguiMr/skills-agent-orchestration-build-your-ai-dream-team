# Agent team

For Mona's Project Pulse dashboard, I will use a custom multi-agent team defined under `.github/agents/` and orchestrated through GitHub Copilot CLI in a Codespace.

- Planner — Model: Claude Opus 4.7 (copilot). Responsibility: research the repo, inspect dependencies and edge cases, and produce a practical implementation plan with file ownership, validation steps, and sequencing. Definition: `.github/agents/planner.agent.md`.
- Orchestrator — Model: Claude Opus 4.7 (copilot). Responsibility: break the work into phases, delegate tasks to specialist agents, keep file scopes clear, coordinate parallel work where safe, and verify that the final result integrates cleanly. Definition: `.github/agents/orchestrator.agent.md`.
- Coder — Model: GPT-5.5 (copilot). Responsibility: implement the actual code, fix bugs, and add app support files when needed, such as the Project Pulse launch setup for a runnable dashboard. Definition: `.github/agents/coder.agent.md`.
- Designer — Model: Gemini 3.1 Pro (copilot). Responsibility: design the dashboard experience, including UI/UX, accessibility, interaction flow, responsive layout, and the polished visual treatment expected for Project Pulse. Definition: `.github/agents/designer.agent.md`.

Using GitHub Copilot CLI in a Codespace, the Orchestrator will manage handoffs between the Planner, Coder, and Designer so the dashboard work stays structured, focused, and easy to validate.
