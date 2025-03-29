# **GitHub Cheat Sheet**

## **1. Basic Git Commands**

| Command                    | Description                        |
| -------------------------- | ---------------------------------- |
| `git clone <repo-url>`     | Clone a repository                 |
| `git init`                 | Initialize a new Git repo          |
| `git status`               | Check changes in working directory |
| `git add <file>`           | Stage a file for commit            |
| `git add .`                | Stage all changes                  |
| `git commit -m "message"`  | Commit changes with a message      |
| `git push origin <branch>` | Push changes to GitHub             |
| `git pull origin <branch>` | Pull latest changes from GitHub    |
| `git fetch`                | Download objects without merging   |

## **2. Branching & Merging**

| Command                    | Description                        |
| -------------------------- | ---------------------------------- |
| `git branch`               | List all branches                  |
| `git branch <name>`        | Create a new branch                |
| `git checkout <branch>`    | Switch to a branch                 |
| `git checkout -b <branch>` | Create & switch to a new branch    |
| `git merge <branch>`       | Merge a branch into current branch |
| `git rebase <branch>`      | Rebase current branch onto another |

## **3. GitHub-Specific Workflows**

### **Pull Requests (PRs)**

- **Fork & Clone** → Make changes → `git push` → Open PR on GitHub.
- **Review PRs**: Comment, request changes, or approve.
- **Merge PRs**: Squash, rebase, or standard merge.

### **GitHub CLI (`gh`)**

| Command                | Description                            |
| ---------------------- | -------------------------------------- |
| `gh repo clone <repo>` | Clone a repo (faster than `git clone`) |
| `gh pr create`         | Create a PR from CLI                   |
| `gh pr checkout <PR#>` | Checkout a PR locally                  |
| `gh issue create`      | Create an issue                        |

### **GitHub Actions (CI/CD)**

- **Workflow file**: `.github/workflows/main.yml`
- **Triggers**: `push`, `pull_request`, etc.
- **Jobs**: Define steps (build, test, deploy).

## **4. Undoing Mistakes**

| Command                   | Description                              |
| ------------------------- | ---------------------------------------- |
| `git reset --hard HEAD`   | Discard all local changes                |
| `git revert <commit>`     | Create a new commit undoing changes      |
| `git reset --soft HEAD~1` | Undo last commit but keep changes staged |

## **5. Useful GitHub Shortcuts**

- **`t`** → Search files in repo.
- **`.` (dot)** → Open repo in VS Code online.
- **`Shift + ?`** → See all keyboard shortcuts.

---

### **Quick GitHub Flow**

1. **Fork** a repo (if not yours).
2. **Clone** → `git clone <your-fork-url>`.
3. **Branch** → `git checkout -b feature-branch`.
4. **Commit** → `git add . && git commit -m "message"`.
5. **Push** → `git push origin feature-branch`.
6. **Open PR** on GitHub.
