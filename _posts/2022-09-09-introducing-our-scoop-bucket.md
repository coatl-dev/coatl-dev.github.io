---
layout: post
title: Introducing our Scoop bucket
subtitle: A new way to install Ignition for Windows
tags:
- announcements
- bucket
- ignition
- jython
- manifest
- scoop
- windows
date: 2022-09-09 23:54 -0700
last-updated: 2022-09-10 22:10 -0700
---
We've been recently experimenting with a way to install and distribute software for Windows, and while there are alternatives such as Microsoft's winget, Chocolatey, among others, we chose Scoop as we felt more comfortable with it since we're comfortable with Homebrew.

Our first _manifest_ was [`jython`](https://github.com/coatl-dev/scoop-coatl-dev/blob/coatl/bucket/jython.json), and after that we started working on Ignition.

In order to be able to install our `ignition` manifest, first you'll have to add our bucket by running the following command:

```powershell
scoop bucket add scoop-coatl-dev https://github.com/coatl-dev/scoop-coatl-dev
```

Then you must edit scoop's configuration file, which is typically found at `%USERPROFILE%\.config\scoop\config.json`, and add or edit the `private_hosts` section:

```json
{
  "lastUpdate": "2022-09-09T15:41:41.2176701-07:00",
  "SCOOP_REPO": "https://github.com/ScoopInstaller/Scoop",
  "SCOOP_BRANCH": "master",
  "private_hosts": [
    {
      "match": "https://files.inductiveautomation.com/*",
      "headers": "Referer=https://inductiveautomation.com/"
    }
  ]
}

```

And just like that you'll be ready to install `ignition`. Just execute `scoop install ignition`, but please consider that you will be asked to grant "elevated" access when the service is installed.

This is what it would look like on your terminal:

```powershell
$ scoop install ignition
Installing 'ignition' (8.1.20) [64bit] from scoop-coatl-dev bucket
Ignition-windows-x86-64-8.1.20.zip (1.4 GB) [===============================================================] 100%
Checking hash of Ignition-windows-x86-64-8.1.20.zip ... ok.
Extracting Ignition-windows-x86-64-8.1.20.zip ... done.
Linking ~\scoop\apps\ignition\current => ~\scoop\apps\ignition\8.1.20
Creating shim for 'start-ignition'.
Creating shim for 'stop-ignition'.
Persisting data
Persisting logs
Running post_install script...
Runtime needs to be unzipped. unzipping..
Microsoft (R) Windows Script Host Version 5.812
Copyright (C) Microsoft Corporation. All rights reserved.

Runtime unzipped.
C:\Users\thecesrom\scoop\apps\ignition\current\lib\runtime
wrapperm | Ignition Gateway service installed.
wrapperm | Starting the Ignition Gateway service...
wrapperm | Ignition Gateway service started.
'ignition' (8.1.20) was installed successfully!
```

You will notice that the installation process creates two _shims_, `start-ignition` and `stop-ignition`, which are command-line shortcuts for `start-ignition.bat` and `stop-ignition.bat`, respectively. This means that starting and stopping Ignition can be done from the command-line without having to be at Ignition's installation directory, just like using `ignition start` and `ignition stop` commands in macOS.

Another key point is that Scoop allows us to _persist_ certain directories. In our case we are persisting both `data` and `logs` directories. This means these directories will not be affected whenever we upgrade Ignition to the latest version, as they are kept under Scoop's persist directory.

So next time Inductive Automation releases a new version, we will update the `ignition` manifest and all you'll have to do is run a single command:

```powershell
scoop update ignition
```

{: .box-note}
We do recommend you create a Gateway backup as a precaution whenever you update or uninstall. To learn more, [click here](https://docs.inductiveautomation.com/display/DOC81/Gateway+Backup+and+Restore).

If at some point you'd like to uninstall, you could simply run `scoop uninstall ignition`, or if you'd like to remove persisted data (`data` and `logs` directories), run the following command:

```powershell
$ scoop uninstall -p ignition
Uninstalling 'ignition' (8.1.20).
Running uninstaller script...
wrapperm | Service is running.  Stopping it...
wrapperm | Ignition Gateway service stopped.
wrapperm | Ignition Gateway service removed.
Removing shim 'start-ignition'.
Removing shim 'start-ignition.cmd'.
Removing shim 'stop-ignition'.
Removing shim 'stop-ignition.cmd'.
Unlinking ~\scoop\apps\ignition\current
Removing persisted data.
'ignition' was uninstalled.
```

Optionally, you can `hold` Ignition at a certain version until you're ready to upgrade. Simply run `scoop hold ignition`, and it will not be updated until you `unhold` it by running `scoop unhold ignition`.

If you run into any issue, feel free to post something at [coatl-dev Scoop bucket](https://github.com/coatl-dev/discussions/discussions/categories/coatl-dev-scoop-bucket).

To learn more about Scoop, head onto <https://scoop.sh/>.

Thanks for reading.
