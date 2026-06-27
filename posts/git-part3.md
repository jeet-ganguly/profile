# Git & GitHub (Part 3)

> This part continues the Git & GitHub notes by covering how to undo commits, work with remote repositories, synchronize projects with GitHub, and create version tags.
>
> These commands are essential for collaboration, backup, project recovery, and managing software versions.

These notes cover:

* `git reset`
* Remote Repository
* `git remote`
* `git clone`
* `git pull`
* `git push`
* `git tag`
* Cybersecurity Perspective
* Quick Revision Sheet

---

# Table of Contents

* [19. git reset](#19-git-reset)
* [20. Remote Repository](#20-remote-repository)
* [21. git remote](#21-git-remote)
* [22. git clone](#22-git-clone)
* [23. git pull](#23-git-pull)
* [24. git push](#24-git-push)
* [25. git tag](#25-git-tag)
* [26. Cybersecurity Perspective](#26-cybersecurity-perspective)
* [27. Quick Revision Sheet](#27-quick-revision-sheet)

---

# 19. `git reset`

The `git reset` command is used to move the current branch to a previous commit.

It can also remove commits or undo staged changes.

---

## `git reset --soft`

Moves the HEAD to a previous commit **without deleting your changes**.

Syntax:

```bash
git reset --soft HEAD~1
```

Example:

```bash
git reset --soft HEAD~1
```

Result:

```text
Last Commit Removed

↓

Changes Remain in Staging Area
```

Use this when you want to modify the last commit and commit again.

---

## `git reset --hard`

Moves the HEAD to a previous commit and **permanently deletes all changes**.

Syntax:

```bash
git reset --hard HEAD~1
```

Example:

```bash
git reset --hard HEAD~1
```

Result:

```text
Last Commit Removed

↓

All Changes Deleted
```

⚠️ Use this command carefully because deleted changes cannot be recovered easily.

---

## Difference Between Soft and Hard Reset

| Command            | Commit Removed | Changes Preserved |
| ------------------ | -------------- | ----------------- |
| `git reset --soft` | Yes            | Yes               |
| `git reset --hard` | Yes            | No                |

---

# 20. Remote Repository

A **Remote Repository** is a copy of your Git repository stored on another system or cloud platform.

Common remote hosting platforms include:

* GitHub
* GitLab
* Bitbucket

---

## Local vs Remote Repository

```text
Developer PC
(Local Repository)

       │
   git push
       ▼

GitHub
(Remote Repository)

       ▲
   git pull
       │

Developer PC
```

The remote repository allows multiple developers to collaborate on the same project.

---

# 21. `git remote`

The `git remote` command is used to connect a local repository with a remote repository.

---

## Add Remote Repository

Syntax:

```bash
git remote add origin <repository-url>
```

Example:

```bash
git remote add origin https://github.com/user/project.git
```

---

## View Remote Repository

```bash
git remote -v
```

Example Output:

```text
origin  https://github.com/user/project.git (fetch)

origin  https://github.com/user/project.git (push)
```

---

## Display Detailed Information

```bash
git remote show origin
```

Shows:

* Fetch URL
* Push URL
* Tracking Branch
* Remote Branches

---

# 22. `git clone`

The `git clone` command downloads an existing repository from GitHub to your local machine.

Syntax:

```bash
git clone <repository-url>
```

Example:

```bash
git clone https://github.com/user/project.git
```

---

## Workflow

```text
GitHub Repository

       │

 git clone

       ▼

Local Repository
```

---

## Why Clone?

* Download existing projects
* Contribute to open-source software
* Backup repositories
* Continue work on another computer

---

# 23. `git pull`

The `git pull` command downloads the latest changes from the remote repository and updates the local repository.

Syntax:

```bash
git pull
```

Example:

```bash
git pull origin main
```

---

## Workflow

```text
GitHub

      │

 git pull

      ▼

Local Repository Updated
```

---

## Why Pull?

Use `git pull` before starting work to ensure you have the latest version of the project.

---

# 24. `git push`

The `git push` command uploads local commits to the remote repository.

Syntax:

```bash
git push
```

Example (first push):

```bash
git push -u origin main
```

Later pushes:

```bash
git push
```

---

## Workflow

```text
Local Repository

      │

 git push

      ▼

GitHub Repository
```

---

## Why Push?

* Backup your work
* Share changes with team members
* Update the remote repository

---

# 25. `git tag`

Git tags are used to mark important commits such as software releases.

Examples:

```text
v1.0

v2.0

v3.0
```

---

## Create a Tag

```bash
git tag v1.0
```

---

## View Tags

```bash
git tag
```

---

## Show Tag Details

```bash
git show v1.0
```

---

## Delete a Tag

```bash
git tag -d v1.0
```

---

## Push Tag to GitHub

```bash
git push origin v1.0
```

---

## Why Tags are Useful?

Tags are commonly used for:

* Software Releases
* Stable Versions
* Production Builds

---

# 26. Cybersecurity Perspective

Git repositories often contain sensitive project information. Improper usage can lead to accidental data exposure.

---

## Never Commit Sensitive Files

Do **not** commit:

* Passwords
* API Keys
* Access Tokens
* SSH Private Keys
* Cloud Credentials
* Database Passwords

These should be excluded using `.gitignore`.

---

## Review Changes Before Committing

Always review modified files before running:

```bash
git add
```

or

```bash
git commit
```

This helps prevent accidental leakage of sensitive information.

---

## Verify Remote Repository

Before pushing code, verify the remote repository.

```bash
git remote -v
```

This helps ensure that code is uploaded to the correct repository.

---

## Review Differences

Before every commit, check:

```bash
git diff
```

to verify that only intended changes are included.

---

## Protect Commit History

Avoid using:

```bash
git reset --hard
```

unless you are certain, as it permanently deletes local changes.

---

## Common Security Mistakes

* Uploading `.env` files
* Committing API keys
* Exposing SSH private keys
* Pushing confidential documents
* Using public repositories for private projects

---

# 27. Quick Revision Sheet

Repository Initialization:

```bash
git init
```

---

Check Repository Status:

```bash
git status
```

---

Stage Files:

```bash
git add .
```

---

Create Commit:

```bash
git commit -m "message"
```

---

View Commit History:

```bash
git log

git log --oneline
```

---

Compare Changes:

```bash
git diff
```

---

View Previous Commit:

```bash
git show <commit-id>
```

---

Restore Files:

```bash
git restore file
```

---

Undo Commit (Keep Changes):

```bash
git reset --soft HEAD~1
```

---

Undo Commit (Delete Changes):

```bash
git reset --hard HEAD~1
```

---

Connect Remote Repository:

```bash
git remote add origin <repository-url>
```

---

Download Repository:

```bash
git clone <repository-url>
```

---

Download Latest Changes:

```bash
git pull
```

---

Upload Changes:

```bash
git push
```

---

Create Tag:

```bash
git tag v1.0
```

---

Biggest Concept:

```text
Git manages project history locally.

GitHub stores Git repositories remotely,
allowing backup and collaboration between developers.
```

---

*End of Git & GitHub Notes (Part 3)*
