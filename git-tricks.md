## Confusing Parts

### Branches, HEAD, Commits

Commits are snapshots. You can revisit previous snapshots at any time. Those commits (or snapshots) never get destroyed. Even so-called destructive operations like `rebase` leave references behind.

Commits (or commit objects) are uniquely identified by their SHA hash.

`git commit` saves a snapshot of what's on the Staging Area (or index).

Despite their name, *branches* are not the collection of commits you think they are. They act as references to <u>a single commit</u>. Each *branch* has a corresponding *head*. Normally (when you `checkout` a branch), the *branch* and the *head* point to the same commit, namely the most recent one. 

When you `commit` changes on the *staging area*, it creates a new snapshot that points to the previous HEAD. The *branch* and *HEAD* then update to point to the newly created *commit object*.

*HEAD* (capitalized) is a more specific form of *head*. *HEAD* refers to the currently active *head*.

### Reoccurring Flags  `-p`

This command works for `reset` and `add`. It means partial additions or resets.

### What do the `--soft` `--hard` flags mean for `reset`?

`--soft`  means keep the *staging area* and *working directory* as is. `--hard` means clear the *staging area* and then set the *working directory* to match the *commit object* you're switching to.

Stating neither means the default behavior, or transferring *staged changes* while resetting the *working directory*.

### What's a fast-forward merge/ three-way merge?

*Fast-forward merges* are not really merges. If one branch is the direct ancestor of another, then the ancestor is simply moved up to match the other. The branches haven't diverged, and it's a simple linear path, so no fancy stuff happens. All the old references stay in place.

*Three-way merges* are real merges. They take the *heads* of two different branches, typically the *HEAD* (currently active *head*) and the head of a different branch, and creates a new snapshot from the combination of the two. It does this by see generating *diff files* with respect to their *base file* (or common ancestor). It then combines the *diffs*. The resulting *commit object* will have an additional ancestor. The main ancestor will be on the active branch. So if you did `git checkout master`, then `master` would be the active branch. If you then did `git merge feature

### What's `master`, `origin`, and `upstream` supposed to mean?

They're conventions. `master` typically refers to the default branch (or at least the main branch), `origin` typically refers to the remote branch. 

`master` by itself tends to refer to a local branch, but it doesn't have to. For example, `origin/master` could refer to the main branch online, while `master` could refer to the local main branch.

`upstream` may or may not apply. `upstream` is typically used for forking scenarios. `upstream` would point to the original remote.

### How do `pull`, `push`, `fetch` , `merge` and `rebase` relate to each other?

`git pull` is short for `git fetch; git merge {origin/CURRENT_BRANCH}` 

`git pull` can be either `--merge` or `--rebase`. By default, it merges and is typically a *three-way merge*.

`git push` sends changes to the associated remote branch. It must be a *fast-foward merge*. Otherwise, you will be asked to modify your working branch.

## FAQ

### How do I rename a file across multiple commits?

`git filter-branch --tree-filter {SHELL COMMANDS} {START_COMMIT}...{END_COMMIT}`

This is the nuclear option of rewriting history. Exercise caution.

E.g

```sh
$ git filter-branch --tree-filter "
if [ -d old_folder ];
	then mv old_folder new_folder
fi
" master...feature-scraper
```

This executes a command for every commit. This one renames `old_folder` to `new_folder` for every commit from the master branch to the feature-scraper branch.

### How do I abort a commit / rebase / merge?

Two options:

* Delete all lines in a message.
* **(Preferred)** `:cq` or `:cquit` in Vim. This exits with a non-zero code. 

### How do I undo the previous commit while still keeping my current changes?

`git reset --soft HEAD^` or `git reset --soft HEAD~1` 

`reset` lets you set the current branch HEAD wherever you want.

`--soft` means leave the working directory and index along. Index is where the staged changes are, and working directory is where the unstaged changes are, so keep the diffs as usual.

`HEAD^` means the parent of HEAD. The carrot `^` means parent of.

Useful alias: `git config --global alias.uncommit "git reset --soft HEAD~1"`

### I made changes that were supposed to go into a different branch. How do I switch into a new branch with the changes I have now?

Use `git stash -u`. Then `git checkout -b {NEW_BRANCH}`. Then `git stash pop`. This command takes your unstaged changes and hides them away until you pop it open. **The `-u` flag is to add untracked (newly added) files as well.** 

### I've created a new local branch, but don't have a corresponding remote branch. What do I do?

Push as normal. `git push -u origin {LOCAL_BRANCH}` This is short for `git push {REMOTE_NAME} {LOCAL_BRANCH_NAME}:{REMOTE_BRANCH_NAME}`. Because we omit the `:{REMOTE_BRANCH_NAME}`, git will assume that the remote branch name is supposed to match the local branch name and automatically create it. 

Best practice is to use a `-u` flag to set the new remote branch as upstream to your local branch.

### Can I rebase a pending pull request?

Yes. Branches are throwaway.

### How do I reword the last commit?

`git commit --amend`

You can also use this to commit new changes or even new files on top of the original commit. These changes will not show up in `git log`, but they will show up in `git reflog`

### I want to fix  a minor typo in a previous commit. It's before the HEAD. What do I do?

Use `git commit --fixup {COMMIT}` or `git commit --squash {COMMIT}` .

E.g.

```shell
$ git log --oneline
3d13055 Update DEV_NOTES.md
a07b66e Create nextBusArrivals.js to fetch from Bustime API
1b09ffa Install babel-cli for transpilation

$ git add fileWithTypo.js
$ git commit --fixup a07b66e
[master 68a9160] fixup! Create nextBusArrivals.js to fetch from Bustime API
 1 file changed, 6 insertions(+), 6 deletions(-)
```

These commands automatically create a new commit whose title corresponds to a previous commit. Here the title `fixup! Create nextBusArrivals.js to fetch from Bustime API` corresponds to `Create nextBusArrivals.js to fetch from Bustime API`

An even easier way that doesn't involve typing SHAs in is to use the first word of the commit message you care about:

```shell
$ git commit --fixup :/Create
```

Later on, you can `git rebase -i --autosquash {BRANCH_NAME}` and bash will automatically realign the commits for you:

```shell
pick 1b09ffa Install babel-cli for transpilation
pick a07b66e Create nextBusArrivals.js to fetch from Bustime API
fixup 68a9160 fixup! Create nextBusArrivals.js to fetch from Bustime API
pick 3d13055 Update DEV_NOTES.md
```

Note, you still have the freedom to rearrange and rebase as you normally do.

You can turn the `--autosquash` behavior on by default with 

```shell
$ git config --global rebase.autosquash true
```

`--squash` and `--fixup` differ in that squashes will prompt you on which commit message you want to use. Fixups will not prompt you. It will simply combine the changes in the later commit into the earlier commit and use the earlier commit's message.

### How do I rewrite the history of my last N commits?

`git rebase -i HEAD~{N}`

For 4 commits, that would be `git rebase -i HEAD~4`

### How do I rebase my branch on top of master?

**Danger: DO NOT REBASE UPSTREAM/MASTER ON TOP OF YOUR BRANCH. MASTER IS PUBLIC AND YOU WILL SCREW UP EVERYONE ELSE'S WORKFLOW.**

This workflow assumes you have a personal fork.

```SHELL
$ git remote add upstream {ORIGINAL_FORK}

# get the latest commits
$ git fetch upstream

$ git checkout {OUR_LOCAL_FEATURE_BRANCH}
# you may need to deal with conflicts when rebasing
$ git rebase upstream/master

# only push to your origin fork, not the original
$ git push --force origin master
```



**Danger: DO NOT REBASE UPSTREAM/MASTER ON TOP OF YOUR BRANCH. MASTER IS PUBLIC AND YOU WILL SCREW UP EVERYONE ELSE'S WORKFLOW.**

### What are alternatives to `git push --force`?

You should almost always use `git pull --rebase` This will rebase your branch on top of the public branch.

### What's the differences between all the `git reset` options?

`git reset` will remove changes from the staging area, but not the actual files. `git reset --hard` will go back to a previous commit, destroying any unstaged changes. It will still keep newly created files, however.

Note that `git reset` is distinct from `git revert`. `git reset` actually rewinds the branch history to a previous point in time. `git revert {COMMIT}` is more like a nullification function, or like an anti-commit. It creates a new commit object that is the opposite of a previous commit's diff message.

### How can I slim down `git log` message?

`git log --oneline --decorate`

`--oneline` flag limits output to SHA + title

`--decorate` adds branch information, blue for HEAD, green for local branches, and red for remote branches.

### How can I embellish my `git log` with what others are doing?

`git log --graph --all` gives us commits where people decided to branch off.

`-graph` adds ASCII art to identify branching

`-all` gives info for all current branches

### How can I read logs for a particular file?

`git log {FILENAME}` 

### How can I read the diff for a given commit?

`git show {COMMIT}` where commit can either be a SHA or a search string (E.g. `:/string`)

### How can I read the most recent changes?

`git show` is the cleanest. It will retrieve commit message + diff file for the most recent commit by default.

### How can I remove a single file from the staging area?

`git reset {FILENAME}`. It's not the most semantic. An easy way to deal with it is to`git config --global alias.unstage reset`. The semantics are better than way

## `git diff`

### Basics

This command usually shows what the *working directory* is adding or subtracting with respect to something else.

`git diff` shows what the *working directory* is adding or subtracting to the *staging area*. If no changes have been added to the *staging area*, then the *staging area* will look the exact same as the HEAD. `git diff` will be identical to `git diff HEAD` then.

`git diff HEAD` or `git diff {BRANCH_NAME}` shows what the *working directory* is adding to either the current HEAD or the head of a different branch.

`git diff {COMMIT} {COMMIT}` shows what the second commit is adding to the first commit.

`git diff --cached` is the odd one out. It shows what the *staging area* is gonna add to the HEAD.

### Only List for Some Files

`git diff -- {...FILES}` You can include as many as you want.

### Minimal Diff Algorithm

`git diff --diff-algorithm=minimal` lets you spend extra time to generate the smallest possible diff. 

### Hide Noise

`git diff -w`: Strips out whitespace like code indents and dedents from the diff message.

`git diff --word-diff`: Replaces line changes with word changes. Useful for mild content changes.

## References

* [Little Things I Like to Do with Git](https://csswizardry.com/2017/05/little-things-i-like-to-do-with-git/)