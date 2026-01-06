# Workflow

#### Changing Several Commits in Bulk

If commits were done with a wrong author, use Git Rebase to fix the authors of the commits:

    $ git rebase -i -p <commit-id>
    $ git commit --amend --author="John Doe <john@doe.org>"
    $ git rebase --continue
    $ git push -f origin master

The rebase starts from the commit after the informed `<commit-id>`. It wouldn't work if the rebase needs to consider the very first commit. To include the first commit, start an interactive rebase of all commits using `git rebase -i --root`.

#### Change the Most Recent Commit Message

The command below will open the text editor where we can change the commit message:

    $ git commit --amend

#### Adding a File to the Most Recent Commit

    $ git add missed-file.txt
    $ git commit --amend
    
#### Undo the Most Recent Commit

    $ git reset HEAD~
    
#### Undo One or More Commits Pushed to Remote

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

#### Remove a File From the Repository Without Deleting It

For a single file:

    $ git rm --cached mylogfile.log

For a single directory:

    $ git rm --cached -r logs


#### Changing Several Commits in Bulk

If commits were done with a wrong author, use Git Rebase to fix the authors of the commits:

    $ git rebase -i -p <commit-id>
    $ git commit --amend --author="John Doe <john@doe.org>"
    $ git rebase --continue
    $ git push -f origin master

The rebase starts from the commit after the informed `<commit-id>`. It wouldn't work if the rebase needs to consider the very first commit. To include the first commit, start an interactive rebase of all commits using `git rebase -i --root`.

#### Change the Most Recent Commit Message

The command below will open the text editor where we can change the commit message:

    $ git commit --amend

#### Adding a File to the Most Recent Commit

    $ git add missed-file.txt
    $ git commit --amend
    
#### Undo the Most Recent Commit

    $ git reset HEAD~
    
#### Undo One or More Commits Pushed to Remote

Update the working branch to have it as a backup:

    $ cd ~/java/projects/project
    $ git pull origin master

Create a new clone to use as workshop:

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

#### Remove a File From the Repository Without Deleting It

For a single file:

    $ git rm --cached mylogfile.log

For a single directory:

    $ git rm --cached -r logs

#### Restore a Deleted Branch

The follow commands recover a branch that was deleted locally with the command `git branch -D issue-52`. Use `reflog` to figure out the _<sha>_ of the deleted branch:

    $ git reflog

Take note of the _<sha>_ and jump into it:

    $ git checkout -b issue-52 dc4b3ff

Look at the log to see if it contains what you are looking for:

    $ git log

Finally, move to the master branch and merge the recovered branch into it:

    $ git checkout master
    $ git merge issue-52
