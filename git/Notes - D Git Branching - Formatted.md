# Git: Branching

## What is a Branch?
* A **branch** represents an independent, isolated line of development or workspace.
* It allows you to work on features or bug fixes without affecting the main codebase (`main`/`master`).

## Managing Branches
* **Create a branch**:
  ```bash
  git branch <branch-name>
  ```
* **Switch to a branch**:
  ```bash
  git switch <branch-name>
  # (Older command: git checkout <branch-name>)
  ```
* **Rename a branch**:
  ```bash
  git branch -m <old-name> <new-name>
  ```
* **Delete a branch**:
  * Safe delete (checks if merged): `git branch -d <branch-name>`
  * Force delete (discards unmerged changes): `git branch -D <branch-name>`

## Comparing Branches
* **Compare two branches**: View differences between the tips of two branches.
  ```bash
  git diff <branch1>..<branch2>
  ```

## Stashing Uncommitted Changes
Git prevents switching branches if you have uncommitted changes that would be overwritten. To resolve this without committing half-written code, use **Git Stash**:
* **Save current modifications to stash**:
  ```bash
  git stash push -m "your description"
  # (Saves changes and cleans working directory; doesn't appear in commit history)
  ```
* **List all saved stashes**:
  ```bash
  git stash list
  ```
* **Show stash details**:
  ```bash
  git stash show stash@{<index>}
  ```
* **Apply stashed changes**:
  * Apply and keep in stash: `git stash apply stash@{<index>}`
  * Apply and delete from stash: `git stash pop`
* **Discard a stash**:
  ```bash
  git stash drop stash@{<index>}
  ```

## Merging Branches
### 1. Fast-Forward (FF) Merge
* Occurs when there is a direct linear path from the target branch to the source branch.
* Git simply moves the branch pointer forward to the latest commit.
  ```bash
  git merge <branch-name>
  ```

### 2. 3-Way Merge (Diverged Branches)
* Occurs when changes are made on both branches.
* Git finds the common ancestor, compares the changes, and creates a new "merge commit".

### 3. Non-Fast-Forward Merge
* Forces the creation of a merge commit even if a fast-forward merge is possible. This keeps the branch's historical boundary visible in the history graph.
  ```bash
  git merge --no-ff <branch-name>
  ```

### Handling Merge Conflicts
* If changes clash, Git pauses the merge. To abort the merge and restore the pre-merge state:
  ```bash
  git merge --abort
  ```

## Undoing Changes: Reset vs. Revert
* **Git Reset**: Moves the `HEAD` pointer backward to a specific commit.
  * **`--soft`**: Moves HEAD. Leaves staging area and working directory unchanged.
  * **`--mixed`** (default): Moves HEAD and updates staging area, but leaves working directory unchanged.
  * **`--hard`**: Moves HEAD, updates staging area, and overwrites working directory. **Warning: Uncommitted changes will be permanently lost!**
  > [!IMPORTANT]
  > Use `git reset` only for local-only commits. Never reset history that has been pushed to a shared remote repository.
* **Git Revert**: Safe for shared history. It creates a new commit that applies the exact inverse changes of a targeted commit.
  ```bash
  git revert <commit-id>
  ```

## Advanced Branching Tools
* **Squash Merging**: Combines all commits from a branch into a single commit on the target branch during a merge, keeping the history graph clean.
  ```bash
  git merge --squash <branch-name>
  ```
* **Rebasing**: Rewrites history by moving or "replaying" your branch's commits on top of another base commit (e.g., the latest commit of `main`). This results in a completely linear commit history.
  ```bash
  git rebase <base-branch>
  ```
* **Cherry-Picking**: Selects and applies a single specific commit from another branch without merging the entire branch.
  ```bash
  git cherry-pick <commit-id>
  ```
* **Restore File from Another Branch**: Bring a file from a different branch directly into your current working directory:
  ```bash
  git restore --source=<branch-name> -- <filename>
  ```

---

## Summary
This note covers:
* Defining branches and managing them (creating, switching, renaming, deleting).
* Comparing branch differences using `git diff`.
* Saving, listing, applying, and deleting temporary work using `git stash`.
* Merging strategies: Fast-Forward, 3-Way, Non-Fast-Forward (`--no-ff`), and handling conflicts.
* The differences between `git reset` (soft, mixed, hard) and `git revert` for local vs. shared history.
* Advanced features: Squash Merging, Rebasing, Cherry-Picking, and restoring files across branches.