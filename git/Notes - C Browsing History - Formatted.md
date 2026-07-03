# Git: Browsing History

## Detailed Log Views
* **View files changed in commits**: Show stats of files modified in each commit.
  ```bash
  git log --oneline --stat
  ```
* **View actual diffs (patches) in commits**: Show detailed code changes.
  ```bash
  git log --oneline --patch # (or git log -p)
  ```
* **View history of a specific file**:
  ```bash
  git log <filename>
  ```
* **View history of a deleted file**: When a file has been deleted, you must use `--` to separate the path from the commit reference:
  ```bash
  git log -- <filename>
  ```

## Filtering and Customizing Logs
* **Applying Filters**: You can filter logs by author, date, message query, etc. (e.g., `git log --author="Name"` or `git log --grep="bug"`).
* **Custom Format**: Format the output of `git log` to display exactly the information you want:
  ```bash
  git log --pretty=format:"%h - %an, %ar : %s"
  ```
* **Git Shortlog**: Group commits by author to see who contributed what:
  ```bash
  git shortlog
  ```

## Git Aliases
To speed up your workflow, you can create custom shortcuts (aliases) for common Git commands:
```bash
git config --global alias.<alias-name> "<original-command>"
# Example:
git config --global alias.co checkout
git config --global alias.lg "log --oneline --graph --decorate"
```

## HEAD and Branch States
* **HEAD**: A reference pointing to the current checkout commit or branch.
* **master/main**: The default branch name in Git.
* **Detached HEAD State**: 
  * Occurs when you check out a specific commit instead of a branch (e.g., `git checkout <commit-id>`).
  * **Warning**: Avoid making commits while in a detached HEAD state. If you commit changes here, they are not attached to any branch. Once you switch back to another branch, those commits will become unreachable and may be garbage collected.

## Advanced History Tools
* **Git Bisect**: Uses binary search to find the exact commit that introduced a bug:
  ```bash
  git bisect start
  git bisect bad             # Mark current commit as bad
  git bisect good <commit>   # Mark an older known-good commit
  # Git will checkout commits for you to test until the culprit is found.
  git bisect reset           # End bisect session
  ```
* **Git Blame**: Shows line-by-line who modified a file and in which commit:
  ```bash
  git blame <filename>
  ```
* **Tags**: Add meaningful tags (like version numbers) to specific commits:
  ```bash
  git tag v1.0.0 <commit-id>
  # You can also use GUI extensions like GitLens in VS Code to manage tags easily.
  ```

---

## Summary
This note covers:
* How to view commit history with file modifications (`--stat`) and patches (`--patch`).
* Filtering, grouping (`git shortlog`), and customizing `git log` outputs.
* Creating Git command aliases to streamline development.
* Explanations of `HEAD`, default branches, and the risks of a **detached HEAD** state.
* Using `git log` for deleted files and tracing authors with `git blame`.
* Finding buggy commits using `git bisect` and managing release tags.