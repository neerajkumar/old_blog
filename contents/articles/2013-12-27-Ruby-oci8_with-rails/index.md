---
template: article.jade
title: Ruby-oci8 with Rails
date: 2013-11-29 15:38
author: neeraj
aliases: ['/post/2007/01/08/code-and-stuff/', '/post/2007/01/08/first/', '/post/2008/01/08/first']
categories: [Ruby]
tags: [ruby]
excerpt: Ruby-oci8 is a rubygem which we use to build the connection with oracle DB in your rails application.
---
<a href="http://rubygems.org/gems/ruby-oci8">Ruby-oci8</a> is a rubygem which we use to build the connection with oracle DB in your rails application. But installation of <a href="http://rubygems.org/gems/ruby-oci8">ruby-oci8</a> on linux or mac is not easy, while its a bit tricky. When I started to configure it on my mac first time, it consumed the entire day to google and configure. Therefore, I am trying to explain you the local configuration and installation here. However this post is only for linux and mac, but not for windows.

Hmmmmm, let's start with the basics. I am assuming that you are using rails-3.0+ and ruby-1.8.7+. As we do with Rails-3 applications, We maintain a Gemfile and in order to install gems, we run bundle install. But, if you have specified ruby-oci8 gem, then just wait for few moments. This will be failed unless you have proper configuration.

1). You will need to install and configure <a href="http://www.oracle.com/technetwork/database/features/instant-client/index-097480.html">oracle instant client</a> first. Please download and install <a href="http://www.oracle.com/technetwork/database/features/instant-client/index-097480.html">oracle instant client</a>. The directions for installation is already given, but be careful to choose correct instant client version which could differ according to your machine.

2). Next, you will need to set your environment. Set environment variable LD_LIBRARY_PATH (DYLD_LIBRARY_PATH in case of mac). Just run this command on your terminal or console.

In case of Linux

```
 export LD_LIBRARY_PATH="/usr/lib/oracle/11.2/client64/"
 ```

 In case of Mac

```
 export DYLD_LIBRARY_PATH="/usr/local/oracle/instantclient_11_2"
 ```

 Or, you can add this command in your .bashrc or .zshrc file too and then run the command `source .bashrc` or `source .zshrc`.

 This will set the value of environment variable LD_LIBRARY_PATH as "/usr/lib/oracle/11.2/client64/".

 Also, check that this location does have the client installed. If not, find out where it is and create a soft link to here. ruby-oci8 expects to find it at this location “/usr/lib/oracle/….”.

 3). In this last step, you need to make a symlink. Just run this command

```
sudo ln -s /usr/include/linux/ /usr/include/sysclient
```

In last, it must work after this. If its not working, then just drop a comment here, may be I could help you more.