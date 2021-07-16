---
categories:
  - Elixir
tags:
  - integration testing
  - wallaby
title: How to fix the invalid session id in Wallaby integration testing
date: 2021-04-29
url: /posts/how-to-fix-the-invalid-session-id-in-wallaby-integration-testing/
---


## Problem

When I run the integration test in my [kwtool project](https://github.com/byhbt/kwtool), then the console raises this error:

```bash
test user can visit homepage (KwtoolWeb.HomePage.ViewRegistrationPageTest)
test/kwtool_web/features/registration_page/view_registration_page_test.exs:11
** (RuntimeError) invalid session id
code: |> visit("/")
stacktrace:
(wallaby 0.28.0) lib/wallaby/httpclient.ex:136: Wallaby.HTTPClient.check_for_response_errors/1
(wallaby 0.28.0) lib/wallaby/httpclient.ex:56: Wallaby.HTTPClient.make_request/5
```

Oh, what!? It has run successfully before. Why does it happen today?

At first sight, we can see this is a **RuntimeError**, and the message is "invalid session id".
We can read more details about this error here [https://developer.mozilla.org/en-US/docs/Web/WebDriver/Errors/InvalidSessionID](https://developer.mozilla.org/en-US/docs/Web/WebDriver/Errors/InvalidSessionID).

[Dig a bit deeper](https://github.com/elixir-wallaby/wallaby/issues/468), we can see the reason is the difference between `Google Chrome` *version* and `chromedriver` *verion*.

## Solution

We can use [this script below](https://github.com/elixir-wallaby/wallaby/issues/468#issuecomment-810518368) to check the version of both applications.

Create a new file, name it as `./test-wallaby.sh` anywhere that easy for you to manage.

Then paste the content below into that file.

```bash
#!/usr/bin/env bash

chromedriver_path=$(command -v chromedriver)

chrome_path="/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"
chromedriver_version=$("${chromedriver_path}" --version)
chrome_version=$("${chrome_path}" --version)

chromedriver_major_version=$("${chromedriver_path}" --version | cut -f 2 -d " " | cut -f 1 -d ".")
chrome_major_version=$("${chrome_path}" --version | cut -f 3 -d " " | cut -f 1 -d ".")

if [ "${chromedriver_major_version}" == "${chrome_major_version}" ]; then
  exit 0
else
  echo "Wallaby often fails with 'invalid session id' if Chromedriver and Chrome have different versions."
  echo "Chromedriver version: ${chromedriver_version} (${chromedriver_path})"
  echo "Chrome version      : ${chrome_version} (${chrome_path})"
  exit 1
fi
```

And the result is:

```shell
➜ projects ./test-wallaby.sh
Wallaby often fails with 'invalid session id' if Chromedriver and Chrome have different versions.
Chromedriver version: ChromeDriver 88.0.4324.96 (68dba2d8a0b149a1d3afac56fa74648032bcf46b-refs/branch-heads/4324@{#1784}) (/usr/local/bin/chromedriver)
Chrome version      : Google Chrome 90.0.4430.93  (/Applications/Google Chrome.app/Contents/MacOS/Google Chrome)
```

Yeah, this could be the issue. So now we go to the [Chromium website](https://chromedriver.chromium.org/downloads) to download the match version of `chromedriver`

After download the correct version, extract the `chromedriver_mac64.zip` file then move it to `/usr/local/bin` by using this command:

```shell
mv chromedriver /usr/local/bin
```
Notice: I am using MacOS so I will put it to `/usr/local/bin`, please check it depends on your OS.

Now back to the integration testing script and run it again. In my case, now the integration test ran properly on my local development.

Thank you for reading, have a good day :)!