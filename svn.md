# SVN

## VOCABULARY
* **repository**: a database of version-controlled files and their complete history which is shared by the team which can be cloned, branched and modified
* **trunk**: the directory where all main development typically happens
* **tags**: a directory used to store named snapshots of the project
* **branches**: a separate line of development off of the trunk
* **working copy**: a screenshot of the repository at a given point in time; a private copy of the repository
* **checkout**: create a new working copy
* **committed changes**: when you make a commit, your changes in the private workplace are stored in the central server


## COMMAND LINE STUFF
```
svn info Trunk
svn info --show-item=url



```


## BASICS
* here is an example using a local repository (normally this would be on a server accessible by everyone on your team)
* **create a new repo** (remember, this isn't where the code is -- it's just the db of files and history)
    - Create a repository (in a new dir or in an existing repo dir) with "Create Repository here..."
        * clicking on "Create folder Structure" will create the default folder structure
    - import material by going to that folder and right-clicking on it and selecting "import"
        * import to a URL ending with "<repo>/trunk/<projectname>":
        ```
        // import files from our 'ledger' directory ------------|
        file:///C:/Users/amanda.ryman/sandbox/svn_repos/trunk/ledger
        ```
        * importing by itself does not create a working copy
    - create a working copy of our repo where we can freely make changes
        * make a new directory somewhere on the drive (say, "ledger-dev")
        * right-click on the folder and do "TortoiseSVN -> Checkout..." and enter in the same URL as above
        * the files from the original 'ledger' folder that we imported will then be populated into our new "ledger-dev" folder for us to play around with/commit/reset as needed
* view a **diff**
    - right-click on a modified file and select "Diff..." -- this will bring up a GUI showing changes
* **make a commit**
    - right click on our working copy folder and select "TortoiseSVN -> Commit"
    - enter a sensible log message explaining your changes
* **add new files**: new files will need to be manually added
    - right-click on the folder and do "TortoiseSVN -> Add" to select multiple files or right click on the file itself
* view the **log**
    - you guessed it... right-click on your project folder and select "TortoiseSVN -> Show log"
* **revert changes**:
    - to revert a single file that has uncommitted changes, use "TortoiseSVN -> Revert" to discard your changes
    - to revert your entire project to a previous commit,





### DON'T SHOOT YOURSELF IN THE FOOT
* do not use move -- it gets weird
* **do not commit from anywhere other than top-level** -- unlike git, svn is weirdly specific to your current location
* when something fails and cleanup does too
    - run `sqlite3 .svn/wc.db "select * from work_queue"` inside your working copy dir to see what's "in progress"
    - run `sqlite3 .svn/wc.db "delete from work_queue"` to drop all tasks -- try running 'clean' again
