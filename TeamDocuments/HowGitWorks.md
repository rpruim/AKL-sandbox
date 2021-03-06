# How Git Works
R Pruim  
June 2015  








[http://pcottle.github.io/learnGitBranching/](http://pcottle.github.io/learnGitBranching/) provides an excellent set of tutorials and a visualization tool for learning about git.


## The basic git vocabulary

**local** = on your machine

 
**remote** = `origin` = github

  * This is all you need to know when you are getting started
  
  * In general, a remote is some other place to which you synch (like github)
    * Remotes may be named anything, but the standard (default) name is **origin**.  When you see `origin`, think github
    * You could rename this to `github`, but since so much documentation out there uses `origin`, it's probably easier to just leave as `origin`.
    * In large projects, you may have multiple remotes (e.g., your github fork of some project and the original github project that you forked).  
    * For small scale things stick with one remote and call it `origin`.
  
**commit** = n. snapshot of all files in the repository; v. to create a commit

  * You can return to the state of any commit at any time.
  * Each commit (except the initialization commit) has at least one predecessor.  
    * Most have just one, **merge commits** have two.
    * There is no record of changes made between commits
  * Each commit has a *unique ID* (called its **SHA**) which is just a jibberish string
  * Each commit has a **message** describing why it was created
  * The chain of commits and their anscestors/descendants represent the history of your project.
  * **Commits** are cheap -- better too many than too few.
  
**stage** = to mark files for the next commit

  * These files are said to be in the **staging area**
    * Only changes to staged files are noticed when commiting.  
    * Unstaged files that have changed will look (to the commit) like they did at the previous commit.  This allows you to work on multiple files but stage them in as many or few commits as you like.
    * Files that have never been committed are said to be **untracked** and are not part of the repository.
  * command line: `git add`
  * most GUIs (e.g., RStudio) do this by selecting a checkbox
    
**branch** = (movable) pointer to a commit.
  * commits have nasty SHA names and don't change
  * branches have nice human-readable names and move from commit to commit as work progresses

**HEAD** = the "current commit" or "current branch"

  * The file system will look like `HEAD` plus any changes that have been made but not yet committed.
  * If a new commit is made, it will be an immediate descendant of `HEAD` which will then be updated to point to the new commit.
  * Usually `HEAD` points to a branch (the current branch).
    * When `HEAD` points to a commit instead, it is called a **detached HEAD**
  
**checkout** = make a branch (or commit) the current branch (or commit) 

  * i.e., move `HEAD` there
  * Checking out a commit can lead to a **detached HEAD** 

**merge** = combine the work from multiple branches

  * merging an anscestor into a descendant does nothing -- the descendant already has all of the history of the anscestor
  * merging a descendant into an anscestor is called a **fast-forward merge** 
    * the only change is that `anscestor` will now point to the same commit as `descendant`
  * other merges can potentially lead to **conflicts** that need resolving.
    * git will resolve conflicts when it can (for example when each file only changes on one of the two branches after their most recent common anscestor) and warn you to take inerventive measures when it cannot.

**push** = copy commits from local to remote

**fetch** = copy commits from remote to local

**pull** = `fetch` + `merge`

## A bit more vocabulary 

You are less likely to need these, but it is good to know that they exist so you can hunt down how to use them when you get yourself into a bind.

**rebase** = replay commits along a different branch

  * this is git's version of revisionist history
  * makes it look like work done in parallel was done sequentially

**reset** = move a branch backwad in time

  * this makes it looks like some work was never done
  * recent commits may be left *dangling*
  * not a good idea if you have already made the work public via `push`
  
**revert** = "undo" by creating a new commit that has the same contents as a previous commit but a different history.  

  * This records the fact that something was done and then undone

**cherry-pick** = grab only certain commits and play them along a branch

  * This can be handy if you are debugging and in the end only need some of the commits you tried to fix the problem.

## Keys to success

 1. Learn the vocabulary above and how to visualize what is happening.
 
 2. Commit often (and use good commit messages).
 
 3. Pull often (to make sure you have everyone else's latest and greatest).
 
 4. Communicate with team members.
 
    * to avoid awkward merges
    * to avoid redundant or counter-productive work
    
 5. Keep things as simple as works.
 
