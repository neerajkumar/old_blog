---
template: article.jade
title: Successful Migration from svn to git with history
date: 2014-03-15 12:26
author: neeraj
aliases: ['/post/2007/01/08/code-and-stuff/', '/post/2007/01/08/first/', '/post/2008/01/08/first']
categories: [Ruby, Git, Svn]
tags: [ruby, git, svn]
excerpt: I am a git lover, not svn. But, when I started my programming, svn was so popular and widespread. But the discovery of git pushed the paradigm shift in world of version control.
---
I am a git lover, not svn. But, when I started my programming, svn was so popular and widespread. But the discovery of git pushed the paradigm shift in world of version control. Git is better than svn in lot of senses which could easily be delineated with the fact that most of the open source contributions and repositories are found on <a href='https://github.com'>Github</a>. <a href='https://github.com'>Github</a> could be recognized as the UI interface for git version control. 

The time has changed, but yet there are many projects are still residing on svn. This article will enable you to successfully migrate from your old svn version control to new git version control with the help of few chunck of statements. The astonishing fact is that this will not lose your history too.

Lets say your project name is dummyapp and its residing on svn say `http://svn/Other/RubyOnRails/apps/dummyapp/trunk`.

1). Please go to your local svn copy on your local workstation. 

```
cd PATH_TO_LOCAL_SVN_COPYYY
```
 
2). Create a file say authors-transform.txt which will be having list of all users of current svn dummyapp application.

```
svn log -q | awk -F '|' '/^r/ {sub("^ ", "", $2); sub(" $", "", $2); print $2" = "$2" <"$2">"}' | sort -u > authors-transform.txt
```

This command will fetch all users who have made any commit to dummyapp svn application before, then sort them and store in authors-transform.txt file.

3). Now, install git-svn on your system. 

```
sudo apt-get install git-svn (on linux)
```

I am not sure about windows, but there will be any application to perform this task in windows also (Please help yourself window users).

4). Now, execute the command 

```
git svn clone svn://svn/Other/RubyOnRails/apps/dummyapp/trunk -A authors-transform.txt  ~/temp
```

This command will take relatively longer time as it fetches all history of the svn application. For every commit in SVN, it will generate corresponding git commit ID, assign the commiter name and by default create the master branch. As well as, it will copy the whole application as a git repository on your local in temp directory (as in my case the temp directory is there in my home folder)

5). Once step 4 is completed, go to temp directory (cd ~/temp). Create origin remote and assign it to github path.

```
git remote add origin git@github.com:dummyapp.git
```

6). Push the code to github.

```
git push origin master
```

Thats it !!!

Congratulation !!! Your application has been successfully moved over github with all of its histories and users etc. 
