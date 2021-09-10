---
layout: post
title: Stay up to date with the Ignition formula
tags:
- homebrew
- ignition
- scada
date: 2021-03-04 13:47 -0800
last-updated: 2021-09-09 22:49 -0700
---
In coatl.dev we have been busy at creating the Ignition formula that will allow you to upgrade automatically as soon as a new version is released. This should give you a similar experience to downloading the Ignition installer and replacing an old version with the latest.

<!-- TOC -->
# Table of contents
- [Install Ignition](#install-ignition)
- [Upgrade Ignition](#upgrade-ignition)
- [Start Ignition](#start-ignition)
- [Check status](#check-status)
- [Uninstall Ignition](#uninstall-ignition)
- [Where do I get support if something goes wrong?](#where-do-i-get-support-if-something-goes-wrong)
- [Further reading](#further-reading)
<!-- END TOC -->

### Install Ignition
To install the latest version available from our Homebrew tap, just run `brew install coatl-dev/coatl-dev/ignition`, and you'll see something like the following:

```bash
$ brew install coatl-dev/coatl-dev/ignition
==> Installing ignition from coatl-dev/coatl-dev
==> Downloading https://files.inductiveautomation.com/release/ia/8.1.3/20210303-0915/Ignition-osx-8.1.3.zip
######################################################################## 100.0%
==> tar -C /usr/local/Cellar/ignition/8.1.3/libexec/lib/runtime -xf /usr/local/Cellar/ignition/8.1.3/libexec/lib/runtime/jre-mac.tar.gz
==> /usr/local/Cellar/ignition/8.1.3/libexec/lib/runtime/jre-mac/bin/java -classpath /usr/local/Cellar/ignition/8.1.3/lib/core/common/common.jar com.inductiveautomation.ignition.common.upgrader.Upgrader . /usr/local/Cellar/ignition/8.1.3/libexec/data /usr/local/Cellar/ignition/8.1.3/libexec/logs file=ignition.conf
==> Caveats
The data and logs folders have been symlinked to:
  data: /usr/local/etc/ignition
  logs: /usr/local/var/ignition

To have launchd start coatl-dev/coatl-dev/ignition now and restart at login:
  brew services start coatl-dev/coatl-dev/ignition
==> Summary
🍺  /usr/local/Cellar/ignition/8.1.3: 1,412 files, 1.5GB, built in 1 minute
```

{: .box-warning}
During installation, you might get the following message `Another installation has been found which may interfere with a Homebrew-built Ignition Gateway from starting up correctly` if one installation (whether via Ignition's installer or from one of our versioned formulae) or `Other installations have been found which may interfere with a Homebrew-built Ignition Gateway from starting up correctly` if more than one installation was found. This is just a warning that you may run into problems if both installations use the same ports (HTTP, HTTPS, and Gateway Network).

And just like that you will be able to start your Ignition Gateway by running `brew services start coatl-dev/coatl-dev/ignition` if you wish to launch Ignition at log-in, or if you don't wish to register Ignition as a service simply run `ignition start`.

As this is a new installation, browsing to `http://localhost:8088/` should take you to the Gateway's provisioning screen where you will be able to choose the type of Gateway; Maker Edition, Standard Ignition, or Ignition Edge.

To learn more about how to install Ignition, please [read this](https://docs.inductiveautomation.com/display/DOC81/Installing+and+Upgrading+Ignition#InstallingandUpgradingIgnition-InstallIgnition){:target="_blank"}.

### Upgrade Ignition
If you have previously installed Ignition via our Homebrew Tap, we will illustrate the common workflow.

{: .box-note}
Note: We have "pinned" Ignition 8.1.2 so we upgrade at our convenience, to learn more about `pin` please refer to Homebrew's documentation [here](https://docs.brew.sh/Manpage#pin-installed_formula-){:targe="blank"}

1. Run `brew update` to retrieve the latest changed to `brew` and all installed `formulae`, and `casks`
    ```bash
    $ brew update
    Updated 1 tap (coatl-dev/coatl-dev).
    ==> Updated Formulae
    ignition ✔
    
    You have 1 outdated formula installed.
    You can update it with brew upgrade.
    ```
    1. To confirm all outdated formula, run the following
        ```bash
        $ brew outdated
        coatl-dev/coatl-dev/ignition (8.1.2) < 8.1.3 [pinned at 8.1.2]
        ```
1. Create a Gateway backup via `gwcmd` or the Web interface **Config > System > System Backup/Restore** and click on **Download Backup**
    ```bash
    $ cd "$(brew --cellar)/ignition/8.1.2/libexec"
    $ ./gwcmd.sh --backup ~/Downloads/ignition-8.1.2.gwbk
    ```
1. If you are ready to upgrade, first you will have to stop Ignition by running the following commands
    1. Stop the `brew` service
        ```bash
        $ brew services stop ignition
        Stopping `ignition`... (might take a while)
        ==> Successfully stopped `ignition` (label: homebrew.mxcl.ignition)
        ```
    1. Stop the Ignition service
        ```bash
        $ ignition stop
        Stopping Ignition-Gateway...
        Waiting for Ignition-Gateway to exit...
        Stopped Ignition-Gateway.
        ```
1. If you had pinned Ignition, first `unpin` it
    ```bash
    $ brew unpin ignition
    ```
1. Finally upgrade
    ```bash
    $ brew upgrade ignition
    ==> Upgrading 1 outdated package:
    coatl-dev/coatl-dev/ignition 8.1.2 -> 8.1.3
    ==> Upgrading coatl-dev/coatl-dev/ignition 8.1.2 -> 8.1.3 
    ==> Downloading https://files.inductiveautomation.com/release/ia/8.1.3/20210303-0915/Ignition-osx-8.1.3.zip
    ######################################################################## 100.0%
    ==> tar -C /usr/local/Cellar/ignition/8.1.3/libexec/lib/runtime -xf /usr/local/Cellar/ignition/8.1.3/libexec/lib/runtime/jre-mac.tar.gz
    ==> /usr/local/Cellar/ignition/8.1.3/libexec/lib/runtime/jre-mac/bin/java -classpath /usr/local/Cellar/ignition/8.1.3/lib/core/common/common.jar com.inductiveautomation.ignition.common.upgrader.Upgrader . /usr/local/Cellar/ignition/8.1.3/libexec/data /usr/local/Cellar/ignition/8.1.3/libexec/logs file=ignition.conf
    ==> Caveats
    The data and logs folders have been symlinked to:
      data: /usr/local/etc/ignition
      logs: /usr/local/var/ignition
    
    To have launchd start coatl-dev/coatl-dev/ignition now and restart at login:
      brew services start coatl-dev/coatl-dev/ignition
    ==> Summary
    🍺  /usr/local/Cellar/ignition/8.1.3: 1,412 files, 1.5GB, built in 1 minute 4 seconds
    Removing: /usr/local/Cellar/ignition/8.1.2... (1,442 files, 1.5GB)
    ```

### Start Ignition
After installing or upgrading Ignition, a symlink called `ignition` will be present in `/usr/local/bin` so you can just run `ignition` as a command.

To install `ignition` as a service that will start automatically at log-in:
```bash
$ brew services start coatl-dev/coatl-dev/ignition
==> Successfully started `ignition` (label: homebrew.mxcl.ignition)
```

Alternatively, just run `ignition start`:
```bash
$ ignition start
Starting Ignition-Gateway...
Waiting for Ignition-Gateway...........
running: PID:13648
```

### Check status
Once installed you may run any of the following commands to check Ignition's status.

1. `ignition status`
    When stopped you might see the following output:
    ```bash
    $ ignition status
    Ignition-Gateway (not installed) is not running.
    ```
    Or if it's already running:
    ```bash
    $ ignition status
    Ignition-Gateway (not installed) is running: PID:13648, Wrapper:STARTED, Java:STARTED
    ```
1. `brew services`
    ```bash
    $ brew services
    Name     Status    User      Plist
    ignition started   thecesrom /Users/thecesrom/Library/LaunchAgents/homebrew.mxcl.ignition.plist
    ```
1. With `gwcmd`
    ```bash
    $ cd "$(brew --prefix ignition)/libexec"
    $ ./gwcmd.sh -i
    Gateway Name: Ignition-Cesars-MacBook-Air.local
    Gateway Version: 8.1.3 (64-bit)
    Web Server Status: RUNNING
    Gateway Status: RUNNING
    Http Port: 8088
    Https Port: 8443
    ```

## Uninstall Ignition
If you wish to uninstall Ignition, we recommend following these steps:

1. Stop `brew` service
    ```bash
    $ brew services stop ignition
    Stopping `ignition`... (might take a while)
    ==> Successfully stopped `ignition` (label: homebrew.mxcl.ignition)
    ```
1. Stop `ignition`
    ```bash
    $ ignition stop
    Stopping Ignition-Gateway...
    Stopped Ignition-Gateway.
    ```
1. Uninstal with `brew uninstall`
    ```bash
    $ brew uninstall coatl-dev/coatl-dev/ignition
    Uninstalling /usr/local/Cellar/ignition/8.1.3... (1,439 files, 1.5GB)
    
    Warning: The following ignition configuration files have not been removed!
    If desired, remove them manually with `rm -rf`:
    /usr/local/etc/ignition
    ...
    ```
1. Finally, some manual cleanup
    ```bash
    $ cd /usr/local/etc
    $ rm -rf ignition
    $ cd /usr/local/var
    $ rm -rf ignition
    ```

## Where do I get support if something goes wrong?
Try the [Discussions](https://github.com/coatl-dev/discussions/discussions/categories/help).

## Further reading
[Installing Ignition via Homebrew]({{ site.url }}/blog/2021/02/09/installing-ignition-via-homebrew)
