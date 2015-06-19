---
layout: post
title: Add Git Branch Name to Terminal Prompt (Mac)
date: "2013-12-22 16:25:06"
comments: true
published: true
---



When in a repository directory, show the name of the currently checked out Git branch in the prompt.


#Requirements
 1. Mac OS X (Lion)
 2. Terminal with Bash



#Method

Open the Terminal app and if the file `~/.bash_profile` does not already exist create it with the following command.



  `touch ~/.bash_profile`




>Harry Cutts points out in the comments that you should be able to use the file `~/.bashrc` instead to make this method applicable to Linux.

  > Open `~/.bash_profile` in your favorite editor and add the following content to the bottom.

```

    # Git branch in prompt.

    parse_git_branch() {

        git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'

    }

    export PS1="\u@\h \W\[\033[32m\]\$(parse_git_branch)\[\033[00m\] $ "
```



>Notes:

>

>* Depending on configuration changes you may have made previously you may already have a `PS1` variable being exported. In this case you will need to add `\$(parse_git_branch)` somewhere in the existing value.

>* The `\[\033[32m\]` parts set the color of the branch text. If you prefer another color check online for a reference on valid values.

>* The `\` in `\$(parse_git_branch)` is important to ensure the function is called each time the prompt is displayed; without it, the displayed branch name would not be updated when, for example, checking out a different branch or moving in and out of a Git repository directory.

>Now when you change to a directory that is within a Git repository, the prompt will be supplemented with the name of the current branch. When you switch branches the prompt will update accordingly.




>If you are using an existing Terminal session, don't forget to make the changes take effect by sourcing the file with the command `source ~/.bash_profile`.





