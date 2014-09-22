---
template: article.jade
title: How To Wire up Ruby on Rails and Ampersand.js As a Single Page Application
date: 2014-09-21 19:36
author: neeraj
aliases: []
categories: [Ruby, Rails, Ampersand.js, Single Page Application, SPA]
tags: [ruby, rails, ampersand.js, single page application]
excerpt: How To Wire up Ruby on Rails and Ampersand.js As a Single Page Application
---

[Ampersand.js](http://ampersandjs.com) is a latest web framework like any other javascript framework e.g. [Angularjs](https://angularjs.org/). By definition, 
> [Ampersand.js](http://ampersandjs.com) - A highly modular, loosely coupled, non-frameworky framework for building advanced JavaScript apps.  

This definition is correct. Ampersandjs is really a non-frameworky framework. Now question comes, <b>Why Ampersand.js?</b>. Because we love [Backbone.js](http://backbonejs.org/), and [Ampersand.js](http://ampersandjs.com) is written on the top of [Backbone.js](http://backbonejs.org/). For more details, please refer http://ampersandjs.com.  

In this article, I am explaining you to build a simple single page application (not simple as much as any hello world or todo app :)). The application will be wired-up with a Rails application and will fetch all its data from rails generated APIs. In fact, behind the scene, Rails application is providing the APIs directly from https://d1zgsxlgpxt59q.cloudfront.net/exchange/betting/rest/v1/en/navigation/lhm.json. Let's start from scratch. 

### Basic Installation
---
Install the ampersand command line by running this in a terminal:

```javascript
npm install ampersand -g
```
then go into rails project directory and run the command
```javascript
npm init 
```
this will create package.json file in your rails root directory. Now

```javascript
npm install browserify --save-dev

npm install underscore --save 
```
will automatically add the dependencies into package.json file. The package.json file will look like
```json
{
  "name": "betfair_nav_demo",
  "version": "0.0.0",
  "description": "Rails App Betfair Nav Demo with Ampersand",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Neeraj Kumar",
  "license": "ISC"
}
```
here app.js file will be responsible to enable your ampersand.js app.

### Framework Structure & Work Flow
---
Now, just have a look at app.js file now.
```javascript
var _ = require('underscore');
var Router = require('./router');
//var tracking = require('./helpers/metrics'); global mixpanel
var MainView = require('./views/main');
var Me = require('./models/me');
var BetfairFactory = require('./models/betfair_factory');
var domReady = require('domready');

module.exports = {
    // this is the the whole app initter
    blastoff: function () {
        var self = window.app = this;

        // create our global 'me' object and an empty collection for our betfair_factory models.
        window.me = new Me();
        this.betfair_roots = new BetfairFactory();

        // init our URL handlers and the history tracker
        this.router = new Router();

        // wait for document ready to render our main view
        // this ensures the document has a body, etc.
        domReady(function () {
            // init our main view
            var mainView = self.view = new MainView({
                model: me,
                el: document.body
            });

            // ...and render it
            mainView.render();

            // listen for new pages from the router
            self.router.on('newPage', mainView.setPage, mainView);

            // we have what we need, we can now start our router and show the appropriate page
            self.router.history.start({pushState: true, root: '/'});
        });
    },

    // This is how you navigate around the app.
    // this gets called by a global click handler that handles
    // all the <a> tags in the app.
    // it expects a url without a leading slash.
    // for example: "costello/settings".
    navigate: function (page) {
        var url = (page.charAt(0) === '/') ? page.slice(1) : page;
        this.router.history.navigate(url, {trigger: true});
    }
};

// run it
module.exports.blastoff();
```
We also maintain a router which will responsible for navigation to different URLs.
```javascript
var Router = require('ampersand-router');
var HomePage = require('./pages/home');
var CollectionDemo = require('./pages/collection-demo');
var EventsCollectionView = require('./pages/events-collection')
var EventTypesCollectionView = require('./pages/event-types-collection')

module.exports = Router.extend({
    routes: {
        '': 'home',
        'collections': 'collectionDemo',
        'event_types/:id': 'EventTypesCollectionView',
        'events/:id': 'EventsCollectionView',
        '(*path)': 'catchAll'
    },
    // ------- ROUTE HANDLERS ---------
    home: function () {
        this.trigger('page', new HomePage({
            model: me
        }));
    },
});
```
According to router, the homepage is residing in ```pages/home```. The home file will be 
```javascript
var PageView = require('./base');
var templates = require('../templates');

module.exports = PageView.extend({
    pageTitle: 'home',
    template: templates.pages.home
});
```
But in our app, we are displaying the collection view on homepage. 
```javascript
    home: function () {
        this.trigger('page', new CollectionDemo({
            model: me,
            collection: app.betfair_roots
        }));
    },
```
Here app.betfair_roots is mapping to a model and will also be responsible to fetch the json data through ajax call. It will actually call the file ```/models/betfair_factory.js``` file.
```javascript
var Collection = require('ampersand-rest-collection');
var BetfairRoot = require('./betfair_root');

module.exports = Collection.extend({
    mainIndex: 'id',
    model: BetfairRoot,
    url: '/api/betfair_roots'
});
```
Now, we have come to our homepage which is also a collection-demo page and rendering the collection of root nodes. The file will be residing in ```./pages/collection-demo.js``` file. 
```javascript
var PageView = require('./base');
var templates = require('../templates');
var BetfairRootView = require('../views/betfair_root');

module.exports = PageView.extend({
    pageTitle: 'Event Types',
    template: templates.pages.collectionDemo,
    render: function () {
        this.renderWithTemplate();
        this.renderCollection(this.collection, BetfairRootView, this.getByRole('betfair-roots-list'));
        if (!this.collection.length) {
            this.fetchCollection();
        }
    },
    fetchCollection: function () {
        this.collection.fetch();
        return false;
    }
});
```
Here
1. pageTitle which is referring to page title. 
2. template: it is referring to the template where all collection will be displayed. 
3. render: it represents the function. The statement ```this.renderCollection(this.collection, BetfairRootView, this.getByRole('betfair-roots-list'));``` is explaining that it will render all the collection nodes in terms of BetfairRootView objects and place will be the div element with role 'betfair-roots-list'.
4. ```this.fetchCollection()``` statement is for triggering the ajax call again if collection data is not present. 

Now lets have a look at templates file where it will be rendering the collection data. 
```javascript
// pages/collectionDemo.jade compiled template
templatizer["pages"]["collectionDemo"] = function tmpl_pages_collectionDemo() {
  return '<section class="page pageOne"><ul role="betfair-roots-list" class="list-group"></ul></section>';
};
```
### Conclusion 
---
That's it. This is the basic structure and work-flow of any basic ampersand.js application with rails. The source code is residing on my github repository () and demo is available on . With the help of source code, you can get to know more about the links and navigation to different pages of the application. You can write your own business logic and bind it with your Ampersand.js application. Lot more can be done.
Thanks for reading my blog. :)
