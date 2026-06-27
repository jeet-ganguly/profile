# Git & GitHub - Part 1 

> Git is a **Distributed Version Control System (DVCS)** used to track changes in files and source code.
>
> It helps developers manage different versions of a project, collaborate efficiently, and recover previous versions whenever required.
>
> GitHub is a cloud-based platform that hosts Git repositories, allowing developers to share and collaborate on projects.
>
> *This topic covered into 3 different parts*


These notes cover:

- What is Git?
- Version Control System (VCS)
- Git vs GitHub
- Installing Git
- Configuring Git
- Creating a Local Repository
- Git Workflow
- Working Directory, Staging Area & Local Repository
- `git status`
- `git add`

---

# Table of Contents

- [1. What is Git?](#1-what-is-git)
- [2. Version Control System (VCS)](#2-version-control-system-vcs)
- [3. Git vs GitHub](#3-git-vs-github)
- [4. Installing Git](#4-installing-git)
- [5. Configuring Git](#5-configuring-git)
- [6. Creating a Local Repository](#6-creating-a-local-repository)
- [7. Git Workflow](#7-git-workflow)
- [8. Working Directory](#8-working-directory)
- [9. Staging Area](#9-staging-area)
- [10. Local Repository](#10-local-repository)
- [11. git status](#11-git-status)
- [12. git add](#12-git-add)

---

# 1. What is Git?

Git is a **Distributed Version Control System (DVCS)**.

It keeps track of every modification made to files and source code during software development.

Instead of storing only the latest version, Git stores the complete history of a project.

---

## Why Git is Needed?

Without Git, developers usually create multiple copies of the same project.

Example:

```text
project.c

project_new.c

project_final.c

project_final_latest.c

project_final_latest2.c
```

Managing these files becomes difficult.

With Git:

```text
Version 1
     │
     ▼
Version 2
     │
     ▼
Version 3
     │
     ▼
Current Version
```

Every version is stored automatically.

---

## Advantages

- Keeps complete project history
- Easily restore old versions
- Supports team collaboration
- Tracks every modification
- Enables parallel development
- Easy backup of source code

---

# 2. Version Control System (VCS)

A **Version Control System (VCS)** is a software tool that records changes made to files over time.

Whenever a file is modified, Git stores:

- What changed
- Who changed it
- When it was changed

This allows developers to:

- Restore previous versions
- Compare versions
- Undo mistakes
- Track project history

---

## Example

Suppose a project contains:

```text
login.py
```

Developer modifies it three times.

Git stores:

```text
Version 1

↓

Version 2

↓

Version 3
```

Any version can be recovered later.

---

# 3. Git vs GitHub

Many beginners think Git and GitHub are the same, but they are different.

| Git | GitHub |
|------|---------|
| Version Control Software | Cloud Hosting Platform |
| Installed on Local Machine | Accessible through Web |
| Tracks Project History | Stores Git Repositories |
| Can Work Offline | Internet Required for Remote Operations |

---

## Relationship

```text
Developer

     │

Uses Git

     │

Local Repository

     │

git push

     ▼

GitHub

(Remote Repository)
```

---

# 4. Installing Git

Before using Git, it must be installed.

## Check Git Version

```bash
git --version
```

Example:

```text
git version 2.43.0
```

---

## Install on Ubuntu

```bash
sudo apt update

sudo apt install git
```

---

## Verify Installation

```bash
git --version
```

---

# 5. Configuring Git

After installation, configure your username and email.

Git stores these details with every commit.

---

## Configure Username

```bash
git config --global user.name "John Doe"
```

Example:

```bash
git config --global user.name "Jeet Ganguly"
```

---

## Configure Email

```bash
git config --global user.email "john@example.com"
```

Example:

```bash
git config --global user.email "jeet@example.com"
```

---

## View Configuration

```bash
git config --list
```

Example Output:

```text
user.name=Jeet Ganguly

user.email=jeet@example.com
```

---

## Why Configuration is Required?

Every commit contains:

- Author Name
- Email Address
- Commit Time
- Commit Message

This helps identify who made each change.

---

# 6. Creating a Local Repository

Create a project directory.

```bash
mkdir MyProject
```

Move into the directory.

```bash
cd MyProject
```

Initialize Git.

```bash
git init
```

Output:

```text
Initialized empty Git repository
```

Git creates a hidden folder:

```text
.git/
```

---

## What is `.git`?

The `.git` directory stores:

- Commit History
- Branch Information
- Configuration
- Repository Metadata
- Object Database

Without this folder, the directory is just a normal folder.

---

## Verify Hidden Files

```bash
ls -la
```

Output:

```text
.git
```

---

# 7. Git Workflow

Git follows a three-stage workflow.

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

Understanding this workflow is one of the most important Git concepts.

---

# 8. Working Directory

The **Working Directory** is the folder where you create or modify files.

Example:

```text
main.c

README.md

notes.txt
```

Files remain only in the working directory until they are added to Git.

---

## Example

Create a file.

```bash
touch hello.txt
```

The file exists, but Git has not started tracking it yet.

---

# 9. Staging Area

The **Staging Area** is a temporary area where Git collects files before creating a commit.

Think of it as a "waiting room" before saving changes permanently.

```text
Working Directory

        │

   git add

        ▼

 Staging Area
```

---

## Why Staging Area?

Suppose you modified three files.

```text
main.c

login.c

README.md
```

You may only want to commit:

```text
main.c
```

The staging area lets you choose exactly which files will be included in the next commit.

---

# 10. Local Repository

The **Local Repository** stores the complete history of the project.

After running:

```bash
git commit
```

Git permanently saves the staged files.

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

---

# 11. `git status`

The `git status` command displays the current state of the repository.

Syntax:

```bash
git status
```

---

## Example

```bash
git status
```

Possible Output:

```text
On branch master

No commits yet

Untracked files:

hello.txt
```

---

## What Does `git status` Show?

- Current Branch
- Modified Files
- Staged Files
- Untracked Files
- Repository Status

It is one of the most frequently used Git commands.

---

# 12. `git add`

The `git add` command moves files from the **Working Directory** to the **Staging Area**.

---

## Add a Single File

```bash
git add hello.txt
```

---

## Add Multiple Files

```bash
git add file1 file2
```

---

## Add All Files

```bash
git add .
```

This stages every modified and newly created file in the current directory.

---

## Workflow Example

```text
Create hello.txt

       │

git add hello.txt

       ▼

Staging Area

       │

Ready for Commit
```

---

## Verify Using `git status`

Before:

```text
Untracked files:

hello.txt
```

After:

```text
Changes to be committed:

hello.txt
```

This confirms that the file has moved to the **Staging Area**.

---

*End of Git & GitHub Notes (Part 1)*