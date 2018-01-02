# Cache credentials for HTTPS {#credential-caching}

If you plan to push/pull using HTTPS, you want Git to cache your credentials (username, password) (or you should set up SSH keys, chapter \@ref(ssh-keys)), so you don't need to enter them over and over again. You'll need to set this up on each computer you want to connect to GitHub from.


## Get a test repository

You need a functioning test Git repository. One that exists locally and remotely on GitHub, with the local repo tracking the remote.

The repository from the previous exercise \@ref(push-pull-github)) from your local computer, will be perfect.

If you have just verified that you can work with GitHub from RStudio (chapter \@ref(rstudio-git-github)), that test repo will also be perfect.

You may proceed when

  * You have a test repo.
  * You know where it lives on your local computer. Example:
    - `/home/tania/tmp/myrepo`
  * You know where it lives on GitHub. Example:
    - `https://github.com/trallard/myrepo`
  * You know local is tracking remote. In a terminal with working directory set to the local Git repo, enter:
  
        git remote -v
        
    Output like this confirms that fetch and push are set to remote URLs that point to your GitHub repo:
    
        origin	https://github.com/trallard/myrepo (fetch)
        origin	https://github.com/trallard/myrepo (push)
        
    Now enter:
    
        git branch -vv
        
    Here we confirm that the local `master` branch has your GitHub master branch (`origin/master`) as upstream remote. Gibberish? Just check that your output looks similar to mine:
    
        master b8e03e3 [origin/master] line added locally

## Verify that your Git is new enough to have a credential helper

In a terminal, do:

    git --version

and verify your version is 1.7.10 or newer. If not, update Git (chapter \@ref(install-git)) or use SSH keys (chapter \@ref(ssh-keys)).
  
## Turn on the credential helper

#### Windows

In the terminal, enter:

    git config --global credential.helper wincred

#### Windows, plan B

If that doesn't seem to work, install an external credential helper.

  * Download the [git-credential-winstore.exe](http://gitcredentialstore.codeplex.com/) application.
  * Run it! It should work if Git is in your `PATH` environment variable. If not, go to the directory where you downloaded the application and run the following:
  
        git-credential-winstore -i "C:\Program Files (x86)\Git\bin\git.exe"
        
        
#### Mac

Find out if the credential helper is already installed. In the terminal, enter:

    git credential-osxkeychain
    
And look for this output:

    usage: git credential-osxkeychain <get|store|erase>

If you don't get this output, it means you need a more recent version of Git, either via command line developer tools or Homebrew. 

Once you've confirmed you have the credential helper, enter:

    git config --global credential.helper osxkeychain

#### Linux

In the terminal, enter:

    git config --global credential.helper 'cache --timeout=10000000'

to store your password for ten million seconds or around 16 weeks, enough for a semester.

### Trigger a username / password challenge

Change a file in your local repo and commit it. Do that however you wish. Here are terminal commands that will work:

    echo "adding a line" >> README.md
    git add -A
    git commit -m "A commit from my local computer"

Now push!

    git push -u origin master

One last time you will be asked for your username and password, which hopefully will be cached.

Now push AGAIN.

    git push
  
You should NOT be asked for your username and password, instead you should see `Everything up-to-date`.
  
Rejoice and close the terminal.
