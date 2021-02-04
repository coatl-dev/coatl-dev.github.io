---
layout: post
title: Installing Ignition via Homebrew
tags:
- brew
- homebrew
- ignition
- scada
date: 2021-02-03 23:52 -0800
---
During the winter break we started working in a way to get [Ignition](https://inductiveautomation.com/ignition/), by Inductive Automation, installed via Homebrew, and recently our efforts have come to fruition.

For those who might not be aware of [Homebrew](https://brew.sh/), Homebrew is "The Missing Package Manager for macOS (or Linux)".

> What Does Homebrew Do?
>
> Homebrew installs [the stuff you need](https://formulae.brew.sh/formula/) that Apple (or your Linux system) didn’t.

### How does it work?
First you'll need Homebrew, see [Installation](https://docs.brew.sh/Installation).

### Ignition via Homebrew
Once you've installed Homebrew, or if you already have it, you'll need to tap coatl-dev's Homebrew tap with the following command:

```bash
$ brew tap coatl-dev/coatl-dev
```

That will *tap* our Homebrew repository and will allow you to install its _formulae_ or _casks_.

Please note that multiple releases can be installed side-by-side, with some considerations like the port number used for each Gateway. 

To get a list of available versions, run `brew search ignition` after you've tapped our repo or refer to [which formulae are available](#which-formulae-are-available).

### Which formulae are available?
You may visit https://formulae.coatl.dev/formula/ for the complete list.

### How do I install these formulae?
You can run `brew install coatl-dev/coatl-dev/<formula>`, or `brew tap coatl-dev/coatl-dev` and then `brew install <formula>` .

Example:
```bash
$ brew tap coatl-dev/coatl-dev
==> Tapping coatl-dev/coatl-dev
Cloning into '/usr/local/Homebrew/Library/Taps/coatl-dev/homebrew-coatl-dev'...
$ brew install ignition@8.1.2
==> Installing ignition@8.1.2 from coatl-dev/coatl-dev
==> Downloading https://files.inductiveautomation.com/release/ia/8.1.2/20210203-1115/Ignition-osx-8.1.2.zip
######################################################################## 100.0%
==> Caveats
To have launchd start coatl-dev/coatl-dev/ignition@8.1.2 now and restart at login:
  brew services start coatl-dev/coatl-dev/ignition@8.1.2
==> Summary
🍺  /usr/local/Cellar/ignition@8.1.2/8.1.2: 1,330 files, 1.4GB, built in 56 seconds
```

After that, just open a new Terminal window or tab, and run `ignition-8.1.2 start` and after that completes successfully just go to http://localhost:8088/ to provision your Gateway.

### How am I able to start/stop/restart Ignition when installed via Homebrew?
The installation creates a symlink located in `/usr/local/bin`, and each Ignition release is named according to its version. E.g. ignition@8.1.1 installs `/usr/local/bin/ignition-8.1.2`, ignition@8.0.17 installs `ignition-8.0.17`, ignition-edge@8.1.1 installs `ignition-edge-8.1.1`, and so on.

So you'll just have to run `ignition-x-x-x start` or `ignition-edge-x-x-x start` for the Edge release.

### How do I start Ignition automatically at Login?
As the installation messages suggests, just run `brew services start coatl-dev/coatl-dev/ignition@x.x.x`. Alternatively you could run `sudo brew services start coatl-dev/coatl-dev/ignition@x.x.x` to start at startup.

Example:
```bash
$ brew services start coatl-dev/coatl-dev/ignition@8.1.2
==> Successfully started `ignition@8.1.2` (label: homebrew.mxcl.ignition@8.1.2)
```

For more information about `brew services`, run `brew services --help`.

### How do I check the status of the Ignition service?
Simply run the following command:

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

### How do I uninstall Ignition?
1. ⚠️ Stop the Gateway by running `ignition-x.x.x stop` or `ignition-edge-x.x.x stop`⚠️
2. Run `brew uninstall ignition@x.x.x` or `brew uninstall ignition-edge-x.x.x`
3. Done

### Where do I get support if something goes wrong?
Try the [Discussions](https://github.com/coatl-dev/discussions/discussions).

### Motivation
The main reason is to give developers the ability to install multiple versions. Granted, this is something that can be accomplished via the [ZIP File Installations](https://docs.inductiveautomation.com/display/DOC81/ZIP+File+Installations), but doing it via Homebrew is a little less involved.

### Caveats and Limitations
This is only currently working on macOS Mojave or higher.

You can only provision or select one version for each release. This means that if you install `ignition@8.1.2` and during provisioning you select Maker, you cannot try to install `ignition@8.1.2` again and select another version while provisioning your Gateway. The same applies to all other formulae.

As mentioned above, it is important to first stop the Gateway, otherwise you might run into an issue if you want to reinstall the same version.

Hopefully you find it useful.