# MIMIR – Claude Session Instructions

## Branch Workflow (Required Every Session)

1. **Never work on `main` directly.** At the start of every session, create or switch to a feature branch:
   ```
   git checkout -b claude/session-<short-description>
   ```
   Use a short, descriptive name that reflects the session's work.

2. **Commit your changes** to that branch with clear, descriptive commit messages as you work.

3. **At session end**, push the branch and stop. Do NOT merge into `main` automatically.

4. **Merging requires explicit user approval.** Before merging, present the user with a summary of all changes made on the branch and ask:
   > "I'm done. Here's what changed on branch `<branch-name>`. Do you approve merging this into `main` and deleting the branch?"
   Only proceed with the merge and branch deletion after the user explicitly says yes.

5. **Merge and cleanup** (only after approval):
   ```
   git checkout main
   git merge --no-ff <branch-name>
   git push origin main
   git branch -d <branch-name>
   git push origin --delete <branch-name>
   ```
