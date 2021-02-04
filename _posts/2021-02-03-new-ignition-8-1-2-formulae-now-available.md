---
layout: post
title: New Ignition 8.1.2 formulae now available
tags:
- announcement
- brew
- formulae
- homebrew
- ignition
- scada
date: 2021-02-03 18:57 -0800
---
We are proud to announce two new formulae!

- [ignition-edge@8.1.2](https://formulae.coatl.dev/formula/ignition-edge@8.1.2)
- [ignition@8.1.2](https://formulae.coatl.dev/formula/ignition@8.1.2)

## How do I install these formulae?
To install these formulae, first tap our Homebrew tap:

```bash
$ brew tap coatl-dev/coatl-dev
```

And then, for `ignition-edge@8.1.2`:

```bash
$ brew install ignition-edge@8.1.2
```

Or for `ignition@8.1.2`:

```bash
$ brew install ignition@8.1.2
```

Or with one single command.

For `ignition-edge@8.1.2`:

```bash
$ brew install coatl-dev/coatl-dev/ignition-edge@8.1.2
```

Or for `ignition@8.1.2`:

```bash
$ brew install coatl-dev/coatl-dev/ignition@8.1.2
```

## How do I start the Ignition Gateway?

{: .box-warning}
Warning: Please make sure you don't have another Ignition Gateway running on port `8088`. If you do, please stop it by running `ignition@X.X.X stop` or `ignition-edge@X.X.X stop` if you've installed from our Tap, or `ignition stop` if you've installed it directly from Inductive Automation.

Once installed, open a new Terminal window and run the following command:

```bash
$ ignition-edge@8.1.2 start
```

Or

```bash
$ ignition@8.1.2 start
```

Once the Service starts, you can go to http://localhost:8088/ to provision your Gateway.

## How do I start Ignition automatically at login?

Run the following command:

```bash
$ brew services start coatl-dev/coatl-dev/ignition-edge@8.1.2
```

Or

```bash
$ brew services start coatl-dev/coatl-dev/ignition@8.1.2
```

If you run into any issue, feel free to post something at our [Discussions](https://github.com/coatl-dev/discussions/discussions).
