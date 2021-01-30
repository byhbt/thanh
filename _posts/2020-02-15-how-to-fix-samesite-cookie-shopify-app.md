---
layout: post
title: "How to fix samesite cookie for Shopify App"
categories:
  - Shopify
tags:
  - cookie
  - samesite
  - php
  - shopify
---

Notice:
- My apps using PHP backend.

### 0. What is cookie?
[https://www.bigcommerce.com/ecommerce-answers/what-cookie-and-why-it-important/](https://www.bigcommerce.com/ecommerce-answers/what-cookie-and-why-it-important/)

### 1. What is **samesite** cookie and how to see it?

Actually the docs of Google is quite long
[https://web.dev/samesite-cookies-explained/](SameSite cookies explained)

I prefer the video first, then back to read the docs of Google
[https://www.youtube.com/watch?v=GPz7onXjP_4](SameSite Cookies - Chrome Update)
[https://www.youtube.com/watch?v=urdtmjuukbk](HTTP Cookie SameSite Attribute)


This article help us spot the issue faster:
[https://www.chromium.org/updates/same-site/test-debug](https://www.chromium.org/updates/same-site/test-debug)

###  2. Shopify supports embedded app so this cause the samesite cookie issue


> Embedded apps and the SameSite attribute
> Since embedded Shopify apps run in an iframe on a different domain than the Shopify admin, they are considered to be in a third-party context.
> To designate cookies for cross-site access, a new cookie setting is available in Chrome: SameSite=None. This attribute can be used by services that are running in a third-party context, such as embedded Shopify apps. It explicitly declares that a cookie is available for cross-site access. The SameSite=None setting must always be paired with another attribute, Secure, which ensures that the cookie can only be accessed by a secure connection.
> If this attribute is not explicitly set, then Chrome defaults the cookie to SameSite=Lax, which prevents cross-site access.

https://shopify.dev/tutorials/migrate-your-app-to-support-samesite-cookies#embedded-apps-and-the-samesite-attribute




### 3. How many way to solve it:

1. PHP, we look for where the code use **setcookie()** method then modify it
php version 7.3 is supporting SameSite cookie but I am sure it's quite new at the moment i write this post, so most our system still running php 7.1, 7.2

[https://wiki.php.net/rfc/same-site-cookie](https://wiki.php.net/rfc/same-site-cookie)

> But, there is a risk involved when using samesite as additional argument to setcookie, setrawcookie and session_set_cookie_params.
> The samesite cookie might not become a standard which might lead browsers to eventually drop the flag.
> If that would be the case, the setcookie, setrawcookie and session_set_cookie_params functions would have a useless samesite argument. For the record, the HttpOnly flag became a standard in 2011.
> The argument to set(raw)cookie function was already added with PHP 5.2.0 in November 2006, almost 5 years ahead of the standard.

#### Step 1:

Base on this document, we will try to looking in our code base where these methods is used.
- setcookie()
- setrawcookie()
- session_set_cookie_params()

#### Step 2:

How we will modify above methods?

```php
<?php

if (!function_exists('set_cookie_same_site')) {
    function set_cookie_same_site($name, $value, $expire, $path = '/', $domain = '', $secure = true, $httpOnly= true)
    {
        if (PHP_VERSION_ID < 70300) {
            setcookie($name, $value, $expire, "$path; SameSite=None", $domain, $secure, $httpOnly);
        } else {
            setcookie($name, $value, [
                'expires' => $expire,
                'path' => $path,
                'domain' => $domain,
                'samesite' => 'None',
                'secure' => $secure,
                'httponly' => $httpOnly,
            ]);
        }
    }
}

```

#### Step 3:

How to modify **PHPSESSID**?

For example here how I modify the session init in Phalcon 3.x framework.

```php
<?php

$di->setShared('session', function () {
    ini_set('session.use_only_cookies', 1);

    $httponly = true;
    $secure = true;
    $cookieParams = session_get_cookie_params();
    $cookieParams['domain'] = $cookieParams['domain'] . "; SameSite=None";
    session_set_cookie_params($cookieParams["lifetime"], $cookieParams["path"], $cookieParams["domain"], $secure, $httponly);

    $session = new SessionAdapter();
    $session->start();

    return $session;
});

```


#### Other cases:

There is a case, even after i mark the cookie as SameSite None and Secure but there are still an issue:

Screenshot:

[https://drive.google.com/file/d/1ubwbCT4RAcSFbmJwmjTnb0rnF93qDgHW/view](https://drive.google.com/file/d/1ubwbCT4RAcSFbmJwmjTnb0rnF93qDgHW/view)

I tried to narrow down the code to see which part of code cause the issue.

Then I got this code, however nothing related to cookie here, it just init the Shopify Embedded SDK,
maybe inside, it makes a request to app server, the get the samesite cookie warning:

```html
  <script src="https://cdn.shopify.com/s/assets/external/app.js"></script>
  <script type="text/javascript">
      ShopifyApp.init({
              apiKey: '{{ API_KEY }}',
              shopOrigin: '{{ shopUrl }}',
      debug: false
    });
    ShopifyApp.Bar.loadingOff();
  </script>
```

Then try to replace this one by Shopify App Bridge, everything is fine again.
Maybe there is something changed in js app.js library.

```html
  <script src="https://unpkg.com/@shopify/app-bridge@1.6/umd/index.js"></script>
  <script type="text/javascript">
      var AppBridge = window['app-bridge'];
      var createApp = AppBridge.default;
      var actions = AppBridge.actions;
      var History = actions.History;

      function initializeApp() {
          var app = createApp({
              apiKey: '{{ API_KEY }}',
              shopOrigin: '{{ shopUrl }}',
          });

          var history = History.create(app);
          history.dispatch(History.Action.PUSH, window.location.pathname);

          return app;
      }
  </script>
```
The problem is solved, please double check the feature related to Shopify Embedded App Library.

### Other fix techniques:

[https://stackoverflow.com/questions/39750906/php-setcookie-samesite-strict](https://stackoverflow.com/questions/39750906/php-setcookie-samesite-strict)
