---
title:  "CI/CD: how to resolve private repository dependencies when using Poetry"
date: 22-11-2021
tags: programming poetry ci
---

[Poetry][1] is a Python tool that resolves packaging and dependency management. Although it is a relatively young tool, it does a number of things well:
 - Fixing dependency versions to major, minor or exact versions
 - Resolving complex dependency chains
 - Automating procedures for building packages and uploading them to PyPI

Poetry has become the *de facto* standard to handle dependencies and virtual environments when our team start new projects. It makes it easy to ensure that everyone in the team (including the GitHub Action runners) are working with the same Python environment and saves valuable time sorting out dependency issues when new libraries introduce backwards incompatible changes.

However, not every DevOps setup becomes immediately trivial. Like many organisations, a single project can encompass multiple private repositories that need to be installed in tandem for a functioning framework or app.
Luckily, Poetry can help us here as well. In this post, we demonstrate how to build a GitHub Actions workflow for a repository depending on other (private) repositories.
<!--more-->


Our model project consists of a web-app named `fred` that has a database called `barney` as a dependency.
Both projects have a `pyproject.toml` describing the setup. *For a new project, these files can be generated with `poetry init`.*
The repositories are hosted on GitHub, in (imaginary) repositories `github.com:bedrock/fred` and `github.com:bedrock/barney`.

## Local setup
Poetry facilitates cloning and setting up packages from GitHub directly. Packages can be added with
```bash
poetry add git+ssh://git@github.com:bedrock/barney.git#main
```
where `git+ssh` specifies that Poetry will connect to GitHub, clone over `ssh` and use the code from the `main` branch (more details in the [documentation][2]).

The dependency is now listed in `pyproject.toml`:
```toml
[tool.poetry.dependencies]
barney = {git = "git@github.com:bedrock/barney.git", rev = "main"}
```
`rev` refers to the name of the branch that should be installed. Keep in mind that because Poetry *fixes* versions, updates to the `main` branch of `barney` won't be installed to `fred` automatically!

A subsequent `poetry install` will install all project dependencies and clone the `barney` repository (provided `git` can find the right SSH keys).

## Running in CI
Usually, this runs without a hitch. However, if you are trying to build from a container associated with `fred` and you want to access the private `barney` repository, you are gonna run into security issues. You don't want to commit valid SSH keys to the repository, nor make `barney` public. One of the easiest solutions is adding the private key as a secret to GitHub.

### 1. Generate and encrypt deploy key
There are a lot of different ways to use, encrypt and decrypt SSH keys, not all equally secure. I am by no means a security expert, so I won't reinvent the wheel and use a standard `ssh-keygen` setup.
```bash
cd /path/to/fred
ssh-keygen -t rsa -b 4096 -f ~/deploy_key -C "fred@bedrock" -N ""
```

### 2. Upload key and passphrase to GitHub
Next, add the public key to the `barney` repository as a deploy key. Go to the repository page, click 'Settings', 'Deploy keys' and `Add deploy key`. 
There, add the public key `~/deploy_key.pub`.

Then, go to the `fred` repository, click 'Settings',`->` 'Secrets' `->` 'Actions' and add a 'New repository secret'. 
Name the secret DEPLOY_KEY and upload the output of
```bash
cat ~/deploy_key | base64 -w 0
```
_Because GitHub Secrets did not like the newlines in the private key, I chose to encode the key in a one-line string._
Finally, remove the key (or store it in a vault in case you need to reuse it).


### 3. Setup GitHub workflow
Now, we can build a workflow that checks out the code, sets up Python 3, installs poetry and builds the project on each push and pull request.

```yaml
{% raw %}
name: CI
on: [push, pull_request]
jobs:
  unittest:
    steps:
      - name: Checkout repo
        uses: actions/checkout@v2
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: '3.x'
      - name: Add SSH key
        env:
            DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
        run: |
          echo $DEPLOY_KEY | base64 -d > ~/deploy_key
          chmod 600 ~/deploy_key
      - name: Install poetry and dependencies
        env:
          GIT_SSH_COMMAND: "ssh -i ~/deploy_key"
        run: |
          python -m pip install pip poetry --upgrade
          poetry install {% endraw %}
```
We use `GIT_SSH_COMMAND` to override the ssh command with a private key location that `git` will use when poetry attempts to clone `barney`.
We decrypt the key with the secret value and impose the right access permissions on the key.

After this setup, we can add automated testing or deployment with additional `run` commands.

The same can be done in automated Docker building. Just remember that you will have to add a build argument in the `Dockerfile` so you can pass the key and use it as an environment variable with a line
```docker
ARG ENCRYPTION_KEY
```
and set that build argument like
```bash
docker build --build-arg ENCRYPTION_KEY=$(keyvar) ...
```

Happy deploying!

[1]: https://python-poetry.org/
[2]: https://python-poetry.org/docs/
