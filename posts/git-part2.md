# Git & GitHub - Part 2

> This part continues the Git & GitHub notes by covering how to save project snapshots, view commit history, compare changes, recover previous versions, and restore files.
>
> These commands are essential for tracking project history, debugging changes, and recovering from mistakes during software development.

These notes cover:

- Git Commit
- Viewing Commit History
- Comparing Changes
- Viewing Previous Versions
- Git Checkout
- Git Restore

---

# Table of Contents

- [13. git commit](#13-git-commit)
- [14. git log](#14-git-log)
- [15. git diff](#15-git-diff)
- [16. git show](#16-git-show)
- [17. git checkout](#17-git-checkout)
- [18. git restore](#18-git-restore)

---

# 13. `git commit`

The `git commit` command saves the staged changes permanently into the **Local Repository**.

A commit represents a snapshot of the project at a particular point in time.

Syntax:

```bash
git commit -m "Commit Message"
```

Example:

```bash
git commit -m "Initial Commit"
```

Example Output:

```text
[master (root-commit) a8b12f4] Initial Commit
1 file changed, 20 insertions(+)
create mode 100644 hello.txt
```

---

## Why Commit is Needed?

Every commit stores:

- Project Snapshot
- Author Name
- Email
- Date & Time
- Commit Message
- Unique Commit ID (SHA)

---

## Git Workflow

```text
Working Directory
        │
   git add
        ▼
 Staging Area
        │
 git commit
        ▼
 Local Repository
```

Once committed, Git permanently stores the project history.

---

## Good Commit Messages

Good Examples:

```text
Added Login Module

Fixed Authentication Bug

Updated README

Improved Error Handling
```

Poor Examples:

```text
Update

Test

abc

Done
```

Commit messages should clearly describe the changes.

---

# 14. `git log`

The `git log` command displays the complete commit history of the repository.

Syntax:

```bash
git log
```

Example:

```bash
git log
```

Example Output:

```text
commit 4ab921c8...

Author: John Doe

Date: Mon Jun 24

Initial Commit
```

---

## Compact Log

```bash
git log --oneline
```

Example:

```text
4ab921c Initial Commit

1fd39ab Added Login

73ce911 Fixed Bug
```

---

## Why `git log` is Useful?

It helps you:

- View commit history
- Identify commit IDs
- Find old versions
- Debug project history

---

# 15. `git diff`

The `git diff` command compares changes between different versions.

Syntax:

```bash
git diff
```

---

## Compare Working Directory with Staging Area

```bash
git diff
```

Shows modifications that have **not yet been staged**.

---

## Compare Staging Area with Local Repository

```bash
git diff --cached
```

Shows staged changes waiting to be committed.

---

## Example

Suppose:

Original File

```text
Hello
```

Modified File

```text
Hello World
```

Running:

```bash
git diff
```

Displays:

```text
-Hello

+Hello World
```

---

# 16. `git show`

The `git show` command displays detailed information about a specific commit.

Syntax:

```bash
git show <commit-id>
```

Example:

```bash
git show 4ab921c
```

Displays:

- Commit Information
- Author
- Date
- Commit Message
- Modified Files
- Actual Changes

---

## Show Previous Version of a File

Syntax:

```bash
git show <commit-id>:<filename>
```

Example:

```bash
git show 4ab921c:hello.txt
```

This displays the contents of **hello.txt** from that commit.

---

# 17. `git checkout`

The `git checkout` command is used to switch between commits or branches.

In this note, it is used to view an older version of the project.

Syntax:

```bash
git checkout <commit-id>
```

Example:

```bash
git checkout 4ab921c
```

Git moves the repository to that commit.

---

## Return to Latest Version

If your default branch is **master**:

```bash
git checkout master
```

If your default branch is **main**:

```bash
git checkout main
```

---

## Workflow

```text
Latest Commit

      │

git checkout

      ▼

Older Commit

      │

git checkout main

      ▼

Latest Commit
```

---

## Detached HEAD

Checking out a commit instead of a branch places Git in a:

```text
Detached HEAD
```

state.

This means you are viewing an older snapshot and are **not currently on a branch**.

---

# 18. `git restore`

The `git restore` command restores files to a previous state.

It is mainly used to discard unwanted modifications.

---

## Restore Modified File

Suppose:

```text
hello.txt
```

was modified accidentally.

Run:

```bash
git restore hello.txt
```

Git restores the file to the latest committed version.

---

## Workflow

```text
Modified File

      │

git restore

      ▼

Previous Version
```

---

## Restore Staged File

Suppose you accidentally staged a file.

Remove it from the staging area.

```bash
git restore --staged hello.txt
```

The file remains in the Working Directory but is removed from the Staging Area.

---

## Restore Working Directory

If the staged version is correct but you want to discard later modifications:

```bash
git restore --worktree hello.txt
```

---

## Restore Both Staging Area and Working Directory

```bash
git restore --staged --worktree hello.txt
```

This restores both areas to the latest committed version.

---

## Recovery Scenarios

### Case 1

Modified but not staged.

```text
Working Directory

Modified

↓

git restore

↓

Original File Restored
```

---

### Case 2

Accidentally staged.

```text
Working Directory

↓

git add

↓

Staging Area

↓

git restore --staged

↓

Back to Working Directory
```

---

### Case 3

Staged, then modified again.

```text
Working Directory

↓

git add

↓

Staging Area

↓

Modified Again

↓

git restore --worktree
```

---

## Difference Between Restore Commands

| Command | Purpose |
|----------|----------|
| `git restore file` | Restore modified file |
| `git restore --staged file` | Remove file from staging area |
| `git restore --worktree file` | Restore working directory |
| `git restore --staged --worktree file` | Restore both staging area and working directory |

---

## Important Note

`git restore` only affects your **local repository**.

It does **not** modify files stored on GitHub.

---

*End of Git & GitHub Notes (Part 2)*