# Git & GitHub

## 1. Local to Remote

* Git works in three main areas: the `Working Directory`, the `Staging Area`, and the `Remote Repository`.
* Instead of typing the full remote git repo URL every time, we just use shorthand name or an alias usually called as `origin`.

### 1.1. Initializing & Connecting

| Command                            | Description / Purpose                            |
|------------------------------------|--------------------------------------------------|
| `git init`                         | Initialize a new local Git repository            |
| `git remote add origin <repo_url>` | Link local repo to a remote (alias: origin)      |
| `git remote -v`                    | Verify connection to the remote repository       |


### 1.2. Staging & Committing

| Command                                | Description / Purpose                            |
|----------------------------------------|--------------------------------------------------|
| `git add .`                            | Stage all modified and new files                 |
| `git add <file>`                       | Stage a specific file only                       |
| `git commit -m "message"`              | Save staged changes as a labeled snapshot        |
| `git commit -m "feat: initial commit"` | Wrap changes into a labeled snapshot             |
| `git commit --amend`                   | Edit the most recent commit message              |


### 1.3. Branching & Pushing

| Command                           | Description / Purpose                            |
|-----------------------------------|--------------------------------------------------|
| `git branch`                      | List all existing local branches                 |
| `git checkout -b <branch-name>`   | Create a new branch and switch to it             |
| `git branch -m <new-branch-name>` | Rename the current active branch                 |
| `git branch -d <branch-name>`     | Delete a merged branch (use `-D` to force)       |
| `git push origin <branch-name>`   | Upload local branch changes to GitHub repository |

<br>

## 2. Collaboration & Retrieval

When working with an existing project or updating your local copy.

| Command             | Purpose                                                          |
|---------------------|------------------------------------------------------------------|
| `git clone <url>`   | Creates a local copy of a remote repo for the first time         |
| `git fetch`         | Downloads updates from remote but does NOT merge them            |
| `git pull`          | Combines fetch and merge; updates local code with remote changes |

<br>

## 3. Versioning with Tags

* Tags are used to mark specific points in history as important.
* They are used typically for Releases, example: v1.0, v2.0, etc.
* Unlike branches, tags do not change.

### 3.1. Listing & Inspecting Tags

| Command                      | Description / Purpose                                      |
|------------------------------|------------------------------------------------------------|
| `git tag`                    | List all existing tags in the local repository             |
| `git tag -n`                 | List tags with their associated messages (1 line)          |
| `git tag -l "v1.*"`          | List tags matching a specific pattern                      |
| `git show v1.0.0`            | View tag details and the associated commit                 |
| `git ls-remote --tags`       | View tags on the remote server                             |
| `git ls-remote --tags origin`| View tags on the remote server                             |

### 3.2. Creating & Pushing Tags

| Command                          | Description / Purpose                               |
|----------------------------------|-----------------------------------------------------|
| `git tag v1.0.0`                 | Create a Lightweight tag (pointer only)             |
| `git tag -a v1.0.0 -m "msg"`     | Create an Annotated tag (includes author/date)      |
| `git tag v1.0.0 <hash>`          | Backdate a tag to a specific past commit            |
| `git push origin v1.0.0`         | Upload a single specific tag to the remote          |
| `git push origin --tags`         | Upload all local tags to the remote at once         |

### 3.3. Maintenance & Usage

| Command                          | Description / Purpose                               |
|----------------------------------|-----------------------------------------------------|
| `git tag -d v1.0.0`              | Delete a tag from your local repository             |
| `git push --delete origin <tag>` | Delete a tag from the remote (GitHub/GitLab)        |
| `git fetch --tags`               | Sync all remote tags to your local machine          |
| `git checkout v1.0.0`            | View code at that tag (results in "Detached HEAD")  |

<br>

## 4. Stashing

* The Stash is essentially a "pause button" for your work.
* It allows you to wipe your working directory clean without losing your progress, which is perfect for shifting focus to an emergency bug fix.

**Scenario**: You are mid-way through a feature, but a bug needs an urgent fix on the *same branch*. You aren't ready to commit your messy code yet.

| Command                                   | Description / Purpose                                                                        |
|-------------------------------------------|----------------------------------------------------------------------------------------------|
| `git stash`                               | Hides uncommitted changes from a tracked file and reverts to a clean HEAD                    |
| `git stash -u`                            | Hides uncommitted changes from a tracked & new file (untracked) and reverts to a clean HEAD  |
| `git stash save "message"`                | Stashes changes with a custom message for easier identification                              |
| `git stash list`                          | Shows a numbered list of all your hidden stashes                                             |
| `git stash pop`                           | Re-applies the latest stash and REMOVES it from the list                                     |
| `git stash apply`                         | Re-applies the latest stash but KEEPS it in the storage                                      |
| `git stash show -p`                       | Shows the actual code changes inside the latest stash                                        |
| `git stash drop`                          | Manually deletes the most recent stash from the list                                         |
| `git stash clear`                         | Deletes all stashes in your storage (use with caution!)                                      |
| `git stash push -m "msg" path/to/file.cs` | If you only want to stash some files                                                         |
| `git stash apply stash@{2}`               | If you have multiple stashes, you can pick a specific one using its index from the list      |

<br>

## 5. Version Control in Infrastructure

* In professional DevOps workflows (like Terraform or NPM), calling a "branch" directly is dangerous because branches change.
* We use `Tags` for "Immutable Infrastructure."
* By using the `?ref=` syntax, you ensure that your infrastructure only updates when you explicitly change the version tag.
* Example: **Terraform Module Call**
  
  ```terraform
  module "vpc" {
    source = "git::https://github.com/Millstack/terraform-aws-vpc.git?ref=v1.1.3"
    vpc_cidr = "10.0.0.0/16"
    ....
  }
  ```

<br>

## 6. Resolving Merge Conflicts

When Git hits a conflict, it effectively stops time and asks us to be the judge and decide what to do next.

### 6.1. The Conflict Resolution Workflow

| Command                     | Description / Purpose                                        |
|-----------------------------|--------------------------------------------------------------|
| `git status`                | Identifies which specific files have "both modified" status  |
| `git diff`                  | View the exact line-by-line differences causing the conflict |
| `git add <file>`            | Stages the file after you manually removed the `<<<<` markers|
| `git merge --continue`      | Completes the merge process after all conflicts are staged   |
| `git merge --abort`         | The "Undo" button: Cancels the merge and returns to normal   |
| `git commit -m "msg"`       | Finalizes the resolution with a merge commit message         |

### 6.2. Understanding the Markers

When we open a conflicted file, Git inserts these markers to show you exactly where the "collision" happened.

```bash
<<<<<<< HEAD
    // This is YOUR local code
    return "My Version";
=======
    // This is THEIR incoming code
    return "Their Version";
>>>>>>> feature-branch
```
>[!NOTE]
> * To minimize these scenarios, pull frequently.
> * The longer your branch stays diverged from the main code, the higher the chance of a "mega-conflict" later.

<br>

## 7. Undoing Mistakes

Every developer makes mistakes. Here is how to travel back in time safely.

| Scenario                 | Command                           | Description / Result                            |
|--------------------------|-----------------------------------|-------------------------------------------------|
| `Unstage a file`         | `git reset <file>`                | Removes file from Staging; keeps local changes  |
| `Fix last commit`        | `git commit --amend -m "msg"`     | Overwrites the last commit with new data/msg    |
| `Undo pushed code`       | `git revert <hash>`               | Creates a NEW commit that undoes a past one     |
| `Discard local changes`  | `git restore <file>`              | Wipes local changes; reverts to last commit     |
| `Undo local commit`      | `git reset --soft HEAD~1`         | Deletes commit but keeps code in Staging        |
| `Hard Reset (DANGEROUS)` | `git reset --hard HEAD~1`         | Deletes commit AND wipes all code changes       |
| `The Ultimate Undo`      | `git reflog`                      | Shows a log of ALL actions (even deleted ones)  |


>[!NOTE]
>* Use `revert` for shared/pushed branches because it adds to the history without deleting anything, which prevents merge conflicts for your teammates.
>* Use `reset` only for your local, unpushed work where you want to clean up your own "messy" history.
>* If you accidentally run a `git reset --hard` and lose your work, don't panic. Run `git reflog`. It tracks every move the HEAD has made in the last 30-90 days, allowing you to recover commits that appear to be "deleted."
>* Be careful with `git commit --amend` if you have already pushed. If you amend a pushed commit, you will have to git push --force, which can disrupt others working on the same branch.

<br>

### 8. Git Log & Inspection

| Command                          | Description / Purpose                               |
|----------------------------------|-----------------------------------------------------|
| `git log --oneline`              | View a condensed, one-line history of commits       |
| `git log --oneline --graph --all`| View a visual tree of all branches and tags         |
| `git diff`                       | Compare working directory vs. the last commit       |
| `git diff --staged`              | Compare staged changes vs. the last commit          |
| `git show <hash>`                | View the specific code changes in a single commit   |
| `git blame <file>`               | See who changed which line of a file and when       |
| `git reflog`                     | The "Master Undo": Log of every action taken locally|




















 
