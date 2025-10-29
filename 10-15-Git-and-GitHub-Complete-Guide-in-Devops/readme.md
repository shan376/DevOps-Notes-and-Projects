# 🧾 **Git and Source Code Management – Complete Notes**

## **Lecture 10: Introduction to Git and Source Code Management**

### 🧠 Concepts Covered

1. What is Source Code Management (SCM)?
2. Centralized Version Control System (CVCS) vs Distributed Version Control System (DVCS)
3. What is Git?

### 📘 Explanation

Source Code Management helps developers track and manage code changes. Git is a powerful tool used for SCM.

**CVCS:** Central repository used; example: SVN.
**DVCS:** Each developer has a full copy; example: Git.

### 🔧 Why Use Git?

* Tracks changes
* Enables collaboration
* Open-source and distributed

---

## **Lecture 11: Git Stages, Setup, and Key Commands**

### 🧠 Concepts Covered

1. Three Git Stages: Workspace, Staging Area, Local Repository
2. Commands: git add, git commit, git push, git pull, git branch
3. Setup Procedure for Git
4. Git Terminology: Repository, Server, Workspace, Commit, Commit ID, Tag, Branch
5. Advantages of Git

---

## **Lecture 12: Installing Git on AWS Linux Machine**

### 🔧 Practical Procedure

1. Create two Linux EC2 instances in different AWS regions.
2. On each instance, install Git using:

   ```
   sudo yum install git -y
   ```
3. Create a GitHub account to push and pull code.

---

## **Lecture 13: Git Commit, Push, Pull and .gitignore**

### 🧠 Concepts and Commands

* Initialize repo: `git init`
* Create file: `touch file.txt`
* Add file: `git add file.txt`
* Commit file: `git commit -m 'message'`
* Push: `git push -u origin master`
* Pull: `git pull origin master`
* Ignore files: create `.gitignore` file and list files to ignore

---

## **Lecture 14: Git Branch, Merge, Stash, and Reset**
### 🧠 Concepts

* Master branch is default
* Create new branch: `git branch branch1`
* Switch branch: `git checkout branch1`
* Merge branch: `git merge branch1`
* Stash: `git stash`, `git stash pop`
* Reset: `git reset --hard HEAD~1`

---

## **Lecture 15: Git Revert, Tags, GitHub Clone, GitLab**

* Revert commit: `git revert <commit-id>`
* Remove untracked: `git clean -f`
* Tag commit: `git tag v1.0 <commit-id>`
* Delete tag: `git tag -d v1.0`
* Clone repo: `git clone <repo-url>`
* View logs: `git log -1`, `git log -2`, `git log --oneline`
* Search commits: `git log --grep='message'`
