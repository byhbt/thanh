---
categories:
  - Rails
tags:
  - template
  - skeleton
title: Rails Application Templates
date: 2020-09-22
---


### How do you start a new Rails project?

As the very first guide we do like this:

```bash
rails new blog -d postgresql
```

After that, you add some essentials gems and configuration, init Devise, add user controller...

- Devise: for authentication.
- Fiago: manage env variable.
- Replace Minitest by RSpec.
- etc...

And then a new project comes, will you do the same? I think the answer is `NO` right?

So as Rails philosophy: **"DRY"** and **"Convention Over Configuration"**.

Rails provided us a better way to create new projects, same command as above and also use `-m` argument.


```bash
rails new blog -d postgresql -m template.rb
```

The `template.rb` can be an URL. For example:
```
 -m https://raw.githubusercontent.com/nimblehq/rails-templates/master/template.rb
```

The `template.rb` will automate init new project for us.
