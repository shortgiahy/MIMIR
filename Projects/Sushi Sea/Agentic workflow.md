# 📘 YOUR GUIDE: How the Multi-Agent Engine Works

*(Keep this for yourself to understand what's happening at a glance)*

  

## The Big Picture (The Factory Analogy)

Think of your game development setup as a 3-person assembly line:

1. **You:** The Studio Head / Producer. You set the direction and make final approval calls.

2. **ChatGPT:** The Lead Architect & Quality Inspector. It creates blueprints, writes strict rules, and audits work. It does not touch raw game files directly.

3. **Claude Code:** The Factory Floor Mechanic. It lives in your computer terminal, reads blueprints, writes Luau code, edits files, and pushes code to GitHub.

  

---

  

## The 4 Steps of Every Feature

  

### Step 1: The Blueprint (ChatGPT)

You tell ChatGPT: *"We need a sprint stamina system."*

ChatGPT doesn't just write loose code; it creates a **GitHub Issue** containing:

- The exact rules (e.g., "Drains at 10 stamina/sec, regenerates after 3 seconds of rest").

- The exact function names and data types.

- The criteria needed to pass inspection.

  

### Step 2: The Build (Claude Code)

Claude Code looks at GitHub via terminal commands (`gh issue list`), sees the new blueprint, and claims it.

- Claude creates a temporary workspace branch (so it doesn't touch your working game).

- Claude writes the code inside your local project folder.

- Claude runs a local spell-check (`selene`) to catch sloppy mistakes before pushing.

  

### Step 3: The Robot Referee (GitHub Actions CI)

Claude pushes its work to GitHub and opens a **Pull Request (PR)** (a formal request saying: *"I'm done, please merge this into the main game"*).

- The moment that PR opens, GitHub's automated CI wakes up.

- It runs the linter and your automated tests on a server.

- If Claude wrote broken code or syntax errors, the CI fails with a Red X and stops right there. Claude must fix it.

  

### Step 4: The Final Audit (ChatGPT)

If the CI check is Green, ChatGPT inspects the code diff.

- ChatGPT checks: *"Did Claude follow the blueprint? Are there security holes where exploiters could give themselves infinite stamina? Are there memory leaks?"*

- **If flawed:** ChatGPT leaves specific correction notes on the PR. Claude reads them and fixes the code.

- **If approved:** ChatGPT gives the green light, the PR merges into `main`, and your game receives the clean update.