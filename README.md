# github

### Git and GitHub: A Complete Tutorial with Commands and Theory on Branches

#### Introduction to Git and GitHub
- **Git** is a version control system that allows developers to track and manage changes to code across versions. It helps in collaborating on code efficiently and allows developers to revert changes, create new versions (branches), and merge them when needed.
- **GitHub** is a web-based platform built on top of Git that provides cloud-based hosting services for Git repositories. It facilitates collaborative coding with features such as pull requests, forking, issue tracking, and more.

---

### 1. **Setting Up Git and GitHub**

#### Install Git
Before using Git, it needs to be installed on your machine. On Linux, you can install it using the package manager:
```bash
sudo apt-get install git    # Debian-based systems
sudo yum install git        # Red Hat-based systems
```

#### Configuring Git
After installation, configure Git with your name and email, which will be associated with your commits.
```bash
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"
```
This configuration is essential because every Git commit stores the author’s name and email.

#### SSH Authentication with GitHub
To securely connect your local Git to your GitHub account without entering credentials every time, it's recommended to set up SSH authentication.

1. Generate an SSH key:
    ```bash
    ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
    ```
2. Add the key to the SSH agent:
    ```bash
    eval "$(ssh-agent -s)"
    ssh-add ~/.ssh/id_rsa
    ```
3. Copy the SSH public key and add it to GitHub under **Settings → SSH and GPG keys**:
    ```bash
    cat ~/.ssh/id_rsa.pub
    ```

---

### 2. **Basic Git Commands**

#### Clone a GitHub Repository
To get a copy of an existing repository to your local machine, use the `git clone` command. You can clone a repository via SSH or HTTPS.
```bash
git clone git@github.com:username/repository.git  # Using SSH
git clone https://github.com/username/repository.git  # Using HTTPS
```
This command creates a local copy of the repository, allowing you to work on it.

#### Check the Status of Changes
```bash
git status
```
This shows which files have been modified, which are staged for commit, and which are untracked (new files).

#### Adding Files to Staging Area
```bash
git add <file>          # Stage a specific file
git add .               # Stage all changes
```
The `git add` command moves changes from the working directory to the **staging area**. Only files in the staging area will be included in the next commit.

#### Committing Changes
```bash
git commit -m "Your commit message"
```
A commit is like a snapshot of the current state of the project. The message helps describe what changes were made.

#### Push Changes to GitHub
```bash
git push origin <branch-name>
```
This uploads your local commits to the remote repository on GitHub. 

#### Pull Changes from GitHub
```bash
git pull origin <branch-name>
```
Fetch and integrate changes from the remote repository into your local branch. This keeps your branch updated with the latest code.

---

### 3. **Working with Branches**

#### What is a Branch in Git?
A **branch** in Git is a pointer to a specific commit. It allows you to diverge from the main codebase (usually `main` or `master`) and work on a feature, bug fix, or experiment without affecting the main project. Once the work is complete, it can be merged back into the main branch.

#### List Branches
```bash
git branch             # List local branches
git branch -r          # List remote branches
```
This shows all the branches available, both locally and on the remote repository.

#### Create a New Branch
```bash
git checkout -b <new-branch-name>
```
This command creates a new branch and switches to it. The `-b` flag combines branch creation and branch switching into one step.

#### Switch Between Branches
```bash
git checkout <branch-name>
```
Switch to another existing branch. This moves you to the snapshot represented by that branch.

#### Push a Branch to GitHub
```bash
git push -u origin <new-branch-name>
```
This uploads the branch to the remote repository and sets up tracking, so future `git push` commands will know where to send the updates.

#### Merge a Branch into Another Branch
```bash
git checkout <target-branch>  # Switch to the branch you want to merge into
git merge <source-branch>     # Merge the source branch into the target branch
```
Merging takes the changes from the source branch and integrates them into the target branch.

#### Delete a Branch
After a branch is merged, you can delete it.
- **Locally:**
    ```bash
    git branch -d <branch-name>
    ```
- **Remotely:**
    ```bash
    git push origin --delete <branch-name>
    ```

---

### 4. **Advanced Git Branching Concepts**

#### Rebase vs Merge
- **Merge** integrates changes by creating a new merge commit, combining two branches' histories.
- **Rebase** reapplies your branch's changes on top of another branch's latest commits, providing a cleaner, linear history.
```bash
git checkout <branch-to-rebase>
git rebase <base-branch>
```

#### Resolve Merge Conflicts
When Git can't automatically resolve changes during a merge or rebase, you'll get a **merge conflict**. To resolve:
1. Manually edit the conflicting files (Git marks them with `<<<<<<` and `>>>>>>`).
2. Once resolved, mark the conflicts as resolved by staging the files:
    ```bash
    git add <resolved-file>
    ```
3. Continue the merge or rebase process:
    ```bash
    git merge --continue
    git rebase --continue
    ```

#### Stashing Changes (Saving work without committing)
If you're working on something but need to switch branches or pull new changes, you can stash your uncommitted changes:
```bash
git stash                 # Save work in progress
git stash pop             # Reapply stashed changes
```

---

### 5. **Forking and Pull Requests**

#### Forking a Repository
A fork is a personal copy of another user's repository. You can fork a repository on GitHub by clicking the "Fork" button. This allows you to make changes without affecting the original repository.

#### Creating a Pull Request (PR)
After forking and making changes, you can create a **Pull Request** to propose your changes to the original repository.
1. Push your changes to your forked repository.
2. Go to the original repository and click **New Pull Request**.
3. Select your branch and submit the PR.

---

### 6. **Useful Git Commands**

#### View Commit History
```bash
git log                  # Detailed log
git log --oneline         # Compact one-line view
```

#### Compare Changes Between Branches
```bash
git diff <branch1> <branch2>
```

---

### Conclusion

By mastering Git and GitHub commands, especially around branching, you can efficiently manage multiple features and work in parallel with other team members. Git branches allow you to isolate your work, test features, and merge changes back into the main codebase without disrupting other parts of the project.
