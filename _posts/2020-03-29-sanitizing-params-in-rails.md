---
layout: post
title: "Sanitizing params in Rails"
categories:
  - Rails
tags:
  - params
  - sanitizing
---

## The problem:
Post data to api
```json
{
  "data": {
    "type": "entry",
    "attributes": {
      "title": "   Chassidy Kozey re    ",
      "content": "     \n  test n tag     and spaces \n another    "
    }
  }
}
```
### Requirement:

- The spaces and the whitespaces should be removed from the leading and trailing.
- Keep the spaces in between only.

*what is the whitespaces?*
- null, horizontal tab, line feed, vertical tab, form feed, carriage return, space.

## Rspec

```ruby
describe 'POST /api/v1/entry' do
  context 'create new entry with spaces' do
    it 'sanitize entry params' do
      entry_params = Fabricate.to_params(:entry,
                                           title: "Lorem ispsum   ",
                                           content: "   \nconsectetur   adipiscing elit\n")
      post api_v1_entry_path, params: json_api_request_body(entry_params)

      entry = Entry.first
      expect(entry.title).to eq 'Lorem ispsum'
      expect(entry.content).to eq 'consectetur   adipiscing elit'
    end
  end
end
```

In the rspec if i use *single quote* instead of *double quote* then i will got this error.
it mean the string are being escaped while running the test.

So here what is the **different** between **single quote** vs **double quotes** in Ruby?
- Double-quoted easier to read, hard to type
- Double-quoted strings support interpolation.

[https://stackoverflow.com/questions/279270/which-style-of-ruby-string-quoting-do-you-favour](https://stackoverflow.com/questions/279270/which-style-of-ruby-string-quoting-do-you-favour)
[https://blog.appsignal.com/2016/12/21/ruby-magic-escaping-in-ruby.html
](https://blog.appsignal.com/2016/12/21/ruby-magic-escaping-in-ruby.html
)


```bash
     expected: "consectetur   adipiscing elit"
            got: "   \\nconsectetur   adipiscing elit\\n"

       (compared using ==)
```
Then let try to experiment it in **irb**

```bash
irb
2.4.0 :001 > '\n'.to_s
 => "\\n"
2.4.0 :002 > "\n".to_s
 => "\n"
2.4.0 :003 >
```

When use *single quote* the `\n` will be escape, adding one more forward slash
before.


## Implement
```ruby
def entry_params
  permitted_attributes = %i(title content)

  params = deserialized_params(require: [], permit: permitted_attributes)
  params[:title].strip!
  params[:content]).strip!

  params
end
```

Not stop at this step, let use IRB to play with the string.

```bash
2.4.0 :015 > foo = "Lorem \n dolor sit amet a"&.strip!&.delete("\n")
 => nil
2.4.0 :016 > foo = "Lorem \n dolor sit amet a  "&.strip!&.delete("\n")
 => "Lorem  dolor sit amet a"
2.4.0 :017 > foo = "Lorem \n dolor sit amet a\n"&.strip!&.delete("\n")
 => "Lorem  dolor sit amet a"
2.4.0 :018 > foo = "Lorem \n dolor sit amet a"&.strip!&.delete("\n")
 => nil
2.4.0 :019 >
```
**Why it return `nil`?**

[https://apidock.com/ruby/String/strip%21](https://apidock.com/ruby/String/strip%21)

> Removes leading and trailing whitespace from str. Returns nil if str was not altered.

**What if we try to `delete` before `strip`? And the final decision is**

```ruby
params[:content]&.strip!
params[:content]&.delete!("\n")
```

Let combine them:

```ruby
params[:content]&.strip!&.delete!("\r\n\t")
```
But here the `strip` will return `nil` when there is nothing to alter.
Then reverse the `delete` and `strip` to avoid the `nil`

### Final

Remove whitespaces leading and trailing, in between also keep the spaces in the middle.

```ruby
params[:content]&.delete!("\r\n\t")&.strip!
```
