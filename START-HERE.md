# START HERE (instructions for the agent)

You're reading this because the person pasted the kit's one prompt: *"Clone <repo> to ~/agent-starter-kit, read START-HERE.md, and follow it with me."* You are their agent, in Claude Code or Codex, on their Mac. **If they asked for a guided upgrade of a setup they already have, skip to "Already have an agent?" below.**

1. **Confirm the clone.** This file should be at `~/agent-starter-kit/START-HERE.md`. If you cloned it somewhere else, move the clone there (or ask) before continuing. Every path in the kit assumes it. If the clone failed because the repo is private: install `gh` without admin rights (INSTALL.md §2, Session 1 step 3), run `gh auth login --web`, and tell the person in one plain sentence what to click and what code to type. Then clone again.
2. **Don't install anything and don't read any of their mail yet.** Everything from here is driven by the onboarding skill, and it reads nothing until the two-minute privacy gate is answered.
3. **Run the onboarding:** read `~/agent-starter-kit/skills/onboard/SKILL.md` in full and follow it. The shape is: gate → discover sources → seed the last 30 days → orientation brief (within the hour) → pause for corrections → generate and place instructions → the multi-day grind → proposals one at a time. If the person's prompt mentioned "rehearsal" or "dry run," use rehearsal mode (sandboxed; the seed is read-only or clearly-labeled fake data).
4. **Tell the person, once,** how to come back later: "Next time, open Terminal and type `cd ~/agent-starter-kit` and then `claude` (or `codex`), and say `/onboard resume`." (The kit ships `.claude/skills/onboard` and `.agents/skills/onboard` so the command works in both.) After the pause, `kit status` and `kit grind status` work from anywhere.

## Already have an agent?

If the person's prompt said to read `skills/guided-upgrade/SKILL.md` or to run a **guided upgrade** of their existing setup, or if, before the gate, they tell you they already run their own agent setup and only want to borrow ideas: **don't run the onboarding.** Confirm the clone (step 1), then read `~/agent-starter-kit/skills/guided-upgrade/SKILL.md` in full and follow it instead. It's read-only until they approve each change, never touches secrets, mail or messages, and never overwrites their files. Their entry prompt for that path is: *"Clone https://github.com/labenz/agent-starter-kit to ~/agent-starter-kit, read skills/guided-upgrade/SKILL.md, and run a guided upgrade of my existing setup with me."* Later sessions: `/guided-upgrade resume` from `~/agent-starter-kit`.

Human-readable docs: `WELCOME.md` (why, from Nathan), `INSTALL.md` (exact requirements, the Takeout path, grind expectations, and every install step).
