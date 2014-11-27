---
template: article.jade
title: Is ActiveSupport::Concern really Another Concern?
date: 2014-11-26 00:04
author: neeraj
aliases: []
categories: [Ruby, Rails, ActiveSupport, ActiveSupport Concern]
tags: [ruby, rails, active_support]
excerpt: Is ActiveSupport::Concern really Another Concern?
---

In Ruby world, [Rails](guides.rubyonrails.org) is a well known web-framework and [ActiveSupport](https://github.com/rails/rails/tree/master/activesupport) is an integrated component. [ActiveSupport](https://github.com/rails/rails/tree/master/activesupport) is a ruby gem which comes with rails by default. But, we can also use this library or gem separately. [ActiveSupport](https://github.com/rails/rails/tree/master/activesupport) is very easy to understand upto the extend that if you are planning to read the source code of rails, then start with [ActiveSupport](https://github.com/rails/rails/tree/master/activesupport). So, today I am going to throw some light on [ActiveSupport::Concern](https://github.com/rails/rails/blob/master/activesupport/lib/active_support/concern.rb) which is also one of the component of [ActiveSupport](https://github.com/rails/rails/tree/master/activesupport). Is ActiveSupport::Concern really Another Concern?

This post is for those who are new to ActiveSupport. Who constantly do utility work like encoding, or decoding JSON, generating random number, number to currency conversion etc. But never got the chance to find out the reason behind this. Have you ever looked into the source code of any rubygem where you found the statement like ```extend ActiveSupport::Concern```. If yes, are you familiar with this statement? Why do we use this? Trust me [ActiveSupport::Concern](https://github.com/rails/rails/blob/master/activesupport/lib/active_support/concern.rb) is not a concern, but to make your life easy. 

ActiveSupport::Concern is useful in mixin or modules. It is more useful whenever you are building your own rubygem. 

### Without ActiveSupport::Concern
---

We can definitely achieve our goals without ActiveSupport::Concern. But it will insist you to write more code and introduce more complexity in your code base.  

```ruby
# my Foo mixin or module
module Foo
  module ClassMethods
    def class_method
      puts "I am inside bar method"
    end
  end

  module InstanceMethods
    def instance_method
      puts "I am inside foo method"
    end
  end

  def self.included(base)
    base.send :include, InstanceMethods
    base.send :extend, ClassMethods
    puts "FooBar included"
  end
end
```
```ruby
# my Bar class
class Bar
  include Foo
end

Bar.class_method
Bar.new.instance_method
```
Here I created a Foo module and included this module into my Bar class. Whenever ```include Foo``` will execute, it will trigger my overridden method ```self.included(base)``` method in Foo module. base is the reference of the class in which I have included my module. And then Inside ```self.included(base)```, I am manually including and extending my ClassMethods and InstanceMethonds.

But I could say that this is not a good code. Why? Because first, I am over-ridding included method which is not good. Second, it is decreasing the readability of code. People have to struggle with code in order to understand the whole process.

### With ActiveSupport::Concern
Now the same code could be written with the help of ActiveSupport::Concern. Trust me it will reduce the complexity and increase the readability. 
```ruby
# my Foo mixin or module
require 'active_support/concern.rb'

module Foo
  extend ActiveSupport::Concern

  module ClassMethods
    def class_method
      puts "I am inside bar method"
    end
  end

  def instance_method
    puts "I am inside foo method"
  end

  included do
    puts "FooBar included"
  end
end
```
```ruby
# my Bar class
class Bar
  include Foo
end

Bar.class_method
Bar.new.instance_method
```
module ```InstanceMethods``` is also removed because methods inside Foo module will come under ```InstanceMethods``` module by default. You can also notice that ```self.included(base)``` is also changed to included only. Now You can override included method for your other useful purpose.
