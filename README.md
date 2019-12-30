##  [How to see the changes in a Git commit?](https://stackoverflow.com/questions/17563726/how-to-see-the-changes-in-a-git-commit)
  
To see the diff for a particular  `COMMIT`  hash:

`git diff COMMIT~ COMMIT`  will show you the difference between that  `COMMIT`'s ancestor and the  `COMMIT`. See the man pages for  [git diff](http://jk.gs/git-diff.html)  for details about the command and  [gitrevisions](http://jk.gs/gitrevisions.html)  about the  `~`  notation and its friends.

Alternatively,  `git show COMMIT`  will do something very similar.

## [What does cherry-picking a commit with Git mean?](https://stackoverflow.com/questions/9339429/what-does-cherry-picking-a-commit-with-git-mean)

Cherry picking in Git means to choose a commit from one branch and apply it onto another.

This is in contrast with other ways such as  `merge`  and  `rebase`  which normally apply many commits onto another branch.

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

Use  `git rebase -i <after-this-commit>`  and replace "pick" on the second and subsequent commits with "squash" or "fixup", as described in  [the manual](http://git-scm.com/docs/git-rebase#_interactive_mode).

In this example,  `<after-this-commit>`  is either the SHA1 hash or the relative location from the HEAD of the current branch from which commits are analyzed for the rebase command. For example, if the user wishes to view 5 commits from the current HEAD in the past the command is  `git rebase -i HEAD~5`.