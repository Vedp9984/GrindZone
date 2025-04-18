# **Git and Version Control Notes**


## **1. Introduction to Version Control**

### **What is Version Control?**
- A system that records changes to files over time so you can recall specific versions later.
- Essential for collaboration, tracking changes, and reverting to previous states.

### **Types of Version Control Systems**
1. **Local Version Control** (e.g., manual file copying)  
   - Pros: Simple.  
   - Cons: No collaboration, prone to errors.  

2. **Centralized Version Control (CVCS)** (e.g., SVN, Perforce)  
   - Single central server stores all versions.  
   - **Pros**: Enables team collaboration.  
   - **Cons**: Single point of failure (if server crashes).  

3. **Distributed Version Control (DVCS)** (e.g., Git, Mercurial)  
   - Every user has a full copy of the repository.  
   - **Pros**: No single point of failure, faster operations.  
   - **Cons**: Larger storage usage (entire history is cloned).  

---

## **2. Git Basics**

### **What is Git?**
- A **distributed** version control system created by Linus Torvalds (2005).  
- Key Features:  
  - Fast, scalable, and supports non-linear development (branches).  
  - Snapshots changes instead of tracking file differences.  

### **Key Concepts**
1. **Repository (Repo)**:  
   - A directory where Git tracks changes.  
   - Initialize with `git init`.  

2. **Commit**:  
   - A snapshot of changes at a point in time.  
   - Created with `git commit -m "message"`.  

3. **Branch**:  
   - A parallel line of development.  
   - Default branch: `main` (formerly `master`).  

4. **Remote**:  
   - A shared repository (e.g., GitHub, GitLab).  
   - Linked with `git remote add origin <url>`.  

5. **Staging Area**:  
   - Intermediate area to review changes before committing (`git add`).  

### **Basic Commands Cheatsheet**
| Command | Description |
|---------|-------------|
| `git init` | Initialize a new Git repo |
| `git add <file>` | Stage changes for commit |
| `git commit -m "msg"` | Commit staged changes |
| `git status` | Check untracked/staged changes |
| `git log` | View commit history |

---

## **3. Git Workflow**

## **Basic Git Workflow**
1. **Working Directory**: Make changes to files.
2. **Staging Area (`git add`)**: Select changes to commit.
3. **Repository (`git commit`)**: Permanently store changes.

### **Detailed Commands with Flags**

#### **1. Tracking Changes**
| Command | Flags | Description |
|---------|-------|-------------|
| `git status` | `-s` (short format) | Show concise status |
| `git diff` | `--staged` | Show staged (not committed) changes |
| | `--color-words` | Word-level diff |

#### **2. Committing Changes**
| Command | Flags | Description |
|---------|-------|-------------|
| `git commit` | `-m "message"` | Commit with message |
| | `-a` | Stage & commit all tracked files (skips `git add`) |
| | `--amend` | Modify the last commit |

####  What is **atomic commit** ?

An atomic commit is a single Git commit that encapsulates one logical change: a self-contained, focused update that doesn't break existing functionality.

(e.g: "fix the login button," not "fix buttons and add user profiles")

#### **3. Viewing History (`git log`)**
| Command | Flags | Description |
|---------|-------|-------------|
| `git log` | `--oneline` | Compact commit history |
| | `--graph` | Show branch structure |
| | `-p` | Show changes (patch) |
| | `-n 5` | Show last 5 commits |
| | `--author="name"` | Filter by author |
| | `--since="1 week ago"` | Time-based filtering |

#### **4. Git push and Git pull**
| Push to the remote branch 
```bash
# Push changes to the remote repository
git push origin main  # Replace 'main' with your branch name
```
| Pull command fetches changes from the remot
```bash
git pull origin main  # Pulls updates into your current branch
```

## **4. Branching and Merging**