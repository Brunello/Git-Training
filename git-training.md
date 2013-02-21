#Brunello Git Training#

##What is Git?##

###Git vs Github vs Assembla vs ...###

* Developed by Linus Torvalds, first released in 2005
* Github is a popular git Source Code Host
* Assembla does many things well. Hosting git repos is not one of them.
* Drupal.org hosts it's own git repo for Drupal Core and Contrib Modules

###Centralized vs Decentralized###

* Commits, history, reverts don't require communicating with a centralized
  server (note the difference between committing and pushing)
* Built-in redundancy
* Each working copy is effectively a fork and a repository in it's own
* Git data for a repo is all stored in one top-level directory
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
    
    ----------------------------------------------------------------
    |                   ------WORKING COPY------                   |
    +----------- ------------ ------------ ------------ -----------+
    | Ignored  | |Untracked | | Tracked  | | Tracked  | | Tracked  |
    |          | |          | |Unmodified| | Modified | | Staged   |
    |          | |          | |          | |          | |          |
    |   <===ignore===<      | |          | |          | |          |
    |          | |          | |          | |          | |          |
    |          | |   >================add==================>       |
    |          | |          | |          | |          | |          |
    |          | |          | |    >====edit====>     | |          |
    |          | |          | |          | |          | |          |
    |          | |          | |          | |    >===add====>       |
    |          | |          | |          | |          | |          |
    |          | |          | |    <========commit=========<       |
    |          | |          | |     \    | |          | |          |
    |          | |    <=====rm=====< \   | |          | |          |
    |          | |          | |       \  | |          | |          |
    ------------ ------------ ---------\-- ------------ ------------
                                  ^     \           __________
                                  |      \          |        |
                                  |       \->>PUSH>>| Remote | 
                                  |                 |  repo  |
                                  |                 |        |
                                  ----------<<PULL<<|        |
                                                    ----------

##Git vs SVN Commands##

* `svn checkout [repo]` -> `git clone [repo] [optional destination]`
* `svn update` -> `git pull`
* `svn commit -m"message"` -> `git commit -m"message"`

> Git ignore file is a plain-text file. You can use GUI to ignore files and
> folders, you can edit the .gitignore file directly, or you can use the `rm`
> command.

##Install and configure Git##

###Git Client###

> Good to know: Git won't add an icon to your dock, it's not that sort of
> application - https://help.github.com/articles/set-up-git

**OS X**

* [Download.dmg](http://git-scm.com/download/mac)
* Same as installing any other piece of software (double click .pkg file)

**Linux**

* (Ubuntu/Debian) - `aptitude install git`
* (Fedora/Cent/RHEL) - `yum install git`

###Configure Git###

**Your Computer**
* Set your username - this will be included in all commits

    git config --global user.name "Your Name Here"

> There are global and local Configuration Variables. You can override any 
> global config var by using `--local` instead of `--global` in the 
> configuration declaration. You must run local config commands from within an
> initialized git folder.

* You can view a list of all config values:

    $ git config --list

* Set your email address. Used for the same purposes as username

    $ git config --global user.email "your_email@brunellocreative.com"

* Nice (but optional) color highlighting when using command line

    $ git config --global color.ui always

###Git GUI###

I use [SourceTree](http://www.sourcetreeapp.com/). "What about 
client X or client Y?". Use whatever you want. I use SourceTree.

There are [plenty](http://stackoverflow.com/questions/315911/git-for-beginners-the-definitive-practical-guide/323559#323559) 
of [lists](http://shiningthrough.co.uk/Mac-OS-X-Git-Clients-Roundup) 
[comparing](http://www.techrepublic.com/blog/mac/choosing-the-right-git-gui-client-for-mac-os-x/1080) Git GUIs available.

##SSH Keys##

SVN has support for SSH key authentication, but by default, it stores your
username and password as plain-text - and we have used that method.

Git uses SSH key authentication by default.

> **What is SSH Key Authentication?**
> In short, it's a way for a remote computer to validate the authenticity 
> of a remote host. The result is exactly the same as entering a password.

> **Further reading:** [Public Key Cryptography: Diffie-Hellman Key Exchange](http://www.youtube.com/watch?v=3QnD2c4Xovk)  
> The server's public key is the RSA Fingerprint that you see when connecting
> to a host for the first time.

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
> Also note that in our [typical server setup](https://github.com/Brunello/server-configuration/blob/master/Add-Site.md), 
> each site hosted on a server has a unique user account. So each time you add
> a site to a server, you'll need to generate keys for the account that owns 
> that site.

###Generate (OS X)###

1. Generate the keys and save them in your .ssh folder.

    ```
    $ cd ~/.ssh
    $ ssh-keygen -t rsa
    ```

2. Copy the keys to your clipboard

    ```
    $ cat id_rsa.pub | pbcopy
    ```

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

    ```
    $ xclip -sel clip < ~/.ssh/id_rsa.pub
    ```

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
    which you are generating the keys) they could access your private key file 
    and use it to decrypt data that is encrypted with your public key. If you 
    enter a password here, the ssh agent will require that you also enter the 
    passphrase before it will decode that data.    

> **Further Reading:** [http://www.dribin.org/dave/blog/archives/2007/11/28/ssh_agent_leopard/](http://www.dribin.org/dave/blog/archives/2007/11/28/ssh_agent_leopard/)

##Cloning a repo##

###Best Practices###

* For Drupal, only store the contents of the `/sites` directory in the repo
* Exclude (git ignore):
    * settings.php
    * files directory
    * (settings.php can be included if this is a private repo)
* See [SVN Instructions](http://192.168.15.42/code-snippets) for more information
* Defaults to home directory

###Destination Path###

**Apache Overview**

* Apache listens (usually on port 80) for incoming http requests.
* When a request is received, Apache notes what domain was used to make the
  initial request and scans it's configuration files for a match. (Note that 
  Apache can also serve different directories based on IP Address if the 
  machine has more than one IP)
* If a declaration is found (usually in the form of a vhost) Apache serves the
  request from the corresponding directory found in the configuration file.
* If no declaration is found, Apache serves it's default directory or, if no
  default is defined, the first vhost alphabetically.

**Directory**

Based on our understanding of Apache, we can determine where to save the
locally cloned working copy.

On my computer, MAMP's root directory is ~/Sites/Local. So I usually untar
Drupal core there and rename it something like `adam.domain.com`. Then, to
finish setting up the site files (Assuming the repo contains the contents of 
the /sites directory):

1. Delete the default contents of the `/sites` directory.
2. Checkout the contents of the repo (either via command line using `.` to 
   direct Git to not include the parent Directory, or via SourceTree taking care
   to rename the parent directory after previously deleting `/sites`.
3. Create a new version of settings.php or edit the existing.
4. Create new files directory or symlink to files directory.
5. Everything else (DB create/export/import, symlinks, truncate users table,
   etc)

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

    ```
    # Define the location of the repo
    $ git remote add origin https://github.com/Brunello/Git-Training.git

    # Add all of the files to the local repo
    $ git add .

    # Commit all of the files to the local repo
    $ git commit -m 'initial commit'

    # Push the files to the repo
    $ git push origin master
    ```

In most cases, you can run the above commands from the prod server to be 
guaranteed that your repo starts off with the latest code from production.
Then you can use SourceTree to clone a local copy for dev and.

##Adding Remote Repos##

It's possible for each distributed working copy to list other working copies as
remote repos. Since Git is distributed and not centralized, even the origin 
repo is technically a working copy and isn't any different than any other 
working copy.

**Workflow**

1. Alice clones ProjectX from Origin
2. Bob clones ProjectX from Origin
3. Alice adds Bob's working copy as Remote Repo
4. Bob makes a change and commits it, but does not push to Origin
5. Alice runs `git pull --all`
6. Alice now sees Bob's unpushed commit in her working copy

##Merging##

* Pull overwrites local Tracked Modified files. You must commit your changes 
  first.
* Once you commit, pulling will automatically merge (if possible) and add an
  additional commit to push.

###Fetch vs Pull###

Simply put, a `git pull` runs a `git fetch` followed by a `git pull`.
SourceTree periodically runs `git fetch` but don't rely on it. Before starting
work on a working copy, it's always wise to run a `git fetch` first and `git
pull` if neccessary.

`git pull` is comprised of `fetch + merge`.

> In SourceTree, if a `git fetch` returns any changes, you will see a red
> circle over the "Pull" icon with the number of commits that are available.

##Misc Advantages##

###Features Updates###

**Features background**
Features module allows you to store configuration in flat files; e.g. a view.

The features admin page compares the configuration stored in the database with
what is defined in the file system. If it finds a discrepancy, the feature is
marked as overriden. From here, you can _Revert_ (move the configuration 
defined in the files to the database) or _Recreate_ create new files to match
the current db configuration.

**Benefit**
Since all repo info is stored in one, single, top level folder, you can delete
entire folders from your repo and recreate them with different content without
causing a conflict.

The diff window also allows you to see exactly what has changed in the case of
a features module.
