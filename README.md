## [How to see the changes in a Git commit?](https://stackoverflow.com/questions/17563726/how-to-see-the-changes-in-a-git-commit)

To see the diff for a particular `COMMIT` hash:

`git diff COMMIT~ COMMIT` will show you the difference between that `COMMIT`'s ancestor and the `COMMIT`. See the man pages for [git diff](http://jk.gs/git-diff.html) for details about the command and [gitrevisions](http://jk.gs/gitrevisions.html) about the `~` notation and its friends.

Alternatively, `git show COMMIT` will do something very similar.

## [What does cherry-picking a commit with Git mean?](https://stackoverflow.com/questions/9339429/what-does-cherry-picking-a-commit-with-git-mean)

Cherry picking in Git means to choose a commit from one branch and apply it onto another.

This is in contrast with other ways such as `merge` and `rebase` which normally apply many commits onto another branch.

1.  Make sure you are on the branch you want to apply the commit to.

    ```
    git checkout master

    ```

2.  Execute the following:

    ```
    git cherry-pick <commit-hash>

    ```

N.B.:

1.  If you cherry-pick from a public branch, you should consider using

    ```
    git cherry-pick -x <commit-hash>

    ```

    This will generate a standardized commit message. This way, you (and your co-workers) can still keep track of the origin of the commit and may avoid merge conflicts in the future.

2.  If you have notes attached to the commit they do not follow the cherry-pick. To bring them over as well, You have to use:

    ```
    git notes copy <from> <to>
    ```

## [Squash my last X commits together using Git](https://stackoverflow.com/questions/5189560/squash-my-last-x-commits-together-using-git)

Use `git rebase -i <after-this-commit>` and replace "pick" on the second and subsequent commits with "squash" or "fixup", as described in [the manual](http://git-scm.com/docs/git-rebase#_interactive_mode).

In this example, `<after-this-commit>` is either the SHA1 hash or the relative location from the HEAD of the current branch from which commits are analyzed for the rebase command. For example, if the user wishes to view 5 commits from the current HEAD in the past the command is `git rebase -i HEAD~5`.

# [Delete commits from a branch in Git](https://stackoverflow.com/questions/1338728/delete-commits-from-a-branch-in-git)

**Careful:** `git reset --hard` _WILL DELETE YOUR WORKING DIRECTORY CHANGES_. Be sure to **stash any local changes you want to keep** before running this command.
Assuming you are sitting on that commit, then this command will wack it...

```
git reset --hard HEAD~1
```

The `HEAD~1` means the commit before head.
Or, you could look at the output of `git log`, find the commit id of the commit you want to back up to, and then do this:

```
git reset --hard <sha1-commit-id>
```

---

If you already pushed it, you will need to do a force push to get rid of it...

```
git push origin HEAD --force
```

**However**, if others may have pulled it, then you would be better off starting a new branch. Because when they pull, it will just merge it into their work, and you will get it pushed back up again.
If you already pushed, it may be better to use `git revert`, to create a "mirror image" commit that will undo the changes. However, both commits will be in the log.

---

FYI -- `git reset --hard HEAD` is great if you want to get rid of WORK IN PROGRESS. It will reset you back to the most recent commit, and erase all the changes in your working tree and index.

---

Lastly, if you need to find a commit that you "deleted", it is typically present in `git reflog` unless you have garbage collected your repository.

## [How to save username and password in git?](https://stackoverflow.com/questions/35942754/how-to-save-username-and-password-in-git)

Run

```
git config --global credential.helper store
```

then

```
git pull
```

provide a username and password and those details will then be remembered later. The credentials are stored in a file on the disk, with the disk permissions of "just user readable/writable" but still in plaintext.

If you want to change the password later

```
git pull
```

Will fail, because the password is incorrect, git then removes the offending user+password from the `~/.git-credentials` file, so now re-run

```
git pull
```

to provide a new password so it works as earlier.

---

`INFO:`

The storage format is a `.git-credentials` file, stored in plaintext.

Also, you can use other helpers for the `git config credential.helper`, namely memory cache:

```
git config credential.helper cache <timeout>

```

which takes an optional `timeout parameter`, determining for how long the credentials will be kept in memory. Using the helper, the credentials will never touch the disk and will be erased after the specified timeout. The `default` value is `900 seconds (15 minutes).`

---

**WARNING** : If you use this method, your git account passwords will be saved in `plaintext` format, in the `global .gitconfig file`, e.g in linux it will be `/home/[username]/.gitconfig`

If this is undesirable to you, use an `ssh key` for your accounts instead.

## [How do I rename a local Git branch?](https://stackoverflow.com/questions/6591213/how-do-i-rename-a-local-git-branch)

If you want to rename a branch while pointed to any branch, do:

    git branch -m <oldname> <newname>

If you want to rename the current branch, you can do:

    git branch -m <newname>

A way to remember this is -m is for "move" (or mv), which is how you rename files.

If you are on Windows or another case-insensitive filesystem, and there are any capitalization change in the name, you need to use -M, otherwise, git will throw branch already exists error:

    git branch -M <newname>

# [Git - how to set username and email?](https://dirask.com/q/git-how-to-set-username-and-email-MDgRQ1)

## 1. Overview

Usually we configure global username and email address after installing Git.

We can also set configuration for single specific repository.

In this post we cover both configurations:

1.  global (all repositories)
2.  single repository

Git configuration works the same under Windows, Linux and macOS.

## 2. Git - set global username and email configuration

### Open command line (eg git bash)

### Set your username:

      git config --global user.name "FIRST_NAME LAST_NAME"

### Set your email address:

       git config --global user.email "OUR_NAME@example.com"

## 3.Git - set username and email configuration for single repository

#### Open command line (eg git bash) and change directory into specific repository

### Set your username

      git config user.name "FIRST_NAME LAST_NAME"

### Set your email

    git config user.email "OUR_NAME@example.com"

## 4.Verify your configuration by showing username and email

### Show username

    git config user.name


### Show email

     git config user.email

# [How to change the commit author of commit?](https://stackoverflow.com/questions/3042437/how-to-change-the-commit-author-for-one-specific-commit)

Interactive rebase off of a point earlier in the history than the commit you need to modify (`git rebase -i <earliercommit>`). In the list of commits being rebased, change the text from  `pick`  to  `edit`  next to the hash of the one you want to modify. Then when git prompts you to change the commit, use this:

```
git commit --amend --author="Author Name <email@address.com>" --no-edit

```
----------

For example, if your commit history is  `A-B-C-D-E-F`  with  `F`  as  `HEAD`, and you want to change the author of  `C`  and  `D`, then you would...

1.  Specify  `git rebase -i B`  ([here is an example of what you will see after executing the  `git rebase -i B`  command](https://help.github.com/articles/about-git-rebase/#an-example-of-using-git-rebase))
    -   if you need to edit  `A`, use  `git rebase -i --root`
2.  Change the lines for both  `C`  and  `D`  from  `pick`  to  `edit`
3.  Exit the editor (for vim, this would be pressing Esc and then typing  `:wq`).
4.  Once the rebase started, it would first pause at  `C`
5.  You would  `git commit --amend --author="Author Name <email@address.com>"` or set `git config initially` and type `git commit --amend --reset-author --no-edit`
6.  Then  `git rebase --continue`
7.  It would pause again at  `D`
8.  Then you would  `git commit --amend --author="Author Name <email@address.com>"`  again OR `git commit --amend --reset-author --no-edit`
9.  `git rebase --continue`
10.  The rebase would complete.
11.  Use  `git push -f`  to update your origin with the updated commits.


# [How do you commit code as a different user?](https://stackoverflow.com/questions/3696938/how-do-you-commit-code-as-a-different-user)

Use -c option along with git-commit to override any previous configuration. It will not touch your global/project configuration. For example, to override name and email:

```
git -c user.name='My Name' -c user.email='my@email.com' commit -m "Custom message"

```

However, if you intend to keep it as an additional setting, I would suggest to use an alias. Edit your  `~/.gitconfig`  file and append a new alias for each non-default user and email.

```
[user]
  name = My Name
  email = default@email.com

[alias]
  commit-x = -c user.name='My X Name' -c user.email='mr_x@email.com' commit
  commit-y = -c user.name='My Y Name' -c user.email='mr_y@email.com' commit
  commit-z = -c user.name='My Z Name' -c user.email='mr_z@email.com' commit

```

Alias will be applied globally. Test it.

```
git commit -m "Custom message with committer and author My Name <default@email.com>"
git commit-x -m "Custom message with committer and author My X Name <mr_x@email.com>"
git commit-y -m "Custom message with committer and author My Y Name <mr_y@email.com>"
git commit-z -m "Custom message with committer and author My Z Name <mr_z@email.com
```



# [git still shows files as modified after adding to .gitignore](https://stackoverflow.com/questions/9750606/git-still-shows-files-as-modified-after-adding-to-gitignore)

`git update-index --assume-unchanged *path/to/file*`