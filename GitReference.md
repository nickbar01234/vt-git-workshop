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
- At a given time, you will checkout a **snapshot** (version) of the project and modify / create files.
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
staged file(s), use the command `git add <file-1> ... [file-N]`.

Note that Git only store the state of the file at the most recent time when you run
`git add ...`. This means in the following snippet, committing will only take
a snapshot of the README with Hello world!

```sh
echo "Hello world" >> README
git add README
echo "Bye world" >> README
git commit
```

> ⚠️ You may come across `git add .` in the wild, which means add all created / modified files to the staging area. If you're new to Git, this can be dangerous as you may inadvertently commit unwanted files.

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

_Scenario_: You accidentally committed `node_modules/` and want to remove it
Git tracking.

You have 2 options:

1.  Delete the folder physically from Git and commit it.
2.  Use `git rm -r --cached node_modules/`. This marks `node_modules` as deleted
    in the staging area, but does not remove it physically. When you run `git
commit`, it will remove `node_modules/` from Git database.

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
since their last commit.

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

You can view all local branches using `git branch` - checkout [the section on Github](#github)
for more information about local branches.

### Create Branch

`git switch -c <name>` creates a new branch from where `HEAD` points to and update
`HEAD` to the new branch.

### Switching Branch

You can switch branch using `git switch <name>`. This command updates `HEAD` to
the branch that you specified.

### Development

With Git branches, you can develop new features independently of other branches.
This means that commits made on one branch does not effect the state of another
branch.

One of the most common workflow with Git is:

1. Keep one branch (i.e `main`) as the source of truth. Any commits on the
   main branch has been tested and reviewed.
2. Develop new features / bug fixes on a different branch.
3. Once you're happy with the changes for a given branch, incorporate the changes
   into the `main` branch.

### Merge

To incorporate changes from a different branch, the general step is:

1. `git switch <target-branch>`
2. `git merge <branch-with-new-changes>`

Running `git merge` will add all the new commits from `<branch-with-new-changes>`
to your current branch.

#### Merge Conflict
