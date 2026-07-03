# Git: Creating Snapshots

## The Git Workflow
The workflow generally flows as follows:
1. **Working Directory (cwd)**: Where you make changes to your files.
2. **Staging Area**: An index file that prepares changes for the next commit. Files remain here after a commit, but they no longer show up as pending changes.
3. **Repository (Local)**: Where Git permanently stores the changes as commits.

## Commits
A commit in Git stores:
* **Commit ID**: A unique SHA-1 hash.
* **Message**: A description of the changes.
* **Metadata**: Date/time, author details, etc.
* **Snapshot**: A complete snapshot of all files at that point in time.

### Best Practices for Commits:
* Commits should not be too big or too small.
* Commit often.
* Make commit messages informative and meaningful.

## Staging and Committing Files
* **Stage a file**:
  ```bash
  git add <filename>
  ```
* **Commit staged changes**:
  ```bash
  git commit -m "Your commit message"
  ```
* **Skipping the Staging Area**: You can use `git commit -am "message"` to stage and commit modified files directly. 
  * *Note: This only works for tracked files. It is generally considered best practice to stage your files explicitly before committing.*

## Managing and Viewing Files
* **Listing Tracked Files**:
  ```bash
  git ls-files
  ```
* **Status Summary**:
  ```bash
  git status -s
  ```
  * Shows a short version of the status. Red indicates unstaged changes, and green indicates staged changes.
* **Removing Files**:
  * If you delete a file manually, it remains in the staging area as a deleted file that needs to be staged.
  * To delete a file from both the working directory and stage the deletion:
    ```bash
    git rm <filename>
    ```
* **Renaming Files**:
  * In Git, renaming a file is treated as deleting the old file and creating a new one.
  * To rename a file and stage the change in one go:
    ```bash
    git mv <old-name> <new-name>
    ```

## Comparing Changes
* **View staged changes**: Compare the staging area with the last commit.
  ```bash
  git diff --staged
  ```
* **Using an External Diff Tool**: You can configure VS Code (or other editors) as your diff tool.

## Browsing Commits & Inspection
* **View Commit History**:
  ```bash
  git log --oneline --reverse
  ```
* **Show Commit Details**:
  * View specific commit: `git show <commit-id>`
  * View latest commit: `git show HEAD`
  * View one commit before HEAD: `git show HEAD~1`
  * View a specific file's content in a past commit: `git show HEAD~1:filename`
* **Inspecting Git Objects**:
  ```bash
  git ls-tree <tree-ish>
  ```
  * In Git's internal object model, a **blob** represents a file (its contents), and a **tree** represents a folder/directory.

## Discarding / Unstaging Changes
* **Restore file to last committed state (discard local changes)**:
  ```bash
  git restore <filename>
  ```
* **Unstage changes (remove from staging area back to working directory)**:
  ```bash
  git restore --staged <filename>
  ```

---

## Summary
This note covers:
* The Git workflow (Working Directory, Staging Area, Local Repository).
* What Git stores in a commit and general commit best practices.
* Commands to stage (`git add`), commit (`git commit`), and skip staging (`git commit -am`).
* Commands to remove (`git rm`) and rename (`git mv`) files.
* How to view short status (`git status -s`) and compare staged changes (`git diff --staged`).
* Inspecting commits and history using `git log`, `git show`, and `git ls-tree`.
* The difference between Git objects: **blobs** (files) and **trees** (folders).
* How to discard working directory changes or unstage files using `git restore`.
