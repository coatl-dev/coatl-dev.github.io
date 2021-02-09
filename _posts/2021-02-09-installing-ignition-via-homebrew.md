---
layout: post
title: Installing Ignition via Homebrew
tags:
- brew
- homebrew
- ignition
- scada
date: 2021-02-09 10:53 -0800
---
<!-- TOC -->
# Table of contents
- [Introduction](#introduction)
- [Motivation](#motivation)
- [How does it work?](#how-does-it-work)
- [Ignition via Homebrew](#ignition-via-homebrew)
- [Which formulae are available?](#which-formulae-are-available)
- [How do I install these formulae?](#how-do-i-install-these-formulae)
- [How do I start/stop/restart Ignition when installed via Homebrew?](#how-do-i-startstoprestart-ignition-when-installed-via-homebrew)
- [How do I start Ignition automatically at Login?](#how-do-i-start-ignition-automatically-at-login)
- [How do I check the status of the Ignition service?](#how-do-i-check-the-status-of-the-ignition-service)
- [How do I run `gwcmd`?](#how-do-i-run-gwcmd)
- [How do I uninstall Ignition?](#how-do-i-uninstall-ignition)
- [Caveats and Limitations](#caveats-and-limitations)
- [Troubleshooting](#troubleshooting)
- [Where do I get support if something goes wrong?](#where-do-i-get-support-if-something-goes-wrong)
<!-- END TOC -->

## Introduction
During the winter break we started working in a way to get [Ignition](https://inductiveautomation.com/ignition/), by Inductive Automation, installed via Homebrew, and recently our efforts have come to fruition.

For those who might not be aware of [Homebrew](https://brew.sh/), Homebrew is "The Missing Package Manager for macOS (or Linux)".

> What Does Homebrew Do?
>
> Homebrew installs [the stuff you need](https://formulae.brew.sh/formula/) that Apple (or your Linux system) didn’t.

## Motivation
The main reason is to give developers the ability to install multiple versions. Granted, this is something that can be accomplished via the [ZIP File Installations](https://docs.inductiveautomation.com/display/DOC81/ZIP+File+Installations), but doing it via Homebrew is a little less involved.

## How does it work?
First you'll need Homebrew, see [Installation](https://docs.brew.sh/Installation).

## Ignition via Homebrew
Once you've installed Homebrew, or if you already have it, you'll need to tap coatl-dev's Homebrew tap with the following command:

```bash
$ brew tap coatl-dev/coatl-dev
```

That will *tap* our Homebrew repository and will allow you to install its _formulae_ or _casks_.

Please note that multiple releases can be installed side-by-side, with some considerations like the port number used for each Gateway. 

To get a list of available versions, run `brew search ignition` after you've tapped our repo or refer to [which formulae are available](#which-formulae-are-available).

## Which formulae are available?
You may visit [coatl.dev formulae](https://formulae.coatl.dev/formula/) for the complete list.

## How do I install these formulae?
You can run `brew install coatl-dev/coatl-dev/<formula>`, or `brew tap coatl-dev/coatl-dev` and then `brew install <formula>` .

Example:
```bash
$ brew tap coatl-dev/coatl-dev
==> Tapping coatl-dev/coatl-dev
Cloning into '/usr/local/Homebrew/Library/Taps/coatl-dev/homebrew-coatl-dev'...
$ brew install ignition@8.1.2
==> Installing ignition@8.1.2 from coatl-dev/coatl-dev
==> Downloading https://files.inductiveautomation.com/release/ia/8.1.2/20210203-1115/Ignition-osx-8.1.2.zip
################################################ 100.0%
==> Caveats
To have launchd start coatl-dev/coatl-dev/ignition@8.1.2 now and restart at login:
  brew services start coatl-dev/coatl-dev/ignition@8.1.2
==> Summary
🍺  /usr/local/Cellar/ignition@8.1.2/8.1.2: 1,330 files, 1.4GB, built in 56 seconds
```

After that, just open a new Terminal window or tab, and run `ignition-8.1.2 start` and after that completes successfully just go to http://localhost:8088/ to provision your Gateway.

## How do I start/stop/restart Ignition when installed via Homebrew?
The installation creates a symlink located in `/usr/local/bin`, and each Ignition release is named according to its version. E.g. ignition@8.1.1 installs `/usr/local/bin/ignition-8.1.2`, ignition@8.0.17 installs `ignition-8.0.17`, ignition-edge@8.1.1 installs `ignition-edge-8.1.1`, and so on.

So you'll just have to run `ignition-x-x-x start` or `ignition-edge-x-x-x start` for the Edge release.

```bash
$ ignition-8.1.2 start
Starting Ignition-Gateway...
Waiting for Ignition-Gateway.........
running: PID:5999
```

The first time you start Ignition you might see the following output:

```bash
$ ignition-8.1.2 start
decompressing runtime..
runtime decompression complete
Starting Ignition-Gateway...
Waiting for Ignition-Gateway.......
running: PID:6811
```

This means that Ignition is decompressing the pre-bundled JRE.

## How do I start Ignition automatically at Login?
As the installation messages suggests, just run `brew services start coatl-dev/coatl-dev/ignition@x.x.x`. Alternatively you could run `sudo brew services start coatl-dev/coatl-dev/ignition@x.x.x` to start at startup.

Example:
```bash
$ brew services start coatl-dev/coatl-dev/ignition@8.1.2
==> Successfully started `ignition@8.1.2` (label: homebrew.mxcl.ignition@8.1.2)
```

For more information about `brew services`, run `brew services --help`.

## How do I check the status of the Ignition service?
Simply run the following command:

```bash
$ ignition-8.1.2 status
Ignition-Gateway (not installed) is running: PID:5999, Wrapper:STARTED, Java:STARTED
```

Or to check the `brew` service:

```bash
$ brew services
Name           Status  User      Plist
ignition@8.1.2 started thecesrom /Users/thecesrom/Library/LaunchAgents/homebrew.mxcl.ignition@8.1.2.plist
```

It may occur that before you restart your Mac you might get `error` as the Status, like this:

```bash
$ brew services
Name           Status  User      Plist
ignition@8.1.2 error   thecesrom /Users/thecesrom/Library/LaunchAgents/homebrew.mxcl.ignition@8.1.2.plist
```

There is no issue with the service, you might just need to restart.

## How do I run `gwcmd`?
For those of you that wish to manage your Gateway through the Gateway Command-line Utility, or `gwcmd`, it can be found at `/usr/local/Cellar/ignition@X.X.X/X.X.X/libexec`.

```bash
$ cd /usr/local/Cellar/ignition@8.1.2/8.1.2/libexec
$ ./gwcmd.sh --info
Gateway Name: Ignition-Cesars-MacBook-Air.local
Gateway Version: 8.1.2 (64-bit)
Web Server Status: RUNNING
Gateway Status: RUNNING
Http Port: 8088
Https Port: 8443
$ ./gwcmd.sh --port 80
HTTP port changed.
$ ./gwcmd.sh --sslport 443
HTTPS port changed.
$ ./gwcmd.sh --restart
Waiting for Gateway restart..
Gateway is now running
$ ./gwcmd.sh --info
Gateway Name: Ignition-Cesars-MacBook-Air.local
Gateway Version: 8.1.2 (64-bit)
Web Server Status: RUNNING
Gateway Status: RUNNING
Http Port: 80
Https Port: 443
```

To see a full list of options run `./gwcmd.sh --help` or read the documentation [here](https://docs.inductiveautomation.com/display/DOC81/Gateway+Command-line+Utility+-+gwcmd).

## How do I uninstall Ignition?
1. ⚠️ Stop the Gateway by running `ignition-x.x.x stop` or `ignition-edge-x.x.x stop`⚠️ ([Learn more](#troubleshooting))
2. Run `brew uninstall ignition@x.x.x` or `brew uninstall ignition-edge@x.x.x`
3. Done

## Caveats and Limitations
This is only currently working on macOS Mojave or higher.

You can only provision or select one version for each release. This means that if you install `ignition@8.1.2` and during provisioning you select Maker, you cannot try to install `ignition@8.1.2` again and select another version while provisioning your Gateway. The same applies to all other formulae.

As mentioned above, it is important to first stop the Gateway, otherwise you might run into an issue if you want to reinstall the same version.

## Troubleshooting
While testing Ignition formulae we noticed that if the formula is uninstalled via `brew uninstall ignition[-edge]-X.X.X` before the service is stopped, the running service does not allow Homebrew to perform proper cleanup so the formula directory cannot be deleted (found at `$(brew --cellar)/ignition[-edge]-X.X.X/X.X.X`).

For example, if you installed `ignition@8.1.2`, then uninstalled without stopping the service, and try to install again, you might get the following error:


```bash
$ brew install ignition@8.1.2
Warning: coatl-dev/coatl-dev/ignition@8.1.2 8.1.2 is already installed, it's just not linked
You can use `brew link ignition@8.1.2` to link this version.
```

Steps to fix this:
1. Stop the Ignition process using `Activity Monitor`, `top`, or `htop`
1. Delete the `ignition[-edge]-X.X.X` folder from your Cellar
    ```bash
    $ brew uninstall ignition@8.1.2
    Uninstalling /usr/local/Cellar/ignition@8.1.2/8.1.2... (6.5KB)
    ```

## Where do I get support if something goes wrong?
Try the [Discussions](https://github.com/coatl-dev/discussions/discussions).


Hopefully you find it useful.