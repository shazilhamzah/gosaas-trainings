# Git: Rewriting History

## Why Rewrite History?
* We clean up commit history to make it readable, logical, and meaningful for other developers.
* A well-crafted history tells a clear, chronological story of how a feature or application was developed.

## The Golden Rule of Rewriting History
> [!CAUTION]
> **Never rewrite public history.** Do not rebase, reset, or amend commits that have already been pushed to a shared remote repository. Other developers who pull those commits will face conflicts, leading to messy merge commits and potential data loss.
* **Rule**: Write history locally, publish globally. Clean up commits locally *before* you push them.

## Undoing Commits: Reset vs. Revert
* **Local Commits (Reset)**: Moves the branch/HEAD pointer backward.
  ```bash
  git reset --hard HEAD~1
  ```
* **Pushed Commits (Revert)**: Safe for shared history. Creates a new commit that records the exact inverse of the targeted commit.
* **Reverting Multiple Commits**: Revert a range of commits in one go without immediately creating a commit for each:
  ```bash
  git revert --no-commit HEAD~3..
  # Resolve any conflicts, then:
  git revert --continue
  ```

## Recovering "Lost" Commits
* When you perform a reset, newer commits disappear from the standard `git log`.
* Git stores a history of all HEAD pointer movements in the **reflog**. You can use it to find the SHA-1 of "lost" commits and restore them:
  ```bash
  git reflog
  ```

## Modifying the Latest Commit
* **Amend the latest commit**: Add/modify files or change the commit message of your last commit:
  ```bash
  git commit --amend
  # (Backend note: Git discards the old commit and creates a brand new commit object)
  ```
* **Remove a file from the latest commit (Locally)**:
  1. Undo the commit but keep changes in the working directory:
     ```bash
     git reset --mixed HEAD~1
     ```
  2. Discard or delete the specific file:
     ```bash
     git restore <filename>
     # If the file is untracked: git clean -f <filename>
     ```
  3. Re-stage and commit the remaining changes.

## Interactive Rebasing (`git rebase -i`)
Interactive rebasing allows you to rewrite, reorder, squash, and delete commits. Because Git commits are linked sequentially, changing an older commit rewrites all subsequent commit hashes.
* **Start interactive rebase**:
  ```bash
  git rebase -i <parent-commit-id> # (e.g., git rebase -i HEAD~5)
  ```

### Interactive Rebase Commands:
* **`pick`**: Keep the commit as is.
* **`reword`**: Keep the commit contents, but edit the commit message.
* **`edit`**: Pause rebase to amend the commit contents.
  * **To Split a Commit**: Set its action to `edit`. When Git pauses:
    ```bash
    git reset --mixed HEAD^
    # Commit the first set of changes:
    git add <file-set-1>
    git commit -m "Part 1 message"
    # Commit the second set of changes:
    git add <file-set-2>
    git commit -m "Part 2 message"
    # Continue the rebase:
    git rebase --continue
    ```
* **`squash`**: Combine the commit with the one above it.
  * *Note: To squash non-adjacent commits, reorder the lines in the interactive editor first so they are next to each other.*
* **`drop`**: Delete the commit from history.

### Managing Rebase Conflicts
* If a conflict occurs, resolve the conflicts in the files (or use `git mergetool`) and continue:
  ```bash
  git rebase --continue
  ```
* To cancel the rebase and restore the state before you started:
  ```bash
  git rebase --abort
  ```

---

## Visual Git Clients
* Graphical tools like **GitKraken**, **Fork**, or IDE extensions (like **GitLens** in VS Code) provide visual interactive graph interfaces that make history rewriting (dragging, dropping, squashing) much easier.

---

## Summary
This note covers:
* The purpose of rewriting history and the **Golden Rule** (never rewrite public history).
* How to use `git reset` (local history) vs. `git revert` (pushed history) to undo changes.
* Utilizing `git reflog` to recover commits that are no longer shown in the standard log.
* Modifying the latest commit using `git commit --amend` or removing a file from it.
* Interactive rebasing (`git rebase -i`) commands including `pick`, `reword`, `edit` (including splitting commits), `squash`, and `drop`.
* Handling interactive rebase conflicts and using visual Git GUI clients like GitKraken.
