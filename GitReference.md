# Git Reference

This guide serves as a quick reference for git commands and concepts that
we discussed in the workshop. If you would like to learn more about Git,
I would recommend reading the first 3 chapters of [Git manual](https://git-scm.com/book/en/v2)
and checkout their [API documentations](https://git-scm.com/docs).

## Notations

- `[X]` indicates that `X` is an optional argument.
- `<X>` indicates that `X` is a required argument.

## Commands

### Making Changes

| Command                         | Explanation                                                                           |
| :------------------------------ | :------------------------------------------------------------------------------------ |
| `git init`                      | Creates a new Git repository locally.                                                 |
| `git add <file-1> ... [file-n]` | Adds `file-x` to staging area; if `file-x` is a folder, recursively add all subfiles. |
| `git commit -m <message>`       | Takes a snapshot of all staged files.                                                 |
| `git log`                       | Displays your Git history in reversed chronological order.                            |
| `git status`                    | Displays the status of your Git repository.                                           |

### Undoing Changes

| Command                       | Explanation                                                                                                                                                                                               |
| :---------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `git rm [-r] --cached file`   | - Removes a file from Git without deleting it from your laptop. <br> - If `file` is a folder, you need to pass `-r` flag. <br> - After running the command, you need to **commit** to remove it from Git. |
| `git restore [--staged] file` | - Without `--staged`, revert `file` to its state since last commit. <br> - Using `--staged`, remove `file` from staging area.                                                                             |

### Git Branches

| Command                       | Explanation                                                                                             |
| :---------------------------- | :------------------------------------------------------------------------------------------------------ |
| `git branch [--all]`          | - Display branches in your repository <br> - To also view remote branches, use `--all` flag             |
| `git switch -c <branch-name>` | - Updates `HEAD` to `branch-name` <br> - If `branch-name` doesn't exist yet, use `-c` flag create to it |
| `git merge <branch-name>`     | Bring changes from a `branch-name` to where `HEAD` points at                                            |

### Working With Remote Repositories

| Command                         | Explanation                                                                  |
| :------------------------------ | :--------------------------------------------------------------------------- |
| `git clone <url>`               | Fetches a remote repository to your laptop.                                  |
| `git push origin <branch-name>` | Updates the remote repository's `branch-name` with commits from your laptop. |
| `git pull origin <branch-name>` | Updates your laptop with commits from the remote repository's `branch-name`. |

## Brief Introduction to Git

Git is a version control tool built by Linus Torvalds, the creator of Linux.
Version control tool is essential to software development, for the following
reasons (and many more):

1. Record changes to the codebase.
2. Add new features / fix bugs safely.
3. Collaborate with other developers.

## Basic Git Worfklow

![Git workflow](https://git-scm.com/book/en/v2/images/areas.png)

- In Git, we refer to a project as a **repository**.
- At a given time, you will checkout a **snapshot** (version) of the project and modify / create / delete files.
- Once you're happy with the changes, you will place the files into the **staging area**.
- Finally, you will commit and take a snapshot of the project!

## Creating A Repository

Inside the folder you want to start a project, run `git init`. This creates
a hidden `.git` folder, where Git stores different versions of your project
(and other metadata).

```sh
git init
```

## Staging Area

The staging area contains the files that will go into your next commit. To
stage file(s), use the command `git add <file-1> ... [file-N]`.

Note that Git only stores the state of the file(s) when they were most recently
staged, using `git add ...`. This means in the following snippet, committing
will only take a snapshot of the README with Hello world!

```sh
echo "Hello world" >> README
git add README
echo "Bye world" >> README
git commit
```

> ⚠️ You may come across `git add .` in the wild, which means add all created / modified / deleted files to the staging area. If you're new to Git, this can be dangerous as you may inadvertently commit unwanted files.

## Status

`git status` displays the current state of the repository. It tells you
important information, such as [the current branch](#git-branch) that you're on;
what files you've modified; and what files are in the staging area.

## Committing

Running `git commit -m <message>` takes a snapshot of all the files in the
staging area. A snapshot means an exact copy - although this is not important
for the purpose of the workshop, it's still interesting to think about how
Git approaches version control 😊.

> ℹ️ If you forget `-m <message>`, Git will open an editor for you to write a commit message. By default, it will open nano.

> ⚠️ Write a good and descriptive commit message. It helps you and other developers to have a good sense of what was added in the commit.

## History

`git log` displays all the commits in the branch that you're on in reversed
chronological order. You will mostly use this to get a quick overview of
the changes introduced by different commits.

## Undo Changes

_Scenario_: You accidentally committed `node_modules/` and want to remove it from Git.

You have 2 options:

1.  Delete the folder physically from Git and commit it.
2.  Use `git rm -r --cached node_modules/`. This marks `node_modules` as deleted
    in the staging area, but does not remove it physically. When you run `git
commit`, it will remove `node_modules/` from Git locally.

```sh
git rm -r --cached node_modules/
git commit -m "Remove node_modules"
```

_Scenario_: You staged a file, but decided you do not want to commit the file yet.

You can remove file(s) from the staging area with `git restore --staged <file-1> ... [file-N]`.

```sh
echo "Hello world" >> unwanted.txt
git add unwanted.txt
git restore --staged unwanted.txt
```

_Scenario_: You would like to discard all changes to a file.

Running `git restore <file-1> ... [file-N]` reverts the file(s) to their state
since last committed.

```sh
echo "Bad changes" >> README
git restore README
```

## .gitignore

Sometimes it's useful to tell Git to not track certain files in the repository,
such as log files or dependencies. With Git, you can create `.gitignore`
file at the root directory and specify all the file(s) and folder(s) you want
Git to ignore.

```sh
# .gitignore

*.log # Do not track any file that has .log extension
node_modules/ # Ignore all files inside node_modules/ directory
a/**/*.txt # Ignore all files inside a/ that has .txt extension
```

Git offers many more functionalities with `.gitignore`, which you can explore
in [their documentation](https://git-scm.com/docs/gitignore).

> ℹ️ If you already committed file X, you will need to run `git rm --cached X` first, otherwise `.gitignore` does nothing.

## Git Branch

### Head

`HEAD` is a special pointer that indicates which commit you're currently viewing
in the repository.

### Branch

Git branch is a friendly name for a commit in Git.

> ℹ️ Running `git init` starts with a default branch. In my case, it is `main`.

> ℹ️ You can change your default branch with `git config --global init.defaultBranch <name>`

### Viewing Branch

You can view all local branches using `git branch` - checkout [the section on Github](#remote-repository)
for more information about local branches.

### Create Branch

`git switch -c <name>` creates a new branch from where `HEAD` points to and update
`HEAD` to the new branch.

### Switching Branch

You can switch branch using `git switch <name>`. This command updates `HEAD` to
the branch that you specified.

### Development

With Git branches, you can develop new features independently of other branches.
This means that commits made on one branch does not affect the state of another
branch.

One of the most common workflow with Git is:

1. Keep one branch (i.e `main`) as the source of truth. All commits made on the
   `main` have been tested and reviewed.
2. Develop new features / bug fixes on a different branch.
3. Once you're happy with the changes on a given branch, incorporate the changes
   into `main`.

### Merge

To incorporate changes from a different branch, the general step is:

1. `git switch <target-branch>`
2. `git merge <branch-with-new-changes>`

Running `git merge` will add all the new commits from `<branch-with-new-changes>`
to your current branch.

#### Merge Conflict

![Merge Conflict Meme](https://slack-imgs.com/?c=1&o1=ro&url=http%3A%2F%2Fwww.quickmeme.com%2Fimg%2F58%2F58cdc50dcb04a16438e9759f2db7a0ac23442e718f69af7b2ac0a3761915e92a.jpg)

A merge conflict occurs when the same file(s) is edited in different ways and
Git does not know how to resolve the differences.

![Merge Conflicts](https://phoenixnap.com/kb/wp-content/uploads/2021/06/git-merge-conflict-error.png)

When you have a merge conflict, the first step you should do is run `git status`
to view all impacted files.

![Merge Conflicts Status](https://css-tricks.com/wp-content/uploads/2014/04/01-unmerged-paths-after-merge.gif)

All the impacted files will be listed under **Unmerged paths**. You will need
to go through each impacted files and manually resolve the conflicts. Let's
take a look at an example of a conflict file:

```
<<<<<<< HEAD
Hello world
=======
Bye world
>>>>>>> feature-branch
```

- The conflicting changes are delinated by `=======`.
- The code between `<<<<<<< HEAD` to `=======` exists on your current current branch.
- The code between `=======` to `>>>>>>> feature-branch` exists on the branch that you're merging from.

When dealing with merge conflicts, you have 3 options:

1. Keep the current changes.
2. Keep the incoming changes.
3. Keep both changes.

The correct answer is context dependent. For example, the most common types
of merge conflicts are:

1. ✅ Different developers introduce different features. In most cases, you want to keep both changes.
2. ❌ Different developers edit the same feature in different ways. This is
   **usually bad**, because it indicates unclear task delegations in the team.

After you resolved the conflicts, run `git add <conflicting-files>` and `git commit -m <message>` to complete the merge.

## Github

Github is a platform to host Git repositories. We will use Github to collaborate
in group project. Everything you've learned with Git applies when working with
Github. The only new concept that you need to know is **remote repository**. A
remote repository is a Git repository hosted on the Internet. This means that:

1. You can develop independently on your laptop and **push** your changes to a
   remote repository.
2. You can **pull** new changes in the remote repository (from other developers) to your laptop.

### Clone

To get a copy of a remote repository, navigate to Github's repository dashboard.
Next, copy the SSH url, and run `git clone <url>`.

![Git clone](https://docs.github.com/assets/cb-14601/mw-1440/images/help/repository/code-button.webp)

### Push

To update the remote repository with your changes, run `git push origin <branch>`,
where `branch` is the name of the branch that you're currently on. Checkout the section
on [remote repository](#remote-repository) to learn more about `origin`.

![Push Meme](https://slack-imgs.com/?c=1&o1=ro&url=https%3A%2F%2Fmiro.medium.com%2Fv2%2Fresize%3Afit%3A1200%2Fformat%3Awebp%2F0*tmfbLDU_hIeg0B3B.jpg)

> ❌ This is only for fun. Please don't use `git push --force` if you're not sure what it does.

### Pull

To update your current branch with changes from the remote repository, run
`git pull origin <branch>`, where `branch` is the name of the branch that you're currently on.

### Remote Repository

When using Github (or any other platform), your laptop tracks the state of
the repository locally and remotely; recall that you can sync your laptop using
`git pull`. By default, `origin` is a friendly alias for a remote repository.

If you have ran `git pull` on a non-empty repository, running `git log` will
show something similar to this:

![Git log](https://miro.medium.com/v2/resize:fit:1170/1*ukGdfvm25qgoYSnN8huc_g.png)

In the abovementioned example, `origin/development` refers to the state of
remote branch. You can switch to a remote branch by running `git switch
<branch-name>`, omitting `origin/`.

> ℹ️ If you're curious what `origin` is, try running `git remote -v`.

### Collaboration

I will outline a common guideline when working in teams with Github.

- Clearly communicate with everyone to delegate tasks.
- Before you begin working on a new feature, checkout `main` and pull the latest `main`.
- Work on the new feature off a branch.
- When you're happy with your changes, push, make a PR, and ping someone else for review.
- If your changes introduce merge conflicts:
  - Checkout the main branch.
  - Pull the latest main.
  - Checkout your branch
  - Merge main to your branch
  - Push your changes

> ❌ Never push directly to `main`, as that's bad practice. Checkout this [Stack Overflow post](https://stackoverflow.com/questions/46146491/prevent-pushing-to-master-on-github) to enable branch protection.

> ✅ A PR should only address 1 feature / change.

> ✅ Keep PRs small and concise. A good rule of thumb is less than 300-500 lines of codes per PR. If you introduces too many changes, it's hard for someone else to give you meaningful feedback.
