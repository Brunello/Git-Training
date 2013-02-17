Date: 2013-2-16
Title: Git Training

##What is Git?##

###Git vs Github vs Assembla vs ...###

* Developed by Linus Torvalds, first released in 2005
* Github is a popular git Source Code Host
* Assembla does many things well. Hosting git repos is not one of them.
* Drupal.org hosts it's own git repo for Drupal Core and Contrib Modules

###Centralized vs Decentralized###

* Committs, history, reverts don't require communicating with a centralized
  server (note the difference between committing and pushing)
* Built-in redundancy
* Each working copy is effectively a fork
* We have all been using SVN for years. If any of these features seem confusing 
  remember that you don't need to use all of them to use Git.

###Workflow (vs Centralized)

**SVN**

1. Add
2. Commit

**Git**

1. Add
2. Commit
3. Push

> Remember, your working copy is a full repo too. The second step commits your 
> changes to your local repo. Push pushes them to the shared repo (on Github, 
> Assembla, Drupal.org, wherever)

**Workflow Diagram**
    ----------------------------------------------------------------            --------
    |                   ------WORKING COPY------                   |            | REPO |
    +----------- ------------ ------------ ------------ -----------+            |      |
    | Ignored  | |Untracked | | Tracked  | | Tracked  | | Tracked  |            |      |
    |          | |          | |Unmodified| | Modified | | Staged   |            |      |
    |          | |          | |          | |          | |          |            |      |
    |   <===ignore===<      | |          | |          | |          |            |      |
    |          | |          | |          | |          | |          |            |      |
    |          | |   >===============add===================>       |  >>PUSH>>  |      |
    |          | |          | |          | |          | |          |            |      |
    |          | |          | |    >====edit====>     | |          |  <<PULL<<  |      |
    |          | |          | |          | |          | |          |            |      |
    |          | |          | |          | |    >===add====>       |            |      |
    |          | |          | |          | |          | |          |            |      |
    |          | |          | |    <========commit=========<       |            |      |
    |          | |          | |          | |          | |          |            |      |
    |          | |    <=====rm=====<     | |          | |          |            |      |
    |          | |          | |          | |          | |          |            |      |
    ------------ ------------ ------------ ------------ ------------            --------



##Git vs SVN Commands##

* `svn update` -> `git pull`
* `svn commit -m"message"` -> `git commit -m"message"`

##Install and configure Git##

###Git Client###

> Good to know: Git won't add an icon to your dock, it's not that sort of
> application - [https://help.github.com/articles/set-up-git][https://help.github.com/articles/set-up-git]

**OS X**

* [Download.dmg][http://git-scm.com/download/mac]
* Same as installing any other piece of software (double click .pkg file)

**Linux**

* (Ubuntu/Debian) - `apt-get install git`
* (Fedora/Cent/RHEL) - `yum install git`

###Configure Git###

**Your Computer**
* Set your username - this will be included in all commits
    
    git config --global user.name "Your Name Here"

> There are global and local Configuration Variables. You can override any 
> global config var by ising `--local` instead of `--global` in the 
> configuration declaration. You must run local config commands from within an
> initialized git folder.

> You can view a list of all config values:

    $ git config --list

* Set your email address. Used for the same purposes as username

    $ git config --global user.email "your_email@brunellocreative.com"

* Nice (but optional) color highlighting when using command line

    $ git config --global color.ui always

###Git GUI###

I use [SourceTree][http://www.sourcetreeapp.com/]. Don't ask me "What about 
client X or client Y?". Use whatever you want. I use SourceTree.

There are [plenty][http://stackoverflow.com/questions/315911/git-for-beginners-the-definitive-practical-guide/323559#323559] 
of [lists][http://shiningthrough.co.uk/Mac-OS-X-Git-Clients-Roundup] 
[comparing][http://www.techrepublic.com/blog/mac/choosing-the-right-git-gui-client-for-mac-os-x/1080] Git GUIs available.

##SSH Keys##

SVN has support for SSH key authentication, but by default, it stores your
username and password as plaintext - and we have used that method.

Git uses SSH key authentication by default.

> **What is SSH Key Authentication?**
> In short, it's a way for a remote computer to validate the authenticity 
> of a remote host. The result is exactly the same as entering a password.

> **Further reading:** [Public Key Cryptography: Diffie-Hellman Key Exchange][http://www.youtube.com/watch?v=3QnD2c4Xovk]  
> The "common secret" is the RSA Fingerprint that you see when connecting to 
> a host for the first time.

> Keys are stored in your user account directory in a hidden folder named 
> `.ssh`. You'll also notice that this is where the `known_hosts` file is stored.
> 
> Each Key has a public and private file. You'll need to upload the contents of
> the public file to the site that hosts the repo so it can verify that you are
> who you say you are when you connect to that server with your computer.
> 
> Keys are unique to each user account on each computer. You'll need to 
> generate and upload a new public key file for each computer and each user 
> account that will push to or pull from the repo. This includes the dev and 
> production servers servers and any personal computer you might use.
> 
> Also note that in our [typical server setup][https://github.com/Brunello/server-configuration/blob/master/Add-Site.md], 
> each site hosted on a server has a unique user account. So each time you add
> a site to a server, you'll need to generate keys for the account that owns 
> that site.

###Generate (OS X)###

1. Generate the keys and save them in your .ssh folder.

    $ cd ~/.ssh
    $ ssh-keygen -t rsa

2. Copy the keys to your clipboard

    $ cat id_rsa.pub | pbcopy

3. Upload (paste) your key into the repo provider.
    * Assembla URL: https://www.assembla.com/user/edit/edit_git_settings
    * Github URL: https://github.com/settings/ssh

> Assuming that you always login to your work computer with the same account,
> you can use the same keys to authenticate for all projects on all repo
> providers. From your .ssh directory, just run the second command above to 
> copy your public key to your clipboard.

###Generate (Linux)###

The only difference between OS X and Linux is that Linux doesn't have native
support for copying to your clipboard. You have two choices:

* Install xclip (`sudo aptitude install xclip`) and run the following command:

    $ xclip -sel clip < ~/.ssh/id_rsa.pub

* Connect to the server using a gui (such as Transmit), open the file in a
  plain-text editor and copy the contents from there.

> The `.ssh` directory already exists (in most cases) on OS X because that's
> where the `known_hosts` file is stored. It's likely that it won't exist the
> first time you run this command in Linux (and as a result, Step 1 above will
> fail. That's not an issue. The second part of Step 1 will create the 
> directory automatically for you.

###What about the questions I am prompted with after entering `ssh-keygen`? (Specifically, do I need to enter a passphrase)###

1. `Enter file in which to save the key`  
    You can safely (and should) stick with the defaults here. Just press enter.

2. `Enter passphrase (empty for no passphrase)`  
    If an attacker were to gain control over your machine (or the machine on 
    which you are generating the keys) they could easily read your rsa_pub file
    and use that information as the start of an escalation attack. If you
    use a passphrase during creation, an attacker would not be able to read
    your key files without also gaining access to the passphrase.

> **Further Reading:** [http://www.dribin.org/dave/blog/archives/2007/11/28/ssh_agent_leopard/][http://www.dribin.org/dave/blog/archives/2007/11/28/ssh_agent_leopard/]

##Cloning a repo##

### Examples###

**Folder**  
*publishcontent module (github)*

**Folder Contents**  
*Linkwell Sites folder (Assembla)*

**Command Line**  
*Drupal Module*

##Add existing site/project to new repo##

> This method is useful if you have an existing project that, for whatever
> reason, isn't currently under source control.

1. Define the location of the reop
2. Add all of the existing files
3. Commit all added files
4. Push the commit

    # Define the location of the repo
    $ git remote add origin https://github.com/Brunello/Git-Training.git

    # Add all of the files to the local repo
    $ git add .

    # Commit all of the files to the local repo
    $ git commit -m 'initial commit'

    # Push the files to the repo
    $ git push origin master

In most cases, you can run the above commands from the prod server to be 
guarunteed that your repo starts off with the latest code from production.
Then you can use SourceTree to clone a local copy for dev and.
