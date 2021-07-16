---
categories:
  - Blog
tags:
  - python
title: Learn Flask
date: 2020-02-04
url: /posts/learn-flask/
---


The very first step in learning new Framework or programming
language is setup the environment and say *hello world* correctly.

Please keep in mind that, never install any library directly to your machine.

Use **virtualenv** instead
```
sudo apt-get install python-virtualenv
```

How to start new project:

```
mkdir learn-flask
cd learn-flask
virtualenv -p /usr/bin/python3 env
```

Activate the env

```
source bin/activate
pip install flask
mkdir src logs
```

Deactivate the env

```
deactivate
```
Now you can start coding in safety without worrying about conflict with global library.