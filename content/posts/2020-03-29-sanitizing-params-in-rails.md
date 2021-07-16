---
categories:
  - Rails
tags:
  - params
  - sanitizing
title: Sanitizing params in Rails
date: 2020-03-29
url: /posts/sanitizing-params-in-rails/
---


## The problem:
Here is the example of data which `POST` from a client to API endpoint:


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

In the `content`, there are whitespaces in the leading, trailing and middle.

## Expectation:

- The spaces and the whitespaces should be removed from the leading and trailing.
- Keep the spaces in between only.

*What is the whitespace?*
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

In the `rspec` if I use *single quote* instead of *double quote* then I got that error.
It means the string is escaped while running the test.

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

Then let try to clarify these result again in `irb`


```bash
irb
2.4.0 :001 > '\n'.to_s
 => "\\n"
2.4.0 :002 > "\n".to_s
 => "\n"
2.4.0 :003 >
```

When use *single quote* the `\n` will be **escaped**, adding one more forward slash
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
**Why it returns `nil`?**

[https://apidock.com/ruby/String/strip%21](https://apidock.com/ruby/String/strip%21)

> Removes leading and trailing whitespace from `str`. Returns `nil` if `str` was **not** altered.

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

## Conclusion

In order to remove the whitespaces __leading__ and __trailing__, and keep the spaces in the middle.

```ruby
params[:content]&.delete!("\r\n\t")&.strip!
```