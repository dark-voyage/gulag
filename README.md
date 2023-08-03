# Gulag Git

NOTE: This project is not actively developed. I decided to use cgit instead.

Simple self-hosted git server, written in Rust

Lighter weight than [gitea](https://gitea.io/en-us/), more modern than
[cgit](https://git.zx2c4.com/cgit/) or
[gitweb](https://git-scm.com/book/en/v2/Git-on-the-Server-GitWeb). For people
who want to run a git server themselves, rather than depending on someone else,
but don't want to put too much work into it.

## Deploying
Build your binary with `cargo build --release`. Then probably move it somewhere
sensible so it's in your $PATH or use `cargo install --release`. Packages and
prebuilt binaries are TBD.

Probably you want to use your linux distro's init system to keep this server
running.

## Setting up your repos
Acquire a Linux server that you have ssh access to, and decide on the best
place to place your repos. You can also do this locally to experiment with it.

To initialize a repo, you'll need to run a few commands. I'm using a
self-hosted instance of the gulag repo as an example. Find a directory where
you want to host your repositories. This is using the default settings found in
`gulag.toml` 

```
git init --bare gulag
cd gulag
touch git-daemon-export-ok
# update "dumb http" server on updates
mv hooks/post-update.sample hooks/post-update
```
Update the `description` file with a description of the repository

Make sure the HEAD in your remote repo points to your default branch (e.g. master vs main)

Pushing your changes is not handled via gulag -- this will be done over ssh. For example:
```
git remote add origin ssh://git@git.kolyma.local:/www/git/gulag
git push -u origin main
```

Set up a reverse proxy on an http server which forwards port 8081 (or whatever port you configure) to your gulag server. 

## Why self-host?
Self-hosting provides self-reliance and independence from large platforms that
using a git hosting platform does not. There are inconvenciences and
disadvantages to self-hosting, but I think there are also advantages as well of
a decentralized, self-hosted network of collaboration. gulag is designed
primraily for hobbyists or open source hosts, so it's easy to setup and
maintain with little effort, rather than an unnecessarily piece of software
like GitLab. The tradeoff is that you lose out on a lot of features.
Self-hosting git isn't for everyone!

## Accepting patches
The simplest way to accept patches when self-hosting Git is through
[git-send-email](https://git-scm.com/docs/git-send-email)
([guide](https://git-send-email.io/)). You can accept patches either to your
personal email or use a mailing list. Basically only obsessive ideologues like
myself still use git-send-email these days, so you will probably lose
contributers, and not being on GitHub means you lose a lot of discoverability,
so make sure that you're willing to accept that when self-hosting git. You can
mitigate these issues by mirroring to GitHub, but that kind of defeats the
purpose of self-hosting.

I am working on a sibling project to this that handles mailing list archives
for exactly this purpose, but it is not ready for the public yet.

## Contributing
* [ticket tracker](https://github.com/kolyma-net/gulag)

This exists on GitHub solely for visibility sake, and probably won't forever,
but while it's here feel free to use GitHub issues, etc. This is alpha
software, please report any bugs, etc!
