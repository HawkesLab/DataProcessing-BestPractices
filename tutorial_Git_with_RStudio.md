How to use git and GitHub with RStudio
================
Marissa Lee
12/18/2018

There can be headaches involved in getting git, GitHub, and RStudio to all play nice together. Here's a tutorial for getting started. Hopefully this tutorial empowers more folks to give it a try.

Getting started
---------------

-   RStudio <https://www.rstudio.com/products/rstudio/#Desktop>: Even if you already have this downloaded to your computer, it's a good idea to update if you haven't done so in a while.
-   GitHub <https://github.com>: Make sure you have an account.
-   Terminal: Make sure you can access your Terminal (Mac users) or something like a Terminal that allows for BASH emulation (PC users)
    -   Mac users -- You should already have Terminal loaded on your computer. Type "Terminal" in spotlight search to find and open it up. Alternatively you can access it by going to `Applications/Utilities/Terminal`
    -   PC users -- The easiest thing to do is download "git for Windows" (<http://gitforwindows.org>). This will set you up with BASH emulation software ("git BASH") that is largely equivalent to the Mac Terminal. You will also get "git" software in this installation. More about "git" later. Atlassian's site has step-by-step instructions for installing this <https://www.atlassian.com/git/tutorials/install-git#windows>.

Check that you have git installed
---------------------------------

Open a Terminal window and simply type this.

``` bash
git
```

If that command returned a big block of text starting with... `usage: git [--version] [--help] [-C <path>] [-c <name>=<value>] [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path] [-p | --paginate | --no-pager] [--no-replace-objects] [--bare] [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]<command> [<args>]`

Then, woohoo you have git! If you do not have git installed and you are working on a Mac OS, you may need to install (or re-install) Xcode (<https://developer.apple.com/xcode/>).

Let's take a sec to appreciate that "git" is very different from "GitHub".

-   *git*: A piece of software that your computer uses to keep track of changes within a given folder. More specifically, this software will "watch" files folders that you choose and keep a detailed log of what those changes are and when they happened.
-   *GitHub*: A website that hosts git-related data.

Clone a GitHub repository using the Terminal
--------------------------------------------

In the Terminal, navigate to a folder that you would like to clone the repository into. The bash command `cd` means "change directory". For example, I want to navigate to my Desktop, so I'll enter this. If you need some tips on how to navigate folders on your computer using bash, I like this resource: <http://jonibologna.com/command-line-primer-primer/>

``` bash
cd Desktop/
```

Use your internet browser to find a GitHub repo that you'd like to clone. For this example, let's clone the repo where this tutorial is located (<https://github.com/marissalee/DataProcessing-BestPractices>). Go to the green "Clone or download" button to see the repo's online address (e.g. <https://>...). Copy the address and enter this in to your Terminal window.

``` bash
git clone https://github.com/marissalee/DataProcessing-BestPractices.git
```

You will be prompted to enter your GitHub username and password. Assuming you typed your password in correctly :), a folder with the repository's name (e.g. `DataProcessing-BestPractices`) should have been created with git tracking in that folder. Check to make sure that the folder of files have downloaded to the location you specified. You can also check to make sure that git tracking is active in that folder by navigating *within* your newly-cloned folder and running this command...

``` bash
git status
```

If git is working, you should see something like this... `On branch master Your branch is up to date with 'origin/master'. nothing to commit, working tree clean`

Congrats! You have successfully "cloned" a git repository from the cloud to your local computer.

Connect RStudio to git and GitHub
---------------------------------

Open RStudio. Find the "Projects" button in the top righthand corner of the RStudio window and select "New Project...". You can also get here by going to File -&gt; New Project.

Select "Existing Directory" and identify the cloned repository folder on your computer. For example, my path is `~/Desktop/DataProcessing-BestPractices`. Click "Create Project".

Now, check to see if git is hooked up correctly. If it is you should see a new tab called "Git" that is sitting next to "Environment" and "History" in your RStudio window. Hopefully you are one of the lucky ones that this works for perfectly.

If you aren't so lucky... RStudio may not be able to find your computer's git software. To check if this is the case, you'll need to know where your git application is on your computer. Open up a Terminal window and type...

``` bash
which git
```

This command will return the location of git on your computer. Now, you'll need to see if this matches where RStudio thinks git is. Open RStudio and go to Tools -&gt; Global Options. Click on the Git/SVN tab. You should see a field that says "Git executable". Replace this text with the correct path and hopefully you are good to go. Thanks to Santiago and this site (<https://github.com/jennybc/happy-git-with-r/issues/8>) for figuring this out.

Version control within RStudio using git and GitHub
---------------------------------------------------

Ok. Now you're probably thinking - what do I do with the stuff in this new "Git" tab. Fortunately it's not too complicated. The "Git" tab keep a running tally of all the files that you have modified since your last commit. If you just cloned the repository you may not have made any changes so there is nothing yet to commit.

### Add a file

For practice, open the cloned repository and add a new R file in the "testing" folder.

-   Open the RStudio project in the cloned repository by either (a) double clicking on the ".Proj" file from your computer's file directory or (b) open RStudio -&gt; File -&gt; Open Project -&gt; navigate to the .Proj file on your computer
-   Create a new R script by going to File -&gt; New File -&gt; R Script
-   Add text, e.g. `# here's my new R script!`
-   Save it in the "testing folder" with a unique name, e.g. "marissas\_script.R"

As soon as you save this new file, your git software takes notice because something has changed in this repository/project folder. Check out the "Git" window in RStudio. You should see your new file listed with a green "A" for "added". Later on when you modify existing files, you will see that file name with a blue "M" for "modified" next to it.

Kind of an aside, but something good to notice at this point if you haven't already. Because you are working in an RStudio "Project", the cloned repository folder is your working directory by default. If you don't believe me, type `getwd()` into your R console. This means that the "Files" tab in your RStudio window gets set to that folder. You can click on folders and files from with RStudio "Files" to quickly open a file in your working directory.

### Make a commit

Click on the button "Commit" within RStudio's "Git" window. It will open an new window. Click the checkbox next to the file that you just added to "stage" it for this commit. When you highlight each file to "stage" it, you'll be able to see line-by-line changes which is particularly helpful for when you are modifying an existing script.

Don't forget to write a commit message in the text box. You'll want to describe the change you are about to save (and you actually can't make a commit without a message by design). Once you've got your message, e.g. "adds my test script", click the "Commit" button. This will open a message box that you can close.

### Push to GitHub

Now if you look closely at the commit window (it says "RStudio: Review Changes") you will see a new banner that has a blue "i" and "Your branch is ahead or 'origin/master' by 1 commit". This means that your local computer's version of this repository differs from the GitHub version by 1 commit. In order to push your changes up to GitHub, you need to click the green arrow "Push" in the top right corner of that window.

Woohoo! You have made a change, committed it, and pushed it up to GitHub in the cloud. Now's the time to open up your browser, go to GitHub, and admire your fine work.

### Pull from GitHub

Did someone add cool code to the GitHub repository that you want to play with? Go ahead and "pull" down all new updates by clicking the blue "Pull" arrow.

Once the pull is complete, you should see new scripts from your collaborators in the testing folder. Check it out.

### Modify an existing file

To practice modifying an existing file, try adding a message to someone else's script. For example, I am going to open up `marissas_script.R` and add `# ...but it doesn't do anything yet.`

-   Save the modified file
-   Check that there is a blue "M" for "modified" next to this filename in your "Git" window of RStudio
-   Stage the change by checking the "staged" box next to the modified file
-   Click "Commit"
-   Write an appropriate commit message and "Commit"
-   "Push" that commit to the cloud for all to see by clicking the green "Push" arrow

Remember that the purpose of git and GitHub is to do version control, so even if you change something on GitHub it can easily be reversed by making a change, another commit, and push.

### Branching

Git branching is a topic for another day. I tend to work linearly (i.e. update the master branch directly) and avoid this issue altogether most of the time. An understanding of how git does branching is useful if you are collaborating with a lot of people and are worried about having the master branch on GitHub function for users.

### More resources

If you want to learn more, I'd recommend checking out Atlassian's resources. Atlassian hosts "Bitbucket", which is an alternative online host for git data (i.e. a GitHub competitor). More people use GitHub, but I think Atlassian does a better job of explaining how git works.

Cheat sheet for using git from the command line: <https://www.atlassian.com/git/tutorials/atlassian-git-cheatsheet>

Description of how to "undo" things in git: <https://www.atlassian.com/git/tutorials/undoing-changes>

How to get a free premium GitHub account with a ".edu" email <https://help.github.com/articles/applying-for-a-student-developer-pack/>
