# Introduction

Git is a distributed version control system used to manage the source code of Digger. We can use apt-get to install Git:

    $ sudo apt-get install git

Before using Git, let's do a few initial configurations:

* The initial branch name to use in all new repositories:

       $ git config --global init.DefaultBranch main

* Identify the author of the commits:

       $ git config --global user.email "you@example.com"
       $ git config --global user.name "Your Name"

#### Configuring Git to Simplify Authentication

For the moment, every time we push code to GitHub the prompt asks for a username and password. We can bypass this step by registering a SSH key. To do that, we first check whether there is already an existing SSH key we can reuse:

    $ ls -al ~/.ssh

If files with the extension .pub are listed then one of them can be reused to authenticate to GitHub. If not, then we can create one:

    $ ssh-keygen -t rsa -b 4096 -C "[firstname.lastname]@domain.com"
      Enter file in which to save the key (/Users/[user]/.ssh/id_rsa): [Press enter]
      Enter passphrase (empty for no passphrase): [Type a passphrase]
      Enter same passphrase again: [Type passphrase again]

The generated keys need to be protected with the right permissions otherwise the access won't work:

    $ chmod 700 ~/.ssh
    $ chmod 644 ~/.ssh/id_rsa.pub
    $ chmod 600 ~/.ssh/id_rsa

The next step is to add the new key - or an existing one - to the ssh-agent. This program runs the duration of a local login session, stores unencrypted keys in memory, and communicates with SSH clients using a Unix domain socket. Everyone who is able to connect to this socket also has access to the ssh-agent. First, we have to enable the ssh-agent:

    $ eval "$(ssh-agent -s)"

And add key to it:

    $ ssh-add ~/.ssh/id_rsa

The next step is to make GitHub aware of the key. For that, we have to copy the exact content of the file `id_rsa.pub` and paste into GitHub. To make no mistake about the copy, install a program called xclip:

    $ sudo apt-get install xclip

And then copy the content of the file `id_rsa.pub` in the clipboard:

    $ xclip -sel clip < ~/.ssh/id_rsa.pub

The command above is the equivalent of opening the file `~/.ssh/id_rsa.pub`, selecting the whole content and pressing `Ctrl+C`. This way, you can paste the content on GitHub when required in the next steps. On the GitHub side:

1. Login at https://github.com

2. In the top right corner of the page, click on the profile photo and select Settings

3. In the user settings sidebar, click SSH keys

4. Then click Add SSH key

5. In the form, define a friendly title for the new key and paste the key in the Key field

6. Click Add Key to finish with GitHub

To make sure everything is working, lets test the connection:

    $ ssh -T git@github.com
      The authenticity of host 'github.com (207.97.227.239)' can't be established.
      RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
      Are you sure you want to continue connecting (yes/no)? yes
      _
      Hi [username]! You've successfully authenticated, but GitHub does not
      provide shell access.
