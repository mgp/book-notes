## Pro Git

from http://progit.org/book/

### Chapter 1: Getting Started
* In distributed version control systems, clients don't just check out the latest copies of files, but fully mirror the repository.
* When you commit, or save the state of your project in Git, it takes a snapshot of all files instead of computing their differences.
* The entire project history is saved locally, so most operations in Git need only local resources and work quickly.
* The working directory is a checkout of one version of the project; these files are pulled out of the compressed git directory, or repository.
* A staged file is modified in the working directory and marked as part of your next commit snapshot to the git directory.

### Chapter 2: Git Basics
* Tracked files were in the last snapshot; untracked files are everything else, or files not in your last snapshot or staging area.
* The `add` command tracks and stages an untracked file, and stages a tracked and modified file, overwriting any staged copy.
* Running `diff` shows what you've changed but not yet staged; adding `--staged` shows what you've staged and will commit next.
* Running `diff` compares files in the working directory with their staged copies, not those in the repository.
* Passing `--cached` to `rm` only removes a file from staging, so it is untracked; omitting it also removes the file from the working directory.
* The `--stat` option for `log` shows summarized diff stats for files; `--graph` shows branch and merge history and is useful with `--pretty`.
* To limit the output of `log` to specific files and paths, append them to the command, preceded by a double dash.
* Run `commit` with `--append` to overwrite your last commit; without adding any files, you will simply overwrite your commit message.
* The `fetch` command doesn't merge new data with your own; `git pull` will try to automatically merge, and works if you've run `clone`.
* When running `git push origin master`, `origin` is the server to push to, and `master` is the branch to push.
* Annotated tags are recommended over lightweight tags; they're stored as full objects, are checksummed, and include a message.
* If you forget to `tag` immediately after a `commit`, specify the `commit` checksum or its unique prefix at the end of the command.

### Chapter 3: Git Branching
* The `git branch` command creates a new brach, but the `HEAD` pointer does not refer to the new branch until you use `checkout`.
* To create a branch and switch to it at the same time, run the `checkout` command with the `-b` option.
* Without a clean working state when switching branches, conflicts may arise; using `stash` and `commit` amending can help.
* If a conflict arises while merging, you must manually edit the file and then use add to put it in the staging area.
* It is safe to delete unstarred branches listed by `git branch --merged`; branches listed by `--no-merged` cannot be deleted yet.
* Topic branches are short-lived branches you create and use for a single particular feature or related work.
* Remote branches refer to the state of branches on remote repositories, which move upon any network communication.
* A `fetch` retrieves new remote branches as read-only pointers prefixed with `origin/`, which you manually `merge` to make editable.
* Tracking branches are local branches directly associated with a remote branch, so `git pull` and `git push` work without arguments.
* Using `rebase` replays changes from one line of work onto another, so the log will show a linear history instead of a parallel one.
* Don't `rebase` commits that you'e pushed to a public repository, as others will base work on commits that you've rebased.

### Chapter 6: Git Tools
* Passing `--abbrev-commit` to the `git log` command will use unique SHA-1 prefixes in the output.
* The reflog only contains changes to your local repository; after you've cloned a repository, your reflog will initially be empty.
* Using `^{n}` a the end of a reference refers to its nth parent; using `~{n}` traverses n parents up the ancestry tree.
* The double-dot shows commits only reachable by the latter reference; the triple-dot shows reachable by either, but not both.
* Running `git add` in interactive mode with `-i` and selecting 5 or `p` allows staging parts of files; when done, use `git commit`.
* The `stash` command takes the dirty state of your working directory and saves it on a stack of unfinished changes you can reapply later.
* To restage files that were staged when you created a stash, run `stash apply` with the `--index` option.
* Using `stash branch` branches from the commit you were on when you stashed your work, then applies and drops the stash.
* Amending a commit is like a very small `rebase`, so don't amend your last commit if you've already pushed it.
* To modify a commit farther back, run `rebase` in interactive mode to rebase them onto the `HEAD` they were originally based.
* The `filter-branch` command allows rewriting your history easily, but don't use it if other people have based work off those commits.
* Passing `-C` to `blame` tries to figure out where snippets of code within the file came from if they were copied from elsewhere.
* After you run `bisect`, don't forget to run `bisect` reset to reset your `HEAD` to where you started, or you'll end up in a weird state.
* Submodules are recorded as the exact commit they're at; this way, the environment an be recreated exactly.
* Subtree merging maps one project as a subdirectory of the other and vice versa, and fixes many problems of submodules.
* To get a `diff` between the branch of a subproject and the superproject it's merged into, you must run `diff-tree` instead of `diff`.

### Chapter 9: GIt Internals
* Low-level git commands are called the plumbing commands; those you use at the command line are porcelain commands.
* Git stores trees and blobs; a tree contains blob or subtree entries, where each entry has a mode, type, and filename.
* Trees capture the content of your staging area or index; commits annotate a tree with who saved the snapshot, when, and why.
* References or refs are files that store SHA-1 values of commits; `HEAD` isn't another SHA-1 value, but a reference name.
* The tag object is like a commit object, containing a tagger, date, message, and pointer, but points to a commit and not a tree.
* While files are normally stored in a "loose object format," pack files that contain delta values are created occasionally.
* All references under `refs/heads` on the server are written to `refs/remotes/origin` locally; branches can be qualified with the latter.
* A refspec mapping remote branches to local references cannot have partial globs, but namespacing achieves the same ends.
* If you reset and lose commits, the reflog tracks changes to `HEAD`, so branching from a reflog entry can surface them again.
* Deleted large files still take up space because the entire history is stored; amending this is destructive to your commit history.

