A. What is Git?
    1. Git, vs Github, vs Assembla vs...
    2. Centralized vs Decentralized
    3. Workflow Diagram

```
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
```

B. Common commands and SVN equivilent
    1. `git clone [repo] [optional destination]` (`svn checkout repo`)
    2. `git pull` (`svn update`)
    3. `git commit -m"Message"` (`svn commit -m"Message"`)

C. Install and Configure Git
    1. Install
        a. OS X
        b. Linux
    2. Configure
        a. What are config vars used for
        b. --global vs --local
    3. Git GUI options

D. SSH Keys
    1. What is SSH Key Authentication?
    2. Generate (OS X & Linux differences)
    3. Adding to repo hosts
    4. Generation options (Passphrase?)

E. Cloning a repo
    1. Best practices
    2. Destination path
        a. Where should I save the working copy?
        b. Apache overview
    3. Examples
        a. Full Folder (Custom module or theme - on github)
        b. Folder contents (Full site - on Assembla)
        c. Command line (Drupal module - from drupal.org)

F. Add existing site/project/app to new repo
    1. Why?
    2. How?

G. Adding Remote Repos (Other than Origin)

H. Merging
    1. Overview of Pull
    2. Fetch vs Pull

