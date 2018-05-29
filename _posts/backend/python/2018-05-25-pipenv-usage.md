---
layout: post
title:  "python: use python like npm - pipenv "
categories: python
---


When using python, I used to use `pyenv` and `virtualenv`. 


I prefer to use `pyenv` in local environment because I can choose python version easily, and it can be used with `autoenv` for convenience.


But either way, I have to use `pip` for insatll python packages.


Recently I found out [`pipenv`](https://github.com/pypa/pipenv). 

> It's a tool that aims to bring the best of all packagiong worlds to the Python world.

> It automatically creates and manages a virtualenv for your projects, as well as adds/removes packages from your Pipfile as you install/uninstall packages. It also generates the ever–important Pipfile.lock, which is used to produce deterministic builds.


If you have experience with `node.js`, you can see file like `package.json`. Like `package.json`, pipenv use `Pipfile.lock` to manage packages.



## Install

Very easy to install.

```bash
brew install pipenv
```

Also can install using pip

```bash
pip install pipenv
```


## Usage

```bash
$ pipenv
Usage: pipenv [OPTIONS] COMMAND [ARGS]...

Options:
  --where          Output project home information.
  --venv           Output virtualenv information.
  --py             Output Python interpreter information.
  --envs           Output Environment Variable options.
  --rm             Remove the virtualenv.
  --bare           Minimal output.
  --completion     Output completion (to be eval'd).
  --man            Display manpage.
  --three / --two  Use Python 3/2 when creating virtualenv.
  --python TEXT    Specify which version of Python virtualenv should use.
  --site-packages  Enable site-packages for the virtualenv.
  --version        Show the version and exit.
  -h, --help       Show this message and exit.


Usage Examples:
   Create a new project using Python 3.6, specifically:
   $ pipenv --python 3.6

   Install all dependencies for a project (including dev):
   $ pipenv install --dev

   Create a lockfile containing pre-releases:
   $ pipenv lock --pre

   Show a graph of your installed dependencies:
   $ pipenv graph

   Check your installed dependencies for security vulnerabilities:
   $ pipenv check

   Install a local setup.py into your virtual environment/Pipfile:
   $ pipenv install -e .

   Use a lower-level pip command:
   $ pipenv run pip freeze

Commands:
  check      Checks for security vulnerabilities and against PEP 508 markers
             provided in Pipfile.
  clean      Uninstalls all packages not specified in Pipfile.lock.
  graph      Displays currently–installed dependency graph information.
  install    Installs provided packages and adds them to Pipfile, or (if none
             is given), installs all packages.
  lock       Generates Pipfile.lock.
  open       View a given module in your editor.
  run        Spawns a command installed into the virtualenv.
  shell      Spawns a shell within the virtualenv.
  sync       Installs all packages specified in Pipfile.lock.
  uninstall  Un-installs a provided package and removes it from Pipfile.
```


## Review


It's really easy to use. Especially compared to pyenv. (It tooks a day to set up pyenv first time)

However, I felt little bit speed issue when using `pipenv`. I have to see more about it to use in real development environment.
