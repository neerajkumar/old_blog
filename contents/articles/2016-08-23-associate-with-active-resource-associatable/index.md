---
template: article.jade
title: Associate with ActiveResourceAssociatable
date: 2016-08-23 17:28
author: neeraj
aliases: []
categories: [Ruby, Rails, ActiveRecord, ActiveResource, ActiveResourceAssociatable]
tags: [ruby, rails, ActiveRecord, ActiveResource, ActiveResourceAssociatable]
excerpt: ActiveRecord and ActiveResource are homogeneous in nature. But ActiveResource is reserved for APIs. ActiveResourceAssociatable follows ActiveRecord's pattern and provides similar association pattern. However it could be achieved by customized methods in models, but ActiveResourceAssociatable provides smarter way. Because world is expanding with smarter folks and a smart folk adopts smarter way of working... 
---

A communication of a web app with others is not a surprise, but it is the demand of mobile world's era. Because new apps want to maintain a centralized DB which could be shared with other apps very easily and very frequently. Recently, I also worked on a similar distributed web app which provided me an idea to implement a rubygem. 

ActiveResource is a ruby library which acts same as ActiveRecord, but perform operations over API. [ActiveResourceAssociatable](https://rubygems.org/gems/active_resource_associatable) is also a ruby library which consolidates same kind of associations methods as ActiveRecord provides. However it could be achieved by own customized methods, but [ActiveResourceAssociatable](https://rubygems.org/gems/active_resource_associatable) provides smarter way of accessing your ActiveRecord and ActiveResource methods. [ActiveResourceAssociatable](https://rubygems.org/gems/active_resource_associatable) comes with some options too which will make you more easy to use this gem.

# ActiveResourceAssociatable
---
This gem is used to establish the association between your ActiveResource class to ActiveRecord class. Why? Because there is no other ruby library to provide such functionality. Its simple and taking care of all options, scopes etc with your associations.

## Installation
---
Add this line to your application's Gemfile:

```ruby
gem 'active_resource_associatable'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install active_resource_associatable

## Usage
---
```include ActiveResourceAssociatable``` in your ActiveRecord or ActiveResource model. Now you can easily call the methods of either of your ActiveRecord or ActiveResource models. This gem facilitates you to call ActiveResource objects in the same way as you call in any of your ActiveRecord model. It supports calls ```has_one_activeresource```, ```belongs_to_activeresource```, ```has_many_activeresources```, ```has_and_belongs_to_many_activeresources``` and ```has_many_through_activeresources```.

#### Has One ActiveResource
Account is non ActiveResource class which is maintaining has one relationship with ActiveRecord class User. 

```ruby
class User < ActiveResource::Base
  self.site = ""

  include ActiveResourceAssociatable

  belongs_to_activeresource :account
end
```
```ruby
class Account < ActiveRecord::Base
  include ActiveResourceAssociatable

  has_one_activeresource :user, class_name: 'UserResource'
end
```

Whereas Reader is a ActiveResource class, but maintaining has one relationship with Account.

```ruby
class Reader < ActiveResource::Base
  self.site = ""

  include ActiveResourceAssociatable

  has_one_activeresource :account
end
```

```ruby
class Account < ActiveRecord::Base
  include ActiveResourceAssociatable

  belongs_to_activeresource :reader
end
```

#### Belongs To ActiveResource
The same example which explained above in Has One ActiveResource section could be an example of belongs_to_activeresource.
```belongs_to_activeresource``` can also be applied in case of ```has_many_activeresources``` associations.

#### Has Many ActiveResources
User is ActiveResource class which maintains the has many relationships with non ActiveResource class Book.

```ruby
class UserResource < ActiveResource::Base
  self.site = ""

  include ActiveResourceAssociatable

  has_many_activeresources :books
end
```
```ruby
class Book < ActiveRecord::Base
  include ActiveResourceAssociatable

  belongs_to_activeresource :user, class_name: 'UserResource'
end
```
Now you can easily call ```user.books``` which will return the array of objects of non ActiveResource class Book.

```ruby
class Library < ActiveRecord::Base
  include ActiveResourceAssociatable

  has_many_activeresources :readers
end
```
```has_many_activeresources``` also comes with options ```class_name```.

```ruby
class Reader < ActiveResource::Base
  self.site = ""

  include ActiveResourceAssociatable

  has_many_activeresources :studying_materials, class_name: "Book"
end
```
```ruby
class Book < ActiveRecord::Base
  include ActiveResourceAssociatable

  belongs_to_activeresource :reader
end
```
Now you can call ```reader.studying_materials``` which will return the array of objects of non ActiveResource class Book.

User can also call books using a specific foreign key. For Example,
```ruby
class UserResource < ActiveResource::Base
  self.site = ""

  include ActiveResourceAssociatable

  has_many_activeresources :books, foreign_key: "user_id"
end
```

#### Has And Belongs To Many ActiveResources
```active_resource_associatable``` provides many to many associations between ActiveResource and ActiveRecord. 
User class is a ActiveResource class and can have many images. Likewise, Image which is a ActiveRecord class can have many users.

```ruby
class User < ActiveResource::Base
  self.site = ""

  include ActiveResourceAssociatable

  has_and_belongs_to_many_activeresources :images
end
```
```ruby
class Image < ActiveRecord::Base
  include ActiveResourceAssociatable

  has_and_belongs_to_many_activeresources :users
end
```

You can use class_name and foreign_key as in ```has_many_activeresources```.
```ruby
class UserResource < ActiveResource::Base
  self.site = ""

  include ActiveResourceAssociatable

  has_and_belongs_to_many_activeresources :images, foreign_key: "user_id"
end
```
```ruby
class Image < ActiveRecord::Base
  include ActiveResourceAssociatable

  has_and_belongs_to_many_activeresources :users, class_name: 'UserResource'
end
```

#### Has Many Through ActiveResources
```has_many_through_activeresources``` can also be achieved using ```active_resource_associatable```. User can have many friends through friendships. Similarly, a friend can also have many users through friendships.

```ruby
class UserResource < ActiveResource::Base
  self.site = ""

  include ActiveResourceAssociatable

  has_many_through_activeresources :friends, through: :friendships
end
```
```ruby
class Friend < ActiveRecord::Base

  include ActiveResourceAssociatable

  has_many_through_activeresources :users, through: :friendships, class_name: 'UserResource'
end
```
```ruby
class Friendship < ActiveRecord::Base
  include ActiveResourceAssociatable
  
  belongs_to_activeresource :user
  belongs_to :friend
end
```

## Conclusion
---
Every ruby or rails programmer should try this rubygem once. I believe you will really like it. Please don't forget to share your valuable feedback. 
