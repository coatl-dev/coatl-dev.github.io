---
layout: post
title: New Ignition 8.1.4 formulae now available
tags:
- announcement
- brew
- formulae
- homebrew
- ignition
- scada
date: 2021-04-01 18:19 -0700
---
We are proud to announce three new formulae!

- [ignition](https://formulae.coatl.dev/formula/ignition) (More info [here]({{ site.url }}/news/2021/03/04/stay-up-to-date-with-the-ignition-formula))
- [ignition-edge@8.1.4](https://formulae.coatl.dev/formula/ignition-edge@8.1.4)
- [ignition@8.1.4](https://formulae.coatl.dev/formula/ignition@8.1.4)

## How do I install these formulae?

To install these formulae, first tap our Homebrew tap:

```bash
brew tap coatl-dev/coatl-dev
```

And then, for `ignition-edge@8.1.4`:

```bash
brew install ignition-edge@8.1.4
```

Or for `ignition@8.1.4`:

```bash
brew install ignition@8.1.4
```

Or for `ignition`:

```bash
brew install ignition
```

Or with one single command.

For `ignition-edge@8.1.4`:

```bash
brew install coatl-dev/coatl-dev/ignition-edge@8.1.4
```

Or for `ignition@8.1.4`:

```bash
brew install coatl-dev/coatl-dev/ignition@8.1.4
```

Or for `ignition`:

```bash
brew install coatl-dev/coatl-dev/ignition
```

## How do I upgrade the `ignition` formula?

First, stop the `ignition` service by running the following commands:

```bash
$ brew services stop ignition
Stopping `ignition`... (might take a while)
==> Successfully stopped `ignition` (label: homebrew.mxcl.ignition)
$ ignition stop
Stopping Ignition-Gateway...
Stopped Ignition-Gateway.
```

Then, if you pinned the previous version, unpin it by running:

```bash
brew unpin ignition
```

Finally run `brew update` and `brew upgrade`:

```bash
$ brew update
==> New Formulae
coatl-dev/coatl-dev/ignition-edge@8.1.4                      coatl-dev/coatl-dev/ignition@8.1.4
==> Updated Formulae
coatl-dev/coatl-dev/ignition ✔

You have 1 outdated formula installed.
You can update it with brew upgrade.
coatl-dev/coatl-dev/ignition (8.1.3) < 8.1.4
$ brew upgrade
==> Upgrading 1 outdated package:
coatl-dev/coatl-dev/ignition 8.1.3 -> 8.1.4
==> Upgrading coatl-dev/coatl-dev/ignition 8.1.3 -> 8.1.4 
==> Downloading https://files.inductiveautomation.com/release/ia/8.1.4/20210401-0932/Ignition-osx-8.1.4.zip
######################################################################## 100.0%
==> tar -C /usr/local/Cellar/ignition/8.1.4/libexec/lib/runtime -xf /usr/local/Cellar/ignition/8.1.4/libexec/lib/runtime
==> /usr/local/Cellar/ignition/8.1.4/libexec/lib/runtime/jre-mac/bin/java -classpath /usr/local/Cellar/ignition/8.1.4/li
==> Caveats
The data and logs folders have been symlinked to:
  data: /usr/local/etc/ignition
  logs: /usr/local/var/ignition

To have launchd start coatl-dev/coatl-dev/ignition now and restart at login:
  brew services start coatl-dev/coatl-dev/ignition
==> Summary
🍺  /usr/local/Cellar/ignition/8.1.4: 1,416 files, 1.5GB, built in 56 seconds
Removing: /usr/local/Cellar/ignition/8.1.3... (1,439 files, 1.5GB)
Pruned 0 symbolic links and 16 directories from /usr/local
```

## How do I start the Ignition Gateway?

{: .box-warning}
Warning: Please make sure you don't have another Ignition Gateway running on port `8088`. If you do, please stop it by running `ignition@X.X.X stop`, `ignition-edge@X.X.X stop` or `ignition stop`.

Once installed, open a new Terminal window and run the following command:

```bash
ignition-edge@8.1.4 start
```

Or

```bash
ignition@8.1.4 start
```

Or

```bash
ignition start
```

Once the Service starts, you can go to <http://localhost:8088/> to provision your Gateway.

## How do I start Ignition automatically at login?

Run the following command:

```bash
brew services start coatl-dev/coatl-dev/ignition-edge@8.1.4
```

Or

```bash
brew services start coatl-dev/coatl-dev/ignition@8.1.4
```

Or

```bash
brew services start coatl-dev/coatl-dev/ignition
```

## Discussions

If you run into any issue, feel free to post something at our [Discussions](https://github.com/coatl-dev/discussions/discussions).
