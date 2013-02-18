1. What is Git?
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

2. Common commands and SVN equivilent
    1. `git clone [repo] [optional destination]` (`svn checkout repo`)
    2. `git pull` (`svn update`)
    3. `git commit -m"Message"` (`svn commit -m"Message"`)

3. Install and Configure Git
    1. Install
        1. OS X
        2. Linux
    2. Configure
        1. What are config vars used for
        2. --global vs --local
    3. Git GUI options

4. SSH Keys
    1. What is SSH Key Authentication?
    2. Generate (OS X & Linux differences)
    3. Adding to repo hosts
    4. Generation options (Passphrase?)

5. Cloning a repo
    1. Best practices
    2. Destination path
        1. Where should I save the working copy?
        2. Apache overview
    3. Examples
        1. Full Folder (Custom module or theme - on github)
        2. Folder contents (Full site - on Assembla)
        3. Command line (Drupal module - from drupal.org)

6. Add existing site/project/app to new repo
    1. Why?
    2. How?

7. Adding Remote Repos (Other than Origin)

8. Merging
    1. Overview of Pull
    2. Fetch vs Pull

