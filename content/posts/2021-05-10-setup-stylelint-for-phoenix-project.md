---
categories:
  - Elixir
tags:
  - stylelint
  - code quality
  - frontend
title: Setup StyleLint for Phoenix project
date: 2021-05-10
url: /posts/setup-stylelint-for-phoenix-project/
---


In this post, I will share how to integrate [StyleLint](https://stylelint.io) with a Phoenix project. It's quite useful for automation validating your CSS code and avoid some common errors.

The main idea is to install the `stylelint` using `package.json`, but we don't use it in Webpack. We use `mix` command instead.

Let start the **first step**, by adding the `stylelint` and its related packages to `devDependencies` section of the `package.json` file:

File: `assets/package.json`

```json
{
  "devDependencies": {
    "...",
    "stylelint": "^13.13.0",
    "stylelint-config-standard": "21.0.0",
    "stylelint-scss": "3.19.0",
  }
}
```

Create `stylelint` config file file `assets/.stylelintrc`, this config is be loaded automatically by `stylelint` command.

For more details about supporting rules and configurations, you can check here [https://stylelint.io/user-guide/rules/about](https://stylelint.io/user-guide/rules/about) then update it to adapt your project requirements.

```json
{
  "plugins": [
    "stylelint-scss"
  ],
  "extends": [
    "stylelint-config-standard"
  ],
  "rules": {
    "scss/at-rule-no-unknown": true,
    "at-rule-no-unknown": [
      true,
      {
        "ignoreAtRules": [
          "apply",
          "each",
          "else",
          "extend",
          "function",
          "if",
          "include",
          "return",
          "tailwind",
          "layer",
          "screen"
        ]
      }
    ]
  }
}

```

Let's move on to the next step, we need to add StyleLint to Phoenix Mix Task by open `mix.es` file, then add these lines to the existing method `aliases`.
You can change `codebase` and `codebase.fix` to whatever you want.

```elixir
  defp aliases do
    [
      codebase: [
        "...",
        "cmd ./assets/node_modules/.bin/stylelint --color ./assets/css"
      ],
      "codebase.fix": [
        "cmd ./assets/node_modules/.bin/stylelint --color --fix ./assets/css"
      ],
    ]
  end
```

From now on, we can use the command `mix codebase` or `mix codebase.fix` to check or clean your stylesheet code.
We also run this command on the CI as well, so it will help us validate the style files on CI.

Thanks my co-worker has [suggested me](https://github.com/byhbt/kwtool/pull/32#discussion_r622694106) this approach. It's great. :)

**Further action:**
- Check how to integrate it with Webpack.
- Integrate with Intellij Idea.