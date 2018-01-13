---
template: article.jade
title: Active Resource Changeable
date: 2016-12-06 16:50
author: neeraj
aliases: []
categories: [Ruby, Rails, ActiveRecord, ActiveResource, ActiveResourceChangeable]
tags: [ruby, rails, ActiveRecord, ActiveResource, ActiveResourceChangeable]
excerpt: ActiveRecord and ActiveResource are homogeneous in nature. But ActiveResource is reserved for APIs. ActiveResourceChangeable follows ActiveRecord's pattern and provides the changes of different attributes of ActiveResource object. 
---

As [ActiveRecord](https://github.com/rails/rails/tree/master/activerecord) provides you to get all the changes of your unsaved object, [active_resource_changeable](https://rubygems.org/gems/active_resource_changeable) gem also serves the same purpose by calling its ```changes``` method.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'active_resource_changeable'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install active_resource_changeable

## Usage

```ruby
include ActiveResourceChangeable
``` 
into your model and then you can call changes method on its objects. For example.

```ruby
class User < ActiveResource::Base
  self.site = ""

  include ActiveResourceChangeable

end

user = User.find(1)
user.firstname = "Neeraj1"
user.lastname = "Kumar1"
user.email = "neeraj1.kumar1@mailinator.com"
user.username = "neeraj1"

user.changes = {"firstname"=>["Neeraj", "Neeraj1"], "lastname"=>["Kumar", "Kumar1"], "username=>["neeraj", "neeraj1], "email"=>["neeraj.kumar@mailinator.com", "neeraj1.kumar@mailinator.com"]}
```

[ActiveResourceChangeable](https://rubygems.org/gems/active_resource_changeable) works for nested ActiveResource objects too and gives the result something like 

```ruby
{"companyName"=>["Skipped Levels 034", "ABC Company"],
 "address"=>{""=>{"line1"=>["1234 Skipped Levels 034", "line1"]}, "countryType"=>{"code"=>["DEU", "IND"]}},
 "countryType"=>{""=>{"code"=>["DEU", "IND"]}},
 "statusType"=>{""=>{"code"=>["A", "B"]}}}
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake test` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/neerajkumar/active_resource_changeable. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](http://contributor-covenant.org) code of conduct.
