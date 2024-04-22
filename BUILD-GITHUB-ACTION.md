# Zimbra FOSS Builder - Build Github Action

## About

**MALDUA'S Zimbra FOSS Builder** brought to you by [BTACTIC, open source & cloud solutions](https://www.btactic.com).

## Introduction

This project aims to ease the build of Zimbra FOSS.

This part is about how to automate Zimbra FOSS builds thanks to Github Actions.

You are supposed to use these instructions if you want to clone this repo and manage to automate Zimbra FOSS builds onto your own Github repo.

## Requisites

### Github SSH Key is needed

Zimbra build scripts use the form `git@github.com:Zimbra/zm-build.git` in order to clone the different git repositories needed to build Zimbra.
In practice this needs an SSH private key associated to an actual Github account so that those `git clone` commands are done properly.

### Git ssh key -  Creation

In your temporary Linux machine you can create a key by doing this:

```
ssh-keygen -t rsa -b 4096 -C "zimbra-github-actions-builder@domain.com" -f zimbra-github-actions-builder.key
```
.

Just press enter twice when asked for the passphrase.

Now you can find:
- zimbra-github-actions-builder.key
- zimbra-github-actions-builder.key.pub

### Read Only Deploy key

Go to your own zimbra-foss-builder repo.
- Settings
- Deploy Keys
- Add deploy key
- Give it 'Zimbra Github Actions Builder' title.
- Copy and paste the key (The zimbra-github-actions-builder.key.pub contents).
- Make sure *Allow write access* is unchecked.
- Click Add key

### Add the private key as a secret

Go to your own zimbra-foss-builder repo.
- Settings
- Secrets and variables
- Actions
- Secrets
- Environment secrets
- Manage environment secrets
- New environment
    - Name: git_clone
    - Configure environment
- Environment secrets. Add secret.
    - Name: read_only_ssh_key
    - Value: (The zimbra-github-actions-builder.key contents).
    - Add secret

## Git ssh key - Remove temporary files

As a security measure make sure to delete:

- zimbra-github-actions-builder.key
- zimbra-github-actions-builder.key.pub

files from your temporary Linux machine.

## Rename your organisation in yml files

Find the workflow yml files such as:
- `.github/workflows/build-docker-ubuntu-18.04.yml`
- `.github/workflows/build-release-rhel-7.yml`
and then make sure to replace the maldua Github organisation with your own Github organisation.

## Rename your builder id in yml files

Find the workflow yml files such as:
- `.github/workflows/build-docker-ubuntu-18.04.yml`
- `.github/workflows/build-release-rhel-7.yml`
and then make sure to replace the maldua builder id (420) to something else between 100 and 999 that it's not used by other community Zimbra builders. Do not use 430 either because it's the default value when no builder id has been specified.

## First steps

### Build Docker

Before trying to build Zimbra you need to be build the different dockers that are used as a base for building it.

Given the current [build-docker-ubuntu-18.04.yml(.github/workflows/build-docker-ubuntu-18.04.yml) file contents you might want to start to build right away with something like:
```
git tag -a 'docker-builds-ubuntu-18.04/build-docker-1' -m 'docker-builds-ubuntu-18.04/build-docker-1'
git push origin 'docker-builds-ubuntu-18.04/build-docker-1'
```
.

Now you will find a running action inside your Github repository Actions tab that, in the end will generate a docker image for you.

### Build Zimbra

Once you have built the Docker in the previous step you are ready to build a Zimbra version.

Given the current [build-release.yml](.github/workflows/build-release.yml) file contents you might want to start to build right away with something like:
```
git tag -a 'builds-dev/10.0.7' -m 'builds-dev/10.0.7'
git push origin 'builds-dev/10.0.7'
```
.

The Zimbra version to be built is infered from the tag... so in this case it would be 10.0.7.

Now you will find a running action inside your Github repository Actions tab that, in the end will generate a Zimbra tgz file in your Repository releases section.

## Similar projects

- [ianw1974's zimbra-build-scripts](https://github.com/ianw1974/zimbra-build-scripts)
- [KontextWork's zimbra-builder](https://github.com/KontextWork/zimbra-builder)

## Extra documentation

- [aschmelyun.com - Using Docker Run inside of GitHub Actions](https://aschmelyun.com/blog/using-docker-run-inside-of-github-actions/)
- [Docker Run Action - Actions - GitHub Marketplace](https://github.com/marketplace/actions/docker-run-action)
- [Superuser - Read Only access to GitHub repo via SSH key](https://superuser.com/questions/1314064/read-only-access-to-github-repo-via-ssh-key)
