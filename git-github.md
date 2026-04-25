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


















 
