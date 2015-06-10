# Github Workflow

This is an outline of the workflow for development and maintenance of 20x materials. 

## Dependencies

Maintainers will need the following available to follow this workflow completely.

  * LaTeX
    * latex packages:
  * RStudio 
  * R packages used in materials: `knitr`
  * R packages for development: `devtools`, `testthat`, `roxygen2` 

## LaTeX workflow (Rnw files)

*Space available to describe LaTeX conventions and practices.*

## Github workflow

This git/github workflow is based primarily on the "Private Small Team" model from the ProGit document with a bit of "Private Managed Team" influence.

### Issues

Github issues are a way to create and monitor "to-do" lists and to record conversations 
about them.  Issues can be created and modified directly on github.  Look for the encircled
bang (!) icon to find the issues.

**Open** issues require further attention.  **Closed** issues no longer need
attention, but can be reviewed (and re-opened) as necessary.  Issues can be categorized
with **labels** and **assigned** to members of the team.  

**Milestones** can be used to 
set deadlines for groups of issues and to chart progress toward meeting the deadline.  
A milestone could be created for each term, for example. 
Issues that should be resolved prior to the term could be assigned to the milestone 
for that term.  When all the issues 
for the milestone have been closed, then things should be ready to go for the term.

You can configure github to
email you whenever an issue is created or modified.  If you reply to these emails, 
it will show up in the issue conversation -- often with more cruft than is 
desireable. You might prefer to 
click on the "view this issue on github" link and compose my response 
that way rather than via email.

Github provides searching over the text in the issues.

#### Issue Practices:

 1. Create an issue whenever you think of somthing that should or could be changed in your project.  

    You can always close the issue if you decide not to do it.  
    But having a record keeps good ideas from being lost or forgotten.
 
 2. Keep issue conversations related to the issue.  
 
    If it is about something else, start a new issue.

 3. Mention issues by number in commit messages (e.g. `Addressing #2` or `Closes #2`).

### Internal/Team Documents

Github has facilities for using either markdown or a wiki to maintain internal documents 
(like this one, which was created using RMarkdown within RStudio.) 

### Setting up RStudio to work with Github

 1. Create a **New Project**
 
 2. Choose **Version Control**, then **Git**
 
 3. Copy and paste the github URL (labeled **SSH clone URL** on Gihub code pages) as the **Repository URL**, and choose a name and location for your project.  
 
    ![Github Project](images/GithubProject.png)
    
 4. Hit **Create Project** and RStudio will clone the repository onto your machine.
 
    Cloning copies to your machine all of the files from
    the remote repository (github) and their "history"
    and puts copies on your machine.  These files behave 
    just like any other files on your machine.  You can open them,
    edit them, move them, rename them, delete them, etc.
    
    We'll learn later how to keep any changes you make and any
    changes others make in synch.
 
That's all there is to it.

### RStudio's Git tab

RStudio projects that are using github for version control 
will have a **Git** tab.  This tab provides easy access 
to the most import git functionality.

 1. History

    Look at what how the repository and the files in it have changed
    over time.
    
 2. Commit
 
    Take a "snapshot" of the repository including any "staged" work.
    
 3. Push
 
    Send your local commits to the remote repository (github)
 
 4. Pull
 
    Bring remote commits from github to your local machine.
    
 5. Branches (checkout)
 
    The Git tab will show you which branch you are in and allow
    you to switch among branches

[http://pcottle.github.io/learnGitBranching/](http://pcottle.github.io/learnGitBranching/) 
provides a nice way to visualize these and other git operations and a series of 
useful tutorials that explain the most important aspects of git.

### Creating new stuff

 1. Create an issue.
 
    If there isn't already an issue, you can create an issue at 
    github describing the new feature to be added and assign 
    the issue to the person doing the coding.

 2. Make sure what you have locally is up to date.
 
    Click **Pull** in RStudio or use one of these
    
```bash
git fetch    # differs from pull in that it allows/requires you to do the merging manually
git pull     # fetches and attempts to merge
```

 3. Select which branch to work on (create a new one if needed).
 
    Name the branch after the issue number (e.g., `issue001`) or describing
    the work to be done (`adding-fish-case-study`).  
 
    If the goal is to eventually update an existing branch (e.g. `beta`), 
    then you can create a new branch that (at first) is the same as the 
    existing branch. If the changes are deemed good, the work from the new 
    branch can be merged into the existing branch.
```bash
git branch issue001 origin/beta  # create the new topic branch based on origin/beta
git checkout issue001            # switch to the new branch (or select in RStudio)
```

    The last two commands can be combined into
```bash
git checkout -b issue001 origin/beta
```

    At this point, **the new topic branch is only on your local machine**.  

    **Note:** You can create and checkout the new branch using
```bash
git checkout -b issue001 beta
```
    but this links your new work to other work you have done since your local beta was in sync with origin/beta.  Sometimes this might be just what you want and if `beta` and `origin/beta` point to the same commit, it doesn't matter which you do.

 4. Create your new stuff, committing frequently as you work.
```bash
git add <files>         # or check box in RStudio
git commit              # or click on commit button in RStudio
# do some stuff
git add <files>         # or check box in RStudio
git commit              # or click on commit button in RStudio
```
  * First line of commit message should be a short description of committed work.  Second line should be blank.  The next few lines can add additional details.
  * Avoid committing too many things all at once.  Commits are cheap.
  * It is possible to get back to the state you were in at any commit.

 5. Need consultation?  Push your topic branch so the rest of us can see it.
```bash
git checkout issue001   # or select branch in RStudio
git commit              # or select commit in RStudio
git branch -a           # list all the branches to make sure that there isn't already an origin/issue001 
git push -u origin issue001  # -u will set things up so local issue001 track origin/issue001 and push/pull work
```
    git will report back that the new branch has been made and tracking established. Now others can inspect origin/issue001 or use git pull to pull the branch to their local machine and work with you to fix problems.  Or they can grab a local copy with
```bash
git checkout --track origin/issue001
```

 6. merge topic branch into `beta`

    When you are to the point where the code in your topic branch is ready to be included in the `beta` branch,
```bash
git checkout beta
git pull                 # or click pull in RStudio (or git fetch plus git merge)
git merge issue001       # incorporate new stuff into local beta
bin/checkpackage.sh      # make sure everything is still good
```
    At this point, you may be done with this branch (see how to delete it below), or you may decide to continue working in this branch.  In the latter case, you should first merge the other way around 
```bash
git checkout issue001
git merge beta
```

    This should be a fast-forward merge because after the previous merge, `beta` will be a descendent of `issue001`.

 7. merge local `beta` into `origin/beta`

    Since we set up the local `beta` branch to track `origin/beta`, this can be done with
```bash
git checkout beta        # or select the beta branch in RStudio
git push                 # or click push in RStudio
```

 8. Tagging and removing branches

    Once work on a branch is completed and has been merged into one of the other branches (`beta`, for example), we don't really need the branch any more.  If new issues arise and we reopen the issue, we'll want to begin work somewhere downstream.  But we might like a record of where we were when we completed work on the issue.  So let's tag the commit first and then delete.  
```bash
git checkout issue001
git tag -a 'issue001-1'    # phase 1 of issue001 completed
git checkout beta
git branch -d issue001     # remove the local branch
git push origin :issue001  # remove remote branch too
```

    Now `issue001` will not show up in lists of branches, and you won't be tempted to do more work there, but you can still see the state of the project when that issue was completed.  As shown above, the tag is pre-merge.  It would also be possible to tag the `beta` branch post-merge.  It's not clear to me which is preferable.  Perhaps pre-merge in case we find a problem with the merge and want to go back to just before the merge happened.  If there were issues with the merge that are cleaned up later, another tag can be created if we like.

 9. When ready for public release, `beta` can be merged into `master`.
```bash
git checkout beta
git merge master
```
 * If no changes have been made to `master`, nothing will happen here.  But if there were parallel changes to `master`, this lets you see them in `beta` before changing `master`.
   
    If everything seems clean in `beta` we can update `master`
```
git checkout master
git merge beta         # should be a fast-forward if done after the above
git tag -a "v2.0"      # tag the release with the version number or some other name
git checkout beta      # get off of master so you don't forget
```
    

### Miscellaneous Git Stuff

#### Renaming a Branch

```bash
git branch -m oldname newname   # to rename branch named oldname
git branch -m newname           # to rename the current branch
```

#### Recovering from "wrong branch syndrome"

If you have accidentally begun working on the wrong branch and catch this early, there is a fairly easy fix.  **But don't do this if you have already pushed changes that you are trying to revert!**

 1. commit your work
```
git commit -m "work in progress"
```

 2. Create a new branch.
```
git branch newbranch
```

 3. Revert the changes by backing up one commit
``` 
git reset --hard HEAD^
```

 4. Switch to the new branch and continue from there.
```
git checkout newbranch
```
    The result of this will leave your original branch (beta, master, whatever) back where it was before you accidentally started working there.  The new branch will have the "work in progress" stuff that you can now clean up and reintegrate when the time is right.  

    If you need to back up more than one commit, you can refer to other commits using `^` and `~` to refer to previous commits along a path.  For example, 3 commits before `HEAD` is either `HEAD^^^` or `HEAD~3`.

    For more information, see [http://effectif.com/git/move-commit-from-one-branch-to-another] 
