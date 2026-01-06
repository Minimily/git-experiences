# Repository

We can test the installation by cloning the Digger repository:

    $ mkdir -p ~/java/projects/digger
    $ cd ~/java/projects/digger
    $ git clone git@github.com:htmfilho/digger.git .

This configuration works only when we use a ssh connection to GitHub. To verify that, go to one of your local GitHub projects and check the url pointing to the server:

    $ cd ~/java/projects/digger
    $ git remote -v

If the url starts with https:// then you are using `https` instead of `ssh`. In this case, you should change the url to the ssh one:

    $ git remote set-url origin git@github.com:htmfilho/digger.git

The automatic authentication should work after that.

By the way, when the repository is too big, with a long versioning history, we
may consider cloning only the latest version to kick-off our day faster. We do
it using the `--depth` parameter:

    $ git clone --depth 1 -b master git@github.com:htmfilho/digger.git

When we have our configuration done and everything is working, before stopping
for the day, run:

    $ git fetch --unshallow

to download the rest of the repo. We want this to enable the "blame" feature in
our IDE.

#### Changing The Author To The One Recognizable by GitHub

In case your default Git author is not the same as GitHub, configure the author of the repository:

    $ git config --local user.name "John Doe"
    $ git config --local user.email "john@doe.org"

It can also be done to a specific commit:

    $ git commit --author#"John Doe <john@doe.org>"

#### Setting Pull Behaviour

The `git pull` command merges the remote branch into the local branch with a merge commit, but we don't think this commit is useful. We want to make sure our commits represent changes made by developers only. So, we would like to ask you to use rebase to merge remote branches locally. You can do it at every `pull` with:

    $ git pull --rebase origin master
    
or change a local configuration to make it the default `pull` behavior:

    $ git config --local pull.rebase true
    
Note: you don't need to run this local configuration if you already have it globally.

#### Pruning Deleted Remote Branches

When branches are removed from origin, this change is not automatically reflected in the local clone when doing a git pull or git fetch. To have deleted remote branches automatically pruned from the local repo, set the following config:

    $ git config --global fetch.prune true