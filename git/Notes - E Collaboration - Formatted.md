# Git: Collaboration

## Remote Repositories
* **`origin`**: The default name Git assigns to the remote repository you cloned from or linked your local repository to. It points to the hosted project on services like GitHub or GitLab.

## Fetching vs. Pulling
### Git Fetch
* Downloads commits, files, and references from a remote repository to your local computer.
* Updates remote-tracking branches (e.g., `origin/main`, `origin/HEAD`) but **does not** modify your working files or merge anything.
* To check if your local branch is behind the remote tracking branch, run:
  ```bash
  git branch -vv
  ```

### Git Pull
* Fetches remote changes and immediately merges them into your active local branch.
* If your local and remote branches have diverged, Git will create a merge commit.
* **Pulling with Rebase**: To keep a clean, linear history by replaying your local commits on top of the fetched remote changes:
  ```bash
  git pull --rebase
  ```

> [!IMPORTANT]
> **Pushing Best Practice**: Do not push blindly. If there are remote updates you don't have yet, Git will reject the push. First run `git pull`, resolve any merge conflicts locally, verify the code, and then push.
> ```bash
> git push origin <branch-name>
> ```

## Working with Remote Branches
* **Pushing a New Local Branch**: A new local branch does not exist on the remote until you push it and establish tracking:
  ```bash
  git push -u origin <branch-name> # -u / --set-upstream
  ```
* **Checking Out a Remote Branch**: After running `git fetch`, a remote branch exists locally as `origin/<branch-name>`. To create a local branch that tracks it:
  ```bash
  git switch <branch-name>
  # (Or explicitly: git switch -c <branch-name> origin/<branch-name>)
  ```

## Managing Tags Remotely
* **Pushing Tags**: Standard `git push` does not transfer tags. You must push tags explicitly:
  ```bash
  git push origin <tag-name>
  # Push all local tags:
  git push origin --tags
  ```
* **Deleting Remote Tags**: Deleting a tag locally does not delete it from GitHub. You must delete it from remote:
  ```bash
  git push origin --delete <tag-name>
  # To delete it locally as well:
  git tag -d <tag-name>
  ```

## Syncing Forked Repositories
When you fork a repository on GitHub, it does not update automatically when the original repository receives updates. To sync changes manually:
1. **Add the original repo as a remote** (conventionally named `upstream`):
   ```bash
   git remote add upstream https://github.com/original-owner/original-repo.git
   ```
2. **Fetch updates from upstream**:
   ```bash
   git fetch upstream
   ```
3. **Merge updates into your local main branch**:
   ```bash
   git checkout main
   git merge upstream/main
   ```
4. **Push the synchronized changes to your GitHub fork (`origin`)**:
   ```bash
   git push origin main
   ```

## GitHub Collaboration Tools
* **Milestones**: Used to group issues and pull requests together toward a specific goal or release. As issues are closed, the milestone automatically calculates and displays a progress percentage.

---

## Summary
This note covers:
* What remote repositories and `origin` represent.
* The differences between `git fetch` (downloading refs) and `git pull` (fetch + merge/rebase).
* Best practices for pushing code to avoid overwriting or reject errors.
* How to publish new local branches and track remote-created branches.
* Pushing and deleting Git tags on remote servers.
* Step-by-step instructions for manually syncing a forked repository using an `upstream` remote.
* Using GitHub Milestones to track progress on issues and pull requests.