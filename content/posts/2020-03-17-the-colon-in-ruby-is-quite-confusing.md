---
categories:
  - Ruby
tags:
  - learn
  - colon
title: The colon in Ruby is quite confusing
date: 2020-03-17
url: /posts/the-colon-in-ruby-is-quite-confusing/
---


The position of the colon in Ruby code confused me for a few days.
## 1. Before a text (:foo)

```ruby
:hello
```

In route definition
```ruby
redirect_to :action => "edit", :id => params[:id]
```
**redirect_to** method

In the view

```ruby
  <%= link_to "Delete", article, confirm: "Are you sure?", method: :delete %>
```

The **method: :delete** is quite confusing.
It's actually **{method: :delete}**

**:delete** is not change in the application. so we can use *:symbol* here.
But why not use **constant**?

> symbols are values, constants are not. (Constants are references to values). Symbols evaluate to themselves, constants evaluate to whatever value they reference.

From: [https://stackoverflow.com/questions/47047954/are-ruby-symbols-the-equivalent-of-php-constants](https://stackoverflow.com/questions/47047954/are-ruby-symbols-the-equivalent-of-php-constants)

Check the [API docs](https://api.rubyonrails.org/) of *link_to* method

```ruby
def link_to(name = nil, options = nil, html_options = nil, &block)
  html_options, options, name = options, name, block if block_given?
  options ||= {}

  html_options = convert_options_to_data_attributes(options, html_options)

  url = url_for(options)
  html_options["href"] ||= url

  content_tag("a", name || url, html_options, &block)
end
```

so the **confirm** and **method** is the **html_options**


This method accepts 2 params
- action
- id


## 2. After (foo:)

### Keyword arguments

```ruby
hello:
```

```ruby
def calculate_total(subtotal:, tax:, discount:)
  subtotal + tax - discount
end
```
**subtotal**, **tax**, **discount** are required and allow switch order of arguments.

```ruby
calculate_total(subtotal: 100, discount: 5, tax: 10)
```
### Hash key

if we want a string is a hash key then must *arrow/rocket syntax*

```ruby
# this will work
my_hash = {
  "cool" => :thing
}
#=> {"cool" => :thing }

# this will not work
my_hash = {
  "cool": :thing
}
#=> { :cool => :thing }
# converts the string "cool" to a symbol
```

```ruby
# in a hash:
my_hash = {
  :cool => :symbol
}
# is the same as...
my_hash = {
  cool: :symbol
}
```

Reference:
- [https://thoughtbot.com/blog/ruby-2-keyword-arguments#keyword-arguments-vs-options-hash](https://thoughtbot.com/blog/ruby-2-keyword-arguments#keyword-arguments-vs-options-hash)

### 3. Double colon
- [https://stackoverflow.com/questions/3009477/what-is-rubys-double-colon](https://stackoverflow.com/questions/3009477/what-is-rubys-double-colon)


