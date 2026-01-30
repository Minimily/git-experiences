# Workflow

Let's first clarify that, when discussing the workflow, we use the term "Issue" to refer to a change request, not a technical issue that could happen when we are dealing with version control.

## Before

Before doing the work.

## During

While doing the work.

### Dealing with Unexpected Issues

Imagine a scenario where you are busy, working on an issue, then a more urgent issue comes up, so you have to pause what you are doing to shift context. It happens all the time. Git is our best friend in this kind of situation, but we have to use it properly.

#### The `dev` Branch Must Be Immutable

I was working on an issue and, in the spirit of saving time, I have commited changes directly to the branch `main`. In this project, we could not push changes to the remote branch `main`, so we needed to create a separate branch from `main` and create a pull request. I thought I could simply do that and move on, which normally works when everything goes well.

However, I may need to test something in `main` before the pull request is accepted, but since it contains changes then it doesn´t behave the same way. I would need to reset my local dev branch to be able to reproduce the issue. It can be done running:

```cmd
$ git reset --hard origin/main
```

### Change the Most Recent Commit Message

The command below will open the text editor where we can change the commit message:

    $ git commit --amend

### Adding a File to the Most Recent Commit

    $ git add missed-file.txt
    $ git commit --amend
    
### Undo the Most Recent Commit

    $ git reset HEAD~

## After

Once the work is done, many situations can happen. Here is how to deal with them.

### Changing Several Commits in Bulk

If commits were done with a wrong author, use Git Rebase to fix the authors of the commits:

    $ git rebase -i -p <commit-id>
    $ git commit --amend --author="John Doe <john@doe.org>"
    $ git rebase --continue
    $ git push -f origin master

The rebase starts from the commit after the informed `<commit-id>`. It wouldn't work if the rebase needs to consider the very first commit. To include the first commit, start an interactive rebase of all commits using `git rebase -i --root`.
    
### Undo One or More Commits Pushed to Remote

Update the working branch to have it as a backup:

    $ cd ~/java/projects/project
    $ git pull origin master

Create a new clone to use as experimentation:

    $ cd ..
    $ git clone git@github.com:htmfilho/project.git project-temp
    $ cd project-temp

You can also clone a specific branch:

    $ git clone --branch bugfix git@github.com:htmfilho/project.git project-temp

Look at the log to see the id of the latest valid commit:

    $ git log

Force the head of the tree to point to the latest valid commit:

    $ git reset –hard 73d48037

Force the new head into the remote branch (origin):

    $ git push –force origin master

The clients that still have the old commits should update their local branches accordingly before the next push:

    $ git reset –hard origin/master

### Remove a File From the Repository Without Deleting It

For a single file:

    $ git rm --cached mylogfile.log

For a single directory:

    $ git rm --cached -r logs

### Restore a Deleted Branch

The follow commands recover a branch that was deleted locally with the command `git branch -D issue-52`. Use `reflog` to figure out the _<sha>_ of the deleted branch:

    $ git reflog

Take note of the _<sha>_ and jump into it:

    $ git checkout -b issue-52 dc4b3ff

Look at the log to see if it contains what you are looking for:

    $ git log

Finally, move to the master branch and merge the recovered branch into it:

    $ git checkout master
    $ git merge issue-52

### Cherry Pick a Commit from One Branch to Another

In some projects, we may have release branches. The ones that are deteched from
the main branch when all the features planned for the release are done. This
release branch allows development work to continue in the main branch without
affecting what is about to be released.

However, when the release branch is deployed to a staging environment, we may
find some bug that needs to be fixed before deploying to production. In this
scenario, we have to push the same fix to the release branch as well as to the
main branch, to ensure the fix is carried on to future releases.

To save some effort, start working on the release branch, commiting the fix and
creating a pull request. Then follow these steps to add the fix to the main
branch:

1. Get the hash or the hard range of all commits related to the fix. You can
   see the hashes using:
   
       $ git log

2. Move to the main branch:

       $ git checkout main
       $ git pull

3. Create a branch for the change:

       $ git checkout -b fix-123

4. Cherry-pick the fix from the release branch using the hashes:

       $ git cherry-pick <hash1> <hash2> <hash3>

After this, you may have to fix code conflicts, making sure the fix is adapted
to recent changes in the main branch.