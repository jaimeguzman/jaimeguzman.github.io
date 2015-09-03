<div id="page">

<div id="main">

<div id="content" class="column" role="main"><a id="main-content"></a>

<hgroup>

# Converting a Subversion repository to Git

## (7 steps to migrate a complete mirror of svn in git)

</hgroup>

<article class="node-210 node node-blog node-promoted view-mode-full clearfix" about="/git/convert-subversion-to-git" typeof="sioc:Item foaf:Document">

<header><span property="dc:title" content="Converting a Subversion repository to Git" class="rdf-meta element-hidden"></span><span property="sioc:num_replies" content="35" datatype="xsd:integer" class="rdf-meta element-hidden"></span>

<span property="dc:date dc:created" content="2010-08-31T21:58:27+08:00" datatype="xsd:dateTime" rel="sioc:has_creator">Submitted by [John](/user/john "View user profile.") on <time pubdate="" datetime="2010-08-31T21:58:27+08:00">Tue, 2010/08/31 - 9:58pm</time></span>

</header>

When I first realized that I needed a version control system, the best system at the time was CVS. (No, really.) Subversion was nearing 1.0, so I waited for its release and then used it everywhere. Well, that was 2003\. Time for a change.

This past year, it became obvious that there were many Git users within the Drupal community, so [Drupal has decided to move to Git](http://drupal.org/community-initiatives/git) . Since then I've started learning and researching the best ways to convert all my development to a Git-based workflow. So far… it rocks.

![svn boxes go into the factory; git ponies come out.](http://john.albin.net/sites/default/files/git-pony-svn-ogre.png)

When getting my toes wet in Git, I started using an extremely useful git command called **git-svn** , which primarily can be used to checkout a Subversion repository to a local Git repo and then push your changes back to the original Subversion repository. That worked great as a stop-gap measure, but now I’m ready to chuck all my svn repos and convert them to Git.

_Supposedly,_ `git-svn` can also be used to convert a Subversion repo to Git. Unfortunately, after reading the [git-svn docs](http://www.kernel.org/pub/software/scm/git/docs/git-svn.html) carefully and several useful resources (like the [slightly-obscure Git FAQ](https://git.wiki.kernel.org/index.php/GitFaq#How_do_I_mirror_a_SVN_repository_to_git.3F) , the [Git Community Book](http://book.git-scm.com/6_scm_migration.html) , [Paul Dowman’s blog](http://pauldowman.com/2008/07/26/how-to-convert-from-subversion-to-git/) and [Alexis Midon’s blog](http://beardedmagnum.com/2009/02/15/converting-git-svn-tag-branches-to-real-tags/) ), it became apparent that all the resources are piecemeal and **nothing gives you the BIG HONKIN’ PICTURE** . So here it is, ponies and all…

## A _complete_ guide to git-svn conversions

Our goal is to do a complete conversion of our Subversion repository and end up with a bare Git repository acceptable for sharing with others (privately or publicly). Bare repositories are ones without a local working checkout of the files available for modifications. They are the recommended format for shared repositories.

### 1\. Retrieve a list of all Subversion committers

Subversion simply lists the username for each commit. Git’s commits have much richer data, but at its simplest, the commit author needs to have a name and email listed. By default the `git-svn` tool will just list the SVN username in both the author and email fields. But with a little bit of work, you can create a list of all SVN users and what their corresponding Git name and emails are. This list can be used by git-svn to transform plain svn usernames into proper Git committers.

From the root of your local Subversion checkout, run this command:

<div class="codeblock">`svn log -q | awk -F '|' '/^r/ {sub("^ ", "", $2); sub(" $", "", $2); print $2" = "$2" <"$2">"}' | sort -u > authors-transform.txt`</div>

That will grab all the log messages, pluck out the usernames, eliminate any duplicate usernames, sort the usernames and place them into a “authors-transform.txt” file. Now edit each line in the file. For example, convert:

`jwilkins = jwilkins <jwilkins>`

into this:

`jwilkins = John Albin Wilkins <johnalbin@example.com>`

### 2\. Clone the Subversion repository using git-svn

<div class="codeblock">`git svn clone [SVN repo URL] --no-metadata -A authors-transform.txt --stdlayout ~/temp`</div>

This will do the standard `git-svn` transformation (using the authors-transform.txt file you created in step 1) and place the git repository in the “~/temp” folder inside your home directory.

### 3\. Convert svn:ignore properties to .gitignore

If your svn repo was using `svn:ignore` properties, you can easily convert this to a `.gitignore` file using:

<div class="codeblock">`cd ~/temp   
git svn show-ignore > .gitignore   
git add .gitignore   
git commit -m 'Convert svn:ignore properties to .gitignore.'`</div>

### 4\. Push repository to a bare git repository

First, create a bare repository and make its default branch match svn’s “trunk” branch name.

<div class="codeblock">`git init --bare ~/new-bare.git   
cd ~/new-bare.git   
git symbolic-ref HEAD refs/heads/trunk`</div>

Then push the temp repository to the new bare repository.

<div class="codeblock">`cd ~/temp   
git remote add bare ~/new-bare.git   
git config remote.bare.push 'refs/remotes/*:refs/heads/*'   
git push bare`</div>

You can now safely delete the ~/temp repository.

### 5\. Rename “trunk” branch to “master”

Your main development branch will be named “trunk” which matches the name it was in Subversion. You’ll want to rename it to Git’s standard “master” branch using:

<div class="codeblock">`cd ~/new-bare.git   
git branch -m trunk master`</div>

### 6\. Clean up branches and tags

`git-svn` makes all of Subversions tags into very-short branches in Git of the form “tags/name”. You’ll want to convert all those branches into actual Git tags using:

<div class="codeblock">`cd ~/new-bare.git   
git for-each-ref --format='%(refname)' refs/heads/tags |   
cut -d / -f 4 |   
while read ref   
do   
  git tag "$ref" "refs/heads/tags/$ref";   
  git branch -D "tags/$ref";   
done`</div>

This step will take a bit of typing. <abbr title="smiling emoticon">:-)</abbr> But, don’t worry; your unix shell will provide a `>` [secondary prompt](http://docstore.mik.ua/orelly/unix/upt/ch09_13.htm) for the extra-long command that starts with `git for-each-ref` .

### 7\. Drink

If you’ve got just the one Subversion repo to convert…Congratulations! You’re done. Go party. Just take your “new-bare.git” folder and [share it](http://book.git-scm.com/4_setting_up_a_private_repository.html) .

If, on the other hand, you’ve got a bunch of Subversion repositories to convert, you’ve got a long, long night in front of you if you want to convert them all by hand. You’re going to need a drink (or several).

Since I had 141 svn repositories that needed to be converted, I wrote a set of wrapper scripts to ease the work… which I’ll discuss in [my next blog post](/git/git-svn-migrate) .

</article>

</div>

</div>

</div>
