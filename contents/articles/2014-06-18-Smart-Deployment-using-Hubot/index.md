---
template: article.jade
title: Smart Deployment using Hubot
date: 2014-06-18 12:10
author: neeraj
aliases: ['/post/2007/01/08/code-and-stuff/', '/post/2007/01/08/first/', '/post/2008/01/08/first']
categories: [Ruby, Hubot, Capistrano]
tags: [ruby, rails, hubot, capistrano, basecamp, campfire]
excerpt: Smart and Easy Deployment using Hubot
---
Github is an invariably well-known name in the world of open source. Githut consistently provides a propitious and favorable platforms for any betterment, reform or innovation in the technology which could also discern as an your open source contribution. <a href='https://hubot.github.com'>Hubot</a> is also one of such examples which posses the sovereign potential to metamorphose the process of deployment of rails apps. I came across the term hubot last year and I also heard that numerous enter-prize level organizations benefiting their processes by hubot. My curiosity enabled me up-to the extent that now I am writing a blog :).

<a href='https://hubot.github.com'>Hubot</a> describes its work as an amazing campfire bot who plays the vital role of an user, deployed on heroku, and will add into your campfire chat room as a normal user through heroku URL. However, the heroku app will be revealing as an error page which could cause a confusion in your mind about credibility of the chat bot.

First of all, deploy a dummy app on heroku which could be initiated using this command. 

```
gem install heroku
```
I am assuming here that you are familiar some what with heroku and have created your profile and deployed some app on heroku. If not, please refer https://id.heroku.com/signup or https://devcenter.heroku.com/articles/git.
Then setup the heroku app:

```
heroku create --stack 
git push heroku master 
heroku ps:scale web=1
```

Now, you will need to setup your HEROKU_URL variable. 
```
heroku config:add HEROKU_URL=
```

<h4 style='float: right'>Work in Progress ... </h4><br/><br/>
